---
description: sp_help_jobstep (Transact-SQL)
title: sp_help_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobstep_TSQL
- sp_help_jobstep
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobstep
ms.assetid: 4a13b804-45f2-4f82-987f-42d9a57dd6db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6a4a59a69612c37a6b6d9208e121b11ea448a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200112"
---
# <a name="sp_help_jobstep-transact-sql"></a>sp_help_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui passaggi di un processo utilizzato dal servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per l'esecuzione di attività automatizzate.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobstep { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] 'job_id'` Numero di identificazione del processo per il quale restituire le informazioni sul processo. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @step_id = ] step_id` Numero di identificazione del passaggio nel processo. Se viene omesso, vengono inclusi tutti i passaggi del processo. *step_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @step_name = ] 'step_name'` Nome del passaggio nel processo. *step_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @suffix = ] suffix` Flag che indica se una descrizione di testo viene aggiunta alla colonna dei **flag** nell'output. il *suffisso* è di **bit** e il valore predefinito è **0**. Se il *suffisso* è **1**, viene aggiunta una descrizione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|Identificatore univoco del passaggio.|  
|**step_name**|**sysname**|Nome del passaggio del processo.|  
|**sottosistema**|**nvarchar(40)**|Sottosistema in cui eseguire il comando del passaggio.|  
|**command**|**nvarchar(max)**|Comando eseguito nel passaggio.|  
|**flags**|**int**|Maschera di bit dei valori che controllano il funzionamento del passaggio.|  
|**cmdexec_success_code**|**int**|Per un passaggio **CmdExec** , questo è il codice di uscita del processo di un comando riuscito.|  
|**on_success_action**|**tinyint**|Azione da eseguire se il passaggio viene eseguito correttamente:<br /><br /> **1** = termina il processo segnalato correttamente.<br /><br /> **2** = chiude l'errore di segnalazione dei processi.<br /><br /> **3** = Vai al passaggio successivo.<br /><br /> **4** = Vai al passaggio.|  
|**on_success_step_id**|**int**|Se **on_success_action** è 4, indica il passaggio successivo da eseguire.|  
|**on_fail_action**|**tinyint**|Azione da eseguire se il passaggio non viene eseguito correttamente. I valori sono uguali a **on_success_action**.|  
|**on_fail_step_id**|**int**|Se **on_fail_action** è 4, indica il passaggio successivo da eseguire.|  
|**server**|**sysname**|Riservato.|  
|**database_name**|**sysname**|Per un passaggio [!INCLUDE[tsql](../../includes/tsql-md.md)], indica il database in cui viene eseguito il comando.|  
|**database_user_name**|**sysname**|Per un passaggio [!INCLUDE[tsql](../../includes/tsql-md.md)], indica il contesto utente del database in cui viene eseguito il comando.|  
|**retry_attempts**|**int**|Numero massimo di tentativi di esecuzione del comando (nel caso in cui non sia stato eseguito correttamente).|  
|**retry_interval**|**int**|Intervallo in minuti che intercorre tra un tentativo e il successivo.|  
|**os_run_priority**|**int**|Riservato.|  
|**output_file_name**|**nvarchar(200)**|File in cui scrivere l'output del comando ( [!INCLUDE[tsql](../../includes/tsql-md.md)] solo per i passaggi, **CmdExec** e **PowerShell** ).|  
|**last_run_outcome**|**int**|Risultato dell'ultima esecuzione del passaggio:<br /><br /> **0** = non riuscito<br /><br /> **1** = operazione completata<br /><br /> **2** = nuovo tentativo<br /><br /> **3** = annullato<br /><br /> **5** = sconosciuto|  
|**last_run_duration**|**int**|Durata (hhmmss) dell'ultima esecuzione del passaggio.|  
|**last_run_retries**|**int**|Numero di tentativi di esecuzione del comando durante l'ultima esecuzione del passaggio.|  
|**last_run_date**|**int**|Data di inizio dell'ultima esecuzione del passaggio.|  
|**last_run_time**|**int**|Ora di inizio dell'ultima esecuzione del passaggio.|  
|**proxy_id**|**int**|Proxy per il passaggio del processo.|  
  
## <a name="remarks"></a>Commenti  
 **sp_help_jobstep** si trova nel database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri di **SQLAgentUserRole** possono visualizzare solo i passaggi di processo per i processi di cui sono proprietari.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-return-information-for-all-steps-in-a-specific-job"></a>R. Restituzione di informazioni su tutti i passaggi di un processo specifico  
 In questo esempio vengono restituiti tutti i passaggi del processo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-information-about-a-specific-job-step"></a>B. Restituzione di informazioni su un determinato passaggio di un processo  
 Nell'esempio seguente vengono restituite informazioni sul primo passaggio del processo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
