---
description: MSSQLSERVER_3414
title: MSSQLSERVER_3414 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3414 (Database Engine error)
ms.assetid: f25852f9-b91c-4356-b817-78bec9ec8db4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fe68125b8b7b5d98e707a9e38d956d1d114db74e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195164"
---
# <a name="mssqlserver_3414"></a>MSSQLSERVER_3414
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3414|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|REC_GIVEUP|  
|Testo del messaggio|Durante il recupero si è verificato un errore che impedisce il riavvio del database '%.*ls' (ID database %d). Individuare gli errori e correggerli oppure eseguire il ripristino da una copia di backup valida nota. Se non è possibile correggere gli errori, contattare il Servizio Supporto Tecnico Clienti Microsoft.|  
  
## <a name="explanation"></a>Spiegazione  
Il database specificato è stato recuperato, ma non è stato possibile avviarlo a causa di errori verificatisi durante il recupero. A causa di questo errore, il database è passato allo stato SUSPECT. Il filegroup primario, e probabilmente altri filegroup, sono sospetti e possono essere danneggiati. Non è possibile recuperare il database durante l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pertanto non è disponibile. Per risolvere il problema, è necessario l'intervento dell'utente. Lo stato SUSPECT sarà visibile sia in SQL Server Management Studio (accanto all'icona del database) sia nella colonna sys.databases.state_desc. Qualsiasi tentativo di usare un database in questo stato comporterà l'errore seguente:

```
Msg 926, Level 14, State 1, Line 1 
Database 'mydb' cannot be opened. It has been marked SUSPECT by recovery. See the SQL Server errorlog for more information
```
  
Si noti che se questo errore si verifica in **tempdb**, l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestata.  

## <a name="cause"></a>Causa
Questo errore può essere causato da una condizione temporanea già presente nel sistema durante un determinato tentativo di avvio dell'istanza del server o di recupero di un database. Può inoltre essere causato da un errore permanente che si verifica ogni volta che si tenta di avviare il database. La causa dell'errore di recupero si trova in genere nei messaggi che precedono l'errore 3414 nel log degli errori o nel log eventi. L'errore precedente nel file di log contiene lo stesso valore di <n>spid. Ad esempio, il seguente errore di recupero è dovuto a un errore di checksum che si verifica durante il tentativo di leggere un blocco di log. Si noti che *spid15s* è presente in tutte le righe:

```
2020-03-31 17:33:13.00 spid15s     Error: 824, Severity: 24, State: 4.  
2020-03-31 17:33:13.00 spid15s     SQL Server detected a logical consistency-based I/O error: (bad checksum). It occurred during a read of page (0:-1) in database ID 13 at offset 0x0000000000b800 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb_log.LDF'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2020-03-31 17:33:13.16 spid15s     Error: 3414, Severity: 21, State: 1.  
2020-03-31 17:33:13.16 spid15s     An error occurred during recovery, preventing the database 'mydb' (database ID 13) from restarting. Diagnose the recovery errors and fix them, or restore from a known good backup. If errors are not corrected or expected, contact Technical Support
```


L'errore di recupero di un database potrebbe avere molte cause diverse. Anche se è necessario valutare ogni errore caso per caso, la risoluzione di un errore di recupero del database è in genere la stessa di quella descritta nella sezione Azione utente di seguito.

## <a name="user-action"></a>Azione utente  
 
Per informazioni sulla causa di questa occorrenza dell'errore 3414, esaminare il log eventi o il log degli errori di Windows per trovare un messaggio precedente che indica l'errore specifico. L'azione appropriata dell'utente varia a seconda che le informazioni nel registro eventi di Windows indichino che l'errore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato causato da una condizione temporanea o da un errore permanente. Il messaggio di errore indica di "individuare gli errori e correggerli oppure eseguire il ripristino da una copia di backup valida". Pertanto, è possibile provare a correggere l'errore riscontrato per consentire il completamento del recupero. Vedere [Errori correggibili e transazioni posticipate](#correctable-errors-and-deferred-transactions).

Se gli errori non possono essere corretti, la prima e la migliore opzione per risolvere il problema consiste nell'eseguire il ripristino di un backup valido. Tuttavia, se non è possibile eseguire il ripristino di un backup, sono disponibili due opzioni aggiuntive che non garantiscono il recupero completo dei dati, ovvero usare il ripristino di emergenza con [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) o provare a copiare la maggior quantità possibile di dati in un altro database. 

 1. Ripristino dell'ultimo backup di database valido noto
 1. Usare il metodo di ripristino di emergenza fornito da [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)
 1. Provare a copiare la maggior quantità possibile di dati in un altro database.

Il primo metodo, ossia il ripristino di un backup di database valido, rappresenta la scelta migliore per ripristinare uno stato coerente noto di un database.  

La seconda scelta migliore, se non è disponibile alcun backup, consiste nel portare il database online e renderlo accessibile. Tuttavia, è necessario tenere presente che non è possibile garantire la coerenza delle transazioni, perché il recupero non è riuscito. Non c'è modo di sapere per quali transazioni sarebbe stato necessario eseguire il rollback o il rollforward ma non è stato fatto a causa dell'errore di recupero. I passaggi per procedere con il ripristino di emergenza sono descritti nella sezione [Risoluzione degli errori di database in modalità di emergenza](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode) nella documentazione di DBCC CHECKDB. 

Se il ripristino di emergenza non funziona e si vuole provare a salvare alcuni dati in un altro database, è necessario impostare il database in modalità di emergenza tramite il comando ALTER DATABASE <dbname> SET EMERGENCY per potervi accedere. Quindi si può provare a copiare i dati dalle tabelle.

### <a name="correctable-errors-and-deferred-transactions"></a>Errori correggibili e transazioni posticipate
Non tutti gli errori rilevati durante il recupero del database determineranno un errore di recupero e un database sospetto:

Gli errori che si verificano alla prima apertura del database e/o dei file di log delle transazioni avvengono prima del recupero. Esempi di tali errori sono [17204](mssqlserver-17204-database-engine-error.md) e [17207](mssqlserver-17207-database-engine-error.md). Una volta corretti questi errori, è possibile che il recupero possa procedere, ma non è garantito che venga completato se si verificano altri errori. Gli errori come 17204 e 17207 non comportano il passaggio del database allo stato SUSPECT. Infatti, quando si verificano questi problemi, lo stato del database è RECOVERY_PENDING. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente il completamento del recupero anche quando si verifica un errore a livello di pagina e manterrà comunque la coerenza delle transazioni. Questo processo ha ridotto il numero di scenari risultanti in un database con lo stato SUSPECT. Si tratta del concetto delle [transazioni posticipate](../backup-restore/deferred-transactions-sql-server.md).

Se l'errore riscontrato durante il recupero indica un problema di una pagina del database, ad esempio un errore di checksum o un messaggio 824, è possibile che il recupero possa essere completato con errori in sospeso. Nel caso in cui non sia stato eseguito il commit di una transazione, un errore in una pagina può generare una [transazione posticipata](../backup-restore/deferred-transactions-sql-server.md), consentendo il completamento del recupero.  

Le voci seguenti del log degli errori mostrano un esempio di un messaggio di errore [824](mssqlserver-824-database-engine-error.md) riscontrato durante il recupero, che però è stato possibile completare con una transazione posticipata. Notare l'assenza dell'errore 3414 in questa situazione e il messaggio che il recupero è stato completato per il database:

```2010-03-31 19:17:18.45 spid7s      Error: 824, Severity: 24, State: 2.   
2010-03-31 19:17:18.45 spid7s      SQL Server detected a logical consistency-based I/O error: incorrect checksum (expected: 0xb2c87a0a; actual: 0xb6c0a5e2). It occurred during a read of page (1:153) in database ID 13 at offset 0x00000000132000 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2010-03-31 19:17:18.45 spid7s      Error: 3314, Severity: 21, State: 1.   
2010-03-31 19:17:18.45 spid7s      During undoing of a logged operation in database 'mydb', an error occurred at log record ID (25:100:19). Typically, the specific failure is logged previously as an error in the Windows Event Log service. Restore the database or file from a backup, or repair the database.
2010-03-31 19:17:18.45 spid7s      Errors occurred during recovery while rolling back a transaction. The transaction was deferred. Restore the bad page or file, and re-run recovery.   
2010-03-31 19:17:18.45 spid7s      Recovery completed for database mydb (database ID 13) in 2 second(s) (analysis 204 ms, redo 25 ms, undo 1832 ms.) This is an informational message only. No user action is required.   
```

Se è necessario eseguire il rollforward di una transazione già sottoposto a commit, la pagina può essere contrassegnata come inaccessibile (tutti i futuri tentativi di accesso alla pagina generano il messaggio 829) e il recupero può essere completato. In questa situazione l'errore deve essere corretto ripristinando la pagina da un backup o deallocando la pagina usando DBCC CHECKDB con il ripristino.


  
## <a name="see-also"></a>Vedere anche  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Ripristini di database completi &#40;modello di recupero con registrazione minima&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[Transazioni posticipate](../backup-restore/deferred-transactions-sql-server.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)

  
