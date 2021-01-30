---
description: SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
title: SET TRANSACTION ISOLATION LEVEL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LEVEL
- LEVEL_TSQL
- SET TRANSACTION ISOLATION LEVEL
- ISOLATION
- ISOLATION_TSQL
- SET_TRANSACTION_ISOLATION_LEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET TRANSACTION ISOLATION LEVEL statement
- row versioning [SQL Server], isolation levels
- TRANSACTION ISOLATION LEVEL option
- isolation levels [SQL Server], setting
- locking [SQL Server], isolation levels
- transactions [SQL Server], isolation levels
ms.assetid: 016fb05e-a702-484b-bd2a-a6eabd0d76fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 888458c724bcf9bb9e880472ca45ce6dae311f04
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076143"
---
# <a name="set-transaction-isolation-level-transact-sql"></a>SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Controlla il funzionamento relativo ai blocchi e al controllo delle versioni delle righe per le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite tramite una connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET TRANSACTION ISOLATION LEVEL
    { READ UNCOMMITTED
    | READ COMMITTED
    | REPEATABLE READ
    | SNAPSHOT
    | SERIALIZABLE
    }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse
  
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```

>[!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] implementa le transazioni ACID. Per impostazione predefinita, il livello di isolamento del supporto transazionale è READ UNCOMMITTED.  È possibile modificarlo in READ COMMITTED SNAPSHOT ISOLATION impostando su ON l'opzione di database READ_COMMITTED_SNAPSHOT per un database utente quando si è connessi al database master.  Dopo l'abilitazione, tutte le transazioni in questo database vengono eseguite con READ COMMITTED SNAPSHOT ISOLATION e l'impostazione READ UNCOMMITTED a livello della sessione non verrà rispettata. Per informazioni dettagliate, vedere [Opzioni di ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 READ UNCOMMITTED  
 Specifica che le istruzioni possono leggere le righe modificate da altre transazioni ma di cui non è ancora stato eseguito il commit.  
  
 Le transazioni eseguite al livello READ UNCOMMITTED non acquisiscono blocchi condivisi per evitare che altre transazioni possano modificare i dati letti dalla transazione corrente. Le transazioni READ UNCOMMITTED, inoltre, non vengono bloccate con blocchi esclusivi che impedirebbero alla transazione corrente di leggere righe modificate da altre transazioni, ma di cui non è stato eseguito il commit. Con l'impostazione di questa opzione è possibile leggere modifiche di cui non è stato eseguito il commit, operazione definita lettura dirty. Prima della fine della transazione è quindi possibile che i dati vengano modificati e che le righe compaiano e scompaiano nel set di dati. Questa opzione equivale all'impostazione NOLOCK per tutte le tabelle in tutte le istruzioni SELECT di una transazione. Tra i livelli di isolamento disponibili, questo è il meno restrittivo.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è inoltre possibile utilizzare le impostazioni seguenti per ridurre al minimo la contesa dei blocchi pur proteggendo le transazioni da letture dirty di modifiche dei dati di cui non è stato eseguito il commit:  
  
-   Livello di isolamento READ COMMITTED con l'opzione di database READ_COMMITTED_SNAPSHOT impostata su ON.  
  
-   Livello di isolamento SNAPSHOT. Per altre informazioni sull'isolamento dello snapshot, vedere [Isolamento dello Snapshot in SQL Server](/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server). 
  
 READ COMMITTED  
 Specifica che le istruzioni non possono leggere i dati modificati da altre transazioni ma di cui non è stato eseguito il commit. In questo modo vengono evitate letture dirty. Altre transazioni possono modificare i dati nell'intervallo tra le singole istruzioni della transazione corrente, con conseguenze come letture irripetibili e la presenza di dati fantasma. Questa è l'opzione predefinita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Il funzionamento di READ COMMITTED varia a seconda dell'impostazione dell'opzione di database READ_COMMITTED_SNAPSHOT:  
  
-   Se l'opzione READ_COMMITTED_SNAPSHOT è impostata su OFF (impostazione predefinita in SQL Server), [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa blocchi condivisi per impedire che altre transazioni possano modificare le righe mentre la transazione corrente sta eseguendo un'operazione di lettura. I blocchi condivisi impediscono inoltre che l'istruzione possa leggere righe modificate da altre transazioni, fino al completamento di tali transazioni. Il tipo di blocco condiviso determina il momento in cui verrà rilasciato. I blocchi a livello di riga vengono rilasciati prima dell'elaborazione della riga successiva. I blocchi a livello di pagina vengono rilasciati nel momento in cui la pagina successiva viene letta, mentre i blocchi di tabella vengono rilasciati al termine dell'istruzione.  
  
-   Se READ_COMMITTED_SNAPSHOT è impostata su ON (impostazione predefinita nel database SQL di Azure), [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa il controllo delle versioni delle righe per presentare a ogni istruzione uno snapshot dei dati coerente dal punto di vista transazionale, rappresentativo dei dati esistenti al momento dell'avvio dell'istruzione. Non vengono utilizzati blocchi per proteggere i dati da aggiornamenti eseguiti da altre transazioni.

> [!IMPORTANT]  
> La scelta di un livello di isolamento delle transazioni non ha effetto sui blocchi acquisiti per proteggere le modifiche dei dati. Una transazione ottiene sempre un blocco esclusivo su qualsiasi dato da essa modificato, che mantiene fino al suo completamento, indipendentemente dal livello di isolamento impostato per la transazione. Inoltre, un aggiornamento effettuato a livello di isolamento READ_COMMITTED usa blocchi di aggiornamento sulle righe di dati selezionate, mentre un aggiornamento effettuato a livello di isolamento SNAPSHOT usa versioni di riga per selezionare le righe da aggiornare. Per le operazioni di lettura, i livelli di isolamento delle transazioni definiscono essenzialmente il livello di protezione dagli effetti delle modifiche apportate da altre transazioni. Per altre informazioni vedere [Guida per il controllo delle versioni delle righe e il blocco della transazione](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md).

> [!NOTE]  
>  L'isolamento dello snapshot supporta dati FILESTREAM. In modalità di isolamento dello snapshot, i dati letti da qualsiasi istruzione in una transazione rappresenteranno la versione consistente dal punto di vista transazionale dei dati presenti all'avvio della transazione.  
  
 Quando l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON, è possibile utilizzare l'hint di tabella READCOMMITTEDLOCK per richiedere il blocco condiviso anziché il controllo delle versioni delle righe per singole istruzioni delle transazioni eseguite con il livello di isolamento READ COMMITTED.  
  
> [!NOTE]  
>  Quando si imposta l'opzione READ_COMMITTED_SNAPSHOT, nel database è consentita solo la connessione che esegue il comando ALTER DATABASE. Nel database non devono essere presenti altre connessioni aperte fino al completamento del comando ALTER DATABASE. Il database non deve essere in modalità utente singolo.  
  
 REPEATABLE READ  
 Specifica che le istruzioni non possono leggere dati modificati da altre transazioni di cui non è ancora stato eseguito il commit e che nessun'altra transazione può modificare i dati letti dalla transazione corrente, fino al completamento della transazione corrente.  
  
 Vengono acquisiti blocchi condivisi per tutti i dati letti da ogni istruzione della transazione e tali blocchi vengono mantenuti attivi fino al completamento della transazione. Ciò impedisce ad altre transazioni di modificare qualsiasi riga letta dalla transazione corrente. Altre transazioni possono inserire nuove righe, se tali righe corrispondono alle condizioni di ricerca delle istruzioni eseguite dalla transazione corrente. Se la transazione corrente ripete l'istruzione in seguito, verranno recuperate le nuove righe con conseguenti letture fantasma. Poiché i blocchi condivisi vengono mantenuti attivi fino alla fine di una transazione, anziché essere rilasciati alla fine di ogni istruzione, il livello di concorrenza è inferiore rispetto a quello consentito dal livello di isolamento predefinito READ COMMITTED. Utilizzare questa opzione solo se necessario.  
  
 SNAPSHOT  
 Specifica che i dati letti da qualsiasi istruzione in una transazione rappresenteranno la versione coerente dal punto di vista transazionale dei dati esistenti al momento dell'avvio della transazione. La transazione può quindi accedere solo alle modifiche dei dati di cui è stato eseguito il commit prima dell'avvio della transazione. Le modifiche ai dati apportate da altre transazioni dopo l'avvio della transazione corrente non sono visibili per le istruzioni eseguite nella transazione corrente. È come se le istruzioni di una transazione ottenessero uno snapshot dei dati di cui è stato eseguito il commit così come si presentavano al momento dell'avvio della transazione.  
  
 Con l'eccezione delle operazioni di recupero di un database, le transazioni SNAPSHOT non richiedono blocchi per la lettura dei dati. Le transazioni SNAPSHOT che eseguono letture di dati non impediscono la scrittura di dati da altre transazioni. Viceversa, le transazioni che eseguono scritture di dati non impediscono la lettura di dati da transazioni SNAPSHOT.  
  
 Durante la fase di rollback di un'operazione di recupero di un database, le transazioni SNAPSHOT richiederanno un blocco se viene eseguito un tentativo di lettura di dati bloccati da un'altra transazione per cui è in corso il rollback. In questo caso, la transazione SNAPSHOT viene bloccata fino al completamento del rollback dell'altra transazione. Il blocco viene rilasciato immediatamente dopo essere stato concesso.  
  
 È necessario impostare l'opzione di database ALLOW_SNAPSHOT_ISOLATION su ON prima di avviare una transazione che utilizza il livello di isolamento SNAPSHOT. Se una transazione che utilizza il livello di isolamento SNAPSHOT accede a dati in più database, l'opzione ALLOW_SNAPSHOT_ISOLATION deve essere impostata su ON in ogni database.  
  
 Non è possibile impostare il livello di isolamento SNAPSHOT per una transazione avviata con un altro livello di isolamento. In questo caso la transazione verrà interrotta. Se la transazione viene avviata con il livello di isolamento SNAPSHOT, è invece possibile cambiare il livello di isolamento e quindi reimpostare il livello SNAPSHOT. Una transazione viene avviata la prima volta che accede a dati.  
  
 Una transazione eseguita con il livello di isolamento SNAPSHOT può visualizzare le modifiche apportate dalla transazione stessa. Ad esempio, se la transazione esegue un'operazione UPDATE su una tabella e quindi esegue un'istruzione SELECT sulla stessa tabella, i dati modificati verranno inclusi nel set dei risultati.  
  
> [!NOTE]  
>  In modalità di isolamento dello snapshot, i dati letti da qualsiasi istruzione in una transazione rappresenteranno la versione consistente dal punto di vista transazionale dei dati presenti all'avvio della transazione, non all'avvio dell'istruzione.  
  
 SERIALIZABLE  
 Specifica quanto segue:  
  
-   Le istruzioni non possono leggere dati modificati da altre transazioni ma di cui non è ancora stato eseguito il commit.  
  
-   Nessun'altra transazione può modificare i dati letti dalla transazione corrente fino al completamento della transazione corrente.  
  
-   Nessun'altra transazione può inserire nuove righe con valori di chiave che rientrerebbero nell'intervallo di chiavi lette da qualsiasi istruzione nella transazione corrente, fino al completamento della transazione corrente.  
  
 Vengono acquisiti blocchi di intervalli di chiavi per l'intervallo dei valori di chiave corrispondenti alle condizioni di ricerca di ogni istruzione eseguita in una transazione. In questo modo si impedisce che altre transazioni possano aggiornare o inserire righe qualificate per l'esecuzione di qualsiasi istruzione della transazione corrente. Ciò significa che in caso di ripetizione di una delle istruzioni di una transazione, la lettura restituirà lo stesso set di righe. I blocchi di intervalli di chiavi vengono mantenuti attivi fino al completamento della transazione. Questo è il livello di isolamento più restrittivo tra quelli disponibili, perché blocca interi intervalli di chiavi e mantiene attivi tali blocchi fino al completamento della transazione. Poiché la concorrenza è inferiore, utilizzare questa opzione solo quando è strettamente necessario. Questa opzione equivale all'impostazione di HOLDLOCK per tutte le tabelle in tutte le istruzioni SELECT di una transazione.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile impostare una sola opzione di livello di isolamento alla volta. Tale impostazione rimane valida per la connessione fino a quando non viene modificata in modo esplicito. Tutte le operazioni di lettura eseguite nell'ambito della transazione rispettano le regole del livello di isolamento specificato a meno che un hint di tabella nella clausola FROM di un'istruzione non specifichi un funzionamento di blocco o di controllo delle versioni diverso per una tabella.  
  
 I livelli di isolamento delle transazioni definiscono i tipi di blocchi acquisiti per le operazioni di lettura. I blocchi condivisi acquisiti per il livello READ COMMITTED o REPEATABLE READ sono in genere blocchi di riga, anche se è possibile che venga eseguita l'escalation a blocchi di pagina o di tabella, se la lettura fa riferimento a un numero significativo di righe in una pagina o una tabella. Se la transazione modifica una riga dopo la lettura, la transazione acquisisce un blocco esclusivo per proteggere tale riga e il blocco viene mantenuto attivo fino al completamento della transazione. Ad esempio, se una transazione REPEATABLE READ acquisisce un blocco condiviso su una riga e quindi modifica tale riga, il blocco di riga condiviso viene convertito in un blocco di riga esclusivo.  
  
 Con una sola eccezione, è possibile passare da un livello di isolamento a un altro in qualsiasi momento durante una transazione. L'eccezione si verifica quando si passa da un qualsiasi livello di isolamento all'isolamento SNAPSHOT. Questa operazione causa un errore nella transazione e il relativo rollback. È tuttavia possibile passare da una transazione avviata in isolamento SNAPSHOT a qualsiasi altro livello di isolamento.  
  
 Quando si modifica il livello di isolamento di una transazione, le risorse lette dopo la modifica saranno protette in base alle regole del nuovo livello. Le risorse lette prima della modifica continuano a essere protette in base alle regole del livello precedente. Se ad esempio una transazione viene modificata da READ COMMITTED a SERIALIZABLE, i blocchi condivisi acquisiti dopo la modifica vengono mantenuti fino alla fine della transazione.  
  
 Se si esegue l'istruzione SET TRANSACTION ISOLATION LEVEL in una stored procedure o un trigger, quando l'oggetto restituisce il controllo il livello di isolamento viene reimpostato sul livello attivo al momento della chiamata dell'oggetto. Ad esempio, se si imposta REPEATABLE READ in un batch e il batch chiama poi una stored procedure che imposta il livello di isolamento su SERIALIZABLE, il livello di isolamento verrà reimpostato su REPEATABLE READ quando la stored procedure restituisce il controllo al batch.  
  
> [!NOTE]  
>  Le funzioni definite dall'utente e i tipi CLR (Common Language Runtime) definiti dall'utente non possono eseguire SET TRANSACTION ISOLATION LEVEL. È tuttavia possibile sostituire il livello di isolamento utilizzando un hint di tabella. Per altre informazioni, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Quando si utilizza sp_bindsession per associare due sessioni, ogni sessione mantiene il livello di isolamento impostato. L'utilizzo dell'istruzione SET TRANSACTION ISOLATION LEVEL per modificare il livello di isolamento di una sessione non influisce sull'impostazione di qualsiasi altra sessione associata.  
  
 L'istruzione SET TRANSACTION ISOLATION LEVEL diventa effettiva in fase di esecuzione, non in fase di analisi.  
  
 Le operazioni ottimizzate di caricamento bulk negli heap bloccano le query eseguite con i livelli di isolamento seguenti:  
  
-   SNAPSHOT  
  
-   READ UNCOMMITTED  
  
-   READ COMMITTED con utilizzo del controllo delle versioni delle righe  
  
 Al contrario, le query eseguite con tali livelli di isolamento bloccano le operazioni ottimizzate di caricamento bulk negli heap. Per altre informazioni sulle operazioni di caricamento bulk, vedere [Importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
 I database abilitati per FILESTREAM supportano i livelli di isolamento della transazione seguenti.  
  
|Livello di isolamento|Accesso Transact-SQL|Accesso al file system|  
|---------------------|-------------------------|------------------------|  
|Read Uncommitted|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Non supportato|  
|Read Committed|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|Repeatable Read|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Non supportato|  
|Serializable|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Non supportato|  
|Snapshot Read Committed|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|Snapshot|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene impostato il livello `TRANSACTION ISOLATION LEVEL` per la sessione. Per ogni istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] successiva, tutti i blocchi condivisi verranno mantenuti attivi da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fino alla fine della transazione.  
  
```sql
USE AdventureWorks2012;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT *   
    FROM HumanResources.EmployeePayHistory;  
GO  
SELECT *   
    FROM HumanResources.Department;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [DBCC USEROPTIONS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
  
