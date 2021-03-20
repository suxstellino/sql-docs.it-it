---
description: Guida per il controllo delle versioni delle righe e il blocco della transazione
title: Guida per il controllo delle versioni delle righe e il blocco della transazione
ms.custom: seo-dt-2019
ms.date: 03/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, transaction locking and row versioning
- transaction locking and row versioning guide
- lock compatibility matrix, [SQL Server]
- lock granularity and hierarchies, [SQL Server]
- deadlocks, [SQL Server]
- lock escalation, [SQL Server]
- lock partitioning, [SQL Server]
ms.assetid: 44fadbee-b5fe-40c0-af8a-11a1eecf6cb7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23647aad9cbd2e4dbad4437d88a9e44d7a4810e0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739631"
---
# <a name="transaction-locking-and-row-versioning-guide"></a>Guida per il controllo delle versioni delle righe e il blocco della transazione
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Nei sistemi con numerosi utenti, la gestione delle transazioni nei database spesso comporta problemi di prestazioni e contesa delle risorse. Con l'aumento progressivo del numero di utenti che accedono ai dati, diventa importante disporre di applicazioni che utilizzino le transazioni in modo efficiente. In questa guida vengono descritti i meccanismi di blocco e controllo delle versioni delle righe utilizzati dal [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per assicurare l'integrità fisica di ogni transazione e fornire informazioni su come le applicazioni possono controllare le transazioni con efficienza.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (da [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] a [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], se non specificato diversamente) e [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 
  
##  <a name="transaction-basics"></a><a name="Basics"></a> Nozioni fondamentali sulle transazioni  
 Una transazione è una sequenza di operazioni eseguite in un'unica unità logica di lavoro. Per essere considerata una transazione, un'unità logica di lavoro deve includere quattro proprietà, dette ACID (atomicità, consistenza, isolamento e durata):  
  
 **Atomicità**  
 Una transazione deve essere un'unità di lavoro atomica, ovvero devono essere eseguite tutte le modifiche dei dati oppure non ne deve essere eseguita nessuna.  
  
 **Coerenza**  
 Al termine della transazione i dati devono essere consistenti. Per salvaguardare l'integrità dei dati in un database relazionale, è necessario che alle modifiche eseguite dalla transazione vengano applicate tutte le regole. Al termine della transazione tutte le strutture di dati interne, ad esempio gli indici ad albero B o gli elenchi collegati doppiamente, devono risultare corrette.  
  
 **Isolamento**  
 Le modifiche eseguite da transazioni simultanee devono essere isolate dalle modifiche eseguite da qualsiasi altra transazione simultanea. Una transazione riconosce lo stato dei dati precedente alla modifica eseguita da una transazione simultanea oppure lo stato successivo al completamento della seconda transazione, ma non è in grado di riconoscere stati intermedi. Questa proprietà viene detta serializzabilità in quanto consente di ricaricare i dati iniziali e di eseguire nuovamente una serie di transazioni in modo da ripristinare lo stato dei dati successivo all'esecuzione delle transazioni originali.  
  
 **Durabilità**  
 Gli effetti di una transazione completamente durevole completata sono permanenti all'interno del sistema. Le modifiche eseguite rimangono valide anche in caso di errore del sistema. [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] e versioni successive consente transazioni durevoli ritardate. Le transazioni durevoli ritardate vengono sottoposte a commit prima che il record del log delle transazioni diventi permanente su disco. Per altre informazioni sulla durabilità delle transazioni ritardate, vedere [Controllo della durabilità delle transazioni](../relational-databases/logs/control-transaction-durability.md).  
  
 I programmatori SQL devono avviare e terminare le transazioni in modo da garantire la consistenza logica dei dati. In particolare, il programmatore deve definire la sequenza delle modifiche dei dati in modo che lo stato dei dati risulti consistente rispetto alle regole business dell'organizzazione. Le istruzioni di modifica devono quindi essere incluse in un'unica transazione in modo da poter applicare l'integrità fisica della transazione stessa in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
 Un sistema di database aziendale, quale un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], deve implementare i meccanismi necessari per garantire l'integrità fisica di ogni transazione. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] include:  
  
-   Meccanismi di blocco che salvaguardano l'isolamento delle transazioni.  
  
-   Meccanismi di registrazione che garantiscono la durabilità delle transazioni. Per le transazioni completamente durevoli, il record del log viene finalizzato su disco prima del commit delle transazioni stesse. Pertanto, anche in caso di errore dell'hardware del server, del sistema operativo o della stessa istanza del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], l'istanza usa i log delle transazioni al riavvio per eseguire automaticamente il rollback delle eventuali transazioni incomplete fino al punto in cui si è verificato l'errore di sistema. Le transazioni durevoli ritardate vengono sottoposte a commit prima che il record del log delle transazioni venga finalizzato su disco. Tali transazioni potrebbero andare perdute se si verifica un errore di sistema prima della finalizzazione del record del log su disco. Per altre informazioni sulla durabilità delle transazioni ritardate, vedere [Controllo della durabilità delle transazioni](../relational-databases/logs/control-transaction-durability.md).  
  
-   Caratteristiche di gestione delle transazioni che ne garantiscono l'atomicità e la consistenza. Una transazione avviata deve essere completata (sottoposta a commit). In caso contrario tutte le modifiche apportate ai dati dall'inizio della transazione verranno annullate dal [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Questa operazione viene definita come rollback di una transazione perché ripristina lo stato dei dati precedente alle modifiche.  
  
### <a name="controlling-transactions"></a>Controllo delle transazioni  
 Nelle applicazioni le transazioni vengono controllate principalmente specificandone l'inizio e la fine. Per specificare tali informazioni, è possibile utilizzare le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] o le funzioni API (Application Programming Interface). Il sistema deve essere in grado di gestire correttamente gli errori che comportano l'interruzione di una transazione prima che questa venga completata. Per altre informazioni, vedere [Transazioni](../t-sql/language-elements/transactions-transact-sql.md), [Transazioni in ODBC](../relational-databases/native-client/odbc/performing-transactions-in-odbc.md) e [Transazioni in SQL Server Native Client (OLEDB)](../relational-databases/native-client-ole-db-transactions/transactions.md).  
  
 Per impostazione predefinita, le transazioni vengono gestite a livello della connessione. Quando viene avviata una transazione su una connessione, tutte le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] eseguite su tale connessione fanno parte della transazione fino a quando questa non viene completata. Tuttavia, nell'ambito di una sessione MARS (Multiple Active Result Set) una transazione [!INCLUDE[tsql](../includes/tsql-md.md)] esplicita o implicita diventa una transazione con ambito batch gestita a livello di batch. Una volta completato il batch, se per la transazione con ambito batch non viene eseguito il commit o il rollback, il rollback viene eseguito automaticamente da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Uso di MARS (Multiple Active Result Set)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
#### <a name="starting-transactions"></a>Avvio di transazioni  
 Utilizzando le funzioni API e le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] è possibile avviare transazioni in un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] come transazioni esplicite, con autocommit o implicite.  
  
 **Transazioni esplicite**  
 Per transazione esplicita si intende una transazione di cui vengono definiti in modo esplicito l'inizio e la fine della transazione tramite una funzione dell'API o l'emissione delle istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] BEGIN TRANSACTION, COMMIT TRANSACTION, COMMIT WORK, ROLLBACK TRANSACTION o ROLLBACK WORK [!INCLUDE[tsql](../includes/tsql-md.md)]. Al termine della transazione, viene ripristinata la modalità precedente all'avvio della transazione esplicita, ovvero la modalità transazione implicita o autocommit.  
  
 In una transazione esplicita è possibile utilizzare tutte le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)], ad eccezione delle seguenti:  
  
:::row:::
    :::column:::
        ALTER DATABASE
    :::column-end:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT INDEX
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        CREATE FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        RECONFIGURE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT INDEX
    :::column-end:::
    :::column:::
        CREATE FULLTEXT INDEX
    :::column-end:::
    :::column:::
        RESTORE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BACKUP
    :::column-end:::
    :::column:::
        DROP DATABASE
    :::column-end:::
    :::column:::
        Stored procedure di sistema full-text
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        sp_dboption per l'impostazione delle opzioni di database o delle procedure di sistema che modificano il database master all'interno di transazioni implicite o esplicite.
    :::column-end:::
:::row-end:::

> [!NOTE]  
> UPDATE STATISTICS può essere utilizzato all'interno di una transazione esplicita. Tuttavia, UPDATE STATISTICS esegue il commit indipendentemente dalla transazione che la contiene e non è possibile eseguire il rollback.  
  
 **Transazioni con autocommit**  
 La modalità autocommit è la modalità predefinita per la gestione delle transazioni in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] e consente di eseguire il commit o il rollback di ogni istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] completata. Se un'istruzione viene completata correttamente, ne viene eseguito il commit. Se invece si verifica un errore, viene eseguito il rollback. In una connessione a un'istanza del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è sempre attivata la modalità predefinita autocommit, a meno che tale modalità non sia stata annullata da transazioni implicite o esplicite. La modalità autocommit è l'impostazione predefinita anche in ADO, OLE DB, ODBC e DB-Library.  
  
 **Transazioni implicite**  
 Se in una connessione è attivata la modalità di esecuzione implicita delle transazioni, dopo che è stato eseguito il commit o il rollback della transazione corrente, nell'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene avviata automaticamente una nuova transazione. Non è necessario specificare l'inizio di una transazione. È sufficiente eseguirne il commit o il rollback. La modalità di esecuzione implicita delle transazioni genera il concatenamento delle transazioni. La modalità di esecuzione implicita delle transazioni viene impostata tramite una funzione API o l'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] SET IMPLICIT_TRANSACTIONS ON.  Questa modalità è anche nota come Autocommit OFF, vedere [Metodo setAutoCommit in JDBC](../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md) 
  
 Se viene impostata la modalità di esecuzione implicita delle transazioni per una connessione, l'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] avvia automaticamente una transazione dopo l'esecuzione di una delle istruzioni seguenti:  
  
:::row:::
    :::column:::
        MODIFICA TABELLA
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DELETE
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::

-  **Transazioni con ambito batch**  
   Applicabile solo a MARS (Multiple Active Result Set). Una transazione definita a livello di ambito di batch è una transazione [!INCLUDE[tsql](../includes/tsql-md.md)] esplicita o implicita che inizia in una sessione MARS. Se per una transazione definita a livello di ambito di batch non è stato eseguito il commit o il rollback quando il batch è completato, il rollback viene eseguito automaticamente da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-  **Transazioni distribuite**  
   Le transazioni distribuite sono estese a due o più server noti come strumenti di gestione delle risorse. La gestione della transazione deve essere coordinata tra i vari strumenti di gestione delle risorse tramite un componente server denominato gestore delle transazioni. Ogni istanza del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] può fungere come strumento di gestione delle risorse per le transazioni distribuite coordinate da gestori delle transazioni quali [!INCLUDE[msCoName](../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) o altri gestori che supportano la specifica Open Group XA per l'elaborazione di transazioni distribuite. Per ulteriori informazioni, vedere la documentazione di MS DTC.  
  
   Una transazione eseguita in una singola istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ed estesa a due o più database è in effetti una transazione distribuita. Nell'istanza le transazioni distribuite vengono gestite internamente e appaiono all'utente come transazioni locali.  
  
   A livello dell'applicazione le transazioni distribuite vengono gestite in modo simile alle transazioni locali. Al termine della transazione l'applicazione ne richiede il commit o il rollback. Il commit di transazioni distribuite deve essere gestito dal gestore delle transazioni in modo diverso per evitare che, in seguito a un errore della rete, alcuni strumenti di gestione delle risorse eseguano il commit correttamente mentre altri eseguano il rollback della transazione. A tale scopo il processo di commit viene gestito in due fasi, ovvero una fase preparatoria e la fase di commit effettivo. Questo tipo di commit è noto come commit in due fasi.  
  
   -  **Fase preparatoria**  
      Quando il gestore delle transazioni riceve una richiesta di commit, invia un comando di preparazione a tutti gli strumenti di gestione delle risorse coinvolti nella transazione. Ogni strumento esegue quindi tutte le operazioni necessarie per rendere la transazione durevole. Tutti i buffer contenenti immagini del log relative alla transazione vengono inoltre trasferiti su disco. Dopo avere completato la fase preparatoria, lo strumento di gestione delle risorse comunica al gestore delle transazioni l'esito, positivo o negativo, della preparazione. Con [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] è stata introdotta la durabilità delle transazioni ritardate. Le transazioni durevoli ritardate vengono sottoposte a commit prima che le immagini di log della transazione vengano scaricate su disco. Per altre informazioni sulla durabilità delle transazioni ritardate, vedere [Controllo della durabilità delle transazioni](../relational-databases/logs/control-transaction-durability.md).  
  
   -  **Fase di commit**  
      Se la fase preparatoria ha esito positivo in tutti gli strumenti di gestione delle risorse, il gestore delle transazioni invia comandi di commit agli strumenti di gestione, i quali possono quindi completare il commit. Se il commit viene eseguito correttamente da parte di tutti gli strumenti di gestione delle risorse, il gestore delle transazioni invia all'applicazione una notifica di operazione riuscita. Se viene segnalato un tentativo di preparazione non riuscito in uno strumento di gestione delle risorse, il gestore delle transazioni invia un comando di rollback a tutti gli strumenti di gestione e segnala all'applicazione che il commit ha avuto esito negativo.  
  
      Le applicazioni [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] possono gestire le transazioni distribuite attraverso [!INCLUDE[tsql](../includes/tsql-md.md)] o le API di database. Per altre informazioni, vedere [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
#### <a name="ending-transactions"></a>Interruzione di transazioni  
 Per interrompere una transazione, è possibile utilizzare l'istruzione COMMIT o ROLLBACK oppure una funzione API corrispondente.  
  
-  **COMMIT**  
   Se una transazione viene completata correttamente, è necessario eseguirne il commit. L'istruzione COMMIT consente di integrare in modo permanente nel database tutte le modifiche apportate dalla transazione. L'istruzione consente inoltre di liberare le risorse, ad esempio i blocchi, utilizzate dalla transazione.  
  
-  **ROLLBACK**  
   Se in una transazione si verifica un errore oppure l'utente decide di annullare la transazione, è necessario eseguirne il rollback. L'istruzione ROLLBACK consente di annullare tutte le modifiche apportate ai dati ripristinandone lo stato corrente all'inizio della transazione. L'istruzione consente inoltre di liberare le risorse utilizzate dalla transazione.  
  
> [!NOTE]  
> Nelle connessioni abilitate per supportare più MARS, non è possibile eseguire il commit di una transazione avviata tramite una funzione API mentre vi sono richieste di esecuzione in sospeso. Qualsiasi tentativo di eseguire il commit di questo tipo di transazione durante l'esecuzione di operazioni in sospeso restituisce un errore.  
  
#### <a name="errors-during-transaction-processing"></a>Errori durante l'elaborazione delle transazioni  
 Se una transazione non viene eseguita correttamente a causa di un errore grave, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne viene eseguito automaticamente il rollback e tutte le risorse utilizzate dalla transazione vengono liberate. Se viene interrotta la connessione di rete tra il client e [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], quando la rete notifica l'istanza dell'interruzione viene eseguito il rollback delle transazioni in sospeso per tale connessione. La connessione viene interrotta anche se si verifica un errore nell'applicazione client oppure se il computer client viene spento o riavviato. Anche in questi casi, dopo la notifica dell'interruzione, nell'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene eseguito il rollback delle transazioni in sospeso. Questa operazione viene eseguita anche se l'applicazione client viene disconnessa.  
  
 Se in un batch si verifica un errore di runtime in un'istruzione, ad esempio la violazione di un vincolo, per impostazione predefinita in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene eseguito solo il rollback dell'istruzione che ha generato l'errore. È possibile modificare questo comportamento usando l'istruzione `SET XACT_ABORT`. Dopo l'esecuzione di `SET XACT_ABORT` ON, qualsiasi errore di runtime di un'istruzione comporta il rollback automatico della transazione corrente. `SET XACT_ABORT` non ha alcun effetto sugli errori di compilazione, ad esempio gli errori di sintassi. Per altre informazioni, vedere [SET XACT_ABORT &#40;Transact-SQL&#41;](../t-sql/statements/set-xact-abort-transact-sql.md).  
  
 Quando si verificano errori, è necessario includere un'azione di correzione (`COMMIT` o `ROLLBACK`) nel codice dell'applicazione. Uno strumento efficace per la gestione degli errori, inclusi quelli nelle transazioni, è rappresentato dal costrutto `TRY...CATCH` di [!INCLUDE[tsql](../includes/tsql-md.md)]. Per altre informazioni ed esempi che includono le transazioni, vedere [TRY...CATCH &#40;Transact-SQL&#41;](../t-sql/language-elements/try-catch-transact-sql.md). A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], è possibile usare l'istruzione `THROW` per generare un'eccezione e trasferire l'esecuzione in un blocco `CATCH` di un costrutto `TRY...CATCH`. Per altre informazioni, vedere [THROW &#40;Transact-SQL&#41;](../t-sql/language-elements/throw-transact-sql.md).  
  
##### <a name="compile-and-run-time-errors-in-autocommit-mode"></a>Errori di compilazione e di run-time in modalità autocommit  
 In modalità autocommit, a volte potrebbe sembrare che in un'istanza del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] venga eseguito il rollback di un intero batch anziché di una sola istruzione SQL. Ciò si verifica solo in caso di errori di compilazione, non di errori di run-time. Un errore di compilazione impedisce infatti a [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] di compilare un piano di esecuzione e non viene pertanto eseguita alcuna parte del batch. Anche se può sembrare che sia stato eseguito il rollback di tutte le istruzioni precedenti a quella che ha generato l'errore, in realtà l'errore impedisce l'esecuzione di qualsiasi elemento del batch. Nell'esempio seguente, a causa di un errore di compilazione non vengono eseguite le istruzioni `INSERT` del terzo batch. In apparenza viene eseguito il rollback delle prime due istruzioni `INSERT`, mentre in realtà tali istruzioni non vengono eseguite.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUSE (3, 'ccc');  -- Syntax error.  
GO  
SELECT * FROM TestBatch;  -- Returns no rows.  
GO  
```  
  
 Nell'esempio seguente, la terza istruzione `INSERT` genera un errore di run-time di chiave primaria duplicata. Poiché le prime due istruzioni `INSERT` hanno avuto esito positivo e ne è stato pertanto eseguito il commit, tali istruzioni rimangono valide anche dopo l'errore di run-time.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUES (1, 'ccc');  -- Duplicate key error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```  
  
 In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene utilizzata la risoluzione dei nomi posticipata, in base alla quale i nomi degli oggetti vengono risolti solo in fase di esecuzione. Nell'esempio seguente vengono eseguite le prime due istruzioni `INSERT` e ne viene quindi eseguito il commit. Le due righe inserite rimangono nella tabella `TestBatch` anche dopo l'errore di run-time generato dalla terza istruzione `INSERT` a causa di un riferimento a una tabella non esistente.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBch VALUES (3, 'ccc');  -- Table name error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```   
  
##  <a name="locking-and-row-versioning-basics"></a><a name="Lock_Basics"></a> Nozioni fondamentali sui blocchi e sul controllo delle versioni delle righe  
 Per garantire l'integrità delle transazioni e mantenere la consistenza dei database se più utenti accedono simultaneamente ai dati, nel [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] vengono utilizzati i meccanismi seguenti:  
  
-  **Blocco**    

   Ogni transazione richiede blocchi di diversi tipi sulle risorse, ad esempio sulle righe, le pagine o le tabelle dalle quali è dipendente. I blocchi impediscono alle altre transazioni di modificare le risorse in modo tale da creare problemi alla transazione che richiede il blocco. Ogni transazione libera i relativi blocchi quando non è più dipendente dalle risorse bloccate.  
  
-  **Controllo delle versioni delle righe**    

   Se viene attivato un livello di isolamento basato sul controllo delle versioni delle righe, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene le versioni di ogni riga modificata. Anziché proteggere tutte le operazioni di lettura con blocchi, nelle applicazioni è possibile specificare che una transazione utilizza le versioni di riga per visualizzare la versione dei dati esistente all'inizio della transazione o della query. Se si utilizza il controllo delle versioni delle righe, le possibilità che un'operazione di lettura possa bloccare altre transazioni vengono ridotte al minimo.  
  
 L'utilizzo di blocchi e il controllo delle versioni delle righe impediscono agli utenti di leggere dati di cui non è stato eseguito il commit e di modificare simultaneamente gli stessi dati. Se non si utilizzano queste funzionalità, è possibile che le query eseguite sui dati generino risultati imprevisti, restituendo dati di cui non è ancora stato eseguito il commit nel database.  
  
 Nelle applicazioni è possibile scegliere il livello di isolamento delle transazioni, ovvero il livello di protezione delle transazioni dalle modifiche eseguite da altre transazioni. Per personalizzare ulteriormente il funzionamento in base ai requisiti specifici di un'applicazione, è possibile specificare hint a livello di tabella per le singole istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)].  
  
### <a name="managing-concurrent-data-access"></a>Gestione dell'accesso ai dati simultaneo  
 Quando più utenti accedono a una risorsa contemporaneamente si parla di accesso simultaneo alla risorsa. L'accesso ai dati simultaneo richiede meccanismi per evitare gli effetti negativi derivanti dal tentativo, da parte di più utenti, di tentare di modificare le risorse che altri utenti stanno utilizzando in modo attivo.  
  
#### <a name="concurrency-effects"></a>Effetti della concorrenza  
 Le operazioni di modifica dei dati eseguite da un utente possono influire sulle operazioni di lettura o modifica degli stessi dati eseguite contemporaneamente da altri utenti. Tale fenomeno è denominato accesso simultaneo ai dati. Se il sistema di archiviazione dati non dispone di controllo della concorrenza, gli utenti potrebbero riscontrare gli effetti collaterali seguenti:  
  
-   **Perdita di aggiornamenti** 
  
     Il problema della perdita degli aggiornamenti si verifica quando due o più transazioni selezionano la stessa riga, la quale viene quindi aggiornata in base al valore selezionato inizialmente. Ogni transazione non è, infatti, a conoscenza dell'esecuzione delle altre transazioni. L'ultimo aggiornamento pertanto sovrascrive quelli eseguiti in precedenza dalle altre transazioni, con conseguente perdita di dati.  
  
     Si supponga, ad esempio, che due revisori creino una copia in formato elettronico del medesimo documento e successivamente modifichino e salvino la propria copia del documento. Con questa operazione la copia originale del documento viene sovrascritta e l'ultimo revisore che esegue il salvataggio sovrascrive la copia modificata dall'altro revisore. Questo problema potrebbe essere evitato impedendo a un revisore di accedere al file finché l'altro non abbia completato l'operazione ed eseguito il commit della transazione.  
  
-   **Dipendenza non sottoposta a commit (lettura dirty)**  
  
     La dipendenza uncommitted si verifica quando una transazione seleziona una riga mentre questa viene aggiornata da un'altra transazione. La seconda transazione legge i dati di cui non è ancora stato eseguito il commit, i quali potrebbero venire modificati dalla transazione che sta eseguendo l'aggiornamento della riga.  
  
     Si supponga, ad esempio, che un revisore stia modificando un documento in formato elettronico e che un secondo revisore apra una copia del documento contenente tutte le modifiche apportate fino a quel momento e lo distribuisca ai destinatari del documento. Si supponga inoltre che il primo revisore decida di eliminare le modifiche apportate e quindi salvi il documento. In questo caso, il documento distribuito contiene modifiche non più esistenti e quindi da considerare come non più valide. Questo problema potrebbe essere evitato impedendo a tutti gli utenti di leggere il documento modificato fino a quando il primo revisore non salva definitivamente le modifiche ed esegue il commit della transazione.  
  
-   **Analisi inconsistente (lettura non ripetibile)**  
  
     L'analisi inconsistente si verifica quando una transazione accede più volte alla stessa riga e legge ogni volta dati diversi. Questo problema è molto simile alla dipendenza uncommitted, in quanto una transazione modifica i dati che una seconda transazione sta leggendo. Nel caso dell'analisi inconsistente, tuttavia, la transazione di modifica ha già eseguito il commit dei dati letti dalla transazione di lettura. Con l'analisi inconsistente vengono, inoltre, eseguite due o più operazioni di lettura della stessa riga e a ogni lettura le informazioni vengono modificate da un'altra transazione. Da ciò deriva il termine lettura non ripetibile.  
  
     Si supponga, ad esempio, che un revisore legga lo stesso documento due volte e che nell'intervallo tra la prima e la seconda lettura l'autore del documento apporti alcune modifiche. Quando il revisore legge il documento la seconda volta, il testo risulta diverso, ovvero la lettura originale non è ripetibile. Questo problema potrebbe essere evitato impedendo all'autore di modificare il documento fino a quando il revisore non ha completato l'ultima lettura.  
  
-   **Lettura di righe fantasma**  
  
     Una lettura fantasma è una situazione che si verifica quando vengono eseguite due query identiche e la raccolta di righe restituite dalla seconda query è diversa. Nell'esempio seguente viene illustrato come può verificarsi questa condizione. Si supponga che le due transazioni seguenti vengano eseguite contemporaneamente. Le due istruzioni `SELECT` nella prima transazione possono restituire risultati diversi perché l'istruzione `INSERT` nella seconda transazione modifica i dati utilizzati da entrambe.  
  
    ```sql  
    --Transaction 1  
    BEGIN TRAN;  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    --The INSERT statement from the second transaction occurs here.  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    COMMIT;  
    ```  
  
    ```sql  
    --Transaction 2  
    BEGIN TRAN;  
    INSERT INTO dbo.employee  
      (Id, Name) VALUES(6 ,'New');  
    COMMIT;   
    ```  
  
-   **Letture di righe mancanti e doppie provocate dagli aggiornamenti della riga**  
  
    -   Riga aggiornata mancante o più visualizzazioni di una riga aggiornata  
  
         Le transazioni eseguite al livello `READ UNCOMMITTED` non generano blocchi condivisi per evitare che altre transazioni possano modificare i dati letti dalla transazione corrente. Le transazioni in esecuzione al livello READ COMMITTED generano blocchi condivisi, ma i blocchi di riga o pagina vengono rilasciati dopo la lettura della riga. In entrambi i casi, quando si esegue l'analisi di un indice, se un altro utente modifica la colonna della chiave di indice della riga durante la lettura, è possibile che la riga venga visualizzata nuovamente se la modifica principale sposta la riga di una posizione in avanti nell'analisi. Analogamente, la riga potrebbe non essere visualizzata se la modifica principale sposta la riga in una posizione già letta all'interno dell'indice. Per evitare questo problema, usare l'hint `SERIALIZABLE` o `HOLDLOCK` oppure il controllo delle versioni delle righe. Per altre informazioni, vedere [Hint di tabella &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
    -   Mancare una o più righe che non rappresentano l'obiettivo dell'aggiornamento  
  
         Quando si usa `READ UNCOMMITTED`, se la query legge le righe usando un'analisi di ordine di allocazione (usando le pagine di IAM), è possibile che si verifichi una perdita di righe se un'altra transazione provoca la suddivisione di una pagina. Questo non può verificarsi con Read committed perché durante la suddivisione di una pagina viene usato un blocco di tabella che non si verifica se la tabella non ha un indice cluster, poiché gli aggiornamenti non provocano suddivisioni di pagine.  
  
#### <a name="types-of-concurrency"></a>Tipi di concorrenza  
 Se molti utenti cercano di modificare contemporaneamente i dati di un database, è necessario implementare un sistema di controlli che impedisca che le modifiche apportate da un utente danneggino il lavoro di altri utenti. Questa funzionalità è denominata controllo della concorrenza.  
  
 I metodi per l'applicazione del controllo della concorrenza rientrano in due diverse categorie:  
  
-   Controllo della concorrenza **pessimistica**  
  
     Un sistema di blocchi impedisce agli utenti di modificare i dati con effetto su altri utenti. Dopo che un utente ha eseguito un'operazione che causa l'applicazione di un blocco, gli altri utenti non sono in grado di eseguire operazioni in conflitto con il blocco finché questo non viene rilasciato dal proprietario. Viene utilizzato l'aggettivo pessimistica in quanto questo tipo di controllo si utilizza principalmente in ambienti con elevata contesa dei dati, in cui il costo della protezione dei dati tramite i blocchi è inferiore al costo rappresentato dal rollback delle transazioni in caso di conflitti di concorrenza.  
  
-   Controllo della concorrenza **ottimistica**  
  
     Quando si utilizza il controllo della concorrenza ottimistica, la lettura dei dati da parte degli utenti non comporta il blocco dei dati. Quando un utente esegue l'aggiornamento dei dati, il sistema verifica se i dati sono stati modificati da un altro utente dopo la lettura. In questo caso, si verifica un errore. In genere, l'utente per cui viene visualizzato l'errore esegue il rollback della transazione e ricomincia dall'inizio. Viene utilizzato l'aggettivo ottimistica in quanto questo tipo di controllo si utilizza principalmente in ambienti con ridotta contesa dei dati, in cui il costo dell'esecuzione occasionale del rollback di una transazione è minore rispetto al costo del blocco dei dati durante la lettura.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta un intervallo del controllo della concorrenza. Gli utenti specificano il tipo di controllo della concorrenza selezionando i livelli di isolamento delle transazioni per le connessioni oppure le opzioni di concorrenza nei cursori. È possibile definire questi attributi utilizzando le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] o le proprietà e gli attributi delle API di database quali ADO, ADO.NET, OLE DB e ODBC.  
  
#### <a name="isolation-levels-in-the-ssdenoversion"></a>Livelli di isolamento nel [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 Le transazioni specificano un livello di isolamento che definisce il grado di isolamento di una transazione dalle modifiche alle risorse o ai dati apportate da altre transazioni. I livelli di isolamento sono descritti in termini di effetti secondari consentiti sulla concorrenza, ad esempio letture dirty o fantasma.  
  
 I livelli di isolamento delle transazioni controllano gli elementi seguenti:  
  
-   Se i blocchi vengono acquisiti alla lettura dei dati e quali tipi di blocchi vengono richiesti.  
-   La durata dei blocchi di lettura.  
-   Se un'operazione di lettura che fa riferimento a righe modificate da un'altra transazione:  
    -   Si blocca fino al rilascio del blocco esclusivo sulla riga.  
    -   Recupera la versione di cui è stato eseguito il commit della riga esistente al momento dell'avvio dell'istruzione o della transazione.  
    -   Legge la modifica dei dati di cui non è stato eseguito il commit.  
  
> [!IMPORTANT]  
> La scelta di un livello di isolamento delle transazioni non ha effetto sui blocchi acquisiti per proteggere le modifiche dei dati. Una transazione ottiene sempre un blocco esclusivo su qualsiasi dato da essa modificato, che mantiene fino al suo completamento, indipendentemente dal livello di isolamento impostato per la transazione. Per le operazioni di lettura, i livelli di isolamento delle transazioni definiscono essenzialmente il livello di protezione dagli effetti delle modifiche apportate da altre transazioni.  
  
 Un livello di isolamento inferiore aumenta la possibilità per un maggior numero di utenti di accedere ai dati contemporaneamente, ma anche la quantità di effetti di concorrenza (ad esempio letture dirty o perdita di aggiornamenti) potenzialmente verificabili. Per contro, un livello di isolamento elevato riduce i tipi di effetti di concorrenza verificabili, ma richiede più risorse di sistema e aumenta le probabilità che una transazione venga bloccata da un'altra. La scelta del livello di isolamento corretto dipende dal giusto equilibrio tra requisiti relativi all'integrità dei dati per l'applicazione e overhead di ogni livello di isolamento. Il livello di isolamento più elevato, Serializable, garantisce che una transazione recuperi esattamente gli stessi dati a ogni ripetizione di un'operazione di lettura. Tuttavia questo avviene applicando un livello di blocco con probabilità effetti su altri utenti in sistemi multiutente. Il livello di isolamento minimo, Read uncommitted, è in grado di recuperare i dati modificati ma di cui non è stato eseguito il commit da altre transazioni. In tale livello possono verificarsi tutti gli effetti secondari della concorrenza, ma l'assenza di blocco in lettura e di controllo delle versioni riduce al minimo l'overhead.  
  
##### <a name="database-engine-isolation-levels"></a>Livelli di isolamento del motore di database  
 Lo standard ISO definisce i livelli di isolamento seguenti, tutti supportati da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]:  
  
|Livello di isolamento|Definizione|  
|---------------------|----------------|  
|**Read uncommitted**|Il livello di isolamento delle transazioni più basso, sufficiente solo a garantire che i dati danneggiati fisicamente non vengano letti. In questo livello sono consentite le letture dirty, quindi una transazione può vedere le modifiche non ancora sottoposte al commit apportate da altre transazioni.|  
|**Read committed**|Consente a una transazione di leggere i dati letti in precedenza (ma non modificati) da un'altra transazione senza attendere che tale transazione venga completata. Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene i blocchi in scrittura (acquisiti sui dati selezionati) fino alla fine della transazione, ma rilascia i blocchi in lettura non appena viene eseguita l'operazione SELECT. Si tratta del livello predefinito del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].|  
|**Repeatable read**|Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene i blocchi in scrittura e lettura che vengono acquisiti sui dati selezionati fino alla fine della transazione. Tuttavia dal momento che i blocchi di intervallo non sono gestiti, è possibile che si verifichino letture fantasma.|  
|**Serializable**|Il livello più alto corrispondente all'isolamento completo di una transazione dall'altra. Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene i blocchi in scrittura e lettura acquisiti sui dati selezionati che vengono rilasciati alla fine della transazione. I blocchi di intervallo vengono acquisiti quando un'operazione SELECT utilizza una clausola WHERE con intervallo, specialmente per evitare letture fantasma.<br /><br /> **Nota:** È possibile che le operazioni e le transazioni DDL in tabelle replicate abbiano esito negativo quando è richiesto il livello di isolamento serializable. Ciò è dovuto al fatto che le query di replica utilizzano hint che possono essere incompatibili con il livello di isolamento serializable.|  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta inoltre due livelli di isolamento delle transazioni aggiuntivi che utilizzano il controllo delle versioni delle righe. il primo è un'implementazione dell'isolamento Read committed, l'altro è un livello di isolamento delle transazioni, Snapshot.  
  
|Livello di isolamento del controllo delle versioni delle righe|Definizione|  
|------------------------------------|----------------|  
|**Snapshot Read Committed (RCSI)**|Quando l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON, l'isolamento Read committed utilizza il controllo delle versioni delle righe per assicurare consistenza in lettura a livello di istruzioni. Le operazioni di lettura richiedono solo i blocchi di livello di tabella SCH-S e nessun blocco di pagina o di riga. Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] usa il controllo delle versioni delle righe per presentare a ogni istruzione uno snapshot dei dati consistente dal punto di vista transazionale, rappresentativo dei dati esistenti al momento dell'avvio dell'istruzione. Non vengono utilizzati blocchi per proteggere i dati da aggiornamenti eseguiti da altre transazioni. Una funzione definita dall'utente può restituire dati sottoposti a commit dopo che è iniziata l'istruzione contenente l'UDF.<br /><br /> Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` è impostata su OFF, l'impostazione predefinita, l'isolamento Read Committed usa blocchi condivisi per evitare che altre transazioni modifichino le righe mentre la transazione corrente esegue un'operazione di lettura. I blocchi condivisi impediscono inoltre che l'istruzione possa leggere righe modificate da altre transazioni, fino al completamento di tali transazioni. Entrambe le implementazioni sono conformi alla definizione ISO di isolamento di cui è stato eseguito il commit in lettura.|  
|**Snapshot**|Il livello di isolamento dello snapshot utilizza il controllo delle versioni delle righe per assicurare consistenza in lettura a livello di transazioni. Le operazioni di lettura non acquisiscono blocchi di pagina o di riga, ma solo blocchi della tabella SCH-S. Quando viene eseguita la lettura delle righe modificate da un'altra transazione, esse recuperano la versione della riga esistente all'avvio della transazione. È possibile usare l'isolamento snapshot in un database solo quando l'opzione di database `ALLOW_SNAPSHOT_ISOLATION` è impostata su ON. Per impostazione predefinita, l'opzione è impostata su OFF per i database utente.<br /><br /> **Nota:** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non supporta il controllo delle versioni dei metadati. Per questo motivo, esistono restrizioni sulle operazioni DDL che possono eseguite in una transazione esplicita che viene eseguita con l'isolamento dello snapshot. Le istruzioni DDL seguenti non sono consentite con l'isolamento snapshot dopo un'istruzione BEGIN TRANSACTION: ALTER TABLE, CREATE INDEX, CREATE XML INDEX, ALTER INDEX, DROP INDEX, DBCC REINDEX, ALTER PARTITION FUNCTION, ALTER PARTITION SCHEME o qualsiasi istruzione DDL di Common Language Runtime (CLR). Queste istruzioni sono consentite quando si usa l'isolamento dello snapshot all'interno di transazioni implicite. Per definizione, una transazione implicita è una sola istruzione che rende possibile l'applicazione della semantica dell'isolamento dello snapshot, anche con le istruzioni DDL. Le violazioni di questo principio possono causare l'errore 3961: `Snapshot isolation transaction failed in database '%.*ls' because the object accessed by the statement has been modified by a DDL statement in another concurrent transaction since the start of this transaction. It is not allowed because the metadata is not versioned. A concurrent update to metadata could lead to inconsistency if mixed with snapshot isolation.`|  
  
 Nella tabella seguente vengono illustrati gli effetti secondari della concorrenza attivati dai diversi livelli di isolamento.  
  
|Livello di isolamento|Lettura dirty|Nonrepeatable read|Lettura fantasma|  
|---------------------|----------------|------------------------|-------------|  
|**Read uncommitted**|Sì|Sì|Sì|  
|**Read committed**|No|Sì|Sì|  
|**Repeatable read**|No|No|Sì|  
|**Snapshot**|No|No|No|  
|**Serializable**|No|No|No|  
  
 Per altre informazioni sui tipi specifici di blocco o di controllo delle versioni delle righe controllati da ogni livello di isolamento delle transazioni, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
 Per impostare i livelli di isolamento delle transazioni, è possibile utilizzare [!INCLUDE[tsql](../includes/tsql-md.md)] o un'API di database.  
  
 **[!INCLUDE[tsql](../includes/tsql-md.md)]**                                      
 Gli script [!INCLUDE[tsql](../includes/tsql-md.md)] usano l'istruzione `SET TRANSACTION ISOLATION LEVEL`.  
  
 **ADO**  
 Nelle applicazioni ADO la proprietà `IsolationLevel` dell'oggetto **Connection** viene impostata suadXactReadUncommitted, adXactReadCommitted, adXactRepeatableRead o adXactReadSerializable.  
  
 **ADO.NET**  
 Le applicazioni ADO.NET che usano lo spazio dei nomi gestito `System.Data.SqlClient` possono chiamare il metodo `SqlConnection.BeginTransaction` e impostare l'opzione *IsolationLevel* su Unspecified, Chaos, ReadUncommitted, ReadCommitted, RepeatableRead, Serializable e Snapshot.  
  
 **OLE DB**  
 Quando si avvia una transazione, le applicazioni che usano OLE DB chiamano `ITransactionLocal::StartTransaction` con l'opzione *isoLevel* impostata su ISOLATIONLEVEL_READUNCOMMITTED, ISOLATIONLEVEL_READCOMMITTED, ISOLATIONLEVEL_REPEATABLEREAD, ISOLATIONLEVEL_SNAPSHOT o ISOLATIONLEVEL_SERIALIZABLE.  
  
 Quando si specifica il livello di isolamento delle transazioni in modalità autocommit, le applicazioni OLE DB possono impostare la proprietà DBPROPSET_SESSION DBPROP_SESS_AUTOCOMMITISOLEVELS su DBPROPVAL_TI_CHAOS, DBPROPVAL_TI_READUNCOMMITTED, DBPROPVAL_TI_BROWSE, DBPROPVAL_TI_CURSORSTABILITY, DBPROPVAL_TI_READCOMMITTED, DBPROPVAL_TI_REPEATABLEREAD, DBPROPVAL_TI_SERIALIZABLE, DBPROPVAL_TI_ISOLATED o DBPROPVAL_TI_SNAPSHOT.  
  
 **ODBC**  
 Le applicazioni ODBC chiamano `SQLSetConnectAttr` con l'opzione *Attribute* impostata su SQL_ATTR_TXN_ISOLATION e l'opzione *ValuePtr* impostata su SQL_TXN_READ_UNCOMMITTED, SQL_TXN_READ_COMMITTED, SQL_TXN_REPEATABLE_READ o SQL_TXN_SERIALIZABLE.  
  
 Per le transazioni snapshot, le applicazioni chiamano `SQLSetConnectAttr` con l'opzione Attribute impostata su SQL_COPT_SS_TXN_ISOLATION e l'opzione ValuePtr impostata su SQL_TXN_SS_SNAPSHOT. È possibile recuperare una transazione snapshot utilizzando SQL_COPT_SS_TXN_ISOLATION oppure SQL_ATTR_TXN_ISOLATION.  
  
##  <a name="locking-in-the-database-engine"></a><a name="Lock_Engine"></a> Blocchi nel motore di database  
 I blocchi costituiscono un meccanismo utilizzato nel [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per sincronizzare l'accesso allo stesso dato eseguito contemporaneamente da più utenti.  
  
 Prima che una transazione acquisisca una dipendenza sullo stato corrente del dato, ad esempio mediante un'operazione di lettura o modifica dei dati, deve proteggersi dagli effetti di un'altra transazione che esegue operazioni di modifica sugli stessi dati. A tale scopo, la transazione richiede il blocco del dato. I blocchi sono caratterizzati da diverse modalità e possono ad esempio essere condivisi o esclusivi. La modalità di blocco consente di definire il livello di dipendenza della transazione sui dati. Non è possibile concedere a una transazione un blocco che determina un conflitto con la modalità di blocco già concessa a un'altra transazione. Se una transazione richiede una modalità di blocco in conflitto con un blocco già concesso sugli stessi dati, l'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] metterà in pausa la transazione richiedente fino al rilascio del primo blocco.  
  
 Quando una transazione modifica un dato, il blocco di protezione della modifica viene mantenuto fino alla fine della transazione. La durata dei blocchi acquisiti da una transazione per proteggere le operazioni di lettura dipende dall'impostazione del livello di isolamento della transazione. I blocchi acquisiti da una transazione vengono rilasciati al completamento della transazione, ovvero in corrispondenza del commit o del rollback.  
  
 I blocchi non vengono in genere richiesti direttamente dalle applicazioni, ma gestiti internamente da un componente di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] denominato Gestione blocchi. Quando un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] elabora un'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)], Query Processor di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] determina le risorse cui accedere, nonché i tipi di blocchi necessari per proteggere le singole risorse in base al tipo di accesso e all'impostazione del livello di isolamento della transazione. Query Processor richiede quindi i blocchi appropriati a Gestione blocchi, che li concede a meno che non si sia verificato un conflitto con blocchi acquisiti da altre transazioni.  
  
## <a name="lock-granularity-and-hierarchies"></a>Granularità dei blocchi e gerarchie  
 Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] supporta il blocco con livelli di granularità diversi, che consente a tipi diversi di risorse di venire bloccati da una transazione. Per ridurre al minimo il costo associato ai blocchi, il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] blocca automaticamente le risorse in base al livello più adatto per l'attività specifica. L'applicazione di un blocco con un livello di granularità inferiore, ad esempio a livello di riga, aumenta la concorrenza, ma anche l'overhead. Il numero dei blocchi da gestire infatti aumenta in modo proporzionale al numero di righe bloccate. L'applicazione di un blocco con un livello di granularità superiore, ad esempio a livello di tabella, comporta invece un aumento dei costi in termini di concorrenza, in quanto bloccando un'intera tabella viene impedito ad altre transazioni di accedere a qualsiasi parte della tabella. L'overhead tuttavia risulta inferiore perché il numero di blocchi da gestire è minore.  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] deve spesso acquisire blocchi a più livelli di granularità per proteggere completamente una risorsa. Un gruppo di blocchi a più livelli di granularità viene denominato gerarchia di blocchi. Ad esempio, per proteggere completamente una lettura di un indice, è possibile che un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] debba acquisire blocchi condivisi sulle righe e blocchi preventivi condivisi sulle pagine e sulla tabella.  
  
 Nella tabella seguente sono illustrate le risorse che il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] può bloccare.  
  
|Risorsa|Descrizione|  
|--------------|-----------------|  
|RID|ID di riga utilizzato per bloccare una singola riga all'interno di un heap.|  
|KEY|Blocco di riga all'interno di un indice utilizzato per proteggere intervalli di chiavi nelle transazioni serializzabili.|  
|PAGE|Pagina di database di 8 kilobyte (KB), ad esempio una pagina di dati o di indice.|  
|EXTENT|Gruppo contiguo di otto pagine, ad esempio di dati o di indice.|  
|HoBT|Heap o albero B. Un blocco che protegge un albero B (indice) o l'heap delle pagine di dati in una tabella che non dispone di un indice cluster.|  
|TABLE|Tabella intera, compresi tutti i dati e gli indici.|  
|FILE|File di database.|  
|APPLICATION|Risorsa specificata dall'applicazione.|  
|METADATI|Blocchi a livello di metadati.|  
|ALLOCATION_UNIT|Unità di allocazione.|  
|DATABASE|Intero database.|  
  
> [!NOTE]  
> Gli HoBT e i blocchi TABLE possono essere interessati dall'opzione LOCK_ESCALATION di [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="lock-modes"></a><a name="lock_modes"></a> Modalità di blocco  
 Nel [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] le risorse vengono bloccate in base a modalità blocco diverse, che determinano il modo in cui le transazioni simultanee possono accedere alle risorse.  
  
 Nella tabella seguente sono incluse le modalità blocco delle risorse utilizzate in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
|Modalità blocco|Descrizione|  
|---------------|-----------------|  
|**Condiviso (S)**|Blocco utilizzato per operazioni di lettura che non comportano la modifica o l'aggiornamento dei dati, ad esempio l'istruzione SELECT.|  
|**Aggiornamento (U)**|Blocco utilizzato per risorse che possono essere aggiornate. Impedisce un tipo comune di deadlock che si verifica quando più sessioni leggono e bloccano le risorse ed eventualmente ne eseguono l'aggiornamento in un momento successivo.|  
|**Esclusivo (X)**|Blocco utilizzato per operazioni di modifica dei dati, ad esempio UPDATE, INSERT e DELETE. Garantisce che non possano essere eseguiti più aggiornamenti simultanei della stessa risorsa.|  
|**Intent**|Blocco utilizzato per definire una gerarchia di blocchi. Tra i tipi di blocchi preventivi sono inclusi i blocchi preventivi condivisi (IS), i blocchi preventivi esclusivi (IX) e i blocchi preventivi esclusivi condivisi (SIX).|  
|**Schema**|Blocco utilizzato quando è in esecuzione un'operazione dipendente dallo schema della tabella. Tra i tipi di blocchi di schema sono inclusi i blocchi di modifica dello schema (Sch-M) e i blocchi di stabilità dello schema (Sch-S).|  
|**Aggiornamento bulk (BU)**|Blocco usato durante le operazioni di copia bulk dei dati in una tabella quando viene specificato l'hint **TABLOCK**.|  
|**Intervalli di chiavi**|Modalità che consente di proteggere l'intervallo di righe lette da una query quando si utilizza il livello di isolamento della transazione Serializable. Garantisce che le altre transazioni non possano inserire righe da includere nelle query della transazione serializzabile se le query sono state nuovamente eseguite.|  
  
### <a name="shared-locks"></a><a name="shared"></a> Blocchi condivisi  
 I blocchi condivisi (S) consentono la lettura (SELECT) di una risorsa da parte di transazioni simultanee con il controllo della concorrenza pessimistica. senza che altre transazioni possano modificare i dati mentre il blocco condiviso (S) è applicato alla risorsa. I blocchi condivisi applicati a una risorsa vengono rilasciati non appena viene completata l'operazione di lettura, a meno che il livello di isolamento della transazione non sia stato impostato su Repeatable Read o superiore oppure non sia in uso l'hint di blocco per mantenere attivo il blocco per l'intera durata della transazione.  
  
### <a name="update-locks"></a><a name="update"></a> Blocchi di aggiornamento  
 I blocchi di aggiornamento (U) impediscono il verificarsi di una forma comune di deadlock. Durante una transazione Repeatable Read o Serializable la transazione legge i dati, acquisisce un blocco condiviso (S) sulla risorsa (pagina o riga) e modifica quindi i dati. Quest'ultima operazione comporta la conversione del blocco in blocco esclusivo (X). Se due transazioni acquisiscono blocchi in modalità condivisa su una risorsa e quindi eseguono un tentativo simultaneo di aggiornamento dei dati, una delle due transazioni eseguirà il tentativo di conversione del blocco in blocco esclusivo. La conversione di un blocco da condiviso a esclusivo non può essere eseguita immediatamente, in quanto il blocco esclusivo relativo a una transazione non è compatibile con il blocco condiviso relativo all'altra transazione. Si verifica pertanto un'attesa di blocco. La seconda transazione tenta di acquisire un blocco esclusivo (X) per la propria operazione di aggiornamento. Poiché, tuttavia, entrambe le transazioni eseguono una conversione in blocco esclusivo (X) e ogni transazione è in attesa che l'altra rilasci il blocco condiviso, si verifica un deadlock.  
  
 Per evitare questo problema, vengono utilizzati i blocchi di aggiornamento (U). Un blocco di aggiornamento (U) per una risorsa può essere acquisito da una sola transazione per volta. Se una transazione modifica una risorsa, il blocco di aggiornamento (U) viene convertito in blocco esclusivo (X).  
  
### <a name="exclusive-locks"></a><a name="exclusive"></a> Blocchi esclusivi  
 I blocchi esclusivi (X) impediscono l'accesso a una risorsa da parte di transazioni simultanee. Con un blocco esclusivo (X), nessun'altra transazione può modificare dati. Le operazioni di lettura possono essere eseguite solo utilizzando l'hint NOLOCK o il livello di isolamento Read Uncommitted.  
  
 Le istruzioni di modifica dei dati, ad esempio INSERT, UPDATE e DELETE, combinano entrambe le operazioni di modifica e di lettura. L'istruzione esegue innanzitutto le operazioni di lettura per acquisire dati prima di eseguire le operazioni di modifica necessarie. Le istruzioni di modifica dei dati, pertanto, richiedono in genere blocchi sia condivisi che esclusivi. Un'istruzione UPDATE, ad esempio, potrebbe comportare la modifica di righe in una tabella basata su un join con un'altra tabella. In questo caso, l'istruzione UPDATE richiede blocchi condivisi sulle righe lette nella tabella di join, nonché blocchi esclusivi sulle righe aggiornate.  
  
### <a name="intent-locks"></a><a name="intent"></a> Blocchi preventivi  
 In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] vengono utilizzati blocchi preventivi per proteggere l'applicazione di un blocco condiviso (S) o esclusivo (X) su una risorsa di livello inferiore nella gerarchia di blocchi. I blocchi preventivi, in inglese "intent lock", sono così denominati in quanto vengono acquisiti prima di un blocco a un livello inferiore e, pertanto, indicano l'intenzione di applicare blocchi a livelli inferiori.  
  
 I blocchi preventivi rispondono ai due obiettivi seguenti:  
  
-   Impedire alle altre transazioni di modificare la risorsa di livello superiore in un modo che annullerebbe la validità del blocco applicato al livello inferiore. 
-   Aumentare l'efficacia di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] nel rilevare i conflitti tra blocchi a un livello di granularità superiore.  
  
 Un blocco preventivo condiviso, ad esempio, viene richiesto a livello di tabella prima che nelle pagine o nelle righe all'interno della tabella vengano richiesti blocchi condivisi (S). L'impostazione di un blocco preventivo a livello di tabella consente di impedire l'acquisizione successiva da parte delle altre transazioni di un blocco esclusivo (X) sulla tabella contenente la pagina specifica. I blocchi preventivi garantiscono prestazioni migliori, in quanto per determinare se una transazione può acquisire un blocco sulla tabella, in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] vengono esaminati questi tipi di blocchi solo a livello di tabella. In questo modo, l'esame di ogni singola riga o pagina della tabella non risulta più necessario.  
  
<a name="lock_intent_table"></a> Tra i blocchi preventivi sono inclusi i blocchi preventivi condivisi (IS), i blocchi preventivi esclusivi (IX) e i blocchi preventivi esclusivi condivisi (SIX).  
  
|Modalità blocco|Descrizione|  
|---------------|-----------------|  
|**Blocco preventivo condiviso (IS)**|Consente di proteggere i blocchi condivisi richiesti o acquisiti su alcune risorse, ma non tutte, di livello inferiore nella gerarchia.|  
|**Blocco preventivo esclusivo (IX)**|Consente di proteggere i blocchi esclusivi richiesti o acquisiti su alcune risorse, ma non tutte, di livello inferiore nella gerarchia. IX rappresenta un superset di IS e consente inoltre di proteggere la richiesta di blocchi condivisi sulle risorse di livello inferiore.|  
|**Blocco condiviso preventivo esclusivo (SIX)**|Consente di proteggere i blocchi condivisi richiesti o acquisiti su tutte le risorse di livello inferiore nella gerarchia e i blocchi preventivi esclusivi su alcune risorse, ma non tutte, di livello inferiore. I blocchi IS simultanei possono essere applicati alle risorse di livello principale. L'acquisizione di un blocco SIX su una tabella, ad esempio, comporta inoltre l'acquisizione di blocchi preventivi esclusivi sulle pagine di cui è in corso la modifica e di blocchi esclusivi sulle righe modificate. In un momento specifico è possibile impostare su ogni risorsa un solo blocco SIX per impedire aggiornamenti della risorsa da parte di altre transazioni, le quali, tuttavia, possono leggere le risorse di livello inferiore nella gerarchia ottenendo blocchi IS a livello di tabella.|  
|**Aggiornamento preventivo (IU)**|Consente di proteggere i blocchi di aggiornamento richiesti o acquisiti su tutte le risorse di livello inferiore nella gerarchia. I blocchi IU vengono utilizzati solo sulle pagine. Tali blocchi vengono convertiti in blocchi IX se viene eseguita un'operazione di aggiornamento.|  
|**Condiviso preventivo aggiornamento (SIU)**|Combinazione di blocchi S e IU, in seguito all'acquisizione di tali blocchi in modo distinto e simultaneo mantenendo entrambi i blocchi. Una transazione, ad esempio, esegue una query con l'hint PAGLOCK e quindi effettua un'operazione di aggiornamento. La query con l'hint PAGLOCK acquisisce il blocco S, mentre l'operazione di aggiornamento acquisisce il blocco IU.|  
|**Aggiornamento preventivo esclusivo (UIX)**|Combinazione di blocchi U e IX, in seguito all'acquisizione di tali blocchi in modo distinto e simultaneo mantenendo entrambi i blocchi.|  
  
### <a name="schema-locks"></a><a name="schema"></a> Blocchi di schema  
 In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] i blocchi di modifica dello schema (Sch-M) vengono utilizzati durante un'operazione DDL (Data Definition Language) su una tabella, ad esempio l'aggiunta di una colonna o l'eliminazione di una tabella. Finché viene mantenuto attivo, il blocco Sch-M impedisce l'accesso simultaneo alla tabella, ovvero blocca tutte le operazioni esterne.  
  
 Alcune operazioni DML (Data Manipulation Language), ad esempio il troncamento delle tabelle, utilizzano i blocchi Sch-M per impedire l'accesso alle tabelle interessate da parte di operazioni simultanee.  
  
 In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] vengono utilizzati i blocchi di stabilità dello schema (Sch-S) durante la compilazione e l'esecuzione delle query. Questo tipo di blocco non esclude i blocchi transazionali, inclusi i blocchi esclusivi (X). Durante la compilazione di una query è pertanto possibile continuare l'esecuzione di altre transazioni, incluse quelle che prevedono blocchi X su tabelle. Non è tuttavia possibile eseguire su tabelle operazioni DDL e DML simultanee che acquisiscono blocchi Sch-M.  
  
### <a name="bulk-update-locks"></a><a name="bulk_update"></a> Blocchi di aggiornamento bulk  
 I blocchi aggiornamenti bulk consentono a più thread di eseguire operazioni simultanee di caricamento bulk dei dati nella stessa tabella, impedendo l'accesso alla tabella ai processi che non eseguono il caricamento bulk. Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilizza i blocchi di aggiornamento bulk quando vengono soddisfatte entrambe le condizioni seguenti.  
  
-   Si usa l'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] BULK INSERT o la funzione OPENROWSET(BULK) oppure si usa uno dei comandi Bulk Insert dell'API, ad esempio .NET SqlBulkCopy, le API Fast Load di OLEDB o le API Bulk Copy di ODBC per eseguire la copia bulk dei dati in una tabella.  
-   L'hint **TABLOCK** è specificato o l'opzione della tabella **table lock on bulk load** è impostata tramite **sp_tableoption**.  
  
> [!TIP]  
> A differenza dell'istruzione BULK INSERT, che contiene un blocco di aggiornamento bulk meno restrittivo, l'istruzione INSERT INTO...SELECT con l'hint TABLOCK contiene un blocco esclusivo (X) sulla tabella che non consente di inserire righe utilizzando operazioni di inserimento parallele.  
  
### <a name="key-range-locks"></a><a name="key_range"></a> Blocchi di intervalli di chiavi  
 I blocchi di intervalli di chiavi consentono di proteggere un intervallo di righe incluse in modo implicito in un set di record letto da un'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] in fase di utilizzo del livello di isolamento Serializable della transazione. Il blocco di intervalli di chiavi impedisce le letture fantasma, Tramite la protezione degli intervalli di chiavi tra righe, tali blocchi impediscono inserimenti o eliminazioni fantasma in un recordset a cui accede una transazione.  
  
## <a name="lock-compatibility"></a><a name="lock_compatibility"></a> Compatibilità tra blocchi  
 La compatibilità tra blocchi consente di stabilire se più transazioni possono acquisire blocchi sulla stessa risorsa contemporaneamente. Se una risorsa è già bloccata da un'altra transazione, è possibile autorizzare una nuova richiesta di blocco solo se la modalità di blocco richiesta è compatibile con quella esistente. In caso contrario, la transazione che richiede il nuovo blocco deve attendere il rilascio del blocco esistente oppure la scadenza dell'intervallo di timeout del blocco. Non vi sono, ad esempio, modalità di blocco compatibili con i blocchi esclusivi. In caso di presenza di un blocco esclusivo (X), nessun'altra transazione può acquisire altri blocchi di qualsiasi tipo, ovvero condivisi, di aggiornamento o esclusivi, sulla stessa risorsa fino a quando il blocco esclusivo (X) non viene rilasciato. Se invece è stato applicato a una risorsa un blocco condiviso (S), le altre transazioni possono acquisire un blocco condiviso o un blocco di aggiornamento (U) sullo stesso elemento anche prima del completamento della prima transazione. Le altre transazioni, tuttavia, possono acquisire un blocco esclusivo solo dopo il rilascio del blocco condiviso.  
  
<a name="lock_compat_table"></a> La tabella seguente illustra la compatibilità delle modalità di blocco più comuni.  
  
||Modalità concessa esistente||||||  
|------|---------------------------|------|------|------|------|------|  
|**Modalità richiesta**|**IS**|**S**|**U**|**IX**|**SIX**|**X**|  
|**Blocco preventivo condiviso (IS)**|Sì|Sì|Sì|Sì|Sì|No|  
|**Condiviso (S)**|Sì|Sì|Sì|No|No|No|  
|**Aggiornamento (U)**|Sì|Sì|No|No|No|No|  
|**Blocco preventivo esclusivo (IX)**|Sì|No|No|Sì|No|No|  
|**Blocco condiviso preventivo esclusivo (SIX)**|Sì|No|No|No|No|No|  
|**Esclusivo (X)**|No|No|No|No|No|No|  
  
> [!NOTE]  
> I blocchi preventivi esclusivi (IX) sono compatibili con la modalità di blocco IX perché tale modalità prevede l'intenzione di aggiornamento solo di alcune righe e non di tutte. Altre transazioni possono pertanto accedere alle risorse per la lettura o l'aggiornamento di alcune righe, a condizione che non utilizzino le righe già in fase di aggiornamento. Se viene eseguito il tentativo di aggiornare la stessa riga da parte di due transazioni, a entrambe viene concesso il blocco IX a livello di tabella e pagina. Un blocco X a livello di riga viene tuttavia concesso a una delle transazioni. L'altra transazione dovrà rimanere in attesa fino alla rimozione di tale blocco.  
  
<a name="lock_matrix"></a> Usare la tabella seguente per determinare la compatibilità di tutte le modalità di blocco disponibili in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 ![lock_conflicts](../relational-databases/media/LockConflictTable.png)  
  
## <a name="key-range-locking"></a>Blocco di intervalli di chiavi  
 I blocchi di intervalli di chiavi consentono di proteggere un intervallo di righe incluse in modo implicito in un set di record letto da un'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] in fase di utilizzo del livello di isolamento Serializable della transazione. Con questo livello di isolamento, una query eseguita durante una transazione deve ottenere sempre lo stesso set di righe ogni volta che viene eseguita all'interno della stessa transazione. Il blocco di intervalli di chiavi garantisce che ciò avvenga impedendo ad altre transazioni di inserire nuove righe le cui chiavi rientrerebbero nell'intervallo di chiavi letto dalla transazione serializzabile.  
  
 Il blocco di intervalli di chiavi impedisce le letture fantasma, nonché gli inserimenti fantasma in un recordset a cui accede una transazione attraverso la protezione degli intervalli di chiavi tra le righe.  
  
 Il blocco di intervalli di chiavi viene applicato a un indice specificando i valori iniziale e finale delle chiavi. In questo modo si impedisce ogni tentativo di inserimento, aggiornamento o eliminazione di una riga con un valore di chiave che rientra nell'intervallo in quanto tali operazioni dovrebbero acquisire un blocco sull'indice. Ad esempio, una transazione serializzabile può eseguire un'istruzione `SELECT` che legge tutte le righe i cui valori di chiave corrispondono alla condizione `BETWEEN 'AAA' AND 'CZZ'`. Un blocco di intervalli di chiavi applicato ai valori di chiave compresi tra **'** AAA **'** e **'** CZZ **'** impedisce ad altre transazioni di inserire righe con valori di chiave all'interno di tale intervallo, ad esempio **'** ADG **'** , **'** BBD **'** o **'** CAL **'** .  
  
### <a name="key-range-lock-modes"></a><a name="key_range_modes"></a> Modalità di blocco di intervalli di chiavi  
 I blocchi di intervalli di chiavi includono sia un componente intervallo sia un componente riga nel formato intervallo-riga:  
  
-   Intervallo indica la modalità di blocco che protegge l'intervallo di righe compreso tra due voci di indice consecutive.  
-   Riga indica la modalità di blocco che protegge la voce di indice.  
-   Modalità indica la modalità di blocco combinato utilizzata. Le modalità di blocco di intervalli di chiavi sono composte da due parti: La prima parte rappresenta il tipo di blocco usato per bloccare l'intervallo di indici (Range *T*), mentre la seconda rappresenta il tipo di blocco usato per bloccare una chiave specifica (*K*). Le due parti sono unite da un segno meno (-), ad esempio Range *T*-*K*.  
  
    |Range|Riga|Mode|Descrizione|  
    |-----------|---------|----------|-----------------|  
    |RangeS|S|RangeS-S|Intervallo condiviso, blocco di risorsa condiviso, analisi intervallo serializzabile.|  
    |RangeS|U|RangeS-U|Intervallo condiviso, blocco di risorsa di aggiornamento; analisi aggiornamento serializzabile.|  
    |RangeI|Null|RangeI-N|Intervallo di inserimento, blocco di risorsa Null; utilizzato per verificare gli intervalli prima di inserire una nuova chiave nell'indice.|  
    |RangeX|X|RangeX-X|Intervallo esclusivo, blocco di risorsa esclusivo; utilizzato per aggiornare una chiave di un intervallo.|  
  
> [!NOTE]  
> La modalità di blocco Null interna è compatibile con tutti gli altri tipi di blocco.  
  
 Le modalità di blocco di intervalli di chiavi sono basate sulla matrice di compatibilità illustrata di seguito che stabilisce quali blocchi sono compatibili con quelli ottenuti dalla sovrapposizione di chiavi e intervalli.  
  
||Modalità concessa esistente|||||||  
|------|---------------------------|------|------|------|------|------|------|  
|**Modalità richiesta**|**S**|**U**|**X**|**RangeS-S**|**RangeS-U**|**RangeI-N**|**RangeX-X**|  
|**Condiviso (S)**|Sì|Sì|No|Sì|Sì|Sì|No|  
|**Aggiornamento (U)**|Sì|No|No|Sì|No|Sì|No|  
|**Esclusivo (X)**|No|No|No|No|No|Sì|No|  
|**RangeS-S**|Sì|Sì|No|Sì|Sì|No|No|  
|**RangeS-U**|Sì|No|No|Sì|No|No|No|  
|**RangeI-N**|Sì|Sì|Sì|No|No|Sì|No|  
|**RangeX-X**|No|No|No|No|No|No|No|  
  
### <a name="conversion-locks"></a><a name="lock_conversion"></a> Blocchi di conversione  
 I blocchi di conversione vengono creati quando un blocco di intervalli di chiavi è sovrapposto a un altro blocco.  
  
|Blocco 1|Blocco 2|Blocco di conversione|  
|------------|------------|---------------------|  
|S|RangeI-N|RangeI-S|  
|U|RangeI-N|RangeI-U|  
|X|RangeI-N|RangeI-X|  
|RangeI-N|RangeS-S|RangeX-S|  
|RangeI-N|RangeS-U|RangeX-U|  
  
 I blocchi di conversione possono essere osservati per un breve periodo di tempo in diverse circostanze complesse, in alcuni casi durante l'esecuzione di processi simultanei.  
  
### <a name="serializable-range-scan-singleton-fetch-delete-and-insert"></a>Analisi intervallo serializzabile, recupero singleton, eliminazione e inserimento  
 Il blocco di intervalli di chiavi garantisce la serializzabilità quando si eseguono le operazioni seguenti:  
  
-   Query di analisi intervallo  
-   Recupero singleton di righe inesistenti  
-   Operazioni di eliminazione  
-   Operazioni di inserimento  
  
 Prima di eseguire un blocco di intervalli di chiavi, è necessario che siano soddisfatte le condizioni seguenti:  
  
-   Il livello di isolamento della transazione deve essere impostato su SERIALIZABLE.  
-   Query Processor deve utilizzare un indice per implementare il predicato di filtro dell'intervallo. La clausola WHERE in un'istruzione SELECT potrebbe, ad esempio, stabilire una condizione di intervallo con il predicato seguente: ColumnX BETWEEN N **'** AAA **'** AND N **'** CZZ **'** . Un blocco di intervalli di chiavi può essere acquisito solo se **ColumnX** è coperto da una chiave di indice.  
  
### <a name="examples"></a>Esempi  
 Gli esempi di blocco di intervalli di chiavi illustrati si basano sulla tabella e sull'indice seguenti.  
  
 ![btree](../relational-databases/media/btree4.png)  
  
#### <a name="range-scan-query"></a>Query di analisi intervallo  
 Per assicurare che una query per l'analisi di intervalli sia serializzabile, è necessario che la query restituisca gli stessi risultati a ogni esecuzione all'interno della stessa transazione. Altre transazioni non devono inserire nuove righe nella query per l'analisi di intervalli in quanto le righe potrebbero diventare inserimenti fantasma. Nella query seguente vengono ad esempio utilizzati la tabella e l'indice illustrati nella figura precedente:  
  
```sql  
SELECT name  
FROM mytable  
WHERE name BETWEEN 'A' AND 'C';  
```  
  
 I blocchi di intervalli di chiavi vengono applicati alle voci di indice che corrispondono all'intervallo delle righe di dati in cui il nome è compreso tra i valori `Adam` e `Dale`, in modo da impedire l'aggiunta o l'eliminazione di nuove righe qualificate nella query precedente. Sebbene il primo nome dell'intervallo sia `Adam`, il blocco di intervalli di chiavi in modalità RangeS-S applicato a questa voce di indice impedisce l'aggiunta prima del nome `Adam` di nuovi nomi che iniziano con la lettera A, ad esempio `Abigail`. In modo analogo, il blocco di intervalli di chiavi RangeS-S applicato alla voce di indice per `Dale` impedisce l'aggiunta dopo il nome `Carlos` di nuovi nomi che iniziano con la lettera C, ad esempio `Clive`.  
  
> [!NOTE]  
> Il numero di blocchi RangeS-S mantenuti attivi è *n*+1, dove *n* è il numero di righe che soddisfano la query.  
  
#### <a name="singleton-fetch-of-nonexistent-data"></a>Recupero singleton di dati inesistenti  
 Se una query di una transazione tenta di selezionare una riga inesistente, la successiva esecuzione della query all'interno della stessa transazione deve restituire il medesimo risultato. A nessun'altra transazione è consentito inserire la riga inesistente. Si consideri ad esempio la query seguente:  
  
```sql  
SELECT name  
FROM mytable  
WHERE name = 'Bill';  
```  
  
 Un blocco di intervalli di chiavi viene applicato alla voce di indice corrispondente all'intervallo di nomi compreso tra `Ben` e `Bing` in quanto il nome `Bill` verrebbe incluso alfabeticamente tra queste due voci di indice. Il blocco di intervalli di chiavi in modalità RangeS-S viene applicato alla voce di indice `Bing`, impedendo così ad altre transazioni di inserire valori, ad esempio `Bill`, tra le voci di indice `Ben` e `Bing`.  
  
#### <a name="delete-operation"></a>Operazioni di eliminazione  
 Quando viene eliminato un valore in una transazione, l'intervallo a cui appartiene tale valore non deve necessariamente rimanere bloccato per l'intera durata della transazione che esegue l'eliminazione. Per mantenere la serializzabilità è infatti sufficiente bloccare il valore della chiave eliminata fino al termine della transazione. Si consideri ad esempio l'istruzione DELETE seguente:  
  
```sql  
DELETE mytable  
WHERE name = 'Bob';  
```  
  
 Alla voce di indice corrispondente al nome `Bob` viene applicato un blocco esclusivo (X). Altre transazioni possono inserire o eliminare valori prima o dopo il valore eliminato `Bob`. I tentativi di lettura, inserimento o eliminazione del valore `Bob` vengono invece bloccati fino a quando non viene eseguito il commit o il rollback della transazione che esegue l'eliminazione.  
  
 È possibile eliminare intervalli usando tre modalità di blocco di base, ovvero il blocco di riga, il blocco di pagina o il blocco di tabella. La strategia di blocco, ovvero il blocco a livello di pagina, di tabella o di riga, viene scelta da Query Optimizer o specificata dall'utente tramite hint di Query Optimizer, ad esempio ROWLOCK, PAGLOCK o TABLOCK. Se si utilizza PAGLOCK o TABLOCK, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] rilascia immediatamente la pagina di indice se vengono eliminate tutte le righe dalla pagina. Quando invece si utilizza ROWLOCK, le righe eliminate vengono semplicemente contrassegnate come eliminate e vengono rimosse dalla pagina di indice in un momento successivo tramite un processo in background.  
  
#### <a name="insert-operation"></a>Operazioni di inserimento  
 Quando si inserisce un valore in una transazione, l'intervallo in cui è compreso non deve essere bloccato per l'intera durata della transazione che esegue l'operazione di inserimento. Per mantenere la serializzabilità è infatti sufficiente bloccare il valore della chiave inserita fino al termine della transazione. Si consideri ad esempio l'istruzione INSERT seguente:  
  
```sql  
INSERT mytable VALUES ('Dan');  
```  
  
 Il blocco di intervalli di chiavi in modalità RangeI-N viene inserito nella voce di indice corrispondente al nome David per verificare l'intervallo. Se il blocco viene concesso, viene inserita la voce `Dan` e viene impostato un blocco esclusivo (X) sul valore `Dan`. Il blocco di intervalli di chiavi in modalità RangeI-N è necessario solo per verificare l'intervallo e non viene mantenuto attivo per l'intera durata della transazione che esegue l'operazione di inserimento. Altre transazioni possono inserire o eliminare valori prima o dopo il valore inserito `Dan`. Le transazioni che tuttavia tentano di leggere, inserire o eliminare il valore `Dan` sono bloccate fino a quando non viene eseguito il commit o il rollback della transazione di inserimento.  
  
## <a name="lock-escalation"></a>Escalation blocchi
L'escalation dei blocchi è il processo di conversione di un numero elevato di blocchi specifici in un numero inferiore di blocchi generici, in modo da diminuire l'overhead di sistema aumentando, contemporaneamente, la probabilità di contese di concorrenza.

Man mano che il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] acquisisce blocchi di basso livello, inserisce anche blocchi preventivi negli oggetti contenenti gli oggetti di basso livello:

-   Quando si bloccano intervalli di chiavi di indice o righe, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] inserisce un blocco preventivo nelle pagine contenenti le chiavi o le righe.
-   Quando si bloccano pagine, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] inserisce un blocco preventivo negli oggetti di livello superiore contenenti le pagine. Oltre a un blocco preventivo sull'oggetto, sono necessari blocchi preventivi a livello di pagina sui seguenti oggetti:
    -  Pagine al livello foglia di indici non cluster
    -  Pagine di dati di indici cluster
    -  Pagine di dati dell'heap

Il [!INCLUDE[ssde_md](../includes/ssde_md.md)] potrebbe bloccare sia la riga che la pagina per la stessa istruzione per ridurre al minimo il numero di blocchi e la probabilità che si renda necessaria l'escalation blocchi. Il motore di database potrebbe, ad esempio, inserire blocchi di pagina in un indice non cluster, se è selezionato un numero sufficiente di chiavi contigue nel nodo dell'indice per soddisfare la query, e blocchi di riga nei dati.

Per eseguire l'escalation blocchi, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] tenta di modificare il blocco preventivo nella tabella nel blocco completo corrispondente, modificando, ad esempio, un blocco preventivo esclusivo (IX) in un blocco esclusivo (X) o un blocco preventivo condiviso (IS) in un blocco condiviso (S). Se il tentativo di escalation dei blocchi ha esito positivo e viene acquisito un blocco completo a livello di tabella, tutti i blocchi a livello di heap o albero B, a livello di pagina (PAGE) o a livello di riga (RID) attivati dalla transazione nell'heap o nell'indice vengono rilasciati. Se non è possibile acquisire un blocco completo, non viene eseguita alcuna escalation blocchi e il motore di database continua ad acquisire blocchi a livello di riga, chiave o pagina.

Il [!INCLUDE[ssde_md](../includes/ssde_md.md)] non esegue l'escalation blocchi a livello di riga o di intervallo di chiavi in blocchi a livello di pagina, ma direttamente in blocchi a livello di tabella. Analogamente, l'escalation dei blocchi a livello di pagina viene sempre eseguita in blocchi a livello di tabella. Il blocco delle tabelle partizionate può essere alzato al livello HoBT per la partizione associata anziché al blocco della tabella. Un blocco a livello HoBT non blocca necessariamente gli HoBT allineati per la partizione.

> [!NOTE]
> I blocchi a livello di HoBT di solito aumentano la concorrenza, ma introducono il potenziale per i deadlock quando le transazioni che bloccano partizioni differenti espandono i loro blocchi esclusivi alle altre partizioni. In rare occasioni, la granularità dei blocchi TABLE potrebbe offrire migliori prestazioni.

Se un tentativo di escalation blocchi non riesce per la presenza di blocchi in conflitto mantenuti da transazioni simultanee, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] ritenterà l'escalation blocchi ogni 1.250 blocchi aggiuntivi acquisiti dalla transazione.

Ogni evento di escalation si verifica principalmente a livello di una singola istruzione [!INCLUDE[tsql](../includes/tsql-md.md)]. All'avvio dell'evento, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] tenta di eseguire l'escalation di tutti i blocchi di proprietà della transazione corrente in tutte le tabelle a cui fa riferimento l'istruzione attiva, a condizione che vengano soddisfatti i requisiti di soglia di escalation. Se l'evento di escalation viene avviato prima dell'accesso dell'istruzione a una tabella, non viene eseguito alcun tentativo di escalation dei blocchi in tale tabella. Se l'escalation dei blocchi viene eseguita correttamente, verrà eseguita l'escalation di eventuali blocchi acquisiti dalla transazione in un'istruzione precedente e ancora presenti al momento dell'avvio dell'evento se l'istruzione corrente fa riferimento alla tabella e se la tabella è inclusa nell'evento di escalation.

Si consideri, ad esempio, una sessione in cui vengono eseguite le operazioni seguenti:

-  Avvio di una transazione.
-  Aggiorna `TableA`. In questo modo, vengono generati blocchi di riga esclusivi in TableA che vengono mantenuti fino al completamento della transazione.
-  Aggiorna `TableB`. In questo modo, vengono generati blocchi di riga esclusivi in TableB che vengono mantenuti fino al completamento della transazione.
-  Esegue un'operazione SELECT per unire in join `TableA` e `TableC`. Il piano di esecuzione della query richiede il recupero delle righe da `TableA` prima che le righe vengano recuperate da `TableC`.
-  L'istruzione SELECT attiva l'escalation blocchi durante il recupero delle righe da `TableA` e prima dell'accesso a `TableC`.

Se l'escalation blocchi viene eseguita correttamente, verrà eseguita l'escalation solo dei blocchi attivati dalla sessione in `TableA`, inclusi i blocchi condivisi dovuti all'istruzione SELECT e i blocchi esclusivi dovuti all'istruzione UPDATE precedente. Mentre per determinare se è necessario eseguire l'escalation blocchi vengono conteggiati solo i blocchi acquisiti dalla sessione in `TableA` per l'istruzione SELECT, dopo che l'escalation ha avuto esito positivo viene eseguita l'escalation di tutti i blocchi attivati dalla sessione in `TableA` in un blocco esclusivo nella tabella e tutti gli altri blocchi con granularità inferiore, inclusi i blocchi preventivi, in `TableA` vengono rilasciati.

Non viene eseguito alcun tentativo di escalation blocchi in `TableB` in quanto nell'istruzione SELECT non vi erano riferimenti attivi a `TableB`. Analogamente, non viene eseguito alcun tentativo di escalation blocchi in `TableC` in quanto al momento dell'escalation non era ancora stato eseguito l'accesso alla tabella.

### <a name="lock-escalation-thresholds"></a>Soglie di escalation dei blocchi

Viene generata l'escalation blocchi se tale escalation non è disabilitata nella tabella usando l'opzione `ALTER TABLE SET LOCK_ESCALATION` e se si verifica una delle seguenti condizioni:

-  Una sola istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] acquisisce almeno 5.000 blocchi su una sola tabella non partizionata o indice.
-  Una sola istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] acquisisce almeno 5.000 blocchi in una sola partizione di una tabella partizionata e l'opzione `ALTER TABLE SET LOCK_ESCALATION` è impostata su AUTO.
-  Il numero di blocchi in un'istanza del [!INCLUDE[ssde_md](../includes/ssde_md.md)] supera la soglia di memoria o di configurazione.

Se l'escalation blocchi non è possibile per la presenza di conflitti di blocchi, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] attiva periodicamente l'escalation blocchi ogni 1.250 nuovi blocchi acquisiti.

### <a name="escalation-threshold-for-a-transact-sql-statement"></a>Soglia di escalation per un'istruzione Transact-SQL
Quando il [!INCLUDE[ssde_md](../includes/ssde_md.md)] controlla la presenza di possibili escalation ogni 1.250 nuovi blocchi acquisiti, viene eseguita un'escalation blocchi solo se un'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] ha acquisito almeno 5.000 blocchi per un singolo riferimento di una tabella. L'escalation blocchi viene attivata quando un'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] acquisisce almeno 5.000 blocchi per un singolo riferimento di una tabella. L'escalation dei blocchi non viene ad esempio attivata se un'istruzione acquisisce 3000 blocchi in un indice e 3000 blocchi in un altro indice della stessa tabella. Allo stesso modo, l'escalation dei blocchi non viene attivata se un'istruzione presenta un self join su una tabella e ogni riferimento alla tabella acquisisce solo 3.000 blocchi nella tabella.

L'escalation dei blocchi viene eseguita solo per le tabelle a cui è stato effettuato l'accesso al momento dell'attivazione dell'escalation. Si supponga che un'unica istruzione SELECT corrisponda a un join che accede a tre tabelle in questo ordine: `TableA`, `TableB` e `TableC`. L'istruzione acquisisce 3.000 blocchi di riga nell'indice cluster per `TableA` e almeno 5.000 blocchi di riga nell'indice cluster per `TableB` ma non ha ancora avuto accesso a `TableC`. Quando il [!INCLUDE[ssde_md](../includes/ssde_md.md)] rileva che l'istruzione ha acquisito almeno 5.000 blocchi di riga in `TableB`, tenta l'escalation di tutti i blocchi posti dalla transazione corrente su `TableB`. Tenta anche l'escalation di tutti i blocchi posti dalla transazione corrente su `TableA`, ma poiché il numero di blocchi su `TableA` è minore di 5.000, l'escalation non riesce. In `TableC` non viene eseguito alcun tentativo di escalation in quanto al momento dell'escalation non è ancora stato effettuato l'accesso alla tabella.

### <a name="escalation-threshold-for-an-instance-of-the-database-engine"></a>Soglia di escalation per un'istanza del Motore di database
Ogni volta che il numero di blocchi è maggiore della soglia di memoria per l'escalation blocchi, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] avvia l'escalation blocchi. La soglia di memoria dipende dall'impostazione dell'[opzione di configurazione locks](../database-engine/configure-windows/configure-the-locks-server-configuration-option.md):

-   Se l'opzione **locks** viene impostata sul valore predefinito 0, la soglia di escalation blocchi viene raggiunta quando la memoria usata dagli oggetti blocco rappresenta il 24% della memoria usata dal motore di database, esclusa la memoria AWE. La struttura di dati utilizzata per rappresentare un blocco è lunga approssimativamente 100 byte. Si tratta di una soglia dinamica, in quanto il motore di database acquisisce e libera la memoria dinamicamente per adattarsi ai diversi carichi di lavoro.

-   Se per l'opzione **locks** viene impostato un valore diverso da 0, la soglia di escalation blocchi corrisponde al 40%, o meno in caso di elevato uso della memoria, del valore dell'opzione.

Il [!INCLUDE[ssde_md](../includes/ssde_md.md)] può scegliere qualsiasi istruzione attiva da qualsiasi sessione per l'escalation e ogni 1.250 nuovi blocchi sceglierà istruzioni per l'escalation a condizione che la memoria dei blocchi usata nell'istanza rimanga al di sopra della soglia.

### <a name="escalating-mixed-lock-types"></a>Escalation di tipi di blocco misti
Quando viene eseguita l'escalation, il blocco selezionato per l'heap o l'indice è sufficientemente forte per soddisfare i requisiti del blocco di livello inferiore più restrittivo.

Si consideri, ad esempio, una sessione in cui vengono eseguite le operazioni seguenti:

-  Avvio di una transazione.
-  Aggiornamento di una tabella contenente un indice cluster.
-  Esecuzione di un'istruzione SELECT che fa riferimento alla stessa tabella.

L'istruzione UPDATE acquisisce i blocchi seguenti:

-  Blocchi esclusivi (X) nelle righe di dati aggiornate.
-  Blocchi preventivi esclusivi (IX) nelle pagine di indice cluster contenenti queste righe.
-  Un blocco IX nell'indice cluster e un altro nella tabella.

L'istruzione SELECT acquisisce i blocchi seguenti:

-  Blocchi condivisi (S) in tutte le righe di dati lette, a meno che la riga sia già protetta da un blocco X dovuto all'istruzione UPDATE.
-  Blocchi preventivi condivisi in tutte le pagine di indice non cluster contenenti queste righe, a meno che la pagina non sia già protetta da un blocco IX.
-  Nessun blocco nell'indice cluster o nella tabella in quanto questi oggetti sono già protetti da blocchi IX.

Se l'istruzione SELECT acquisisce blocchi sufficienti per attivare l'escalation dei blocchi e l'escalation viene eseguita correttamente, il blocco IX nella tabella viene convertito in un blocco X e tutti i blocchi a livello di riga, pagina e indice vengono rilasciati. Sia le operazioni di aggiornamento che quelle di lettura sono protette dal blocco X nella tabella.

### <a name="reducing-locking-and-escalation"></a>Riduzione di blocchi ed escalation
Nella maggior parte dei casi, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] offre prestazioni ottimali quando usa le impostazioni predefinite per le operazioni di blocco ed escalation blocchi. Se un'istanza del [!INCLUDE[ssde_md](../includes/ssde_md.md)] genera molti blocchi e si verificano escalation blocchi frequenti, valutare la possibilità di ridurre la quantità di blocchi eseguendo le operazioni seguenti:

-   Usare un livello di isolamento che non generi blocchi condivisi per le operazioni di lettura:
    -  Livello di isolamento READ COMMITTED quando l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON.
    -  Livello di isolamento SNAPSHOT.
    -  Livello di isolamento READ UNCOMMITTED. Questo livello può essere utilizzato solo per sistemi che possono funzionare con letture dirty.    
  
    > [!NOTE]
    > La modifica del livello di isolamento interessa tutte le tabelle nell'istanza del [!INCLUDE[ssde_md](../includes/ssde_md.md)].

-   Usare gli hint di tabella PAGLOCK o TABLOCK in modo che il motore di database usi blocchi a livello di pagina, heap o indice invece di blocchi di riga. Utilizzando questa opzione, tuttavia, si aumentano i problemi legati a utenti che bloccano altri utenti che tentano di accedere agli stessi dati e pertanto non è consigliabile l'utilizzo in sistemi con molti utenti simultanei.

-   Per le tabelle partizionate usare l'opzione LOCK_ESCALATION di [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) per aumentare i blocchi al livello HoBT invece della tabella o disabilitare l'escalation blocchi.

-   Suddividere le operazioni batch di grandi dimensioni in diverse operazioni più piccole. Si supponga, ad esempio, di aver eseguito la query seguente per rimuovere centinaia di migliaia di record obsoleti da una tabella di controllo, per poi scoprire che l'operazione ha causato un'escalation blocchi che blocca altri utenti:
   
    ```sql
    DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    ```

    Rimuovendo questi record alcune centinaia alla volta, è possibile ridurre notevolmente il numero di blocchi che si accumulano per ogni transazione ed evitare l'escalation blocchi. Ad esempio:

    ```sql
    SET ROWCOUNT 500
    delete_more:
      DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    IF @@ROWCOUNT > 0 GOTO delete_more
    SET ROWCOUNT 0
    ```

-   Ridurre l'impatto a livello di blocco di una query aumentandone il più possibile l'efficienza. Le scansioni estese o la presenza di un numero elevato di ricerche di segnalibri possono aumentare la probabilità di escalation blocchi. Inoltre, aumenta la probabilità di deadlock e, in genere, vi sono effetti negativi sulla concorrenza e sulle prestazioni. Dopo aver individuato la query che causa l'escalation blocchi, cercare opportunità di creare nuovi indici o di aggiungere colonne a un indice esistente per rimuovere le analisi di indici o le scansioni di tabella e per ottimizzare l'efficienza delle ricerche nell'indice. Per eseguire un'analisi automatica degli indici per la query, provare a usare [Ottimizzazione guidata motore di database](../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Per altre informazioni, vedere [Esercitazione: Ottimizzazione guidata motore di database](../tools/dta/tutorial-database-engine-tuning-advisor.md).
    Uno degli obiettivi di questa ottimizzazione è fare in modo che le ricerche nell'indice restituiscano il minor numero di righe possibile per ridurre al minimo il costo delle ricerche tramite segnalibri (massimizzazione della selettività dell'indice per la query specifica). Se il [!INCLUDE[ssde_md](../includes/ssde_md.md)] stima che un operatore logico Bookmark Lookup potrebbe restituire molte righe, può usare PREFETCH per eseguire la ricerca tramite segnalibri. Se il [!INCLUDE[ssde_md](../includes/ssde_md.md)] usa PREFETCH per una ricerca tramite segnalibri, deve aumentare il livello di isolamento della transazione a lettura ripetibile per una parte della query. Questo significa che un'operazione simile nell'aspetto a un'istruzione SELECT con livello di isolamento Read Committed potrebbe acquisire molte migliaia di blocchi di chiave (sia nell'indice cluster che in un indice non cluster) causando il superamento delle soglie di escalation blocchi per una query. Questo è particolarmente importante se si rileva che il blocco con escalation è un blocco di tabella condiviso, che, tuttavia, non è comune con il livello di isolamento Read Committed predefinito. Se una clausola Bookmark Lookup WITH PREFETCH causa l'escalation, è consigliabile aggiungere altre colonne all'indice non cluster visualizzato nell'operatore logico Index Seek o Index Scan sotto l'operatore logico Bookmark Lookup nel piano di query. È possibile creare un indice di copertura, ovvero un indice che includa tutte le colonne di una tabella usate nella query, o almeno un indice a copertura delle colonne usate per i criteri di join o nella clausola WHERE se includere tutto nell'elenco di selezione delle colonne è poco pratico.
    Anche un join a cicli annidati può usare PREFETCH e questo causa lo stesso comportamento di blocco.
   
-   Non è possibile eseguire l'escalation blocchi se uno SPID diverso mantiene attualmente attivo un blocco di tabella non compatibile. La destinazione di un'escalation blocchi è sempre un blocco di tabella e mai un blocco di pagina. Inoltre, se un tentativo di escalation blocchi ha esito negativo perché un altro SPID mantiene attivo un blocco TAB non compatibile, la query che ha tentato l'escalation non viene bloccata durante l'attesa di un blocco TAB. Continua invece ad acquisire blocchi al livello originale, più granulare (riga, chiave o pagina), effettuando periodicamente ulteriori tentativi di escalation. Pertanto, un metodo per evitare l'escalation blocchi in una determinata tabella consiste nell'acquisire e mantenere attivo un blocco su una connessione diversa non compatibile con il tipo di blocco oggetto dell'escalation. Un blocco IX (preventivo esclusivo) a livello di tabella non blocca alcuna riga o pagina, ma è comunque non compatibile con un blocco TAB S (condiviso) o X (esclusivo). Si supponga, ad esempio, di dover eseguire un processo batch che modifica un numero elevato di righe nella tabella mytable e che ha causato un blocco che si verifica a causa dell'escalation blocchi. Se il processo viene sempre completato in meno di un'ora, è possibile creare un processo [!INCLUDE[tsql](../includes/tsql-md.md)] contenente il codice seguente e pianificare il nuovo processo per l'avvio alcuni minuti prima dell'ora di inizio del processo batch:
  
    ```sql
    BEGIN TRAN
    SELECT * FROM mytable (UPDLOCK, HOLDLOCK) WHERE 1=0
    WAITFOR DELAY '1:00:00'
    COMMIT TRAN
    ```
   
    Questa query acquisisce e mantiene attivo un blocco IX su mytable per un'ora, impedendo così l'escalation blocchi sulla tabella durante tale periodo di tempo. Questo batch non modifica i dati né blocca altre query, a meno che l'altra query non forzi un blocco di tabella con l'hint TABLOCK o se un amministratore ha disabilitato i blocchi di pagina o di riga usando una stored procedure sp_indexoption.

È inoltre possibile utilizzare i flag di traccia 1211 e 1224 per disabilitare alcune o tutte le escalation dei blocchi. Tuttavia, questi [flag di traccia](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) disabilitano l'escalation blocchi a livello globale per l'intero [!INCLUDE[ssde_md](../includes/ssde_md.md)]. L'escalation blocchi ha uno scopo molto utile nel [!INCLUDE[ssde_md](../includes/ssde_md.md)] ottimizzando l'efficienza delle query che potrebbero risultare altrimenti rallentate dal sovraccarico dell'acquisizione e del rilascio di diverse migliaia di blocchi. L'escalation blocchi è anche utile per ridurre al minimo la memoria necessaria per tenere traccia dei blocchi. La memoria che il [!INCLUDE[ssde_md](../includes/ssde_md.md)] può allocare dinamicamente per le strutture di blocco è limitata. Se si disabilita l'escalation blocchi e la memoria di blocco cresce abbastanza, i tentativi di allocare ulteriori blocchi per qualsiasi query potrebbero non riuscire con gli errori seguenti:

```Error: 1204, Severity: 19, State: 1
The SQL Server cannot obtain a LOCK resource at this time. Rerun your statement when there are fewer active users or ask the system administrator to check the SQL Server lock and memory configuration.
```

> [!NOTE]
> Quando si verifica l'[errore 1204](../relational-databases/errors-events/mssqlserver-1204-database-engine-error.md) viene arrestata l'elaborazione dell'istruzione corrente e viene eseguito il rollback della transazione attiva. Il rollback stesso può bloccare gli utenti o causare un tempo di ripristino del database lungo se si riavvia il servizio di database.

> [!NOTE]
> L'uso di un hint di blocco, come ROWLOCK, modifica solo il piano di blocco iniziale. Gli hint di blocco non impediscono l'escalation blocchi. 

Inoltre, monitorare l'escalation blocchi usando l'evento esteso (xEvent) `lock_escalation`, come nell'esempio seguente:

```sql
-- Session creates a histogram of the number of lock escalations per database 
CREATE EVENT SESSION [Track_lock_escalation] ON SERVER 
ADD EVENT sqlserver.lock_escalation(SET collect_database_name=(1),collect_statement=(1)
    ACTION(sqlserver.database_id,sqlserver.database_name,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,sqlserver.sql_text,sqlserver.username))
ADD TARGET package0.histogram(SET source=N'sqlserver.database_id')
GO
```

> [!IMPORTANT]
> È consigliabile usare l'evento esteso (xEvent) `lock_escalation` anziché la classe di evento Lock:Escalation in SQL Trace o SQL Profiler.

## <a name="dynamic-locking"></a><a name="dynamic_locks"></a> Blocco dinamico
 L'utilizzo dei blocchi a basso livello, ad esempio blocchi di riga, aumenta la concorrenza riducendo la probabilità che due transazioni richiedano i blocchi sullo stesso elemento di dati contemporaneamente. L'utilizzo dei blocchi a basso livello inoltre aumenta il numero dei blocchi e delle risorse necessarie a gestirli. I blocchi di tabella e di pagina ad alto livello comportano invece una diminuzione dell'overhead, ma a spese della concorrenza.  
  
 ![Costo dei blocchi rispetto a costo della concorrenza](../relational-databases/media/lockcht.png) 
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] usa una strategia di blocco dinamico per determinare la combinazione di blocchi più efficiente. Il tipo di blocco più appropriato per l'esecuzione di una particolare query viene determinato in modo automatico da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] in base alle caratteristiche dello schema e della query. Ad esempio, per ridurre l'overhead associato al blocco quando viene eseguita l'analisi di un indice, Query Optimizer potrebbe scegliere un blocco a livello di pagina per l'indice.  
  
 L'utilizzo del blocco dinamico offre i vantaggi seguenti:  
  
-   Amministrazione del database semplificata. Gli amministratori di database non devono gestire le soglie di escalation dei blocchi.  
-   Prestazioni ottimizzate. L'overhead di sistema viene ridotto al minimo in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tramite l'utilizzo di blocchi adatti al tipo di attività eseguita.  
-   Gli sviluppatori di applicazioni possono dedicarsi interamente alle operazioni di sviluppo, in quanto i blocchi vengono modificati in modo automatico in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
 A partire da [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)], il comportamento dell'escalation dei blocchi è stato modificato con l'introduzione dell'opzione `LOCK_ESCALATION`. Per altre informazioni, vedere l'opzione `LOCK_ESCALATION` di [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md). 
   
## <a name="deadlocks"></a><a name="deadlocks"></a> Deadlock  
 Un deadlock si verifica quando due o più attività si bloccano reciprocamente in modo permanente, in quanto ognuna delle attività prevede un blocco su una risorsa che le altre attività stanno cercando di bloccare. Ad esempio:  
  
-   La transazione A acquisisce un blocco condiviso sulla riga 1.  
-   La transazione B acquisisce un blocco condiviso sulla riga 2.  
-   La transazione A richiede ora un blocco esclusivo sulla riga 2 ed è bloccata fino al completamento della transazione B e del relativo rilascio del blocco condiviso sulla riga 2.  
-   La transazione B richiede ora un blocco esclusivo sulla riga 1 ed è bloccata fino al completamento della transazione A e del relativo rilascio del blocco condiviso sulla riga 1.  
  
 La transazione A non può essere completata fino al completamento della transazione B, ma la transazione B è bloccata dalla transazione A. Questa condizione è detta anche dipendenza ciclica: la transazione A è dipendente dalla transazione B e la transazione B chiude il cerchio con una dipendenza rispetto alla transazione A.  
  
 Entrambe le transazioni in un deadlock restano in attesa per sempre fino a quando il deadlock non viene interrotto da un processo esterno. La funzionalità di monitoraggio dei deadlock del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ricerca periodicamente le attività interessate da un deadlock. Se viene rilevata una dipendenza ciclica, una delle attività viene scelta come vittima e le relative transazioni vengono terminate con un errore. In questo modo l'altra attività potrà completare la propria transazione. L'applicazione la cui transazione è stata terminata con un errore può eseguire un nuovo tentativo di transazione, che viene in genere completato al termine dell'altra transazione bloccata dal deadlock.  
  
 Spesso la condizione di deadlock viene confusa con il blocco normale. Quando una transazione richiede un blocco in una risorsa bloccata da un'altra transazione, la transazione che ha eseguito la richiesta resta in attesa fino quando il blocco non viene rilasciato. Per impostazione predefinita, le transazioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non prevedono alcun timeout, a meno che non sia stata impostata l'opzione LOCK_TIMEOUT. La transazione che ha eseguito la richiesta viene bloccata, ma non tramite un deadlock, in quanto non ha tentato di bloccare la transazione proprietaria del blocco. La transazione proprietaria del blocco completa e rilascia il blocco e quindi il blocco viene assegnato alla transazione che ha eseguito la richiesta, che può procedere.  
  
> [!NOTE]
> I deadlock sono a volte definiti anche blocchi critici.  
  
 Un deadlock si verifica in qualsiasi sistema con più thread e non soltanto in un sistema di gestione di database relazionali, e può interessare anche risorse diverse dai blocchi negli oggetti di database. Un thread, ad esempio, in un sistema operativo a thread multipli può acquisire una o più risorse, ad esempio blocchi di memoria. Se la risorsa che viene acquisita è già utilizzata da un altro thread, il primo thread deve aspettare che la risorsa venga rilasciata. Il thread in attesa è considerato dipendente dal thread proprietario della risorsa richiesta. In un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] le sessioni possono causare un deadlock durante l'acquisizione di risorse non di database, ad esempio la memoria o i thread.  
  
 ![Diagramma che illustra il deadlock della transazione](../relational-databases/media/deadlock.png)  
  
 Nell'illustrazione la transazione T1 è dipendente dalla transazione T2 per la risorsa di blocco della tabella `Part`. In modo analogo, la transazione T2 presenta una dipendenza dalla transazione T1 per la risorsa di blocco della tabella `Supplier`. Poiché queste dipendenze creano un ciclo, si verifica un deadlock tra le transazioni T1 e T2.  
  
 I deadlock possono verificarsi anche quando una tabella è partizionata e l'impostazione `LOCK_ESCALATION` di `ALTER TABLE` è impostata su AUTO. Quando `LOCK_ESCALATION` è impostato su AUTO, la concorrenza aumenta consentendo al [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] di bloccare le partizioni della tabella a livello HoBT anziché a livello di tabella. Tuttavia, quando transazioni separate contengono blocchi di partizioni in una tabella e richiedono un blocco in un punto nella partizione delle altre transazioni, si verifica un deadlock. Questo tipo di deadlock può essere evitato impostando `LOCK_ESCALATION` su `TABLE`. Tuttavia, questa impostazione ridurrà la concorrenza forzando aggiornamenti di grandi dimensioni a una partizione attendere un blocco di tabella.  
  
### <a name="detecting-and-ending-deadlocks"></a>Rilevamento e interruzione di deadlock  
 Un deadlock si verifica quando due o più attività si bloccano reciprocamente in modo permanente, in quanto ognuna delle attività prevede un blocco su una risorsa che le altre attività stanno cercando di bloccare. Nel grafico seguente viene illustrata una vista di alto livello di uno stato di deadlock in cui:  
  
-   L'attività T1 prevede un blocco sulla risorsa R1, indicato dalla freccia da R1 a T1, e ha richiesto un blocco sulla risorsa R2, indicato dalla freccia da T1 a R2.  
-   L'attività T2 prevede un blocco sulla risorsa R2, indicato dalla freccia da R2 a T2, e ha richiesto un blocco sulla risorsa R1, indicato dalla freccia da T2 a R1.  
-   Poiché nessuna attività può continuare fino a quando una risorsa diventa disponibile e nessuna risorsa può essere rilasciata fino a quando l'attività continua, si verifica una stato di deadlock.  
  
 ![Diagramma in cui vengono mostrate le attività in stato di deadlock](../relational-databases/media/Task_Deadlock_State.png)  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] rileva automaticamente i cicli di deadlock in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sceglie una delle sessioni come vittima del deadlock e la transazione corrente viene terminata con un errore per interrompere il deadlock.  
  
#### <a name="resources-that-can-deadlock"></a><a name="deadlock_resources"></a> Risorse che possono causare un deadlock  
 Per conto di ogni sessione utente possono essere in esecuzione una o più attività e ogni attività può acquisire o attendere di acquisire risorse diverse. Di seguito sono elencati i tipi di risorse che possono causare blocchi che potrebbero provocare un deadlock.  
  
-   **Blocchi**. L'attesa di acquisizione di blocchi sulle risorse, ad esempio oggetti, pagine, righe, metadati e applicazioni, può causare deadlock. La transazione T1, ad esempio, prevede un blocco condiviso (S) sulla riga r1 ed è in attesa di ottenere un blocco esclusivo (X) su r2. La transazione T2 prevede un blocco condiviso (S) su r2 ed è in attesa di ottenere un blocco esclusivo (X) sulla riga r1. Il risultato è un ciclo di blocco in cui T1 e T2 attendono che le risorse bloccate vengano rilasciate dall'altra attività.  
  
-   **Thread di lavoro**. Un'attività in coda in attesa di un thread di lavoro disponibile può causare un deadlock. Se l'attività in coda è proprietaria di risorse che bloccano tutti i thread di lavoro, si verifica un deadlock. La sessione S1 avvia ad esempio una transazione e acquisisce un blocco condiviso (S) sulla riga r1 e quindi va in sospensione. Le sessioni attive in esecuzione in tutti i thread di lavoro disponibili cercano di acquisire blocchi esclusivi (X) sulla riga r1. Poiché la sessione S1 non riesce ad acquisire un thread di lavoro, non può eseguire la transazione e rilasciare il blocco sulla riga r1. Di conseguenza, si verifica un deadlock.  
  
-   **Memoria**. Quando richieste simultanee sono in attesa di concessioni di memoria che non possono essere soddisfatte con la memoria disponibile, può verificarsi un deadlock. Due query simultanee, Q1 e Q2, vengono ad esempio eseguite come funzioni definite dall'utente che acquisiscono rispettivamente 10 MB e 20 MB di memoria. Se per ogni query sono necessari 30 MB e la memoria disponibile totale è di 20 MB, Q1 e Q2 devono attendere che ognuna rilasci memoria e di conseguenza si verifica un deadlock.  
  
-   **Risorse per l'esecuzione di query in parallelo**. Thread coordinator, producer o consumer associati a una porta di scambio possono bloccarsi a vicenda causando un deadlock, generalmente quando è incluso almeno un altro processo che non fa parte della query parallela. Quando inoltre viene avviata l'esecuzione di una query parallela, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] determina il grado di parallelismo o il numero di thread di lavoro sulla base del carico di lavoro corrente. Se il carico di lavoro del sistema cambia inaspettatamente, ad esempio per l'avvio di nuove query o per l'esaurimento dei thread di lavoro, è possibile che si verifichi un deadlock.  
  
-   **Risorse MARS (Multiple Active Result Sets)** . Queste risorse sono utilizzate per controllare in che modo più richieste attive vengono intercalate con il servizio MARS. Per ulteriori informazioni. Per altre informazioni, vedere [Uso di MARS (Multiple Active Result Set)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
    -   **Risorsa utente**. Quando un thread è in attesa di una risorsa che è potenzialmente controllata da un'applicazione utente, la risorsa viene considerata risorsa esterna o utente e viene trattata come un blocco.  
  
    -   **Mutex della sessione**. Le attività in esecuzione in una sessione vengono intercalate, ovvero solo un'attività può essere eseguita in una sessione in un determinato momento. Per poter essere eseguita, l'attività deve disporre di accesso esclusivo al mutex della sessione.  
  
    -   **Mutex della transazione**. Tutte le attività in esecuzione in una transazione vengono intercalate, ovvero solo un'attività può essere eseguita in una transazione in un determinato momento. Per poter essere eseguita, l'attività deve disporre di accesso esclusivo al mutex della transazione.  
  
     Per essere eseguita in un servizio MARS, l'attività deve acquisire il mutex della sessione. Se l'attività è in esecuzione in una transazione, deve acquisire il mutex della transazione. Questo consente di garantire che sia attiva una sola attività per volta in una determinata sessione e in una determinata transazione. Dopo che i mutex richiesti sono stati acquisiti, l'attività può essere eseguita. Quando l'attività termine, oppure restituisce il risultato a metà della richiesta,  viene rilasciato prima il mutex della transazione, seguito da quello della sessione, in ordine inverso rispetto a quello di acquisizione. In queste risorse, tuttavia, possono verificarsi deadlock. Nell'esempio di codice seguente vengono illustrate due attività, richiesta utente U1 e richiesta utente U2, in esecuzione nella stessa sessione.  
  
    ```  
    U1:    Rs1=Command1.Execute("insert sometable EXEC usp_someproc");  
    U2:    Rs2=Command2.Execute("select colA from sometable");  
    ```  
  
     La stored procedure in esecuzione dall'attività richiesta utente U1 ha acquisito il mutex della sessione. Se per l'esecuzione della stored procedure è necessario molto tempo, in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene considerato che la stored procedure è in attesa dell'input dell'utente. La richiesta utente U2 è in attesa del mutex della sessione mentre l'utente è in attesa del set di risultati da U2 e U1 è in attesa di una risorsa utente. La rappresentazione logica di tale condizione di deadlock è la seguente:  
  
 ![LogicFlowExamplec](../relational-databases/media/udb9_LogicFlowExamplec.png)  
  
### <a name="deadlock-detection"></a><a name="deadlock_detection"></a> Rilevamento di deadlock  
 Tutte le risorse elencate nella sezione precedente fanno parte dello schema di rilevamento dei deadlock di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Il rilevamento dei deadlock viene eseguito da un thread di monitoraggio dei blocchi tramite il quale viene periodicamente iniziata una ricerca in tutte le attività in un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Il processo di ricerca è descritto dai punti seguenti:  
  
-   L'intervallo predefinito è 5 secondi.  
-   Se tramite il thread di monitoraggio dei blocchi vengono individuati deadlock, l'intervallo di rilevamento dei deadlock scende da 5 secondi a un minimo di 100 millisecondi, in base alla frequenza dei deadlock.  
-   Se tramite il thread di monitoraggio dei blocchi non vengono trovati ulteriori deadlock, in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] gli intervalli tra le ricerche vengono aumentati a 5 secondi.  
-   Se viene rilevato un deadlock, si presuppone che i thread successivi che devono attendete un blocco stiano entrando nel ciclo di deadlock. La prima coppia di attese di blocco dopo il rilevamento di un deadlock genera immediatamente una ricerca di deadlock senza che venga atteso l'intervallo successivo di rilevamento dei deadlock. Se, ad esempio, l'intervallo attuale è di 5 secondi ed è appena stato rilevato un deadlock, l'attesa di blocco successiva provoca l'avvio immediato della funzionalità di rilevamento di deadlock. Se l'attesa di blocco fa parte di un deadlock, verrà rilevata immediatamente e non nel corso della ricerca di deadlock successiva.  
  
 In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] vengono in genere eseguite solo attività di rilevamento di deadlock periodiche. Poiché il numero di deadlock di un sistema in genere è ridotto, il rilevamento periodico consente di ridurre l'overhead associato all'operazione di ricerca.  
  
 Dopo l'avvio di una ricerca di deadlock per un thread specifico, viene identificata la risorsa di cui il thread è in attesa e vengono individuati il proprietario o i proprietari della risorsa. La ricerca di deadlock viene quindi ripetuta in modo ricorsivo per gli stessi thread fino all'individuazione di un ciclo. Un ciclo identificato in questo modo crea un deadlock.  
  
 Dopo essere stato rilevato, un deadlock viene terminato da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tramite la scelta di una vittima del deadlock. Tramite [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene terminato il batch attualmente in esecuzione per il thread, viene eseguito il rollback della transazione della vittima del deadlock e viene restituito all'applicazione un errore 1205. Tramite il rollback della transazione per la vittima del deadlock vengono rilasciati tutti i blocchi della transazione. In questo modo, le transazioni degli altri thread vengono sbloccate e possono continuare. Tramite l'errore 1205 relativo alla vittima del deadlock vengono registrate nel log degli errori le informazioni sulle risorse e i thread coinvolti.  
  
 Per impostazione predefinita, tramite [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene scelta come vittima del deadlock la sessione in cui è in esecuzione la transazione il cui rollback è meno costoso. In alternativa, è possibile specificare la priorità delle sessioni in una situazione di deadlock usando l'istruzione `SET DEADLOCK_PRIORITY`. Il valore di DEADLOCK_PRIORITY può essere impostato su LOW, NORMAL o HIGH oppure è possibile utilizzare qualsiasi valore intero compreso tra -10 e 10. L'impostazione predefinita per priorità di deadlock è NORMAL. Se le priorità di deadlock di due sessioni sono diverse, come vittima del deadlock verrà scelta la sessione con la priorità inferiore. Se entrambe le sessioni hanno la stessa priorità di deadlock, verrà scelta la sessione con la transazione il cui rollback è meno costoso. Se le sessioni coinvolte nel ciclo di deadlock hanno la stessa priorità di deadlock e lo stesso costo, la vittima viene scelta in modo casuale.  
  
 Quando si utilizza l'ambiente CLR, tramite la funzionalità di monitoraggio vengono automaticamente rilevati i deadlock per le  risorse di sincronizzazione, ovvero monitor, blocchi di lettura/scrittura e join di thread, a cui viene eseguito l'accesso all'interno di procedure gestite. Il deadlock viene tuttavia risolto generando un'eccezione nella procedura selezionata come vittima del deadlock. È importante comprendere che l'eccezione non comporta il rilascio automatico delle risorse attualmente di proprietà della vittima, ma le risorse devono essere rilasciate esplicitamente. In modo coerente con il comportamento dell'eccezione, l'eccezione utilizzata per identificare una vittima del deadlock può essere intercettata e ignorata.  
  
### <a name="deadlock-information-tools"></a><a name="deadlock_tools"></a> Strumenti di informazione sui deadlock  
 Per la visualizzazione di informazioni sui deadlock, il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] offre strumenti di monitoraggio costituiti dalla sessione xEvent system\_health, da due flag di traccia e dall'evento Deadlock Graph in SQL Profiler.  

#### <a name="deadlock-extended-event"></a><a name="deadlock_xevent"></a> Evento deadlock esteso
A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], è consigliabile usare l'evento esteso `xml_deadlock_report` (xEvent) anziché la classe di evento Deadlock Graph in SQL Trace o SQL Profiler.

Inoltre, a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], quando si verificano deadlock, la sessione ***system\_health** _ acquisisce già tutti gli xEvent `xml_deadlock_report` che contengono il grafico del deadlock. Dato che la sessione _system\_health* è abilitata per impostazione predefinita, non è necessario configurare una sessione xEvent separata per l'acquisizione delle informazioni sul deadlock. 

L'evento Deadlock Graph acquisito include in genere tre nodi distinti:
-   **victim-list**. Identificatore di processo della vittima del deadlock.
-   **process-list**. Informazioni su tutti i processi coinvolti nel deadlock.
-   **resource-list**. Informazioni sulle risorse coinvolte nel deadlock.

All'apertura del file o del buffer circolare della sessione system\_health, se viene registrato xEvent `xml_deadlock_report`, [!INCLUDE[ssManStudio](../includes/ssManStudio-md.md)] presenta una rappresentazione grafica delle attività e delle risorse coinvolte in un deadlock, come illustrato nell'esempio seguente: 

![Deadlock Graph xEvent](../relational-databases/media/udb9_xEventDeadlockGraphc.png)

La query seguente può visualizzare tutti gli eventi di deadlock acquisiti dal buffer circolare della sessione *system\_health*:

```sql
SELECT xdr.value('@timestamp', 'datetime') AS [Date],
    xdr.query('.') AS [Event_Data]
FROM (SELECT CAST([target_data] AS XML) AS Target_Data
            FROM sys.dm_xe_session_targets AS xt
            INNER JOIN sys.dm_xe_sessions AS xs ON xs.address = xt.event_session_address
            WHERE xs.name = N'system_health'
              AND xt.target_name = N'ring_buffer'
    ) AS XML_Data
CROSS APPLY Target_Data.nodes('RingBufferTarget/event[@name="xml_deadlock_report"]') AS XEventData(xdr)
ORDER BY [Date] DESC
```

[!INCLUDE[ssResult](../includes/ssresult-md.md)]

![system_health_xevent_query_result](../relational-databases/media/system_health_qry.png)

L'esempio seguente illustra l'output dopo aver fatto clic sul primo collegamento del risultato precedente:

```xml
<event name="xml_deadlock_report" package="sqlserver" timestamp="2018-02-18T08:26:24.698Z">
  <data name="xml_report">
    <type name="xml" package="package0" />
    <value>
      <deadlock>
        <victim-list>
          <victimProcess id="process27b9b0b9848" />
        </victim-list>
        <process-list>
          <process id="process27b9b0b9848" taskpriority="0" logused="0" waitresource="KEY: 5:72057594214350848 (1a39e6095155)" waittime="1631" ownerId="11088595" transactionname="SELECT" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27b9f79fac0" lockMode="S" schedulerid="9" kpid="15336" status="suspended" spid="62" sbid="0" ecid="0" priority="0" trancount="0" lastbatchstarted="2018-02-18T00:26:22.893" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="7908" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088595" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p1" line="3" stmtstart="78" stmtend="180" sqlhandle="0x0300050020766505ca3e07008ba8000001000000000000000000000000000000000000000000000000000000">
SELECT c2, c3 FROM t1 WHERE c2 BETWEEN @p1 AND @p1+    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x020000006263ec01ebb919c335024a072a2699958d3fcce60000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p1 4
END
   </inputbuf>
          </process>
          <process id="process27b9ee33c28" taskpriority="0" logused="252" waitresource="KEY: 5:72057594214416384 (e5b3d7e750dd)" waittime="1631" ownerId="11088593" transactionname="UPDATE" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27ba15a4490" lockMode="X" schedulerid="6" kpid="5584" status="suspended" spid="58" sbid="0" ecid="0" priority="0" trancount="2" lastbatchstarted="2018-02-18T00:26:22.890" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="15316" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088593" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p2" line="3" stmtstart="76" stmtend="150" sqlhandle="0x03000500599a5906ce3e07008ba8000001000000000000000000000000000000000000000000000000000000">
UPDATE t1 SET c2 = c2+1 WHERE c1 = @p    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x02000000008fe521e5fb1099410048c5743ff7da04b2047b0000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p2 4
END
   </inputbuf>
          </process>
        </process-list>
        <resource-list>
          <keylock hobtid="72057594214350848" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="cidx" id="lock27b9dd26a00" mode="X" associatedObjectId="72057594214350848">
            <owner-list>
              <owner id="process27b9ee33c28" mode="X" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9b0b9848" mode="S" requestType="wait" />
            </waiter-list>
          </keylock>
          <keylock hobtid="72057594214416384" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="idx1" id="lock27afa392600" mode="S" associatedObjectId="72057594214416384">
            <owner-list>
              <owner id="process27b9b0b9848" mode="S" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9ee33c28" mode="X" requestType="wait" />
            </waiter-list>
          </keylock>
        </resource-list>
      </deadlock>
    </value>
  </data>
</event>
```

Per altre informazioni, vedere [Usare la sessione system_health](../relational-databases/extended-events/use-the-system-health-session.md)

#### <a name="trace-flag-1204-and-trace-flag-1222"></a><a name="deadlock_traceflags"></a> Flag di traccia 1204 e flag di traccia 1222  
 In caso di deadlock, il flag di traccia 1204 e il flag di traccia 1222 restituiscono informazioni che vengono riportate nel log degli errori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il flag di traccia 1204 riporta informazioni sui deadlock formattate per ogni nodo interessato dal deadlock. Il flag di traccia 1222 formatta le informazioni sui deadlock, prima per processi e poi per risorse. È possibile attivare entrambi i flag di traccia per ottenere due diverse rappresentazioni dello stesso evento di deadlock.  

> [!IMPORTANT]
> Evitare di usare i flag di traccia 1204 e 1222 in sistemi con carichi di lavoro intensivi che causano deadlock. L'uso di questi flag di traccia può comportare problemi di prestazioni. Usare invece l'evento esteso di deadlock (#deadlock_xevent).
  
 Nella tabella seguente vengono illustrate le proprietà dei flag di traccia 1204 e 1222, nonché le relative similitudini e differenze.  
  
|Proprietà|Flag di traccia 1204 e flag di traccia 1222|Solo flag di traccia 1204|Solo flag di traccia 1222|  
|--------------|-----------------------------------------|--------------------------|--------------------------|  
|Formato di output|L'output viene acquisito nel log degli errori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|Mirato ai nodi interessati dal deadlock. Ogni nodo ha una sezione dedicata e la sezione finale descrive la vittima del deadlock.|Restituisce informazioni in un formato simile all'XML ma non conforme a uno schema XSD (XML Schema Definition). Il formato presenta tre sezioni principali. Nella prima sezione viene dichiarata la vittima del deadlock. Nella seconda sezione viene descritto ogni processo interessato dal deadlock. Nella terza sezione vengono descritte le risorse che rappresentano un sinonimo dei nodi indicati nel flag di traccia 1204.|  
|Identificazione degli attributi|**SPID:<x\> ECID:<x\>.** Identifica il thread dell'ID del processo di sistema in presenza di processi paralleli. La voce `SPID:<x> ECID:0`, dove <x\> viene sostituita dal valore di SPID, rappresenta il thread principale. La voce `SPID:<x> ECID:<y>`, dove <x\> viene sostituita dal valore di SPID e <y\> è maggiore di 0, rappresenta i sottothread dello stesso SPID.<br /><br /> **BatchID** (**sbid** per il flag di traccia 1222). Identifica il batch da cui l'esecuzione del codice richiede o mantiene un blocco. Quando MARS (Multiple Active Result Set) è disabilitato, il valore di BatchID è 0. Quando MARS è abilitato, il valore per i batch attivi è compreso tra 1 e *n*. Se la sessione non contiene batch attivi, BatchID è 0.<br /><br /> **Mode**. Specifica il tipo di blocco per una determinata risorsa richiesta, concessa o attesa da un thread. Mode può essere IS (Preventivo condiviso), S (Condiviso), U (Aggiornamento), IX (Preventivo esclusivo), SIX (Condiviso preventivo esclusivo) e X (Esclusivo).<br /><br /> **Line #** (**line** per il flag di traccia 1222). Elenca il numero di riga del batch di istruzioni corrente che era in esecuzione quando si è verificato il deadlock.<br /><br /> **Input Buf** (**inputbuf** per il flag di traccia 1222). Elenca tutte le istruzioni del batch corrente.|**Node**. Rappresenta il numero di voce nella catena del deadlock.<br /><br /> **Lists**. Il proprietario del blocco può essere parte degli elenchi seguenti:<br /><br /> **Grant List**. Enumera i proprietari correnti della risorsa.<br /><br /> **Convert List**. Enumera i proprietari correnti che stanno tentando di convertire i propri blocchi a un livello superiore.<br /><br /> **Wait List**. Enumera le nuove richieste di blocco correnti per la risorsa.<br /><br /> **Statement Type**. Descrive il tipo di istruzione DML (SELECT, INSERT, UPDATE o DELETE) su cui i thread hanno autorizzazioni.<br /><br /> **Victim Resource Owner**. Specifica il thread partecipante che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sceglie come vittima per interrompere il ciclo di deadlock. Il thread scelto e tutti i sottothread esistenti vengono terminati.<br /><br /> **Next Branch**. Rappresenta i due o più sottothread legati allo stesso SPID che sono interessati dal ciclo di deadlock.|**deadlock victim**. Rappresenta l'indirizzo di memoria fisica dell'attività (vedere [sys.dm_os_tasks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)) selezionata come vittima del deadlock. In caso di deadlock risolto, il valore può essere 0 (zero). Un'attività in cui è in corso l'esecuzione del rollback non può essere scelta come vittima del deadlock.<br /><br /> **executionstack**. Rappresenta il codice [!INCLUDE[tsql](../includes/tsql-md.md)] in esecuzione al momento del deadlock.<br /><br /> **priority**. Rappresenta la priorità di deadlock. In alcuni casi, la priorità di deadlock potrebbe essere modificata da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per un breve intervallo di tempo, per ottenere una concorrenza migliore.<br /><br /> **logused**. Spazio del log utilizzato dall'attività.<br /><br /> **owner id**. ID della transazione che ha il controllo della richiesta.<br /><br /> **status**. Stato dell'attività. I possibili valori sono i seguenti:<br /><br /> >> **pending**. in attesa di un thread di lavoro.<br /><br /> >> **runnable**. Pronto per l'esecuzione ma in attesa di un quantum.<br /><br /> >> **running**. in esecuzione nell'utilità di pianificazione.<br /><br /> >> **suspended**. Esecuzione sospesa.<br /><br /> >> **done**. Attività completata.<br /><br /> >> **spinloop**. In attesa che venga liberato uno spinlock.<br /><br /> **waitresource**. Risorsa necessaria per l'attività.<br /><br /> **waittime**. Tempo, in millisecondi, di attesa per la risorsa.<br /><br /> **schedulerid**. Utilità di pianificazione associata all'attività. Vedere [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).<br /><br /> **hostname**. Nome della workstation.<br /><br /> **isolationlevel**. Livello di isolamento delle transazioni corrente.<br /><br /> **Xactid**. ID della transazione che ha il controllo della richiesta.<br /><br /> **currentdb**. ID del database.<br /><br /> **lastbatchstarted**. Ultima volta in cui un processo client ha avviato un'esecuzione del batch.<br /><br /> **lastbatchcompleted**. Ultima volta in cui un processo client ha completato un'esecuzione del batch.<br /><br /> **clientoption1 e clientoption2**. Opzioni SET nella connessione client. Si tratta di una maschera di bit che include informazioni sulle opzioni generalmente controllate da istruzioni SET, ad esempio SET NOCOUNT e SET XACTABORT.<br /><br /> **associatedObjectId**. Rappresenta l'ID di HoBT (Heap Or B-Tree).|  
|Attributi risorsa|**RID**. Identifica la singola riga di una tabella su cui un blocco viene mantenuto o richiesto. RID è rappresentato come RID: *db_id:file_id:page_no:row_no*. Ad esempio: `RID: 6:1:20789:0`.<br /><br /> **OBJECT**. Identifica la tabella su cui un blocco viene mantenuto o richiesto. OBJECT è rappresentato come OBJECT: *db_id:object_id*. Ad esempio: `TAB: 6:2009058193`.<br /><br /> **KEY**. Identifica l'intervallo di chiavi di un indice su cui un blocco viene mantenuto o richiesto. KEY è rappresentato come KEY: *db_id:hobt_id* (*valore hash della chiave di indice*). Ad esempio: `KEY: 6:72057594057457664 (350007a4d329)`.<br /><br /> **PAG**. Identifica la risorsa di pagina su cui un blocco viene mantenuto o richiesto. PAG è rappresentato come PAG: *db_id:file_id:page_no*. Ad esempio: `PAG: 6:1:20789`.<br /><br /> **EXT**. Identifica la struttura extent. EXT è rappresentato come EXT: *db_id:file_id:extent_no*. Ad esempio: `EXT: 6:1:9`.<br /><br /> **DB**. Identifica il blocco del database. **DB è rappresentato in uno dei modi seguenti:**<br /><br /> DB: *db_id*<br /><br /> DB: *db_id*[BULK-OP-DB], che identifica il blocco di database applicato dal database di backup.<br /><br /> DB: *db_id*[BULK-OP-LOG], che identifica il blocco applicato dal log di backup per un particolare database.<br /><br /> **APP**. Identifica il blocco applicato da una risorsa di un'applicazione. APP è rappresentato come APP: *lock_resource*. Ad esempio: `APP: Formf370f478`.<br /><br /> **METADATA**. Rappresenta le risorse di metadati interessate da un deadlock. Poiché METADATA ha molte sottorisorse, il valore restituito dipende dalla sottorisorsa interessata dal deadlock. Ad esempio, METADATA.USER_TYPE restituisce `user_type_id =` <*integer_value*>. Per altre informazioni sulle risorse e le sottorisorse METADATA, vedere [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> **HOBT**. Rappresenta un heap o un albero B interessato da un deadlock.|Nessuna esclusiva di questo flag di traccia.|Nessuna esclusiva di questo flag di traccia.|  
  
##### <a name="trace-flag-1204-example"></a>Esempio di flag di traccia 1204  
 L'esempio seguente illustra l'output che si ottiene quando il flag di traccia 1204 è attivo. In questo caso, la tabella in Node 1 è un heap senza indici e la tabella in Node 2 è un heap con un indice non cluster. La chiave di indice in Node 2 è in corso di aggiornamento quando si verifica il deadlock.  
  
```  
Deadlock encountered .... Printing deadlock information  
Wait-for graph  
  
Node:1  
  
RID: 6:1:20789:0               CleanCnt:3 Mode:X Flags: 0x2  
 Grant List 0:  
   Owner:0x0315D6A0 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:55 ECID:0 XactLockInfo: 0x04D9E27C  
   SPID: 55 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
BEGIN TRANSACTION  
   EXEC usp_p2  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x03A3DAD0   
     Mode: U SPID:54 BatchID:0 ECID:0 TaskProxy:(0x04976374) Value:0x315d200 Cost:(0/868)  
  
Node:2  
  
KEY: 6:72057594057457664 (350007a4d329) CleanCnt:2 Mode:X Flags: 0x0  
 Grant List 0:  
   Owner:0x0315D140 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:54 ECID:0 XactLockInfo: 0x03A3DAF4  
   SPID: 54 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
     BEGIN TRANSACTION  
       EXEC usp_p1  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
  
Victim Resource Owner:  
 ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
```  
  
##### <a name="trace-flag-1222-example"></a>Esempio di flag di traccia 1222  
 Nell'esempio seguente viene illustrato l'output che si ottiene quando il flag di traccia 1222 è attivo. In questo caso, una tabella è un heap senza indici e l'altra tabella è un heap con un indice non cluster. Nella seconda tabella, la chiave di indice è in corso di aggiornamento quando si verifica il deadlock.  
  
```  
deadlock-list  
 deadlock victim=process689978  
  process-list  
   process id=process6891f8 taskpriority=0 logused=868   
   waitresource=RID: 6:1:20789:0 waittime=1359 ownerId=310444   
   transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:42.733 XDES=0x3a3dad0   
   lockMode=U schedulerid=1 kpid=1952 status=suspended spid=54   
   sbid=0 ecid=0 priority=0 transcount=2   
   lastbatchstarted=2005-09-05T11:22:42.733   
   lastbatchcompleted=2005-09-05T11:22:42.733   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310444 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p1 line=6 stmtstart=202   
     sqlhandle=0x0300060013e6446b027cbb00c69600000100000000000000  
     UPDATE T2 SET COL1 = 3 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600856aa70f503b8104000000000000000000000000  
     EXEC usp_p1       
    inputbuf  
      BEGIN TRANSACTION  
       EXEC usp_p1  
   process id=process689978 taskpriority=0 logused=380   
   waitresource=KEY: 6:72057594057457664 (350007a4d329)     
   waittime=5015 ownerId=310462 transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:44.077 XDES=0x4d9e258 lockMode=U   
   schedulerid=1 kpid=3024 status=suspended spid=55 sbid=0 ecid=0   
   priority=0 transcount=2 lastbatchstarted=2005-09-05T11:22:44.077   
   lastbatchcompleted=2005-09-05T11:22:44.077   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310462 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p2 line=6 stmtstart=200   
     sqlhandle=0x030006004c0a396c027cbb00c69600000100000000000000  
     UPDATE T1 SET COL1 = 4 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600d688e709b85f8904000000000000000000000000  
     EXEC usp_p2       
    inputbuf  
      BEGIN TRANSACTION  
        EXEC usp_p2      
  resource-list  
   ridlock fileid=1 pageid=20789 dbid=6 objectname=AdventureWorks2016.dbo.T2   
   id=lock3136940 mode=X associatedObjectId=72057594057392128  
    owner-list  
     owner id=process689978 mode=X  
    waiter-list  
     waiter id=process6891f8 mode=U requestType=wait  
   keylock hobtid=72057594057457664 dbid=6 objectname=AdventureWorks2016.dbo.T1   
   indexname=nci_T1_COL1 id=lock3136fc0 mode=X   
   associatedObjectId=72057594057457664  
    owner-list  
     owner id=process6891f8 mode=X  
    waiter-list  
     waiter id=process689978 mode=U requestType=wait  
```  
  
#### <a name="profiler-deadlock-graph-event"></a>Evento Deadlock Graph di Profiler  
Si tratta di un evento in SQL Profiler che presenta una descrizione grafica delle attività e delle risorse coinvolte in un deadlock. L'esempio seguente illustra l'output prodotto da SQL Profiler quando l'evento Deadlock Graph è attivo.  
  
 ![ProfilerDeadlockGraphc](../relational-databases/media/udb9_ProfilerDeadlockGraphc.png)  
  
Per altre informazioni sull'evento deadlock, vedere [Classe di evento Lock:Deadlock](../relational-databases/event-classes/lock-deadlock-event-class.md).

Per altre informazioni sull'esecuzione dell'evento Deadlock Graph di SQL Profiler, vedere [Salvare eventi Deadlock Graph &#40;SQL Server Profiler&#41;](../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md).  
  
### <a name="handling-deadlocks"></a>Gestione di deadlock  
 Quando un'istanza del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sceglie una transazione come vittima di un deadlock, viene terminato il batch corrente, viene eseguito il rollback della transazione e viene restituito il messaggio di errore 1205 all'applicazione.  
  
 `Your transaction (process ID #52) was deadlocked on {lock | communication buffer | thread} resources with another process and has been chosen as the deadlock victim. Rerun your transaction.`  
  
 Poiché tutte le applicazioni che inviano query [!INCLUDE[tsql](../includes/tsql-md.md)] possono essere potenziali vittime del deadlock, devono includere un gestore degli errori in grado di intercettare il messaggio di errore 1205. Se questo errore non viene intercettato, l'applicazione continua a essere eseguita come se il rollback della transazione non avesse avuto luogo, con la conseguente generazione di errori.  
  
 Tramite l'implementazione di un gestore degli errori in grado di intercettare il messaggio di errore 1205, è possibile gestire la condizione di deadlock in un'applicazione ed eseguire gli interventi di correzione necessari, ad esempio il reinvio della query coinvolta nel deadlock. Tramite il reinvio automatico della query l'utente non è necessario che l'utente sia a conoscenza del verificarsi del deadlock.  
  
 È consigliabile sospendere brevemente l'applicazione prima di reinviarne la query. In questo modo, la transazione interessata dal deadlock può completare e rilasciare i relativi blocchi che formano parte del ciclo di deadlock. Ciò riduce la probabilità che il deadlock si verifichi di nuovo quando la query reinviata ne richiede i blocchi.  
  
### <a name="minimizing-deadlocks"></a><a name="deadlock_minimizing"></a> Riduzione dei deadlock  
 I deadlock non possono essere evitati completamente. È tuttavia possibile ridurre il rischio di insorgenza di un deadlock attenendosi a determinate convenzioni di codifica. La riduzione del numero di deadlock comporta un aumento della velocità effettiva delle transazioni e una diminuzione dell'overhead del sistema, in quanto il numero di transazioni su cui è necessario eseguire le operazioni seguenti risulta minimo:  
  
-   Rollback, con il conseguente annullamento del lavoro eseguito.  
-   Riesecuzione tramite l'applicazione, in quanto in corrispondenza del deadlock è stato eseguito il rollback.  
  
 Per ridurre il numero di deadlock, è possibile:  
  
-   Accedere sempre agli oggetti in base allo stesso ordine.  
-   Escludere l'interazione dell'utente nelle transazioni.  
-   Ridurre la lunghezza delle transazioni e inserirle in un solo batch.  
-   Utilizzare un livello di isolamento basso.  
-   Utilizzare un livello di isolamento basato sul controllo delle versioni delle righe.  
    -   Impostare l'opzione di database `READ_COMMITTED_SNAPSHOT` su ON affinché le transazioni Read Committed possano usare il controllo delle versioni delle righe.  
    -   Utilizzare l'isolamento dello snapshot.  
-   Utilizzare connessioni associate.  
  
#### <a name="access-objects-in-the-same-order"></a>Accedere agli oggetti nello stesso ordine  
 Se tutte le transazioni simultanee accedono agli oggetti nello stesso ordine, la possibilità che si verifichi un deadlock risulta notevolmente ridotta. Se, ad esempio, due transazioni simultanee ottengono un blocco prima nella tabella **Supplier** e quindi nella tabella **Part**, una transazione rimane bloccata sulla tabella **Supplier** fino al completamento dell'altra transazione. Dopo il commit o il rollback della prima transazione, l'esecuzione della seconda continua e non si verifica alcun deadlock. L'utilizzo di stored procedure per tutte le modifiche ai dati consente di standardizzare l'ordine di accesso agli oggetti.  
  
 ![deadlock2](../relational-databases/media/dedlck2.png)  
  
#### <a name="avoid-user-interaction-in-transactions"></a>Escludere l'interazione dell'utente nelle transazioni  
 È consigliabile evitare la creazione di transazioni che prevedono l'interazione dell'utente. I batch eseguiti senza alcun intervento da parte dell'utente risultano infatti molto più veloci rispetto ai tempi di risposta di un utente a una query, ad esempio per rispondere alla richiesta di un parametro richiesto da un'applicazione. Si supponga, ad esempio, che una transazione sia in attesa dell'input dell'utente e che l'utente sia a pranzo o abbia lasciato l'ufficio per il fine settimana. In questo caso la transazione non può essere completata. Questa situazione comporta una riduzione della velocità effettiva del sistema, in quanto i blocchi mantenuti attivi dalla transazione vengono rilasciati solo in corrispondenza del commit o del rollback della transazione. Anche se non si verifica una situazione di deadlock, le altre transazioni che tentano di accedere alle stesse risorse vengono bloccate, in attesa del completamento della prima transazione.  
  
#### <a name="keep-transactions-short-and-in-one-batch"></a>Ridurre la lunghezza delle transazioni e l'inserimento in un solo batch  
 Il deadlock si verifica in genere quando nello stesso database vengono eseguite contemporaneamente numerose transazioni estese. La lunghezza della transazione è direttamente proporzionale alla durata dei blocchi esclusivi o di aggiornamento che bloccano qualsiasi altra attività e che possono generare una situazione di deadlock.  
  
 Se le transazioni vengono inserite in un singolo batch, è possibile minimizzare il tempo di round trip in rete durante l'esecuzione delle transazioni, con la conseguente riduzione di possibili ritardi nel completamento della transazione e nel rilascio dei blocchi.  
  
#### <a name="use-a-lower-isolation-level"></a>Usare un livello di isolamento più basso  
 È importante determinare se una transazione è eseguibile a un livello di isolamento inferiore. Il livello di isolamento Read Committed per una transazione consente la lettura di dati letti in precedenza (ma non modificati) da un'altra transazione senza attendere che tale transazione venga completata. Quando si imposta un livello di isolamento basso, quale Read Committed, i blocchi condivisi vengono mantenuti attivi per un periodo più breve rispetto a quello richiesto da un livello di isolamento più alto, quale Serializable, con la conseguente riduzione della contesa tra blocchi.  
  
#### <a name="use-a-row-versioning-based-isolation-level"></a>Utilizzo di un livello di isolamento basato sul controllo delle versioni delle righe  
 Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` è impostata su ON, durante le operazioni di lettura le transazioni eseguite con il livello di isolamento Read Committed usano il controllo delle versioni delle righe anziché i blocchi condivisi.  
  
> [!NOTE]  
> Alcune applicazioni si avvalgono della funzione di blocco del livello di isolamento Read Committed. Per tali applicazioni sono necessarie alcune modifiche prima di abilitare questa opzione.  
  
 Anche il livello di isolamento dello snapshot utilizza il controllo delle versioni delle righe, che non si avvale dei blocchi condivisi durante le operazioni di lettura. Affinché sia possibile eseguire transazioni con il livello di isolamento dello snapshot, è necessario impostare l'opzione di database `ALLOW_SNAPSHOT_ISOLATION` su ON.  
  
 Implementare questi livelli di isolamento per ridurre i deadlock che si possono verificare tra operazioni di lettura e di scrittura.  
  
#### <a name="use-bound-connections"></a>Usare le connessioni associate  
 L'utilizzo di connessioni associate garantisce la cooperazione tra due o più connessioni aperte dalla stessa applicazione. I blocchi acquisiti dalle connessioni secondarie vengono gestiti come se fossero stati acquisiti dalla connessione primaria e viceversa. Di conseguenza non si verificano blocchi reciproci tra le connessioni.  
  
## <a name="lock-partitioning"></a><a name="lock_partitioning"></a> Partizionamento di blocchi  
 Nei sistemi di computer di grandi dimensioni, i blocchi su oggetti di riferimento utilizzati di frequente può creare colli di bottiglia a livello delle prestazioni poiché l'acquisizione e il rilascio di blocchi determina la contesa nelle risorse di blocco interne. Il partizionamento dei blocchi migliora le prestazioni poiché suddivide una singola risorsa di blocco in più risorse di blocco. Questa caratteristica è disponibile unicamente per i sistemi con 16 o più CPU, viene abilitata automaticamente e non è possibile disabilitarla. È possibile partizionare solo i blocchi degli oggetti. I blocchi di oggetti che includono un sottotipo non vengono partizionati. Per altre informazioni, vedere [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="understanding-lock-partitioning"></a>Informazioni sul partizionamento dei blocchi  
 Le attività di blocco accedono a numerose risorse condivise, due delle quali vengono ottimizzate dal partizionamento dei blocchi:  
  
-   **Spinlock**. Consente di controllare l'accesso a una risorsa di blocco, ad esempio una riga o una tabella.  
  
     Senza il partizionamento dei blocchi, uno spinlock gestisce tutte le richieste di blocco per una singola risorsa di blocco. Nei sistemi con un volume di attività elevato, è possibile che si verifichi una contesa tra le risorse di blocco che rimangono in attesa della disponibilità dello spinlock. In questo caso, l'acquisizione dei blocchi può determinare un collo di bottiglia e influire negativamente sulle prestazioni.  
  
     Il partizionamento dei blocchi suddivide una singola risorsa di blocco in più risorse di blocco per ridurre la contesa per una singola risorsa di blocco e distribuisce il carico su più spinlock.  
  
-   **Memoria**. Consente di archiviare le strutture delle risorse di blocco.  
  
     Dopo che lo spinlock è stato acquisito, le strutture di blocco vengono archiviate in memoria ed è quindi possibile accedervi ed eventualmente modificarle. La distribuzione dell'accesso ai blocchi su più risorse consente di eliminare la necessità di trasferire blocchi di memoria tra le CPU e di migliorare pertanto le prestazioni.  
  
### <a name="implementing-and-monitoring-lock-partitioning"></a>Implementazione e monitoraggio del partizionamento dei blocchi  
 Il partizionamento dei blocchi è attivato per impostazione predefinita nei sistemi con 16 o più CPU. Se il partizionamento è abilitato, nel log degli errori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene registrato un messaggio informativo.  
  
 Se vengono acquisiti blocchi su una risorsa partizionata:  
  
-   Su una singola partizione vengono acquisite solo le modalità di blocco NL, SCH-S, IS, IU e IX.  
  
-   L'acquisizione dei blocchi condivisi (S) ed esclusivi (X) e degli altri blocchi in modalità diverse da NL, SCH-S, IS, IU e IX su tutte le partizioni deve essere eseguita a partire dalla partizione con ID 0 e successivamente in base all'ordine degli ID di partizione. Questi blocchi su una risorsa partizionata utilizzano una quantità di memoria maggiore rispetto ai blocchi con la stessa modalità su una risorsa non partizionata, poiché ogni partizione rappresenta in effetti un blocco distinto. L'aumento della memoria è determinato dal numero delle partizioni. I contatori dei blocchi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Performance Monitor di Windows visualizzano informazioni sulla memoria utilizzata dai blocchi partizionati e non partizionati.  
  
 All'avvio della transazione, viene assegnata una transazione a una partizione. Tutte le richieste di blocco partizionabili relative alla transazione utilizzano la partizione assegnata a tale transazione. In questo modo, l'accesso alle risorse di blocco dello stesso oggetto da parte di transazioni diverse viene distribuito su partizioni diverse.  
  
 Nella colonna `resource_lock_partition` della DMV `sys.dm_tran_locks` è disponibile l'ID di partizione di blocco per una risorsa di blocco partizionata. Per altre informazioni, vedere [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="working-with-lock-partitioning"></a>Utilizzo del partizionamento dei blocchi  
 Negli esempi di codice seguenti viene illustrato il partizionamento dei blocchi. Due transazioni vengono eseguite in due sessioni diverse allo scopo di illustrare il partizionamento dei blocchi in un computer con 16 CPU.  
  
 Le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] seguenti creano gli oggetti di prova utilizzati negli esempi successivi.  
  
```sql  
-- Create a test table.  
CREATE TABLE TestTable  (col1 int);  
GO  
  
-- Create a clustered index on the table.  
CREATE CLUSTERED INDEX ci_TestTable   
    ON TestTable (col1);  
GO  
  
-- Populate the table.  
INSERT INTO TestTable VALUES (1);  
GO  
```  
  
#### <a name="example-a"></a>Esempio A  
 Sessione 1:  
  
 Un'istruzione `SELECT` viene eseguita in una transazione. A causa dell'hint di blocco `HOLDLOCK`, l'istruzione acquisisce e mantiene un blocco preventivo condiviso (IS) sulla tabella. Nell'esempio, i blocchi di riga e di pagina vengono ignorati. Il blocco IS viene acquisito solo nella partizione assegnata alla transazione. In questo esempio si presume che il blocco IS venga acquisito nell'ID partizione 7.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Sessione 2:  
  
 Viene avviata una transazione e l'istruzione `SELECT` eseguita nella transazione acquisisce e mantiene un blocco condiviso (S) sulla tabella. Il blocco S viene acquisito in tutte le partizioni e pertanto si ottengono più blocchi di tabella, uno per ogni partizione. Ad esempio, in un sistema con 16 CPU verranno applicati 16 blocchi S negli ID di partizione di blocco da 0 a 15. Il blocco S è compatibile con il blocco IS mantenuto dalla transazione sull'ID di partizione 7 nella sessione 1 e pertanto non si verifica alcun blocco tra le transazioni.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCK, HOLDLOCK);  
```  
  
 Sessione 1:  
  
 L'istruzione `SELECT` seguente viene eseguita nella transazione ancora attiva nella sessione 1. A causa dell'hint di blocco della tabella esclusivo (X), la transazione tenta di acquisire un blocco X sulla tabella. Il blocco S mantenuto dalla transazione nella sessione 2 blocca tuttavia il blocco X in corrispondenza dell'ID di partizione 0.  
  
```sql  
SELECT col1  
FROM TestTable  
WITH (TABLOCKX);  
```  
  
#### <a name="example-b"></a>Esempio B  
 Sessione 1:  
  
 Un'istruzione `SELECT` viene eseguita in una transazione. A causa dell'hint di blocco `HOLDLOCK`, l'istruzione acquisisce e mantiene un blocco preventivo condiviso (IS) sulla tabella. Nell'esempio, i blocchi di riga e di pagina vengono ignorati. Il blocco IS viene acquisito solo nella partizione assegnata alla transazione. In questo esempio si presume che il blocco IS venga acquisito nell'ID di partizione 6.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Sessione 2:  
  
 Un'istruzione `SELECT` viene eseguita in una transazione. A causa dell'hint di blocco `TABLOCKX`, la transazione tenta di acquisire un blocco esclusivo (X) sulla tabella. Tenere presente che il blocco X deve essere acquisito per tutte le partizioni a partire dall'ID di partizione 0. Tale blocco viene acquisito per tutti gli ID di partizione da 0 a 5 ma viene bloccato dal blocco IS acquisito per l'ID di partizione 6.  
  
 Negli ID di partizione da 7 a 15 che non sono stati ancora raggiunti dal blocco X, le altre transazioni possono continuare ad acquisire blocchi.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCKX, HOLDLOCK);  
```   
  
##  <a name="row-versioning-based-isolation-levels-in-the-ssdenoversion"></a><a name="Row_versioning"></a> Livelli di isolamento basati sul controllo delle versioni delle righe nel [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 A partire da [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] include un'implementazione di un livello di isolamento della transazione esistente, Read Committed, che offre uno snapshot a livello di istruzione tramite il controllo delle versioni delle righe. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] offre anche un livello di isolamento della transazione, ovvero uno snapshot, che offre uno snapshot a livello di transazione anch'esso tramite il controllo delle versioni delle righe.  
  
 Il controllo delle versioni delle righe è un framework generale in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che richiama un meccanismo copy-on-write quando una riga viene modificata o eliminata. È pertanto necessario che durante l'esecuzione della transazione, la versione precedente della riga sia disponibile per le transazioni che richiedono un precedente stato consistente dal punto di vista transazionale. Il controllo delle versioni delle righe utilizzato per eseguire quanto segue:  
  
-   Compilare le tabelle **inserite** ed **eliminate** nei trigger. Le righe modificate dal trigger vengono sottoposte al controllo delle versioni. Questo include le righe modificate dall'istruzione che ha avviato il trigger, nonché qualsiasi modifica ai dati eseguita dal trigger.  
-   Supportare più set di risultati attivi (MARS). Se una sessione MARS genera un'istruzione di modifica dei dati (ad esempio `INSERT`, `UPDATE` o `DELETE`) nel momento in cui è attivo un set di risultati, le righe influenzate dall'istruzione di modifica sono sottoposte al controllo delle versioni.  
-   Supportare operazioni di indice che specificano l'opzione ONLINE.  
-   Supportare i livelli di isolamento delle transazioni basate sul controllo delle versioni delle righe:  
    -   Una nuova implementazione del livello di isolamento read-committed che utilizza il controllo delle versioni delle righe per la consistenza di lettura a livello di istruzione.  
    -   Un nuovo livello di isolamento, snapshot, per offrire consistenza di lettura a livello di transazione.  
  
 Il database `tempdb` deve disporre di spazio sufficiente per l'archivio delle versioni. Quando lo spazio in `tempdb` è esaurito, le operazioni di aggiornamento arrestano la generazione di versioni, senza errori, mentre le operazioni di lettura possono avere esito negativo in quanto una particolare versione della riga necessaria non esiste più. Questo ha effetto su operazioni come indicizzazione online, MARS e di trigger.  
  
 L'utilizzo del controllo delle versioni delle righe per transazioni snapshot e read-committed è un processo in due passaggi:  
  
1.  Impostare una o entrambe le opzioni di database `READ_COMMITTED_SNAPSHOT` e `ALLOW_SNAPSHOT_ISOLATION` su ON.  
2.  Impostazione del corretto livello di isolamento delle transazioni in un'applicazione:  

    -   Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` è ON, le transazioni che impostano il livello di isolamento Read Committed usano il controllo delle versioni delle righe.  
    -   Quando l'opzione di database `ALLOW_SNAPSHOT_ISOLATION` è ON, le transazioni possono impostare il livello di isolamento snapshot.  
  
 Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` o `ALLOW_SNAPSHOT_ISOLATION` è impostata su ON, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] assegna un numero di sequenza della transazione (XSN) a ogni transazione che modifica i dati usando il controllo delle versioni delle righe. Le transazioni vengono avviate all'esecuzione di un'istruzione `BEGIN TRANSACTION`. Tuttavia, il numero di sequenza della transazione inizia con la prima operazione di lettura o scrittura dopo l'istruzione BEGIN TRANSACTION. Il numero di sequenza della transazione viene incrementato di uno a ogni assegnazione.  
  
 Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` o `ALLOW_SNAPSHOT_ISOLATION` è impostata su ON, le copie logiche (versioni) vengono mantenute per tutte le modifiche ai dati eseguite nel database. Ogni volta che una riga viene modificata da una transazione specifica, l'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] archivia una versione della precedente immagine della riga di cui è stato eseguito il commit in `tempdb`. Ogni versione è contrassegnata con il numero di sequenza della transazione che ha eseguito la modifica. Le versioni delle righe modificate vengono concatenate utilizzando un elenco di collegamenti. Il valore di riga più recente viene sempre archiviato nel database corrente e concatenato alle righe con versioni archiviate in `tempdb`.  
  
> [!NOTE]  
> Per la modifica di oggetti LOB, solo il frammento modificato viene copiato nell'archivio delle versioni archiviato in `tempdb`.  
  
 Le versioni delle righe vengono mantenute abbastanza a lungo da soddisfare i requisiti delle transazioni in esecuzione con i livelli di isolamento basati sul controllo delle versioni delle righe. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tiene traccia del primo numero di sequenza della transazione significativo ed elimina periodicamente tutte le versioni di riga che includono numeri di sequenza della transazione inferiori al primo numero di sequenza significativo.  
  
 Quando entrambe le opzioni di database sono impostate su OFF, solo le righe modificate da trigger o sessioni MARS, oppure lette da operazioni di indice ONLINE, vengono sottoposte al controllo delle versioni. Queste versioni di riga vengono rilasciate quando non sono più necessarie. Un thread in background viene eseguito periodicamente per rimuovere le versioni di riga non aggiornate.  
  
> [!NOTE]  
> Per le transazioni con esecuzione rapida, è possibile che una versione di una riga modificata venga memorizzata nel pool di buffer senza essere scritta nei file su disco del database `tempdb`. Se la necessità della riga con controllo delle versioni è di breve durata, essa verrà semplicemente eliminata dal pool di buffer, senza che si verifichi necessariamente un overhead I/O.  
  
### <a name="behavior-when-reading-data"></a>Comportamento durante la lettura dei dati  
 Quando le transazioni in esecuzione con l'isolamento basato sul controllo delle versioni delle righe eseguono la lettura dei dati, le operazioni di lettura non acquisiscono blocchi condivisi (S) sui dati da leggere, e pertanto non bloccano le transazioni che stanno modificando i dati. Inoltre, l'overhead delle risorse di blocco è ridotto al minimo in quanto il numero di blocchi acquisiti si riduce. L'isolamento read-committed tramite controllo delle versioni delle righe e isolamento dello snapshot è pensato per assicurare consistenze a livello di istruzione o di transazione dei dati sottoposti a controllo delle versioni.  
  
 Tutte le query, incluse le transazioni in esecuzione con i livelli di isolamento basati sul controllo delle versioni delle righe, acquisiscono blocchi di stabilità dello schema (Sch-S) durante le fasi di compilazione ed esecuzione. Per questo motivo, le query vengono bloccate quando una transazione simultanea mantiene attivo un blocco di modifica dello schema sulla tabella. Ad esempio, un'operazione DDL (Data Definition Language) acquisisce un blocco Sch-M prima di modificare le informazioni dello schema della tabella. Le transazioni delle query, incluse quelle in esecuzione con un livello di isolamento basato sul controllo delle versioni delle righe, vengono quindi bloccate durante il tentativo di acquisizione di un blocco Sch-S. Una query con blocco Sch-S blocca invece le transazioni simultanee che tentano di acquisire un blocco Sch-M.  
  
 Quando una transazione che utilizza il livello di isolamento dello snapshot ha inizio, l'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] registra tutte le transazioni attualmente attive. Quando la transazione snapshot legge una riga che include una catena delle versioni, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] segue la catena e recupera la riga per la quale il numero di sequenza della transazione è:  
  
-   Più vicino ma inferiore al numero di sequenza della transazione snapshot che sta leggendo la riga.  
  
-   Non incluso nell'elenco delle transazioni attive quando la transazione snapshot ha avuto inizio.  
  
 Le operazioni di lettura eseguite da una transazione snapshot recuperano l'ultima versione di ogni riga di cui è stato eseguito il commit al momento dell'avvio della transazione snapshot. Questo determina uno snapshot dei dati, consistente dal punto di vista transazionale, nello stato in cui si trovavano all'inizio della transazione.  
  
 Le transazioni read-committed che utilizzano il controllo delle versioni delle righe funzionano in modo analogo. La differenza è rappresentata dal fatto che la transazione read-committed non utilizza un proprio numero di sequenza della transazione nella scelta delle versioni delle righe. A ogni avvio di un'istruzione, la transazione read-committed legge l'ultimo numero di sequenza della transazione generato per l'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Si tratta del numero di sequenza della transazione utilizzato per selezionare le versioni di riga corrette per l'istruzione. Questo consente alle transazioni read-committed di visualizzare uno snapshot dei dati nello stato in cui si trovavano all'inizio di ogni istruzione.  
  
> [!NOTE]  
> Anche se le transazioni read-committed che utilizzano il controllo delle versioni delle righe offrono una vista consistente dei dati dal punto di vista transazionale a livello di istruzione, le versioni delle righe generate o utilizzate da questo tipo di transazione vengono mantenute fino al completamento della transazione.  
  
### <a name="behavior-when-modifying-data"></a>Comportamento durante la modifica dei dati  
 In una transazione read-committed che utilizza il controllo delle versioni delle righe, la selezione delle righe da aggiornare viene eseguita utilizzando un'analisi di blocco in cui un blocco di aggiornamento (U) viene utilizzato sulla riga di dati durante la lettura dei valori dei dati. Questo avviene anche per una transazione read-committed che non utilizza il controllo delle versioni delle righe. Se la riga di dati non rispetta i criteri di aggiornamento, il blocco di aggiornamento viene rilasciato sulla riga e la riga successiva viene bloccata e analizzata.  
  
 Le transazioni eseguite con isolamento dello snapshot adottano un approccio ottimistico alla modifica dei dati, in quanto acquisiscono blocchi sui dati prima di eseguire la modifica con l'unico scopo di imporre i vincoli. In caso contrario, non vengono acquisiti blocchi sui dati fino a quando questi non devono essere modificati. Quando una riga di dati rispetta i criteri di aggiornamento, la transazione snapshot verifica che la riga di dati non sia stata modificata da una transazione simultanea che abbia eseguito il commit dopo l'inizio della transazione snapshot. Se la riga di dati è stata modificata all'esterno della transazione snapshot, si verifica un conflitto di aggiornamento e la transazione snapshot viene interrotta. Il conflitto di aggiornamento viene gestito da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] e non è possibile disabilitare il rilevamento dei conflitti di aggiornamento.  
  
> [!NOTE]  
> Le operazioni di aggiornamento in esecuzione con il livello di isolamento dello snapshot vengono eseguite internamente con isolamento read-committed quando la transazione snapshot accede a uno degli elementi seguenti:  
>  
> Una tabella con un vincolo FOREIGN KEY.  
>  
> Una tabella cui viene fatto riferimento nel vincolo FOREIGN KEY di un'altra tabella.  
>  
> Una vista indicizzata che fa riferimento a più tabelle.  
>  
> Tuttavia, anche in queste condizioni l'operazione di aggiornamento continuerà per verificare che i dati non siano stati modificati da un'altra transazione. In questo caso, la transazione snapshot rileverà un conflitto di aggiornamento e verrà interrotta.  
  
### <a name="behavior-in-summary"></a>Riepilogo  
 Nella tabella seguente vengono illustrate le differenze tra isolamento dello snapshot e isolamento read-committed utilizzando il controllo delle versioni delle righe.  
  
|Proprietà|Livello di isolamento read-committed che utilizza il controllo delle versioni delle righe|Livello di isolamento dello snapshot|  
|--------------|----------------------------------------------------------|------------------------------|  
|L'opzione di database che deve essere impostata su ON per attivare il supporto necessario.|READ_COMMITTED_SNAPSHOT|ALLOW_SNAPSHOT_ISOLATION|  
|Modalità con cui una sessione richiede il tipo specifico di controllo delle versioni delle righe.|Utilizzare il livello di isolamento read-committed predefinito o eseguire l'istruzione SET TRANSACTION ISOLATION LEVEL per specificare il livello di isolamento READ COMMITTED. L'operazione può essere eseguita dopo l'inizio della transazione.|Richiede l'esecuzione di SET TRANSACTION ISOLATION LEVEL per specificare il livello di isolamento SNAPSHOT prima dell'inizio della transazione.|  
|La versione dei dati letta dalle istruzioni.|Tutti i dati di cui è stato eseguito il commit prima dell'inizio di ogni istruzione.|Tutti i dati di cui è stato eseguito il commit prima dell'inizio di ogni transazione.|  
|Procedura di gestione degli aggiornamenti.|Esegue il ripristino dalle versioni delle righe ai dati attuali per selezionare le righe per l'aggiornamento e utilizza i blocchi di aggiornamento sulle righe di dati selezionate. Acquisisce blocchi esclusivi sulle righe di dati effettive da modificare. Nessun rilevamento dei conflitti di aggiornamento.|Utilizza le versioni delle righe per selezionare le righe da aggiornare. Tenta di acquisire un blocco esclusivo sulla riga di dati effettiva da modificare. Se i dati sono stati modificati da un'altra transazione, si verifica un conflitto di aggiornamento e la transazione snapshot viene interrotta.|  
|Rilevamento dei conflitti di aggiornamento.|No.|Supporto integrato. Non disabilitabile.|  
  
### <a name="row-versioning-resource-usage"></a>Uso delle risorse di controllo delle versioni delle righe  
 L'infrastruttura di controllo delle versioni delle righe supporta le caratteristiche seguenti disponibili in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]:  
  
-   Trigger  
-   MARS (Multiple Active Result Set)  
-   Indicizzazione online  
  
 L'infrastruttura di controllo delle versioni delle righe supporta inoltre i livelli di isolamento delle transazioni basati sul controllo delle versioni delle righe seguenti, disattivati per impostazione predefinita:  
  
-   Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` è impostata su ON, le transazioni `READ_COMMITTED` offrono coerenza di lettura a livello di istruzioni tramite il controllo delle versioni delle righe.  
-   Quando l'opzione di database `ALLOW_SNAPSHOT_ISOLATION` è impostata su ON, le transazioni `SNAPSHOT` offrono coerenza di lettura a livello di transizioni tramite il controllo delle versioni delle righe.  
  
 I livelli di isolamento basati sul controllo delle versioni delle righe consentono di ridurre il numero di blocchi acquisiti per transazione eliminando l'utilizzo di blocchi condivisi nelle operazioni di lettura. In questo modo, vengono garantite prestazioni di sistema migliori grazie alla riduzione delle risorse utilizzate per la gestione dei blocchi. Le prestazioni risultano inoltre migliorate grazie al minor numero di volte in cui una transazione viene bloccata dai blocchi acquisiti da altre transazioni.  
  
 I livelli di isolamento basati sul controllo delle versioni delle righe consentono di ridurre le risorse necessarie per le modifiche dei dati. L'attivazione di tali opzioni comporta il controllo delle versioni di tutte le modifiche dei dati per il database. Una copia dei dati precedente la modifica viene archiviata nel database tempdb anche quando non vi è alcuna transazione attiva che utilizza l'isolamento basato sul controllo delle versioni delle righe. I dati in seguito alla modifica includono un indicatore di misura ai dati a cui è stato applicato il controllo delle versioni archiviati in tempdb. Per oggetti di grandi dimensioni, in tempdb viene copiata solo la parte dell'oggetto modificata.  
  
#### <a name="space-used-in-tempdb"></a>Spazio utilizzato in tempdb  
 Per ogni istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], in tempdb deve essere disponibile una quantità di spazio sufficiente per contenere le versioni di riga generate per ogni database incluso nell'istanza. L'amministratore del database deve fare in modo che in tempdb sia disponibile una quantità di spazio elevata per supportare l'archivio versioni. In tempdb sono inclusi due archivi versioni:  
  
-   L'archivio versioni compilazione degli indici online viene utilizzato per le compilazioni degli indici online in tutti i database.  
-   L'archivio versioni comune viene utilizzato per tutte le altre operazioni di modifica dei dati in tutti i database.  
  
 Le versioni di riga devono essere archiviate per tutto il tempo necessario a una transazione attiva per accedervi. Una volta al minuto un thread in background rimuove le versioni di riga che non sono più necessarie e libera il relativo spazio utilizzato in tempdb. Una transazione con esecuzione prolungata impedisce che venga liberato lo spazio nell'archivio versioni se si verificano le condizioni seguenti:  
  
-   Viene utilizzato l'isolamento basato sul controllo delle versioni delle righe.  
-   Vengono utilizzati trigger, MARS o operazioni di compilazione di indici online.  
-   Vengono generate versioni di riga.  
  
> [!NOTE]  
> Quando viene richiamato un trigger all'interno di una transazione, le versioni di riga create dal trigger vengono mantenute fino alla fine della transazione, anche se le versioni di riga non sono più necessarie al termine del trigger. Questa situazione riguarda inoltre le transazioni Read committed che utilizzano il controllo delle versioni delle righe. Con questo tipo di transazione, una vista del database consistente a livello di transazioni è necessaria solo per ogni istruzione della transazione. Le versioni di riga create per un'istruzione della transazione non sono pertanto più necessarie al termine dell'istruzione. Le versioni di riga create da ogni istruzione della transazione vengono tuttavia mantenute fino al termine della transazione.  
  
 Quando in tempdb viene esaurito tutto lo spazio, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] forza la compattazione degli archivi versioni. Durante il processo di compattazione, le transazioni con esecuzione prolungata che non hanno ancora generato versioni di riga vengono contrassegnate come vittime. Nel log degli errori viene generato un messaggio 3967 per ogni transazione vittima. Se una transazione viene contrassegnata come vittima, non può più leggere le versioni di riga nell'archivio versioni. Quando la transazione tenta di leggere le versioni di riga, viene generato un messaggio 3966 e viene eseguito il rollback della transazione. Se il processo di compattazione ha esito positivo, lo spazio viene reso disponibile in tempdb. In caso contrario, lo spazio di tempdb si esaurisce e si verificano le situazioni seguenti:  
  
-   L'esecuzione delle operazioni di scrittura prosegue, ma non vengono generate versioni. Nel log degli errori viene visualizzato un messaggio informativo (3959), ma la transazione che scrive i dati non subisce alcun effetto.  
  
-   Le transazioni che tentano di accedere alle versioni di riga non generate a causa di un rollback completo di tempdb vengono terminate con un errore 3958.  
  
#### <a name="space-used-in-data-rows"></a>Spazio utilizzato nelle righe di dati  
 Ogni riga del database può utilizzare fino a 14 byte alla fine della riga per le informazioni relative al controllo delle versioni delle righe. Tali informazioni contengono il numero di sequenza della transazione che ha eseguito il commit della versione e il puntatore alla riga di cui è stato eseguito il controllo delle versioni. Questi 14 byte vengono aggiunti alla prima modifica della riga oppure quando viene inserita una nuova riga se si verifica una delle condizioni seguenti:  
  
-   Le opzioni `READ_COMMITTED_SNAPSHOT` o `ALLOW_SNAPSHOT_ISOLATION` sono impostate su ON.  
-   La tabella include un trigger.  
-   Viene utilizzato MARS (Multiple Active Results Set).  
-   Nella tabella sono in esecuzione operazioni di compilazione di indici online.  
  
 I 14 byte vengono rimossi dalla riga del database alla prima modifica della riga quando si verificano tutte le condizioni seguenti:  
  
-   Le opzioni `READ_COMMITTED_SNAPSHOT` e `ALLOW_SNAPSHOT_ISOLATION` sono impostate su OFF.  
-   Il trigger non è più presente nella tabella.  
-   Non si utilizza MARS.  
-   Le operazioni di compilazione di indici online non sono in esecuzione.  
  
 Se si utilizza una qualsiasi delle caratteristiche di controllo delle versioni delle righe, potrebbe essere necessario allocare spazio su disco sufficiente per includere i 14 byte per ogni riga del database. L'aggiunta delle informazioni relative al controllo delle versioni delle righe può provocare la divisione delle pagine di indice o l'allocazione di una nuova pagina di dati se nella pagina corrente non è disponibile spazio sufficiente. Se la lunghezza media della riga, ad esempio, è 100 byte, i 14 byte aggiuntivi provocano un aumento del 14 percento della tabella esistente.  
  
 La riduzione del [fattore di riempimento](../relational-databases/indexes/specify-fill-factor-for-an-index.md) potrebbe impedire o ridurre la frammentazione di pagine di indice. Per visualizzare informazioni sulla frammentazione dei dati e degli indici per una tabella o visualizzazione specificata, è possibile usare [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).  
  
#### <a name="space-used-in-large-objects"></a>Spazio utilizzato in oggetti di grandi dimensioni  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] supporta sei tipi di dati in grado di includere stringhe estese di un massimo di 2 gigabyte (GB) di lunghezza: `nvarchar(max)`, `varchar(max)`, `varbinary(max)`, `ntext`, `text` e `image`. Le stringhe estese per cui vengono utilizzati questi tipi di dati vengono archiviate in una serie di frammenti di dati collegati alla riga di dati. Le informazioni relative al controllo delle versioni delle righe vengono archiviate in ogni frammento utilizzato per archiviare tali stringhe estese. I frammenti di dati sono una raccolta di pagine dedicate a oggetti di grandi dimensioni in una tabella.  
  
 Man mano che nel database vengono aggiunti nuovi valori di grandi dimensioni, questi valori vengono allocati utilizzando un massimo di 8040 byte di dati per frammento. Nelle versioni precedenti di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] vengono archiviati fino a 8080 byte di dati `ntext`, `text` o `image` per frammento.  
  
 I dati LOB (Large Object) esistenti `ntext`, `text` e `image` non vengono aggiornati per lasciare spazio alle informazioni relative al controllo delle versioni delle righe quando un database viene aggiornato a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] da una versione precedente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La prima volta che i dati LOB vengono modificati, tuttavia, vengono aggiornati dinamicamente per consentire l'archiviazione delle informazioni relative al controllo delle versioni. Questa operazione viene eseguita anche se non vengono generate versioni di riga. In seguito all'aggiornamento dei dati LOB, il numero massimo di byte archiviati per frammento viene ridotto da 8080 byte a 8040 byte. Il processo di aggiornamento equivale all'eliminazione del valore LOB e al reinserimento dello stesso valore. I dati LOB vengono aggiornati anche se viene modificato un solo byte. Si tratta di un'operazione occasionale per ogni colonna `ntext`, `text` o `image`, ma ogni operazione può generare una quantità elevata di allocazioni di pagina e un'ingente attività di I/O, a seconda delle dimensioni dei dati LOB. Tali operazioni possono inoltre generare un'attività di registrazione elevata se la modifica viene registrata completamente. Viene eseguita la registrazione minima delle operazioni WRITETEXT e UPDATETEXT se la modalità di recupero del database non è impostata su FULL.  
  
 I tipi di dati `nvarchar(max)`, `varchar(max)` e `varbinary(max)` non sono disponibili nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per questo motivo, tali tipi dati non sono interessati da problemi di aggiornamento.  
  
 Per soddisfare questo requisito, è necessario allocare spazio su disco sufficiente.  
  
#### <a name="monitoring-row-versioning-and-the-version-store"></a>Controllo delle versioni delle righe e archivio versioni  
 Per il controllo delle versioni delle righe, l'archivio versioni e i processi di isolamento dello snapshot per le prestazioni e i problemi, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] offre strumenti in forma di viste a gestione dinamica e contatori delle prestazioni di Windows System Monitor.  
  
##### <a name="dmvs"></a>Viste a gestione dinamica  
 Le seguenti viste a gestione dinamica forniscono informazioni sullo stato attuale del sistema di tempdb e l'archivio versioni, nonché delle transazioni che utilizzano il controllo delle versioni delle righe.  
  
 sys.dm_db_file_space_usage. Restituisce informazioni sull'utilizzo dello spazio per ogni file nel database. Per altre informazioni, vedere [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md).  
  
 sys.dm_db_session_space_usage. Restituisce informazioni relative alle attività di allocazione e deallocazione delle pagine per sessione per il database. Per altre informazioni, vedere [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md).  
  
 sys.dm_db_task_space_usage. Restituisce informazioni sulle allocazioni e deallocazioni delle pagine per ogni attività per il database. Per altre informazioni, vedere [sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md).  
  
 sys.dm_tran_top_version_generators. Restituisce una tabella virtuale per gli oggetti indicante la maggior parte delle versioni nell'archivio versioni. Raggruppa le 256 lunghezze superiori dei record di aggregazione per database_id e rowset_id. Utilizzare questa funzione per trovare i maggiori consumer dell'archivio versioni. Per altre informazioni, vedere [sys.dm_tran_top_version_generators &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-top-version-generators-transact-sql.md).  
  
 sys.dm_tran_version_store. Restituisce una tabella virtuale in cui vengono visualizzati tutti i record di versione nell'archivio versioni comune. Per altre informazioni, vedere [sys.dm_tran_version_store &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md).  

 sys.dm_tran_version_store_space_usage. Restituisce una tabella virtuale che visualizza lo spazio totale in tempdb usato dai record dell'archivio versioni per ogni database. Per altre informazioni, vedere [sys.dm_tran_version_store_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-space-usage.md).  

> [!NOTE]  
> sys.dm_tran_top_version_generators e sys.dm_tran_version_store possono essere funzioni molto costose da eseguire, poiché entrambe inviano query all'intero archivio versioni, che potrebbe avere dimensioni estremamente elevate.  
> sys.dm_tran_version_store_space_usage è efficiente e non costosa da eseguire poiché non naviga attraverso i singoli record dell'archivio versioni e restituisce lo spazio dell'archivio versioni aggregato usato in tempdb per ogni database
  
 sys.dm_tran_active_snapshot_database_transactions. Restituisce una tabella virtuale per tutte le transazioni attive in tutti i database dell'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che utilizzano il controllo delle versioni delle righe. Le transazioni di sistema non vengono visualizzate in questa DMV. Per altre informazioni, vedere [sys.dm_tran_active_snapshot_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md).  
  
 sys.dm_tran_transactions_snapshot. Restituisce una tabella virtuale in cui vengono visualizzati gli snapshot creati da ogni transazione. Lo snapshot contiene il numero di sequenza delle transazioni attive che utilizzano il controllo delle versioni delle righe. Per altre informazioni, vedere [sys.dm_tran_transactions_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-transactions-snapshot-transact-sql.md).  
  
 sys.dm_tran_current_transaction. Restituisce una singola riga in cui vengono visualizzate le informazioni sullo stato correlate al controllo delle versioni delle righe per la transazione nella sessione corrente. Per altre informazioni, vedere [sys.dm_tran_current_transaction &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md).  
  
 sys.dm_tran_current_snapshot. Restituisce una tabella virtuale in cui vengono visualizzate tutte le transazioni attive al momento in cui è stata avviata la transazione di isolamento dello snapshot corrente. Se la transazione corrente utilizza l'isolamento dello snapshot, questa funzione non restituisce alcuna riga. sys.dm_tran_current_snapshot è simile a sys.dm_tran_transactions_snapshot, ad eccezione del fatto che restituisce solo le transazioni attive per lo snapshot corrente. Per altre informazioni, vedere [sys.dm_tran_current_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-snapshot-transact-sql.md).  
  
##### <a name="performance-counters"></a>Contatori delle prestazioni  
 I contatori delle prestazioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] includono informazioni sulle prestazioni del sistema su cui incidono i processi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. I contatori delle prestazioni seguenti consentono di monitorare tempdb e l'archivio versioni, nonché le transazioni che utilizzano il controllo delle versioni delle righe. I contatori delle prestazioni sono inclusi nell'oggetto prestazione SQLServer:Transactions.  
  
 **Spazio disponibile in tempdb (KB)** . Esegue il monitoraggio della quantità, in kilobyte (KB), di spazio libero nel database tempdb. In tempdb deve essere disponibile una quantità di spazio sufficiente per gestire l'archivio versioni che supporta l'isolamento dello snapshot.  
  
 Nella formula seguente viene eseguito un calcolo approssimativo delle dimensioni dell'archivio versioni. Per le transazioni con esecuzione prolungata, può essere utile monitorare la frequenza di generazione e pulizia per stimare le dimensioni massime dell'archivio versioni.  
  
 [dimensioni dell'archivio versioni comune] = 2 * [dati dell'archivio versioni generati al minuto] \* [tempo massimo (in minuti) di esecuzione della transazione]  
  
 Il tempo massimo di esecuzione delle transazioni non deve includere le compilazioni di indici online. Poiché queste operazioni possono richiedere una quantità di tempo prolungata nelle tabelle di dimensioni molto elevate, le compilazioni di indici online utilizzano un archivio versioni distinto. Le dimensioni approssimative dell'archivio versioni compilazioni di indici online equivale alla quantità di dati modificati nella tabella, inclusi tutti gli indici, quando è attiva la compilazione di indici online.  
  
 **Dimensioni archivio versioni (KB)** . Esegue il monitoraggio delle dimensioni in KB di tutti gli archivi versioni. Queste informazioni consentono di determinare la quantità di spazio necessario nel database tempdb per l'archivio versioni. Il monitoraggio di questo contatore per un certo periodo di tempo offre una stima utile dell'ulteriore spazio necessario per tempdb.  
  
 `Version Generation rate (KB/s)`. Esegue il monitoraggio della frequenza di generazione delle versioni in KB al secondo in tutti gli archivi versioni.  
  
 `Version Cleanup rate (KB/s)`. Esegue il monitoraggio della frequenza di pulizia delle versioni in KB al secondo in tutti gli archivi versioni.  
  
> [!NOTE]  
> Le informazioni incluse in Frequenza generazione versioni (KB/s) e Frequenza pulizia versioni (KB/s) possono essere utilizzate per prevedere i requisiti di spazio del database tempdb.  
  
 **Conteggio unità archivio versioni**. Esegue il monitoraggio del conteggio delle unità dell'archivio versioni.  
  
 **Creazione unità archivio versioni**. Esegue il monitoraggio del numero totale di unità dell'archivio versioni create per archiviare le versioni di riga dal momento in cui è stata avviata l'istanza.  
  
 **Troncamento unità archivio versioni**. Esegue il monitoraggio del numero totale di unità dell'archivio versioni troncate dal momento in cui è stata avviata l'istanza. Un'unità dell'archivio versioni viene troncata quando in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene determinato che nessuna delle righe di versione archiviata nell'unità è necessaria per eseguire transazioni attive.  
  
 **Percentuale conflitti aggiornamento**. Esegue il monitoraggio del rapporto tra le transazione snapshot di aggiornamento in cui sono presenti conflitti di aggiornamento e il numero totale di transazioni snapshot di aggiornamento.  
  
 **Tempo massimo esecuzione transazione**. Esegue il monitoraggio del tempo massimo di esecuzione in secondi di qualsiasi transazione che utilizza il controllo delle versioni delle righe. Queste informazioni possono essere utilizzate per determinare se alcune transazioni vengano eseguite in una quantità di tempo eccessiva.  
  
 **Transazioni**. Esegue il monitoraggio del numero totale di transazioni attive. Non sono incluse le transazioni di sistema.  
  
 `Snapshot Transactions`. Esegue il monitoraggio del numero totale di transazioni snapshot attive.  
  
 `Update Snapshot Transactions`. Esegue il monitoraggio del numero totale di transazioni snapshot attive tramite cui vengono eseguite operazioni di aggiornamento.  
  
 `NonSnapshot Version Transactions`. Esegue il monitoraggio del numero totale di transazioni non snapshot attive che generano record di versione.  
  
> [!NOTE]  
> La somma dei valori indicati in Transazioni snapshot di aggiornamento e Transazioni di versione non snapshot rappresenta il numero totale di transazioni coinvolte nella generazione delle versioni. La differenza tra i valori indicati in Transazioni snapshot e Transazioni snapshot di aggiornamento rappresenta il numero di transazioni snapshot di sola lettura.  
  
### <a name="row-versioning-based-isolation-level-example"></a>Esempio di livello di isolamento basato sul controllo delle versioni delle righe  
 Nell'esempio seguente vengono illustrate le differenze di comportamento tra le transazioni di isolamento dello snapshot e le transazioni Read Committed che utilizzano il controllo delle versioni delle righe.  
  
#### <a name="a-working-with-snapshot-isolation"></a>R. Utilizzo dell'isolamento dello snapshot  
 In questo esempio una transazione di isolamento dello snapshot legge dati che verranno successivamente modificati da un'altra transazione. La transazione snapshot non blocca l'operazione di aggiornamento eseguita dall'altra transazione e continua a leggere dati dalla riga con versione, ignorando la modifica dei dati. Quando, tuttavia, la transazione snapshot tenta di modificare dati che sono già stati modificati da un'altra transazione, viene generato un errore e la transazione snapshot termina.  
  
 Nella sessione 1:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Enable snapshot isolation on the database.  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Start a snapshot transaction  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Nella sessione 2:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under snapshot isolation shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Nella sessione 1:  
  
```sql  
    -- Reissue the SELECT statement - this shows  
    -- the employee having 48 vacation hours.  The  
    -- snapshot transaction is still reading data from  
    -- the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Nella sessione 2:  
  
```sql  
-- Commit the transaction; this commits the data  
-- modification.  
COMMIT TRANSACTION;  
GO  
```  
  
 Nella sessione 1:  
  
```sql  
    -- Reissue the SELECT statement - this still   
    -- shows the employee having 48 vacation hours  
    -- even after the other transaction has committed  
    -- the data modification.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- Because the data has been modified outside of the  
    -- snapshot transaction, any further data changes to   
    -- that data by the snapshot transaction will cause   
    -- the snapshot transaction to fail. This statement   
    -- will generate a 3960 error and the transaction will   
    -- terminate.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION  
GO  
```  
  
#### <a name="b-working-with-read-committed-using-row-versioning"></a>B. Utilizzo di Read Committed con il controllo delle versioni delle righe  
 In questo esempio una transazione Read committed che utilizza il controllo delle versioni delle righe viene eseguita simultaneamente a un'altra transazione. La transazione Read committed si comporta in modo diverso da una transazione snapshot. Come una transazione snapshot, la transazione Read committed legge le righe con versione anche dopo che i dati sono stati modificati dall'altra transazione. A differenza di una transazione snapshot, tuttavia, la transazione Read committed:  
  
-   Legge i dati modificati dopo che l'altra transazione ha eseguito il commit delle modifiche dei dati.  
-   È in grado di aggiornare i dati modificati dall'altra transazione, a differenza della transazione snapshot.  
  
 Nella sessione 1:  
  
```sql  
USE AdventureWorks2016;  -- Or any earlier version of the AdventureWorks database.  
GO  
  
-- Enable READ_COMMITTED_SNAPSHOT on the database.  
-- For this statement to succeed, this session  
-- must be the only connection to the AdventureWorks2016  
-- database.  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
GO  
  
-- Start a read-committed transaction  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Nella sessione 2:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under read-committed using row versioning shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Nella sessione 1:  
  
```sql  
    -- Reissue the SELECT statement - this still shows  
    -- the employee having 48 vacation hours.  The  
    -- read-committed transaction is still reading data   
    -- from the versioned row and the other transaction   
    -- has not committed the data changes yet.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Nella sessione 2:  
  
```sql  
-- Commit the transaction.  
COMMIT TRANSACTION;  
GO  
```  
  
 Nella sessione 1:  
  
```sql  
    -- Reissue the SELECT statement which now shows the   
    -- employee having 40 vacation hours.  Being   
    -- read-committed, this transaction is reading the   
    -- committed data. This is different from snapshot  
    -- isolation which reads from the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- This statement, which caused the snapshot transaction   
    -- to fail, will succeed with read-committed using row versioning.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION;  
GO  
```  
  
### <a name="enabling-row-versioning-based-isolation-levels"></a>Attivazione dei livelli di isolamento basati sul controllo delle versioni delle righe  
 Gli amministratori di database definiscono le impostazioni a livello di database per il controllo delle versioni delle righe usando le opzioni di database `READ_COMMITTED_SNAPSHOT` e `ALLOW_SNAPSHOT_ISOLATION` dell'istruzione ALTER DATABASE.  
  
 Quando l'opzione di database `READ_COMMITTED_SNAPSHOT` è impostata su ON, i meccanismi usati per supportare l'opzione vengono attivati immediatamente. Quando si imposta l'opzione READ_COMMITTED_SNAPSHOT, nel database è consentita solo la connessione che esegue il comando `ALTER DATABASE`. Nel database non devono essere presenti altre connessioni aperte fino al completamento del comando ALTER DATABASE. Il database non deve essere in modalità utente singolo.  
  
 L'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] seguente abilita `READ_COMMITTED_SNAPSHOT`:  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
```  
  
 Quando l'opzione di database `ALLOW_SNAPSHOT_ISOLATION` è impostata su ON, l'istanza del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] non genera versioni di riga per i dati modificati fino al completamento di tutte le transazioni attive che includono dati modificati del database. Se sono presenti transazioni di modifica attive, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] imposta lo stato dell'opzione su `PENDING_ON`. Dopo il completamento di tutte le transazioni di modifica, lo stato dell'opzione viene modificato in ON. Gli utenti non possono avviare una transazione snapshot nel database fino a quando l'opzione non è impostata completamente su ON. Il database passa a uno stato PENDING_OFF quando l'amministratore del database imposta l'opzione `ALLOW_SNAPSHOT_ISOLATION` su OFF.  
  
 L'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] seguente consente di attivare l'opzione ALLOW_SNAPSHOT_ISOLATION:  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
```  
  
 Nella tabella seguente sono inclusi e descritti gli stati possibili dell'opzione ALLOW_SNAPSHOT_ISOLATION. L'utilizzo dell'istruzione ALTER DATABASE con l'opzione ALLOW_SNAPSHOT_ISOLATION non blocca l'accesso in corso da parte degli utenti ai dati del database.  
  
|Stato del framework di isolamento dello snapshot per il database corrente|Descrizione|  
|----------------------------------------------------------------|-----------------|  
|OFF|Il supporto per le transazioni di isolamento dello snapshot non è attivato. Non è consentita alcuna transazione di isolamento dello snapshot.|  
|PENDING_ON|Il supporto per le transazioni di isolamento dello snapshot è in stato di transizione (da OFF a ON). Le transazioni aperte devono essere completate.<br /><br /> Non è consentita alcuna transazione di isolamento dello snapshot.|  
|ON|Il supporto per le transazioni di isolamento dello snapshot è attivato.<br /><br /> Le transazioni snapshot sono consentite.|  
|PENDING_OFF|Il supporto per le transazioni di isolamento dello snapshot è in stato di transizione (da ON a OFF).<br /><br /> Le transazioni snapshot avviate dopo questo momento non possono accedere al database. Le transazioni di aggiornamento subiscono le conseguenze negative del controllo delle versioni nel database. Le transazioni snapshot esistenti possono continuare ad accedere al database senza problemi. Lo stato PENDING_OFF non viene modificato in OFF fino al termine di tutte le transazioni snapshot attive quando lo stato di isolamento dello snapshot del database era impostato su ON.|  
  
 Utilizzare la vista del catalogo `sys.databases` per determinare lo stato di entrambe le opzioni di database per il controllo delle versioni delle righe.  
  
 Tutti gli aggiornamenti alle tabelle utente e ad alcune tabelle di sistema archiviate nei database master e msdb generano versioni di riga.  
  
 L'opzione `ALLOW_SNAPSHOT_ISOLATION` viene impostata automaticamente su ON nei database master e msdb e non può essere disabilitata.  
  
 Gli utenti non possono impostare l'opzione `READ_COMMITTED_SNAPSHOT` su ON nel database master, tempdb o msdb.  
  
### <a name="using-row-versioning-based-isolation-levels"></a>Utilizzo di livelli di isolamento basati sul controllo delle versioni delle righe  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] l'infrastruttura di controllo delle versioni delle righe è sempre abilitata e viene utilizzata da più caratteristiche. Oltre a offrire livelli di isolamento basati sul controllo delle versioni delle righe, viene utilizzata per supportare le modifiche apportate in trigger e sessioni MARS (Multiple Active Result Sets) e le letture di dati per le operazioni sugli indici ONLINE.  
  
 I livelli di isolamento basati sul controllo delle versioni delle righe sono attivati a livello di database. Le applicazioni che accedono agli oggetti da database abilitati possono eseguire query utilizzando i livelli di isolamento seguenti:  
  
-   Read Committed, che utilizza il controllo delle versioni delle righe mediante l'impostazione dell'opzione di database `READ_COMMITTED_SNAPSHOT` su `ON`, come illustrato nell'esempio di codice seguente:  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET READ_COMMITTED_SNAPSHOT ON;  
    ```  
  
     Quando il database è abilitato per `READ_COMMITTED_SNAPSHOT`, tutte le query in esecuzione nel livello di isolamento Read Committed usano il controllo delle versioni delle righe. Ciò significa che le operazioni di lettura non comportano il blocco di quelle di aggiornamento.  
  
-   Isolamento dello snapshot, mediante l'impostazione dell'opzione di database `ALLOW_SNAPSHOT_ISOLATION` su `ON`, come illustrato nell'esempio di codice seguente:  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET ALLOW_SNAPSHOT_ISOLATION ON;  
    ```  
  
     Una transazione in esecuzione in un livello di isolamento dello snapshot può accedere alle tabelle nel database abilitate per lo snapshot. Per consentire l'accesso alle tabelle non abilitate per lo snapshot, è necessario modificare il livello di isolamento. Nell'esempio di codice seguente viene ad esempio illustrata un'istruzione `SELECT` che unisce in join due tabelle mentre è in esecuzione in una transazione snapshot. Una tabella appartiene a un database in cui non è stato attivato l'isolamento dello snapshot. Quando l'istruzione `SELECT` viene eseguita nel livello di isolamento dello snapshot, l'esecuzione non avviene correttamente.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
     Nell'esempio di codice seguente viene illustrata la stessa istruzione `SELECT` modificata in modo che il livello di isolamento della transazione sia Read Committed. Grazie a questa modifica, l'istruzione `SELECT` viene eseguita correttamente.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            WITH (READCOMMITTED)  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
#### <a name="limitations-of-transactions-using-row-versioning-based-isolation-levels"></a>Limiti per le transazioni che utilizzano livelli di isolamento basati sul controllo delle versioni delle righe  
 Quando si utilizzano livelli di isolamento basati sul controllo delle versioni delle righe, considerare i limiti seguenti:  
  
-   Non è possibile abilitare `READ_COMMITTED_SNAPSHOT` in tempdb, msdb o master.  
-   Le tabelle temporanee globali sono archiviate in tempdb. Quando si accede a una tabella temporanea globale all'interno di una transazione snapshot, è necessario che si verifichi una delle condizioni seguenti:  
    -   Impostare l'opzione di database `ALLOW_SNAPSHOT_ISOLATION` su ON in tempdb.  
    -   Utilizzare un hint di isolamento per modificare il livello di isolamento per l'istruzione.  
-   Le transazioni snapshot non hanno esito positivo nei casi seguenti:  
    -   Quando un database viene impostato come di sola lettura dopo l'avvio della transazione snapshot, ma prima dell'accesso al database da parte della transazione stessa.  
    -   Se si accede agli oggetti da più database, quando lo stato di un database è stato modificato in modo che il recupero del database sia avvenuto dopo l'avvio di una transazione snapshot, ma prima dell'accesso al database da parte della transazione stessa. Quando, ad esempio, il database è stato impostato su OFFLINE e quindi su ONLINE, si è chiuso automaticamente e quindi si è aperto oppure è stato scollegato e quindi collegato.  
-   Le transazioni distribuite, incluse le query in database partizionati distribuiti, non sono supportate nel livello di isolamento dello snapshot.  
-   Tramite [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non vengono mantenute più versioni dei metadati di sistema. Le istruzioni DLL (Data Definition Language) in tabelle e altri oggetti di database, ad esempio indici, viste, tipi di dati, stored procedure e funzioni CLR, comportano la modifica dei metadati. Se un oggetto viene modificato da un'istruzione DLL, eventuali riferimenti simultanei all'oggetto nel livello di isolamento dello snapshot causano un errore della transazione. Le transazioni Read Committed non hanno questo limite quando l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON.  
  
     Un amministratore del database esegue, ad esempio, l'istruzione `ALTER INDEX` seguente.  
  
    ```sql  
    USE AdventureWorks2016;  
    GO  
    ALTER INDEX AK_Employee_LoginID  
        ON HumanResources.Employee REBUILD;  
    GO  
    ```  
  
     Le transazioni snapshot attive al momento dell'esecuzione di `ALTER INDEX` ricevono un errore nel caso in cui cerchino di fare riferimento alla tabella `HumanResources.Employee` successivamente all'esecuzione dell'istruzione `ALTER INDEX`. Le transazioni Read Committed che utilizzano il controllo delle versioni delle righe non sono interessate.  
  
    > [!NOTE]  
    > Le operazioni di tipo BULK INSERT possono comportare modifiche ai metadati della tabella di destinazione, ad esempio quando si disabilita la verifica dei vincoli. In questo caso, le transazioni di isolamento dello snapshot simultanee che accedono a tabelle in cui è stato eseguito l'inserimento bulk non hanno esito positivo.  
  
   
## <a name="customizing-locking-and-row-versioning"></a>Personalizzazione dell'utilizzo di blocchi e del controllo delle versioni delle righe  
  
### <a name="customizing-the-lock-time-out"></a>Personalizzazione del timeout del blocco  
 Se un'istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] non può concedere un blocco a una transazione perché un'altra transazione è già proprietaria di un blocco in conflitto nella risorsa, la prima transazione viene bloccata in attesa del rilascio del blocco esistente. Per impostazione predefinita, non è previsto alcun periodo di timeout obbligatorio e non è disponibile alcun metodo per verificare se, prima dell'applicazione di un blocco, una risorsa è bloccata. L'unica situazione rilevabile è il tentativo di accesso ai dati con il potenziale rischio di attivazione di un blocco per un tempo indeterminato.  
  
> [!NOTE]  
> In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usare la vista a gestione dinamica **sys.dm_os_waiting_tasks** per determinare se un processo è bloccato e qual è l'origine del blocco. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usare la stored procedure di sistema **sp_who**.  
  
 L'impostazione `LOCK_TIMEOUT` consente a un'applicazione di definire il periodo di tempo massimo durante il quale un'istruzione rimane in attesa di una risorsa bloccata. Quando il periodo di attesa di un'istruzione supera il valore massimo impostato nell'opzione LOCK_TIMEOUT, l'istruzione bloccata viene annullata automaticamente e nell'applicazione viene restituito il messaggio di errore 1222 (`Lock request time-out period exceeded`). Qualsiasi transazione contenente l'istruzione, tuttavia, non viene sottoposta a rollback o annullata tramite [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pertanto, l'applicazione deve avere un gestore degli errori in grado di intercettare il messaggio di errore 1222. Se questo errore non viene intercettato, l'applicazione continua a essere eseguita come se l'istruzione della transazione non fosse stata annullata. In questo caso possono verificarsi errori, in quanto le istruzioni successive della transazione potrebbero dipendere dall'istruzione che non è mai stata eseguita.  
  
 Mediante l'implementazione di un gestore degli errori in grado di intercettare il messaggio di errore 1222, è possibile gestire la condizione di timeout ed eseguire gli interventi di correzione necessari, ad esempio la riesecuzione automatica dell'istruzione bloccata oppure il rollback dell'intera transazione.  
  
 Per determinare l'impostazione `LOCK_TIMEOUT` corrente, eseguire la funzione `@@LOCK_TIMEOUT`:  
  
```sql  
SELECT @@lock_timeout;  
GO  
```  
  
### <a name="customizing-transaction-isolation-level"></a>Personalizzazione del livello di isolamento delle transazioni  
 READ COMMITTED è il livello di isolamento predefinito per [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Se per un'applicazione è necessario impostare un livello di isolamento diverso, è possibile utilizzare i metodi seguenti:  
  
-   Eseguire l'istruzione [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
-   Le applicazioni ADO.NET che usano lo spazio dei nomi gestito System.Data.SqlClient possono specificare un'opzione *IsolationLevel* usando il metodo SqlConnection.BeginTransaction.  
-   Le applicazioni che utilizzano ADO possono impostare la proprietà `Autocommit Isolation Levels`.  
-   Quando si avvia una transazione, le applicazioni che usano OLE DB possono chiamare ITransactionLocal::StartTransaction con *isoLevel* impostato sul livello di isolamento delle transazioni desiderato. Quando si specifica il livello di isolamento in modalità autocommit, le applicazioni che utilizzano OLE DB possono impostare la proprietà DBPROPSET_SESSION DBPROP_SESSION_AUTOCOMMITISOLEVELS sul livello di isolamento delle transazioni desiderato.  
-   Le applicazioni che usano ODBC possono impostare l'attributo SQL_COPT_SS_TXN_ISOLATION usando SQLSetConnectAttr.  
  
Quando si specifica il livello di isolamento, il comportamento del blocco per tutte le query e le istruzioni DLM nella sessione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dipende dal livello di isolamento stesso. Il livello di isolamento viene mantenuto come valido fino al termine della sessione o all'impostazione di un livello di isolamento diverso.  
  
Nell'esempio seguente viene impostato il livello di isolamento di `SERIALIZABLE`:  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
SELECT BusinessEntityID  
    FROM HumanResources.Employee;  
GO  
```  
  
Se necessario, il livello di isolamento impostato può essere ignorato per singole query o istruzioni DML specificando hint a livello di tabella, che non hanno alcun effetto sulle altre istruzioni della sessione. È consigliabile utilizzare gli hint a livello di tabella per modificare il funzionamento predefinito del blocco solo se assolutamente necessario.  
  
Potrebbe essere necessario per [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] acquisire blocchi durante la lettura dei metadati anche se il livello di isolamento è impostato in modo da non richiedere blocchi di condivisione durante la lettura dei dati. Una transazione in esecuzione con il livello di isolamento Read uncommitted, ad esempio, non acquisisce blocchi di condivisione durante la lettura dei dati, ma potrebbe talvolta richiedere blocchi durante la lettura di una vista del catalogo di sistema. È pertanto possibile che una transazione Read uncommitted provochi un blocco durante l'esecuzione di una query su una tabella quando una transazione simultanea modifica i metadati della tabella.  
  
Per determinare il livello di isolamento attualmente impostato, utilizzare l'istruzione `DBCC USEROPTIONS` come illustrato nell'esempio seguente. Il set di risultati può variare rispetto a quello del sistema.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
DBCC USEROPTIONS;  
GO  
```  
  
[!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```
Set Option                   Value  
---------------------------- -------------------------------------------  
textsize                     2147483647  
language                     us_english  
dateformat                   mdy  
datefirst                    7  
...                          ...  
Isolation level              repeatable read  
 
(14 row(s) affected)   
 
DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```  
  
### <a name="locking-hints"></a>Hint di blocco  
 È possibile specificare hint di blocco per riferimenti a una singola tabella nelle istruzioni SELECT, INSERT, UPDATE e DELETE. Gli hint specificano il tipo di blocco o di controllo delle versioni delle righe utilizzato dall'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per i dati della tabella. Gli hint di blocco a livello di tabella possono essere utilizzati quando è necessario un controllo più fine dei tipi di blocco acquisiti su un oggetto. Questi hint sono prioritari rispetto al livello di isolamento della transazione impostato per la sessione.  
  
 Per altre informazioni sugli hint di blocco e il relativo comportamento, vedere [Hint di tabella &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
> [!NOTE]  
> Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sceglie quasi sempre il livello di blocco appropriato. È consigliabile utilizzare gli hint di blocco a livello di tabella per modificare il comportamento predefinito del blocco solo se necessario. La disattivazione di un livello di blocco può influire negativamente sulla concorrenza.  
  
 È possibile che il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] debba acquisire blocchi durante la lettura di metadati, anche quando esegue l'elaborazione di un'istruzione SELECT con un hint di blocco che impedisce le richieste di blocchi di condivisione in fase di lettura dei dati. Ad esempio, un'istruzione `SELECT` che usa l'hint `NOLOCK` non acquisisce blocchi di condivisione durante la lettura dei dati, ma può richiedere blocchi durante la lettura di una vista del catalogo di sistema. Ciò significa che è possibile che un'istruzione `SELECT` che usa `NOLOCK` venga bloccata.  
  
 Come illustrato nell'esempio seguente, se il livello di isolamento della transazione è impostato su `SERIALIZABLE` e l'hint di blocco a livello di tabella `NOLOCK` viene utilizzato con l'istruzione `SELECT`, i blocchi intervalli di chiavi in genere utilizzati per mantenere le transazioni serializzabili non vengono accettati.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT JobTitle  
    FROM HumanResources.Employee WITH (NOLOCK);  
GO  
  
-- Get information about the locks held by   
-- the transaction.  
SELECT    
        resource_type,   
        resource_subtype,   
        request_mode  
    FROM sys.dm_tran_locks  
    WHERE request_session_id = @@spid;  
  
-- End the transaction.  
ROLLBACK;  
GO  
```  
  
 L'unico blocco accettato che fa riferimento a `HumanResources.Employee` è un blocco di stabilità dello schema (Sch-S). In questo caso la serializzabilità non è garantita.  
  
 In [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] l'opzione `LOCK_ESCALATION` di `ALTER TABLE` può non accettare blocchi di tabella e attivare i blocchi HoBT sulle tabelle partizionate. Questa opzione non è un hint di blocco, ma può essere usata per ridurre l'escalation blocchi. Per altre informazioni, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="customizing-locking-for-an-index"></a><a name="Customize"></a> Personalizzazione dei blocchi per un indice  
 La strategia di blocco dinamico utilizzata dal [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] prevede la selezione automatica del livello di granularità dei blocchi ottimale per le query nella maggior parte dei casi. Si consiglia di non ignorare i livelli di blocco predefiniti, in cui è attivato il blocco a livello di pagina e di riga, a meno che i modelli di accesso a tabelle e indici non siano chiari e coerenti ed esista un problema di contesa tra risorse da risolvere. Se un livello di blocco viene ignorato, è possibile che l'accesso simultaneo a una tabella o a un indice venga ostacolato. Se ad esempio si specificano solo blocchi a livello di tabella in una tabella di notevoli dimensioni usata molto frequentemente, è possibile che si verifichino colli di bottiglia perché gli utenti devono attendere il rilascio del blocco a livello di tabella prima di accedere alla tabella.  
  
 In alcuni casi, se i modelli di accesso sono chiari e coerenti, la disattivazione del blocco a livello di pagina o di riga può risultare vantaggioso. Si supponga, ad esempio, che un'applicazione di database utilizzi una tabella di ricerca che viene aggiornata con frequenza settimanale in un processo batch. L'accesso alla tabella da parte di utenti simultanei avviene con un blocco condiviso, mentre quello dell'aggiornamento batch settimanale con un blocco esclusivo (X). Se il blocco a livello di pagina e di riga viene disattivato sulla tabella, l'overhead dei blocchi risulta ridotto nell'intera settimana, consentendo agli utenti di accedere simultaneamente alla tabella tramite blocchi a livello di tabella condivisi. Quando viene eseguito, il processo batch può essere completato in modo efficiente, in quanto ottiene un blocco esclusivo sulla tabella.  
  
 La disattivazione del blocco a livello di pagina o di riga può essere o meno accettabile perché l'aggiornamento batch settimanale impedirà agli utenti di accedere simultaneamente alla tabella durante l'esecuzione dell'aggiornamento. Se il processo batch modifica solo alcune righe o pagine, è possibile modificare il livello di blocco per consentire il blocco a livello di riga o di pagina, consentendo in questo modo la lettura della tabella da parte di altre sessioni senza blocchi. Se il processo batch include un numero elevato di aggiornamenti, per assicurarsi che venga completato in modo efficiente è consigliabile ottenere un blocco esclusivo sulla tabella.  
  
 A volte, quando due operazioni simultanee acquisiscono blocchi a livello di riga sulla stessa tabella e quindi si bloccano perché richiedono entrambe il blocco della pagina, si verifica un deadlock. La disattivazione dei blocchi a livello di riga impone a una delle operazioni di attendere, evitando il deadlock.  
  
 La granularità dei blocchi usata in un indice può essere impostata usando le istruzioni `CREATE INDEX` e `ALTER INDEX`. Le impostazioni del blocco si applicano sia alle pagine di indice che alle pagine di tabella. Inoltre, le istruzioni `CREATE TABLE` e `ALTER TABLE` possono essere usate per impostare la granularità dei blocchi nei vincoli `PRIMARY KEY` e `UNIQUE`. Per garantire la compatibilità con le versioni precedenti, la stored procedure di sistema `sp_indexoption` è anch'essa in grado di impostare la granularità. Per visualizzare l'opzione di blocco corrente per un determinato indice, usare la funzione `INDEXPROPERTY`. È possibile impedire l'applicazione di blocchi a livello di pagina o di riga e della combinazione di questi due livelli di blocco per un particolare indice.  
  
|Blocchi non consentiti|Accesso all'indice da parte di|  
|----------------------|-----------------------|  
|A livello di pagina|Blocchi a livello di riga e tabella|  
|A livello di riga|Blocchi a livello di pagina e di tabella|  
|A livello di pagina e di riga|Blocchi a livello di tabella|  
  
##  <a name="advanced-transaction-information"></a><a name="Advanced"></a> Informazioni avanzate sulle transazioni  
  
### <a name="nesting-transactions"></a>Nidificazione delle transazioni  
 Le transazioni esplicite possono essere nidificate. Ciò consente principalmente il supporto delle transazioni all'interno di stored procedure che è possibile richiamare sia da un processo già incluso in una transazione che da processi privi di transazioni attive.  
  
 Nell'esempio seguente viene illustrato l'utilizzo delle transazioni nidificate. La procedura *TransProc* applica la rispettiva transazione indipendentemente dalla modalità impostata in ogni processo da cui viene eseguita. Se la procedura *TransProc* viene richiamata quando una transazione è attiva, la transazione nidificata in *TransProc* viene ignorata e, a seconda dell'azione finale eseguita per la transazione esterna, viene eseguito il commit o il rollback delle relative istruzioni `INSERT`. Se la procedura `TransProc` viene eseguita da un processo privo di transazione attiva, l'istruzione `COMMIT TRANSACTION` alla fine della procedura esegue correttamente il commit delle istruzioni `INSERT`.  
  
```sql  
SET QUOTED_IDENTIFIER OFF;  
GO  
SET NOCOUNT OFF;  
GO  
CREATE TABLE TestTrans(Cola INT PRIMARY KEY,  
               Colb CHAR(3) NOT NULL);  
GO  
CREATE PROCEDURE TransProc @PriKey INT, @CharCol CHAR(3) AS  
BEGIN TRANSACTION InProc  
INSERT INTO TestTrans VALUES (@PriKey, @CharCol)  
INSERT INTO TestTrans VALUES (@PriKey + 1, @CharCol)  
COMMIT TRANSACTION InProc;  
GO  
/* Start a transaction and execute TransProc. */  
BEGIN TRANSACTION OutOfProc;  
GO  
EXEC TransProc 1, 'aaa';  
GO  
/* Roll back the outer transaction, this will  
   roll back TransProc's nested transaction. */  
ROLLBACK TRANSACTION OutOfProc;  
GO  
EXECUTE TransProc 3,'bbb';  
GO  
/* The following SELECT statement shows only rows 3 and 4 are   
   still in the table. This indicates that the commit  
   of the inner transaction from the first EXECUTE statement of  
   TransProc was overridden by the subsequent rollback. */  
SELECT * FROM TestTrans;  
GO  
```  
  
 Il commit delle transazioni interne viene ignorato da the [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. A seconda dell'operazione eseguita alla fine della transazione più esterna, viene eseguito il commit o il rollback della transazione. In caso di commit della transazione esterna, viene eseguito il commit anche delle transazioni nidificate e in caso di rollback della transazione esterna viene eseguito il rollback anche di tutte le transazioni interne, indipendentemente dal fatto che sia stato eseguito il commit delle singole transazioni interne.  
  
 Ogni chiamata a `COMMIT TRANSACTION` o `COMMIT WORK` si applica all'ultima esecuzione di `BEGIN TRANSACTION`. Se le istruzioni `BEGIN TRANSACTION` sono nidificate, l'istruzione `COMMIT` viene applicata solo all'ultima transazione nidificata, ovvero alla transazione più interna. Anche se un'istruzione *transaction_name* di `COMMIT TRANSACTION` all'interno di una transazione nidificata fa riferimento al nome della transazione esterna, il commit si applica solo alla transazione più interna.  
  
 Il parametro *transaction_name* di un'istruzione `ROLLBACK TRANSACTION` non può fare riferimento alle transazioni interne di un set di transazioni nidificate denominate. *transaction_name* può fare riferimento solo al nome della transazione più esterna. Se a qualsiasi livello di un set di transazioni nidificate viene eseguita un'istruzione ROLLBACK TRANSACTION *transaction_name* che usa il nome della transazione esterna, viene eseguito il rollback di tutte le transazioni nidificate. Se a qualsiasi livello di un set di transazioni nidificate viene eseguita un'istruzione `ROLLBACK WORK` o `ROLLBACK TRANSACTION` priva del parametro *transaction_name*, viene eseguito il rollback di tutte le transazioni nidificate, compresa la transazione più esterna.  
  
 La funzione `@@TRANCOUNT` registra il livello di nidificazione corrente delle transazioni. Ogni istruzione `BEGIN TRANSACTION` incrementa `@@TRANCOUNT` di uno. Ogni istruzione `COMMIT TRANSACTION` o `COMMIT WORK` decrementa `@@TRANCOUNT` di uno. Un'istruzione `ROLLBACK WORK` o `ROLLBACK TRANSACTION` priva del nome della transazione esegue sempre il rollback di tutte le transazioni nidificate e decrementa `@@TRANCOUNT` a 0. Un'istruzione `ROLLBACK TRANSACTION` che usa il nome di transazione della transazione più esterna in un set di transazioni nidificate esegue il rollback di tutte le transazioni nidificate e decrementa `@@TRANCOUNT` a 0. Per determinare se ci si trova all'interno di una transazione, verificare che il valore di `SELECT @@TRANCOUNT` sia 1 o un valore maggiore. Se `@@TRANCOUNT` è 0, non ci si trova all'interno di una transazione.  
  
### <a name="using-bound-sessions"></a>Utilizzo di sessioni associate  
 Le sessioni associate semplificano il coordinamento di azioni tra più sessioni nello stesso server. Tramite le sessioni associate due o più sessioni possono condividere la stessa transazione e gli stessi blocchi, utilizzando gli stessi dati senza conflitti di blocco. È possibile creare sessioni associate in base a più sessioni nella stessa applicazione oppure in base a più applicazioni con sessioni distinte.  
  
 Per partecipare a una sessione associata, una sessione chiama `sp_getbindtoken` o `srv_getbindtoken` (tramite il servizio ODS) per ottenere un token di associazione. Un token di associazione è una stringa di caratteri che identifica in modo univoco ogni transazione associata. Il token di associazione viene quindi inviato alle altre sessioni da associare alla sessione corrente. Le altre sessioni vengono associate alla transazione chiamando **sp_bindsession** e usando il token di associazione ricevuto dalla prima sessione.  
  
> [!NOTE]  
> Una sessione deve disporre di una transazione utente attiva affinché `sp_getbindtoken` o `srv_getbindtoken` abbia esito positivo.  
  
 I token di associazione devono essere trasmessi dal codice dell'applicazione che esegue la prima sessione al codice dell'applicazione che successivamente associa le sessioni alla prima sessione. Non esistono istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] o funzioni API utilizzabili in un'applicazione per ottenere il token di associazione per una transazione avviata da un altro processo. Di seguito vengono descritti alcuni dei metodi che è possibile utilizzare per la trasmissione di un token di associazione:  
  
-   Se tutte le sessioni vengono iniziate dallo stesso processo dell'applicazione, i token di associazione possono essere archiviati nella memoria globale o passati come parametri di funzioni.  
  
-   Se le connessioni vengono eseguite da processi dell'applicazione distinti, i token di associazione possono essere trasmessi utilizzando la comunicazione interprocesso (IPC), ad esempio tramite DDE (scambio dinamico dei dati) o un chiamata RPC (Remote Procedure Call).  
  
-   I token di associazione possono essere archiviati in un'istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] che può essere letta dai processi in attesa di essere associati alla prima sessione.  
  
 In un set di sessioni associate può essere attiva una sola sessione per volta. Se una sessione esegue un'istruzione nell'istanza o è in attesa di risultati dall'istanza, le altre sessioni associate possono accedere all'istanza solo dopo il completamento dell'elaborazione della sessione corrente o l'annullamento dell'istruzione corrente. Se l'istanza è occupata per l'elaborazione di un'istruzione da un'altra delle sessioni associate, viene generato un errore indicante che lo spazio della transazione è in uso e che è necessario riprovare la sessione in un secondo momento.  
  
 Quando si associano sessioni, ogni sessione mantiene il relativo livello di isolamento. L'utilizzo dell'istruzione SET TRANSACTION ISOLATION LEVEL per modificare il livello di isolamento di una sessione non influisce sull'impostazione di qualsiasi altra sessione associata.  
  
#### <a name="types-of-bound-sessions"></a>Tipi di sessioni associate  
 Sono disponibili due tipi di sessioni associate, ovvero locali e distribuite.  
  
-   **Sessione associata locale**  
    Consente alle sessioni associate di condividere lo spazio di una singola transazione in una sola istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
-   **Sessione associata distribuita**  
    Consente alle sessioni associate di condividere lo stesso spazio della transazione in due o più istanze fino a quando non viene eseguito il commit o il rollback dell'intera transazione tramite [!INCLUDE[msCoName](../includes/msconame-md.md)] Microsoft Distributed Transaction Coordinator (servizio MSDTC).  
  
 Le sessioni associate distribuite non sono identificate da un token di associazione di tipo stringa, ma dai numeri di identificazione delle transazioni distribuite. Se una sessione associata partecipa a una transazione locale ed esegue una chiamata RPC in un server remoto con l'opzione `SET REMOTE_PROC_TRANSACTIONS ON`, la transazione associata locale viene automaticamente promossa a transazione associata distribuita da MS DTC e viene avviata una sessione di MS DTC.  
  
#### <a name="when-to-use-bound-sessions"></a>Uso delle sessioni associate  
 Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] le sessioni associate vengono prevalentemente utilizzate nello sviluppo di stored procedure estese che devono eseguire istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] per conto del processo che le chiama. Quando tramite il processo chiamante il token di associazione viene passato come parametro della stored procedure estesa, la procedura può partecipare allo spazio della transazione del processo chiamante e viene pertanto integrata in tale processo.  
  
 In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] le stored procedure scritte utilizzando CLR sono più protette, scalabili e stabili rispetto alle stored procedure estese. Le stored procedure CLR usano l'oggetto **SqlContext**, anziché `sp_bindsession`, per unire il contesto della sessione chiamante.  
  
 Le sessioni associate possono essere utilizzate per lo sviluppo di applicazioni a tre livelli in cui la logica di business è incorporata in programmi distinti che operano congiuntamente in una singola transazione aziendale. Tali programmi devono essere sviluppati in modo da coordinarne accuratamente l'accesso a un database. Poiché le due sessioni condividono gli stessi blocchi, i due programmi non devono tentare di modificare gli stessi dati simultaneamente. In qualsiasi momento può essere in funzione una sola sessione come parte della transazione. L'esecuzione parallela non è possibile. La transazione può essere passata da una sessione all'altra solo in specifici punti definiti, ad esempio dopo il completamento di tutte le istruzioni DML e il recupero dei relativi risultati.  
  
### <a name="coding-efficient-transactions"></a>Codifica di transazioni efficienti  
 È importante mantenere le transazioni il più brevi possibile. Quando viene avviata una transazione, un sistema di gestione di database (DBMS, Database Management System) deve tenere occupate molte risorse fino alla fine della transazione in modo da proteggerne le proprietà di atomicità, consistenza, isolamento e durevolezza (ACID, Atomicity, Consistency, Isolation and Durability). Se i dati vengono modificati, le righe modificate devono essere protette con blocchi esclusivi che ne impediscono la lettura da parte di altre transazioni. Tali blocchi devono essere mantenuti attivi fino al commit o al rollback della transazione. In base alle impostazioni del livello di isolamento della transazione, le istruzioni `SELECT` possono acquisire blocchi che è necessario mantenere attivi fino a quando non è stato eseguito il commit o il rollback della transazione. Nei sistemi con numerosi utenti è necessario mantenere le transazioni più brevi possibile per ridurre la possibilità che si verifichino contese di risorse tra connessioni simultanee. Le transazioni non efficienti che richiedono tempi di esecuzione prolungati potrebbero non presentare alcun problema se il numero di utenti è ridotto, mentre sono assolutamente da evitare in sistemi con migliaia di utenti. A partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta le transazioni durevoli ritardate. Tali transazioni non garantiscono la durabilità. Per altre informazioni, vedere l'argomento [Durabilità delle transazioni](../relational-databases/logs/control-transaction-durability.md).  
  
#### <a name="coding-guidelines"></a><a name="guidelines"></a> Linee guida per la codifica  
 Di seguito vengono riportate alcune linee guida per la codifica di transazioni efficienti.  
  
-   Evitare di richiedere l'input dell'utente durante una transazione.  
    Richiedere agli utenti l'input necessario prima di avviare una transazione. Se durante una transazione è necessario input dell'utente aggiuntivo, è consigliabile eseguire il rollback della transazione corrente e riavviarla dopo avere ottenuto l'input desiderato. Anche se gli utenti rispondono immediatamente, i tempi di reazione umani sono in larga misura inferiori alla velocità del computer. Tutte le risorse utilizzate dalla transazione vengono tenute occupate per periodi di tempo estremamente prolungati e possono pertanto causare potenziali problemi di blocco. Se gli utenti non rispondono, la transazione rimane attiva e continua a bloccare risorse critiche fino a quando non riceve una risposta, ovvero anche dopo diversi minuti o addirittura diverse ore.  
  
-   Se possibile, evitare di aprire una transazione durante l'analisi dei dati.  
    Le transazioni devono essere avviate solo dopo il completamento dell'analisi preliminare dei dati.  
  
-   Mantenere la transazione il più breve possibile.  
    Dopo avere determinato quali modifiche è necessario eseguire, avviare la transazione, eseguire le istruzioni di modifica e quindi eseguire immediatamente il commit o il rollback. Evitare di aprire la transazione prima che sia necessario.  
  
-   Per limitare i blocchi, è consigliabile utilizzare per le query di sola lettura un livello di isolamento basato sul controllo delle versioni delle righe.  
  
-   Utilizzare in modo intelligente i livelli bassi di isolamento delle transazioni.  
    Molte applicazioni possono essere codificate rapidamente in modo da utilizzare il livello di isolamento delle transazioni Read Committed. Non tutte le transazioni richiedono il livello di isolamento Serializable.  
  
-   Utilizzare in modo intelligente le opzioni di concorrenza dei cursori di livello più basso, ad esempio le opzioni di concorrenza ottimistica.  
    In un sistema in cui è improbabile che vengano eseguiti aggiornamenti simultanei, l'overhead associato all'errore occasionale causato dalla modifica dei dati da parte di altri utenti successivamente alla lettura dei dati stessi può essere molto più ridotto dell'overhead associato al blocco di ogni riga letta.  
  
-   Durante una transazione, accedere alla quantità minima di dati possibile.  
    Ciò consente ridurre il numero di righe bloccate e pertanto anche la contesa tra le transazioni.  
    
-   Evitare gli hint di blocco pessimistici, come HOLDLOCK, quando possibile. 
    Gli hint come il livello di isolamento HOLDLOCK o SERIALIZABLE possono causare l'attesa dei processi anche per i blocchi condivisi e riducono la concorrenza

-   Evitare di usare transazioni implicite quando le possibili transazioni implicite possono introdurre comportamenti imprevedibili a causa della loro natura. Vedere [Transazioni implicite e i problemi di concorrenza](#implicit-transactions-and-avoiding-concurrency-and-resource-problems)

-   Progettare gli indici con un [fattore di riempimento](indexes/specify-fill-factor-for-an-index.md) ridotto. La riduzione del fattore di riempimento può contribuire a impedire o ridurre la frammentazione delle pagine di indice, riducendo così i tempi di ricerca negli indici, soprattutto per il recupero da disco. Per visualizzare informazioni sulla frammentazione dei dati e degli indici per una tabella o vista, è possibile usare sys.dm_db_index_physical_stats. 
  
#### <a name="implicit-transactions-and-avoiding-concurrency-and-resource-problems"></a>Transazioni implicite ed evitare problemi di concorrenza e risorse  
 Per evitare problemi di concorrenza e delle risorse, è necessario gestire le transazioni implicite con la massima attenzione. Quando si usano transazioni implicite, l'istruzione [!INCLUDE[tsql](../includes/tsql-md.md)] che segue l'istruzione `COMMIT` o `ROLLBACK` avvia automaticamente una nuova transazione. Ciò potrebbe comportare l'apertura di una nuova transazione mentre nell'applicazione è in corso l'esame dei dati o addirittura quando viene richiesto l'input dell'utente. Dopo che è stata completata l'ultima transazione necessaria per la protezione delle modifiche ai dati, disabilitare le transazioni implicite fino a quando non è nuovamente necessaria una transazione per la protezione delle modifiche. In questo modo, quando nell'applicazione è in corso l'esame dei dati e viene richiesto l'input dell'utente, in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene utilizzata la modalità autocommit.  
  
 Inoltre, se è abilitato il livello di isolamento dello snapshot, anche se una nuova transazione non manterrà attivi i blocchi, una transazione con esecuzione prolungata consentirà di evitare la rimozione delle versioni precedenti da `tempdb`.  
  
### <a name="managing-long-running-transactions"></a>Gestione di transazioni con esecuzione prolungata  
 Una *transazione con esecuzione prolungata* è una transazione attiva su cui non è stato eseguito tempestivamente il commit o il rollback. Se, ad esempio, l'inizio e la fine di una transazione sono controllati dall'utente, una causa tipica di una transazione con esecuzione prolungata è l'avvio di una transazione da parte di un utente senza la successiva immissione di una risposta.  
  
 Una transazione con esecuzione prolungata può causare seri problemi a un database, come indicato di seguito:  
  
-   Se l'istanza di un server viene chiusa dopo che una transazione attiva ha eseguito diverse modifiche non sottoposte a commit, la fase di recupero del successivo riavvio può impiegare molto più tempo di quanto specificato dall'opzione di configurazione del server **Intervallo di recupero** o dall'opzione `ALTER DATABASE ... SET TARGET_RECOVERY_TIME`. Queste opzioni controllano la frequenza di checkpoint attivi e indiretti, rispettivamente. Per altre informazioni sui tipi di checkpoint, vedere [Checkpoint di database &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md).  
  
-   Inoltre, sebbene una transazione in attesa di una risposta possa produrre un aumento minimo del log, posticipa il troncamento del log in modo indefinito con una conseguente crescita delle relative dimensioni con rischio di riempimento. Se log delle transazioni si riempie, il database non può essere più aggiornato. Per altre informazioni, vedere [Architettura e gestione del log delle transazioni di SQL Server](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md), [Risolvere i problemi relativi a un log delle transazioni completo &#40;Errore di SQL Server 9002&#41;](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md), e [Log delle transazioni &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md).  
  
#### <a name="discovering-long-running-transactions"></a>Individuare le transazioni a esecuzione prolungata  
 Per individuare le transazioni con esecuzione prolungata, utilizzare una delle alternative seguenti:  
  
-   **sys.dm_tran_database_transactions**  
  
    Questa vista a gestione dinamica restituisce informazioni sulle transazioni a livello di database. Per una transazione a esecuzione prolungata, le colonne particolarmente rilevanti sono quelle che indicano l'ora del primo record di log (**database_transaction_begin_time**), lo stato corrente della transazione (**database_transaction_state**) e il numero di sequenza del file di log (LSN) del record iniziale nel log delle transazioni (**database_transaction_begin_lsn**).  
  
    Per altre informazioni, vedere [sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md).  
  
-   `DBCC OPENTRAN`  
  
    Questa istruzione consente di identificare l'ID utente del proprietario della transazione, in modo da risalire, se lo si desidera, all'origine della transazione per terminarla in modo più appropriato, ovvero tramite il commit invece del rollback. Per altre informazioni, vedere [DBCC OPENTRAN &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-opentran-transact-sql.md).  
  
#### <a name="stopping-a-transaction"></a>Arresto di una transazione  
 Potrebbe essere necessario utilizzare l'istruzione KILL. Eseguire l'istruzione con cautela, soprattutto quando sono in esecuzione processi importanti. Per altre informazioni, vedere [KILL &#40;Transact-SQL&#41;](../t-sql/language-elements/kill-transact-sql.md).  
  
##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Ulteriori informazioni   
[Overhead del controllo delle versioni delle righe](/archive/blogs/sqlserverstorageengine/overhead-of-row-versioning)   
[Eventi estesi](../relational-databases/extended-events/extended-events.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)     
[Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)      
[Funzioni e viste a gestione dinamica relative alle transazioni &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)