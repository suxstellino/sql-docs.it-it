---
description: sp_update_jobstep (Transact-SQL)
title: sp_update_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_jobstep
- sp_update_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_jobstep
ms.assetid: e158802c-c347-4a5d-bf75-c03e5ae56e6b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c102cee625bc81611bd67209306d6bc5f9398660
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183575"
---
# <a name="sp_update_jobstep-transact-sql"></a>sp_update_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica l'impostazione di un passaggio in un processo utilizzato per l'esecuzione di operazioni automatizzate.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_update_jobstep   
     {   [@job_id =] job_id   
       | [@job_name =] 'job_name' } ,  
     [@step_id =] step_id  
     [ , [@step_name =] 'step_name' ]  
     [ , [@subsystem =] 'subsystem' ]   
     [ , [@command =] 'command' ]  
     [ , [@additional_parameters =] 'parameters' ]  
     [ , [@cmdexec_success_code =] success_code ]  
     [ , [@on_success_action =] success_action ]   
     [ , [@on_success_step_id =] success_step_id ]  
     [ , [@on_fail_action =] fail_action ]   
     [ , [@on_fail_step_id =] fail_step_id ]  
     [ , [@server =] 'server' ]   
     [ , [@database_name =] 'database' ]  
     [ , [@database_user_name =] 'user' ]   
     [ , [@retry_attempts =] retry_attempts ]  
     [ , [@retry_interval =] retry_interval ]   
     [ , [@os_run_priority =] run_priority ]  
     [ , [@output_file_name =] 'file_name' ]   
     [ , [@flags =] flags ]  
     [ ,  {   [ @proxy_id = ] proxy_id   
            | [ @proxy_name = ] 'proxy_name' }   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo a cui appartiene il passaggio. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null. È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @job_name = ] 'job_name'` Nome del processo a cui appartiene il passaggio. *job_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @step_id = ] step_id` Numero di identificazione del passaggio del processo da modificare. Questo numero non è modificabile. *step_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @step_name = ] 'step_name'` È un nuovo nome per il passaggio. *step_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subsystem = ] 'subsystem'` Sottosistema utilizzato da Microsoft SQL Server Agent per eseguire il *comando*. il *sottosistema* è di **tipo nvarchar (40)** e il valore predefinito è null.  
  
`[ @command = ] 'command'` Comandi da eseguire tramite il *sottosistema*. *Command* è di **tipo nvarchar (max)** e il valore predefinito è null.  
  
`[ @additional_parameters = ] 'parameters'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @cmdexec_success_code = ] success_code` Il valore restituito da un comando del sottosistema **CmdExec** per indicare che il *comando* è stato eseguito correttamente. *success_code* è di **tipo int** e il valore predefinito è null.  
  
`[ @on_success_action = ] success_action` Azione da eseguire se il passaggio ha esito positivo. *success_action* è di **tinyint** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione (azione)|  
|-----------|----------------------------|  
|**1**|Uscita in caso di esito positivo|  
|**2**|Uscita in caso di esito negativo|  
|**3**|Esecuzione del passaggio successivo|  
|**4**|Andare al passaggio *success_step_id.*|  
  
`[ @on_success_step_id = ] success_step_id` Numero di identificazione del passaggio del processo da eseguire se il passaggio ha esito positivo e *success_action* è **4**. *success_step_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @on_fail_action = ] fail_action` Azione da eseguire se il passaggio ha esito negativo. *fail_action* è di **tinyint** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione (azione)|  
|-----------|----------------------------|  
|**1**|Uscita in caso di esito positivo|  
|**2**|Uscita in caso di esito negativo|  
|**3**|Esecuzione del passaggio successivo|  
|**4**|Andare al passaggio *fail_step_id * *.*|  
  
`[ @on_fail_step_id = ] fail_step_id` Numero di identificazione del passaggio del processo da eseguire se il passaggio ha esito negativo e *fail_action* è **4**. *fail_step_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]il *Server* è di **tipo nvarchar (128)** e il valore predefinito è null.  
  
`[ @database_name = ] 'database'` Nome del database in cui eseguire un [!INCLUDE[tsql](../../includes/tsql-md.md)] passaggio. il *database* è di **tipo sysname**. I nomi racchiusi tra parentesi quadre ([ ]) non sono ammessi. Il valore predefinito è NULL.  
  
`[ @database_user_name = ] 'user'` Nome dell'account utente da utilizzare durante l'esecuzione di un [!INCLUDE[tsql](../../includes/tsql-md.md)] passaggio. *User* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @retry_attempts = ] retry_attempts` Numero di tentativi da usare in caso di errore di questo passaggio. *retry_attempts* è di **tipo int** e il valore predefinito è null.  
  
`[ @retry_interval = ] retry_interval` Quantità di tempo in minuti tra i tentativi di ripetizione. *retry_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @os_run_priority = ] run_priority` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @output_file_name = ] 'file_name'` Nome del file in cui viene salvato l'output di questo passaggio. *file_name* è di **tipo nvarchar (200)** e il valore predefinito è null. Questo parametro è valido solo con comandi eseguiti nei sottosistemi [!INCLUDE[tsql](../../includes/tsql-md.md)] o CmdExec.  
  
 Per impostare di nuovo il output_file_name su NULL, è necessario impostare *output_file_name* su una stringa vuota ('') o su una stringa di caratteri vuoti, ma non è possibile utilizzare la funzione **char (32)** . Ad esempio, impostare questo argomento su una stringa vuota nel modo descritto di seguito:  
  
 **@output_file_name = ' '**  
  
`[ @flags = ] flags` Opzione che controlla il comportamento. *Flags* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|L'output sovrascrive il contenuto del file di output.|  
|**2**|L'output viene aggiunto alla fine del file di output.|  
|**4**|L'output del passaggio del processo Transact-SQL viene scritto nella cronologia dei passaggi|  
|**8**|Il log viene scritto nella tabella. La cronologia esistente viene sovrascritta|  
|**16**|Il log viene scritto nella tabella in aggiunta alla cronologia esistente|  
  
`[ @proxy_id = ] proxy_id` Numero ID del proxy in cui viene eseguito il passaggio di processo. *proxy_id* è di tipo **int** e il valore predefinito è null. Se non viene specificato alcun *proxy_id* , non viene specificato alcun *proxy_name* e non viene specificato alcun *user_name* , il passaggio del processo viene eseguito come account del servizio per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy in cui viene eseguito il passaggio di processo. *proxy_name* è di tipo **sysname** e il valore predefinito è null. Se non viene specificato alcun *proxy_id* , non viene specificato alcun *proxy_name* e non viene specificato alcun *user_name* , il passaggio del processo viene eseguito come account del servizio per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_update_jobstep** deve essere eseguito dal database **msdb** .  
  
 L'aggiornamento di un passaggio di processo comporta un incremento del numero di versione del processo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri di **sysadmin** possono aggiornare un passaggio di processo di proprietà di un altro utente.  
  
 Se il passaggio di processo richiede l'accesso a un proxy, l'autore del passaggio deve disporre dell'accesso al proxy per tale passaggio. Tutti i sottosistemi, a esclusione di Transact-SQL, richiedono un account proxy. I membri di **sysadmin** hanno accesso a tutti i proxy e possono usare l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio Agent per il proxy.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene modificato il numero di tentativi per il primo passaggio del processo `Weekly Sales Data Backup`. Dopo aver eseguito questo esempio, il numero di tentativi sarà `10`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1,  
    @retry_attempts = 10 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare o modificare i processi](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_delete_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
