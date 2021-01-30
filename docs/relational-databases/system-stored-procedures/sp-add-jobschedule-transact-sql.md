---
description: sp_add_jobschedule (Transact-SQL)
title: sp_add_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobschedule
- sp_add_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_jobschedule
ms.assetid: ffce19d9-d1d6-45b4-89fd-ad0f60822ba0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6c44f473d71f67718e9444ab2994fcfc17258411
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206470"
---
# <a name="sp_add_jobschedule-transact-sql"></a>sp_add_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una pianificazione per un processo di SQL Agent.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
  > [!IMPORTANT]  
  > In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_jobschedule [ @job_id = ] job_id, | [ @job_name = ] 'job_name', [ @name = ] 'name'  
     [ , [ @enabled = ] enabled_flag ]  
     [ , [ @freq_type = ] frequency_type ]  
     [ , [ @freq_interval = ] frequency_interval ]  
     [ , [ @freq_subday_type = ] frequency_subday_type ]  
     [ , [ @freq_subday_interval = ] frequency_subday_interval ]  
     [ , [ @freq_relative_interval = ] frequency_relative_interval ]  
     [ , [ @freq_recurrence_factor = ] frequency_recurrence_factor ]  
     [ , [ @active_start_date = ] active_start_date ]  
     [ , [ @active_end_date = ] active_end_date ]  
     [ , [ @active_start_time = ] active_start_time ]  
     [ , [ @active_end_time = ] active_end_time ]  
     [ , [ @schedule_id = ] schedule_id OUTPUT ]
     [ , [ @schedule_uid = ] _schedule_uid OUTPUT ]
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo a cui viene aggiunta la pianificazione. *job_id* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito.  
  
`[ @job_name = ] 'job_name'` Nome del processo a cui viene aggiunta la pianificazione. *job_name* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @name = ] 'name'` Nome della pianificazione. *Name* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito.  
  
`[ @enabled = ] enabled_flag` Indica lo stato corrente della pianificazione. *enabled_flag* è di **tinyint** e il valore predefinito è **1** (abilitato). Se è **0**, la pianificazione non è abilitata. Quando la pianificazione è disabilitata, il processo non viene eseguito.  
  
`[ @freq_type = ] frequency_type` Valore che indica quando deve essere eseguito il processo. *frequency_type* è di **tipo int** e il valore predefinito è **0**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile rispetto a *frequency_interval.*|  
|**64**|All'avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent|  
|**128**|Quando il computer è inattivo|  
  
`[ @freq_interval = ] frequency_interval` Giorno in cui viene eseguito il processo. *frequency_interval* è di **tipo int** e il valore predefinito è 0 e dipende dal valore di *frequency_type* , come indicato nella tabella seguente:  
  
|Valore|Effetto|  
|-----------|------------|  
|**1** (una volta)|*frequency_interval* non è utilizzato.|  
|**4** (giornaliera)|Ogni *frequency_interval* giorni.|  
|**8** (settimanale)|*frequency_interval* è uno o più degli elementi seguenti (combinati con un operatore logico OR):<br /><br /> 1 = domenica<br /><br /> 2 = lunedì<br /><br /> 4 = martedì<br /><br /> 8 = mercoledì<br /><br /> 16 = giovedì<br /><br /> 32 = venerdì<br /><br /> 64 = sabato|  
|**16** (ogni mese)|Il *frequency_interval* giorno del mese.|  
|**32** (mensile relativo)|*frequency_interval* è uno dei seguenti:<br /><br /> 1 = domenica<br /><br /> 2 = lunedì<br /><br /> 3 = martedì<br /><br /> 4 = mercoledì<br /><br /> 5 = giovedì<br /><br /> 6 = venerdì<br /><br /> 7 = sabato<br /><br /> 8 = giorno<br /><br /> 9 = giorno feriale<br /><br /> 10 = giorno festivo|  
|**64** (all' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avvio del servizio Agent)|*frequency_interval* non è utilizzato.|  
|**128**|*frequency_interval* non è utilizzato.|  
  
`[ @freq_subday_type = ] frequency_subday_type` Specifica le unità per *frequency_subday_interval*. *frequency_subday_type* è di **tipo int** e non prevede alcun valore predefinito. i possibili valori sono i seguenti:  
  
|Valore|Descrizione (unità)|  
|-----------|--------------------------|  
|**0x1**|All'ora specificata|  
|**0x2**|Secondi|  
|**0x4**|Minuti|  
|**0x8**|Ore|  
  
`[ @freq_subday_interval = ] frequency_subday_interval` Numero di periodi di *frequency_subday_type* tra le esecuzioni del processo. *frequency_subday_interval* è di **tipo int** e il valore predefinito è 0.  
  
`[ @freq_relative_interval = ] frequency_relative_interval` Definisce ulteriormente il *frequency_interval* quando *frequency_type* è impostato su **32** (mensile relativo).  
  
 *frequency_relative_interval* è di **tipo int** e non prevede alcun valore predefinito. i possibili valori sono i seguenti:  
  
|Valore|Descrizione (unità)|  
|-----------|--------------------------|  
|**1**|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
  
 *frequency_relative_interval* indica l'occorrenza dell'intervallo. Se ad esempio *frequency_relative_interval* è impostato su **2**, *frequency_type* è impostato su **32** e *frequency_interval* è impostato su **3**, il processo pianificato verrà eseguito il secondo martedì di ogni mese.  
  
`[ @freq_recurrence_factor = ] frequency_recurrence_factor` Numero di settimane o mesi che intercorre tra l'esecuzione pianificata del processo. *frequency_recurrence_factor* viene utilizzato solo se *frequency_type* è impostato su **8**, **16** o **32**. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_start_date = ] active_start_date` Data in cui può iniziare l'esecuzione del processo. *active_start_date* è di **tipo int** e non prevede alcun valore predefinito. La data è nel formato AAAAMMGG. Se *active_start_date* è impostato, la data deve essere maggiore o uguale a 19900101.  
  
 Al termine della creazione della pianificazione, esaminare la data di inizio per verificare che corrisponda alla data corretta. Per ulteriori informazioni, vedere la sezione "pianificazione della data di inizio" in [creare e alleghi pianificazioni ai processi](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
`[ @active_end_date = ] active_end_date` Data in cui l'esecuzione del processo può essere arrestata. *active_end_date* è di **tipo int** e non prevede alcun valore predefinito. La data è nel formato AAAAMMGG.  
  
`[ @active_start_time = ] active_start_time` Ora di ogni giorno tra *active_start_date* e *active_end_date* per iniziare l'esecuzione del processo. *active_start_time* è di **tipo int** e non prevede alcun valore predefinito. L'ora è in formato HHMMSS a 24 ore.  
  
`[ @active_end_time = active_end_time_` Ora di ogni giorno tra *active_start_date* e *active_end_date* per terminare l'esecuzione del processo. *active_end_time* è di **tipo int** e non prevede alcun valore predefinito. L'ora è in formato HHMMSS a 24 ore.  
  
`[ @schedule_id = schedule_idOUTPUT` Pianificare il numero di identificazione assegnato alla pianificazione, se è stato creato correttamente. *schedule_id* è una variabile di output di tipo **int** e non prevede alcun valore predefinito.  
  
`[ @schedule_uid = ] _schedule_uidOUTPUT` Identificatore univoco per la pianificazione. *schedule_uid* è una variabile di tipo **uniqueidentifier**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 È possibile gestire le pianificazioni dei processi in modo indipendente dai processi. Per aggiungere una pianificazione a un processo, usare **sp_add_schedule** per creare la pianificazione e **sp_attach_schedule** per alleghiare la pianificazione a un processo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
 
 ## <a name="example"></a>Esempio
 Nell'esempio seguente viene assegnata una pianificazione del processo a `SaturdayReports` che verrà eseguita ogni sabato alle 2:00 AM.
```sql  
EXEC msdb.dbo.sp_add_jobschedule 
        @job_name = N'SaturdayReports', -- Job name
        @name = N'Weekly_Sat_2AM',  -- Schedule name
        @freq_type = 8, -- Weekly
        @freq_interval = 64, -- Saturday
        @freq_recurrence_factor = 1, -- every week
        @active_start_time = 20000 -- 2:00 AM
```
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e alconnessione di pianificazioni ai processi](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [Pianificare un processo](../../ssms/agent/schedule-a-job.md)   
 [Creare una pianificazione](../../ssms/agent/create-a-schedule.md)   
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_update_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
