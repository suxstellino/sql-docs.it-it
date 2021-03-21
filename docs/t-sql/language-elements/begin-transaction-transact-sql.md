---
description: BEGIN TRANSACTION (Transact-SQL)
title: BEGIN TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN_TRANSACTION_TSQL
- TRANSACTION_TSQL
- TRANSACTION
- BEGIN TRANSACTION
- BEGIN TRAN
- BEGIN_TRAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction logs [SQL Server], BEGIN TRANSACTION statement
- marked transactions [SQL Server], BEGIN TRANSACTION statement
- BEGIN TRANSACTION statement
- local transactions [SQL Server]
- marking transactions [SQL Server]
- transactions [SQL Server], starting
- transaction names [SQL Server]
- starting point marked for transactions
- starting transactions
ms.assetid: c6258df4-11f1-416a-816b-54f98c11145e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3c93301d165b8adbd00942b92c1ebdeb87701ffa
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747911"
---
# <a name="begin-transaction-transact-sql"></a>BEGIN TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contrassegna il punto di inizio di una transazione locale esplicita. Le transazioni esplicite iniziano con l'istruzione BEGIN TRANSACTION e terminano con l'istruzione COMMIT o ROLLBACK.  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
--Applies to SQL Server and Azure SQL Database
 
BEGIN { TRAN | TRANSACTION }   
    [ { transaction_name | @tran_name_variable }  
      [ WITH MARK [ 'description' ] ]  
    ]  
[ ; ]  
```  
 
```syntaxsql
--Applies to Azure Synapse Analytics and Parallel Data Warehouse
 
BEGIN { TRAN | TRANSACTION }   
[ ; ]  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *transaction_name*  
 **SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure
 
 Nome assegnato alla transazione. *transaction_name* deve essere conforme alle regole per gli identificatori, ma non sono consentiti identificatori composti da più di 32 caratteri. Nel caso di istruzioni BEGIN...COMMIT o BEGIN...ROLLBACK nidificate, utilizzare solo i nomi di transazione nella coppia esterna. In *transaction_name* viene sempre applicata la distinzione tra maiuscole e minuscole, anche quando l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non prevede distinzione tra maiuscole e minuscole.  
  
 @*tran_name_variable*  
 **SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure
 
 Nome di una variabile definita dall'utente contenente un nome di transazione valido. La variabile deve essere dichiarata con un tipo di dati **char**, **varchar**, **nchar** o **nvarchar**. Se alla variabile vengono passati più di 32 caratteri, verranno utilizzati solo i primi 32 e i restanti caratteri saranno troncati.  
  
 WITH MARK [ '*description*' ]  
**SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure

Viene specificato che la transazione è contrassegnata nel log. *description* è una stringa che descrive il contrassegno. Un argomento *description* con più di 128 caratteri viene troncato in corrispondenza di questo limite prima di essere archiviato nella tabella msdb.dbo.logmarkhistory.  
  
 Se si utilizza WITH MARK, è necessario specificare un nome di transazione. Questa opzione consente di ripristinare un log delle transazioni fino al punto, o contrassegno, specificato.  
  
## <a name="general-remarks"></a>Osservazioni generali
L'istruzione BEGIN TRANSACTION incrementa la funzione @@TRANCOUNT di una unità.
  
L'istruzione BEGIN TRANSACTION rappresenta un punto in cui i dati a cui viene fatto riferimento in una connessione sono consistenti dal punto di vista logico e fisico. Se vengono rilevati uno o più errori, è possibile eseguire il rollback di tutte le modifiche apportate ai dati dopo l'istruzione BEGIN TRANSACTION per ripristinare questo stato di consistenza noto dei dati. Una transazione risulta aperta fino a quando non viene eseguita l'istruzione COMMIT TRANSACTION per rendere permanenti le modifiche se non si è verificato alcun errore oppure fino a quando non viene eseguita l'istruzione ROLLBACK TRANSACTION per annullare tutte le modifiche se vengono rilevati errori.  
  
L'istruzione BEGIN TRANSACTION avvia una transazione locale per la connessione in cui viene eseguita. In base alle impostazioni correnti del livello di isolamento della transazione, molte risorse utilizzate per il supporto delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite dalla connessione vengono bloccate dalla transazione fino al relativo completamento con un'istruzione COMMIT TRANSACTION o ROLLBACK TRANSACTION. Le transazioni che rimangono in sospeso per un periodo di tempo prolungato possono impedire l'accesso a queste risorse bloccate da parte di altri utenti nonché il troncamento del log.  
  
 Le transazioni locali avviate tramite l'istruzione BEGIN TRANSACTION vengono registrate nel log delle transazioni solo quando l'applicazione esegue un'operazione che richiede la registrazione nel log, ad esempio un'istruzione INSERT, UPDATE o DELETE. Anche se un'applicazione esegue varie operazioni, ad esempio l'acquisizione di blocchi per proteggere il livello di isolamento della transazione di istruzioni SELECT, la registrazione nel log avviene solo quando l'applicazione esegue un'operazione di modifica.  
  
 L'assegnazione di un nome di transazione a più transazioni nidificate ha un effetto limitato sulla transazione. Nel sistema viene registrato solo il nome della prima transazione (quella più esterna). Se si tenta di eseguire il rollback fino a un nome diverso da quello di un punto di salvataggio valido, viene generato un errore. Quando si verifica l'errore, nessuna delle istruzioni eseguite prima del rollback viene annullata. Il rollback delle istruzioni viene eseguito solo in corrispondenza del rollback della transazione esterna.  
  
 La transazione locale avviata dall'istruzione BEGIN TRANSACTION viene trasformata mediante escalation in una transazione distribuita se vengono eseguite le operazioni seguenti prima del commit o del rollback dell'istruzione:  
  
-   Esecuzione di un'istruzione INSERT, DELETE o UPDATE in cui viene fatto riferimento a una tabella remota in un server collegato. L'istruzione INSERT, UPDATE o DELETE ha esito negativo se il provider OLE DB usato per l'accesso al server collegato non supporta l'interfaccia ITransactionJoin.  
  
-   Esecuzione di una chiamata a una stored procedure remota quando l'opzione REMOTE_PROC_TRANSACTIONS è impostata su ON.  
  
 La copia locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diventa il controller della transazione e utilizza [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) per gestire la transazione distribuita.  
  
 È possibile eseguire una transazione in modo esplicito come transazione distribuita tramite l'istruzione BEGIN DISTRIBUTED TRANSACTION. Per altre informazioni, vedere [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Quando l'opzione SET IMPLICIT_TRANSACTIONS è impostata su ON, tramite un'istruzione BEGIN TRANSACTION vengono create due transazioni nidificate. Per ulteriori informazioni, vedere [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)  
  
## <a name="marked-transactions"></a>Transazioni contrassegnate  
 Quando si specifica l'opzione WITH MARK, il nome della transazione viene inserito nel log delle transazioni. La transazione contrassegnata può essere utilizzata in sostituzione della data e dell'ora per il ripristino di uno stato precedente di un database. Per altre informazioni, vedere [Usare transazioni contrassegnate per recuperare coerentemente i database correlati &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) e [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 I contrassegni del log delle transazioni sono inoltre necessari quando si desidera recuperare uno stato consistente dal punto di vista logico per un set di database correlati. È possibile inserire contrassegni nei log delle transazioni dei database correlati tramite una transazione distribuita. Il recupero fino a questi contrassegni consente di ottenere un set di database consistenti dal punto di vista transazionale. Per la posizione di contrassegni in database correlati, è necessario seguire procedure specifiche.  
  
 Il contrassegno viene inserito nel log delle transazioni solo se il database viene aggiornato dalla transazione contrassegnata. Le transazioni che non modificano i dati non sono contrassegnate.  
  
 È possibile annidare l'istruzione BEGIN TRAN *new_name* WITH MARK in una transazione esistente non contrassegnata. In questo modo, *new_name* diventa il nome di contrassegno della transazione, indipendentemente dall'eventuale nome già assegnato alla transazione. Nell'esempio seguente `M2` è il nome del contrassegno.  
  
```sql  
BEGIN TRAN T1;  
UPDATE table1 ...;  
BEGIN TRAN M2 WITH MARK;  
UPDATE table2 ...;  
SELECT * from table1;  
COMMIT TRAN M2;  
UPDATE table3 ...;  
COMMIT TRAN T1;  
```  
  
 Se si tenta di contrassegnare una transazione già contrassegnata quando si nidificano transazioni, viene visualizzato un messaggio di avviso (non di errore):  
  
 BEGIN TRAN T1 WITH MARK  
  
 UPDATE table1 ...  
  
 BEGIN TRAN M2 WITH MARK  
  
 Messaggio 3920, livello 16, stato 1, linea 3  
  
 L'opzione WITH MARK viene applicata solo alla prima istruzione BEGIN TRAN WITH MARK.  
  
 L'opzione verrà ignorata.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-an-explicit-transaction"></a>R. Uso di una transazione esplicita
**SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Parallel Data Warehouse

Questo esempio usa AdventureWorks. 

```sql
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```

### <a name="b-rolling-back-a-transaction"></a>B. Rollback di una transazione
**SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Parallel Data Warehouse

Nell'esempio seguente viene illustrato l'effetto del rollback di una transazione. In questo esempio l'istruzione ROLLBACK esegue il rollback dell'istruzione INSERT, ma la tabella creata sarà ancora presente.

```sql
CREATE TABLE ValueTable (id INT);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  

```

### <a name="c-naming-a-transaction"></a>C. Denominazione di una transazione 
**SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure

Nell'esempio seguente viene illustrato come denominare una transazione.  
  
```sql
DECLARE @TranName VARCHAR(20);  
SELECT @TranName = 'MyTransaction';  
  
BEGIN TRANSACTION @TranName;  
USE AdventureWorks2012;  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
  
COMMIT TRANSACTION @TranName;  
GO  
```  
  
### <a name="d-marking-a-transaction"></a>D. Contrassegno di una transazione  
**SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure

Nell'esempio seguente viene illustrato come contrassegnare una transazione. Viene contrassegnata la transazione `CandidateDelete`.  
  
```sql  
BEGIN TRANSACTION CandidateDelete  
    WITH MARK N'Deleting a Job Candidate';  
GO  
USE AdventureWorks2012;  
GO  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
GO  
COMMIT TRANSACTION CandidateDelete;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
