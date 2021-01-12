---
description: KILL (Transact-SQL)
title: KILL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL_TSQL
- KILL
dev_langs:
- TSQL
helpviewer_keywords:
- WITH STATUSONLY option
- terminating distributed transactions
- distributed transactions [SQL Server], terminating
- in-doubt transactions
- IDs [SQL Server], user processes
- ending distributed transactions [SQL Server]
- stopping distributed transactions
- session IDs [SQL Server]
- system process termination [SQL Server]
- stopping processes
- ending processes [SQL Server]
- UOW [SQL Server]
- orphaned transactions [SQL Server]
- unit of work [SQL Server]
- process termination [SQL Server]
- KILL statement
- terminating process
ms.assetid: 071cf260-c794-4b45-adc0-0e64097938c0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bc07759604aea6c702b991d390849e1bf9fe1a46
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98084355"
---
# <a name="kill-transact-sql"></a>KILL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Termina un processo utente in base all'ID di sessione o all'unità di lavoro (UOW, Unit Of Work). Se all'ID di sessione o al valore UOW specificato sono associate numerose azioni di rollback, l'esecuzione dell'istruzione KILL può richiedere del tempo, soprattutto quando implica il rollback di una transazione lunga.  
  
KILL termina una connessione normale, operazione che arresta internamente le transazioni associate all'ID di sessione specificato. A volte può essere in uso [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC). In tal caso, è anche possibile usare l'istruzione per terminare le transazioni distribuite orfane e in dubbio.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server  
  
KILL { session ID [ WITH STATUSONLY ] | UOW [ WITH STATUSONLY | COMMIT | ROLLBACK ] }    

```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
KILL 'session_id'  
[;]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

_ID sessione_   
ID di sessione del processo da terminare. `session_id` è un valore intero univoco (**int**) assegnato a ogni connessione utente al momento dell'attivazione. Il valore dell'ID di sessione rimane associato alla connessione per tutta la durata della connessione. Al termine della connessione, il valore intero viene rilasciato ed è possibile riassegnarlo a una nuova connessione.  

La query seguente consente di identificare il `session_id` che si intende eliminare:  

 ```sql  
 SELECT conn.session_id, host_name, program_name,
     nt_domain, login_name, connect_time, last_request_end_time 
FROM sys.dm_exec_sessions AS sess
JOIN sys.dm_exec_connections AS conn
    ON sess.session_id = conn.session_id;

```  


_UOW_   
Identifica l'ID dell'unità di lavoro delle transazioni distribuite. _UOW_ è un GUID che è possibile ottenere dalla colonna request_owner_guid della vista a gestione dinamica `sys.dm_tran_locks`. È anche possibile ottenere il valore _UOW_ dal log degli errori o tramite il monitoraggio MS DTC. Per ulteriori informazioni sul monitoraggio di transazioni distribuite, vedere la documentazione di MS DTC.  
  
Usare l’istruzione KILL \<UOW> per arrestare le transazioni distribuite non risolte. Queste transazioni non sono associate a un ID di sessione effettivo, ma sono invece associate in modo artificiale all'ID di sessione -2. Questo ID di sessione semplifica l'identificazione delle transazioni non risolte tramite l'esecuzione di una query sulla colonna dell'ID di sessione nella vista a gestione dinamica `sys.dm_tran_locks`, ` sys.dm_exec_sessions` o `sys.dm_exec_requests`.  

_WITH STATUSONLY_   
Viene utilizzata per generare un report di stato per un valore di _UOW_ o `session_id` specificato sottoposto a rollback a causa di un’istruzione KILL precedente. L’istruzione KILL WITH STATUSONLY non termina o esegue il rollback dell’unità di lavoro o dell’ID di sessione. Il comando visualizza esclusivamente lo stato corrente del rollback.

_WITH COMMIT_   
Viene utilizzata per terminare una transazione distribuita non risolta con commit. Applicabile solo alle transazioni distribuite, per usare questa opzione è necessario specificare un valore per _UOW_.  Per altre informazioni, vedere [Transazioni distribuite](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions).

_WITH ROLLBACK_   
Viene utilizzata per terminare una transazione distribuita non risolta con rollback. Applicabile solo alle transazioni distribuite, per usare questa opzione è necessario specificare un valore per _UOW_.  Per altre informazioni, vedere [Transazioni distribuite](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions).


  
## <a name="remarks"></a>Osservazioni  
L'istruzione KILL viene normalmente usata per terminare un processo che blocca altri processi importanti con blocchi. Può essere usata anche per arrestare un processo che sta eseguendo una query che usa risorse di sistema necessarie. I processi di sistema e i processi che eseguono una stored procedure estesa non possono essere terminati.  
  
Eseguire l'istruzione KILL con cautela, in particolare quando sono in esecuzione processi critici. Non è possibile terminare un proprio processo. Inoltre, è consigliabile non terminare i processi seguenti:  
  
-   AWAITING COMMAND  
-   CHECKPOINT SLEEP  
-   LAZY WRITER  
-   LOCK MONITOR  
-   SIGNAL HANDLER  
  
Usare @@SPID per visualizzare il valore dell'ID di sessione della sessione corrente.  
  
Per ottenere un report dei valori di ID della sessione attiva, eseguire una query sulla colonna session_id delle viste a gestione dinamica sys.dm_tran_locks, sys.dm_exec_sessions e sys.dm_exec_requests. Si può anche visualizzare la colonna SPID restituita dalla stored procedure di sistema sp_who. Se è in corso il rollback per un processo SPID specifico, nella relativa colonna cmd del set di risultati sp_who viene indicato KILLED/ROLLBACK.  
  
Quando una determinata connessione mantiene attivo un blocco su una risorsa del database e blocca l'avanzamento di un'altra connessione, l'ID di sessione della connessione di blocco viene visualizzato nella colonna `blocking_session_id` di `sys.dm_exec_requests` o nella colonna `blk` restituita da `sp_who`.  
  
È possibile utilizzare il comando KILL per risolvere le transazioni distribuite in dubbio. Queste sono le transazioni distribuite non risolte che si verificano in seguito a riavvii non pianificati del server di database o di MS DTC. Per altre informazioni sulla creazione di transazioni contrassegnate, vedere la sezione "Commit in due fasi" in [Usare transazioni contrassegnate per recuperare coerentemente i database correlati &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="using-with-statusonly"></a>Utilizzo di WITH STATUSONLY  
Il comando KILL WITH STATUSONLY genera un report solo se è in corso il rollback dell'ID di sessione o del valore UOW in seguito all'esecuzione di un'istruzione KILL _session ID_|_UOW_ precedente. Il report di stato indica la percentuale di rollback completata e il periodo di tempo rimanente, espresso in secondi, nel formato seguente:  
  
`Spid|UOW <xxx>: Transaction rollback in progress. Estimated rollback completion: <yy>% Estimated time left: <zz> seconds`  
  
Se quando si esegue l'istruzione KILL _session ID_|_UOW_ WITH STATUSONLY l'operazione di rollback dell'ID di sessione o del valore UOW è già stata completata, KILL _session ID_|_UOW_ WITH STATUSONLY restituisce il messaggio di errore seguente:  
  
```
"Msg 6120, Level 16, State 1, Line 1"  
"Status report cannot be obtained. Rollback operation for Process ID <session ID> is not in progress."
```  
Questo errore si verifica anche se non è in corso il rollback di ID di sessione o UOW


È possibile ottenere lo stesso report di stato ripetendo la stessa istruzione KILL _session ID_|_UOW_ senza usare l'opzione WITH STATUSONLY. Tuttavia, non è consigliabile ripetere l'opzione in questo modo. Se si ripete un'istruzione KILL _session ID_, il nuovo processo potrebbe infatti arrestarsi se dopo il completamento del rollback l'ID di sessione viene riassegnato a una nuova attività prima dell'esecuzione della nuova istruzione KILL. Impedire l'arresto del nuovo processo specificando WITH STATUSONLY.  
  
## <a name="permissions"></a>Autorizzazioni  
**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:** È richiesta l'autorizzazione ALTER ANY CONNECTION. L'autorizzazione ALTER ANY CONNECTION viene concessa mediante l'appartenenza al ruolo server predefinito sysadmin o processadmin.  
  
**[!INCLUDE[ssSDS](../../includes/sssds-md.md)]:** È richiesta l'autorizzazione KILL DATABASE CONNECTION. L'account di accesso a livello di server principale ha l'autorizzazione KILL DATABASE CONNECTION.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-kill-to-stop-a-session"></a>R. Uso di KILL per arrestare una sessione  
 L'esempio seguente illustra come arrestare l'ID di sessione `53`.  
  
```sql  
KILL 53;  
GO  
```  
  
### <a name="b-using-kill-session-id-with-statusonly-to-obtain-a-progress-report"></a>B. Utilizzo di KILL session ID WITH STATUSONLY per ottenere un report di stato  
Nell'esempio seguente viene generato un report di stato del processo di rollback per l'ID di sessione specifico.  
  
```sql  
KILL 54;  
KILL 54 WITH STATUSONLY;  
GO  
  
--This is the progress report.  
spid 54: Transaction rollback in progress. Estimated rollback completion: 80% Estimated time left: 10 seconds.  
```  
  
### <a name="c-using-kill-to-stop-an-orphaned-distributed-transaction"></a>C. Uso di KILL per arrestare una transazione distribuita orfana  
L'esempio seguente illustra come arrestare una transazione distribuita orfana (ID di sessione = -2) con un valore *UOW* di `D5499C66-E398-45CA-BF7E-DC9C194B48CF`.  
  
```sql  
KILL 'D5499C66-E398-45CA-BF7E-DC9C194B48CF';  
```  

  
## <a name="see-also"></a>Vedere anche  
[KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)   
[KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
[Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
[SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)   
[@@SPID &#40;Transact-SQL&#41;](../../t-sql/functions/spid-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
[sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
[sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
