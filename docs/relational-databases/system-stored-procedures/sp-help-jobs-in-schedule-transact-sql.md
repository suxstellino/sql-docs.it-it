---
description: sp_help_jobs_in_schedule (Transact-SQL)
title: sp_help_jobs_in_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobs_in_schedule_TSQL
- sp_help_jobs_in_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobs_in_schedule
ms.assetid: 1168aa2c-136b-4ba3-b18e-9070d95a26fa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1890fc4b8aa3757b55dfc5ad7e33070126300897
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200143"
---
# <a name="sp_help_jobs_in_schedule-transact-sql"></a>sp_help_jobs_in_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni sui processi ai quali è associata una pianificazione specifica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobs_in_schedule   
     [ @schedule_name = ] 'schedule_name' ,  
     [ @schedule_id = ] schedule_id   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @schedule_id = ] schedule_id` Identificatore della pianificazione per cui elencare le informazioni. *schedule_id* è di **tipo int** e non prevede alcun valore predefinito. È possibile specificare *schedule_id* o *schedule_name* .  
  
`[ @schedule_name = ] 'schedule_name'` Nome della pianificazione per cui elencare le informazioni. *schedule_name* è di **tipo sysname** e non prevede alcun valore predefinito. È possibile specificare *schedule_id* o *schedule_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Restituisce il set di risultati seguente:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID univoco del processo.|  
|**originating_server**|**nvarchar(30)**|Nome del server di provenienza del processo.|  
|**nome**|**sysname**|Nome del processo.|  
|**abilitato**|**tinyint**|Indica se il processo è abilitato per l'esecuzione.|  
|**description**|**nvarchar(512)**|Descrizione del processo.|  
|**start_step_id**|**int**|ID del passaggio del processo da cui deve iniziare l'esecuzione.|  
|**category**|**sysname**|Categoria del processo.|  
|**proprietario**|**sysname**|Proprietario del processo.|  
|**notify_level_eventlog**|**int**|Maschera di bit che indica le condizioni per la registrazione dell'evento di notifica nel registro applicazioni di Microsoft Windows. I possibili valori sono i seguenti:<br /><br /> **0** = mai<br /><br /> **1** = in caso di esito positivo di un processo<br /><br /> **2** = quando il processo ha esito negativo<br /><br /> **3** = ogni volta che il processo viene completato (indipendentemente dal risultato del processo)|  
|**notify_level_email**|**int**|Maschera di bit che indica le condizioni per l'invio di un messaggio di posta elettronica di notifica al termine di un processo. I valori possibili sono identici a quelli per **notify_level_eventlog**.|  
|**notify_level_netsend**|**int**|Maschera di bit che indica le condizioni per l'invio di un messaggio in rete al termine di un processo. I valori possibili sono identici a quelli per **notify_level_eventlog**.|  
|**notify_level_page**|**int**|Maschera di bit che indica le condizioni per l'invio di un messaggio su cercapersone al termine di un processo. I valori possibili sono identici a quelli per **notify_level_eventlog**.|  
|**notify_email_operator**|**sysname**|Nome di posta elettronica dell'operatore a cui inviare la notifica.|  
|**notify_netsend_operator**|**sysname**|Nome di computer o di utente specificato quando si invia un messaggio in rete.|  
|**notify_page_operator**|**sysname**|Nome di computer o di utente specificato quando si invia un messaggio su cercapersone.|  
|**delete_level**|**int**|Maschera di bit che indica le condizioni per l'eliminazione di un processo al termine del processo. I valori possibili sono identici a quelli per **notify_level_eventlog**.|  
|**date_created**|**datetime**|Data di creazione del processo.|  
|**date_modified**|**datetime**|Data dell'ultima modifica del processo.|  
|**version_number**|**int**|Versione del processo aggiornata automaticamente in corrispondenza di ogni modifica del processo.|  
|**last_run_date**|**int**|Data dell'ultimo avvio dell'esecuzione del processo.|  
|**last_run_time**|**int**|Ora dell'ultimo avvio dell'esecuzione del processo.|  
|**last_run_outcome**|**int**|Risultato dell'ultima esecuzione del processo:<br /><br /> **0** = non riuscito<br /><br /> **1** = operazione completata<br /><br /> **3** = annullato<br /><br /> **5** = sconosciuto|  
|**next_run_date**|**int**|Data pianificata per la successiva esecuzione del processo.|  
|**next_run_time**|**int**|Ora pianificata per la successiva esecuzione del processo.|  
|**next_run_schedule_id**|**int**|ID della successiva pianificazione di esecuzione.|  
|**current_execution_status**|**int**|Stato di esecuzione corrente.|  
|**current_execution_step**|**sysname**|Passaggio di esecuzione corrente all'interno del processo.|  
|**current_retry_attempt**|**int**|Se il processo è in esecuzione e il passaggio è stato ripetuto, indica il numero del tentativo in corso.|  
|**has_step**|**int**|Numero di passaggi del processo.|  
|**has_schedule**|**int**|Numero di pianificazioni del processo.|  
|**has_target**|**int**|Numero di server di destinazione del processo.|  
|**type**|**int**|Tipo del processo:<br /><br /> **1** = processo locale.<br /><br /> **2** = processo multiserver.<br /><br /> **0** = il processo non dispone di server di destinazione.|  
  
## <a name="remarks"></a>Commenti  
 Questa stored procedure visualizza le informazioni sui processi associati alla pianificazione specificata.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri di **SQLAgentUserRole** possono solo visualizzare lo stato dei processi di cui sono proprietari.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono visualizzati i processi associati alla pianificazione `NightlyJobs`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_jobs_in_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
