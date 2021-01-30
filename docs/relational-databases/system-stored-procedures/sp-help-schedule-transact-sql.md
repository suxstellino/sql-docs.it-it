---
description: sp_help_schedule (Transact-SQL)
title: sp_help_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_schedule
- sp_help_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_schedule
ms.assetid: b2fc4ce1-0a8e-44d2-b206-7dc7b258d8c9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 24a0d594e02fad844d651933bab8d89bc9d35bb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193532"
---
# <a name="sp_help_schedule-transact-sql"></a>sp_help_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco di informazioni relative alle pianificazioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_schedule   
     [ @schedule_id = ] id ,  
     [ @schedule_name = ] 'schedule_name'   
     [ , [ @attached_schedules_only = ] attached_schedules_only ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @schedule_id = ] id` Identificatore della pianificazione da elencare. *schedule_name* è di **tipo int** e non prevede alcun valore predefinito. È possibile specificare *schedule_id* o *schedule_name* .  
  
`[ @schedule_name = ] 'schedule_name'` Nome della pianificazione da elencare. *schedule_name* è di **tipo sysname** e non prevede alcun valore predefinito. È possibile specificare *schedule_id* o *schedule_name* .  
  
`[ @attached_schedules_only = ] attached_schedules_only ]` Specifica se visualizzare solo le pianificazioni a cui è associato un processo. *attached_schedules_only* è di **bit** e il valore predefinito è **0**. Quando *attached_schedules_only* è **0**, vengono visualizzate tutte le pianificazioni. Quando *attached_schedules_only* è **1**, il set di risultati contiene solo le pianificazioni associate a un processo.  
  
`[ @include_description = ] include_description` Specifica se includere le descrizioni nel set di risultati. *include_description* è di **bit** e il valore predefinito è **0**. Quando *include_description* è **0**, la colonna *schedule_description* del set di risultati contiene un segnaposto. Quando *include_description* è **1**, la descrizione della pianificazione viene inclusa nel set di risultati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Questa procedura restituisce il set di risultati seguente:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Numero di identificazione della pianificazione.|  
|**schedule_uid**|**uniqueidentifier**|Identificatore della pianificazione.|  
|**schedule_name**|**sysname**|Nome della pianificazione.|  
|**abilitato**|**int**|Indica se la pianificazione è abilitata (**1**) o non abilitata (**0**).|  
|**freq_type**|**int**|Valore che indica la frequenza di esecuzione del processo:<br /><br /> **1** = una volta<br /><br /> **4** = giornaliero<br /><br /> **8** = settimanale<br /><br /> **16** = mensile<br /><br /> **32** = mensile rispetto al **freq_interval**<br /><br /> **64** = esecuzione all'avvio del servizio SQLServerAgent.|  
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
|**schedule_description**|**nvarchar(4000)**|Descrizione in inglese della pianificazione, se richiesta.|  
|**job_count**|**int**|Restituisce il numero di processi che fanno riferimento a questa pianificazione.|  
  
## <a name="remarks"></a>Commenti  
 Se non vengono specificati parametri, **sp_help_schedule** elenca le informazioni per tutte le pianificazioni nell'istanza di.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri di **SQLAgentUserRole** possono visualizzare solo le pianificazioni di cui sono proprietari.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-information-for-all-schedules-in-the-instance"></a>R. Visualizzazione di un elenco di informazioni per tutte le pianificazioni nell'istanza  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per tutte le pianificazioni nell'istanza.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_schedule ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-schedule"></a>B. Visualizzazione di un elenco di informazioni per una pianificazione specifica  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per la pianificazione denominata `NightlyJobs`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
