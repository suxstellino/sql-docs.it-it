---
description: sp_help_jobschedule (Transact-SQL)
title: sp_help_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobschedule
- sp_help_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobschedule
ms.assetid: 2cded902-9272-4667-ac4b-a4f95a9f008e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9765feaa9e3237f1ccd8d3a6dcc203c88758aefa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200124"
---
# <a name="sp_help_jobschedule-transact-sql"></a>sp_help_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulla pianificazione dei processi utilizzati da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per l'esecuzione di attività automatizzate.  
 
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobschedule { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @schedule_name = ] 'schedule_name' ]  
     [ , [ @schedule_id = ] schedule_id ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]
> È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.

`[ @schedule_name = ] 'schedule_name'` Nome dell'elemento di pianificazione per il processo. *schedule_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @schedule_id = ] schedule_id` Numero di identificazione dell'elemento di pianificazione per il processo. *schedule_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @include_description = ] include_description` Specifica se includere la descrizione della pianificazione nel set di risultati. *include_description* è di **bit** e il valore predefinito è **0**. Quando *include_description* è **0**, la descrizione della pianificazione non è inclusa nel set di risultati. Quando *include_description* è **1**, la descrizione della pianificazione viene inclusa nel set di risultati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Numero di identificazione della pianificazione.|  
|**schedule_name**|**sysname**|Nome della pianificazione.|  
|**abilitato**|**int**|Indica se la pianificazione è abilitata (**1**) o non abilitata (**0**).|  
|**freq_type**|**int**|Valore che indica la frequenza di esecuzione del processo:<br /><br /> **1** = una volta<br /><br /> **4** = giornaliero<br /><br /> **8** = settimanale<br /><br /> **16** = mensile<br /><br /> **32** = mensile rispetto al **freq_interval**<br /><br /> **64** = esecuzione all'avvio del servizio **SQLServerAgent** .|  
|**freq_interval**|**int**|Giorni in cui viene eseguito il processo. Il valore dipende dal valore di **freq_type**. Per ulteriori informazioni, vedere [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_subday_type**|**int**|Unità per **freq_subday_interval**. Per ulteriori informazioni, vedere [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_subday_interval**|**int**|Numero di periodi di **freq_subday_type** tra le esecuzioni del processo. Per ulteriori informazioni, vedere [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_relative_interval**|**int**|Occorrenza del processo pianificato del **freq_interval** di ogni mese. Per ulteriori informazioni, vedere [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_recurrence_factor**|**int**|Numero di mesi tra l'esecuzione pianificata del processo.|  
|**active_start_date**|**int**|Data di attivazione della pianificazione.|  
|**active_end_date**|**int**|Data di fine della pianificazione.|  
|**active_start_time**|**int**|Ora di inizio della pianificazione.|  
|**active_end_time**|**int**|Ora di fine della pianificazione.|  
|**date_created**|**datetime**|Data di creazione della pianificazione.|  
|**schedule_description**|**nvarchar(4000)**|Descrizione in inglese della pianificazione derivata dai valori nelle **pianificazionimsdb.dbo.sys**. Quando *include_description* è **0**, questa colonna contiene testo indicante che la descrizione non è stata richiesta.|  
|**next_run_date**|**int**|Data della successiva esecuzione del processo in base alla pianificazione.|  
|**next_run_time**|**int**|Ora della successiva esecuzione del processo in base alla pianificazione.|  
|**schedule_uid**|**uniqueidentifier**|Identificatore della pianificazione.|  
|**job_count**|**int**|Numero di processi restituiti.|  
  
> **Nota: sp_help_jobschedule** restituisce valori dalle tabelle di sistema **dbo.sysJobSchedules** e **dbo.syspianificazioni** in **msdb**. **sysjobschedules** di aggiornamento ogni 20 minuti. Ciò potrebbe influire sui valori restituiti dalla stored procedure.  
  
## <a name="remarks"></a>Commenti  
 I parametri di **sp_help_jobschedule** possono essere utilizzati solo in determinate combinazioni. Se *schedule_id* è specificato, non è possibile specificare né *job_id* né *job_name* . In caso contrario, è possibile utilizzare i parametri *job_id* o *job_name* con *schedule_name*.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri di **SQLAgentUserRole** possono visualizzare solo le proprietà delle pianificazioni di processo di cui sono proprietari.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-job-schedule-for-a-specific-job"></a>R. Restituzione della pianificazione di un processo specifico  
 Nell'esempio seguente vengono restituite informazioni sulla pianificazione del processo `BackupDatabase`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'BackupDatabase' ;  
GO  
```  
  
### <a name="b-returning-the-job-schedule-for-a-specific-schedule"></a>B. Restituzione della pianificazione di un processo per una pianificazione specifica  
 Nell'esempio seguente vengono restituite informazioni sulla pianificazione `NightlyJobs` e sul processo `RunReports`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule   
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="c-returning-the-job-schedule-and-schedule-description-for-a-specific-schedule"></a>C. Restituzione della pianificazione di un processo e della descrizione della pianificazione per una pianificazione specifica  
 Nell'esempio seguente vengono restituite informazioni sulla pianificazione `NightlyJobs` e sul processo `RunReports`. Il set dei risultati restituiti include una descrizione della pianificazione.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs',  
    @include_description = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_update_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
