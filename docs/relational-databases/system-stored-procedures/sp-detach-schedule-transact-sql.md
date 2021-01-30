---
description: sp_detach_schedule (Transact-SQL)
title: sp_detach_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_detach_schedule
- sp_detach_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_schedule
ms.assetid: 9a1fc335-1bef-4638-a33a-771c54a5dd19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 513287fadf671cc645ffa210e96f56615e237983
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200752"
---
# <a name="sp_detach_schedule-transact-sql"></a>sp_detach_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un'associazione tra una pianificazione e un processo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_detach_schedule   
     { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,  
       { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @delete_unused_schedule = ] delete_unused_schedule   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo da cui rimuovere la pianificazione. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo da cui rimuovere la pianificazione. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @schedule_id = ] schedule_id` Numero di identificazione della pianificazione da rimuovere dal processo. *schedule_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @schedule_name = ] 'schedule_name'` Nome della pianificazione da rimuovere dal processo. *schedule_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *schedule_id* o *schedule_name* , ma non è possibile specificarli entrambi.  
  
`[ @delete_unused_schedule = ] delete_unused_schedule` Specifica se eliminare le pianificazioni di processi non utilizzate. *delete_unused_schedule* è di **bit** e il valore predefinito è **0**, che indica che verranno mantenute tutte le pianificazioni, anche se non vi sono processi che vi fanno riferimento. Se impostato su **1**, le pianificazioni dei processi non utilizzate vengono eliminate se non vi sono processi che vi fanno riferimento.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Notare che il proprietario del processo può collegare un processo a una pianificazione e può scollegare un processo da una pianificazione senza dovere essere anche il proprietario della pianificazione. Tuttavia, non è possibile eliminare una pianificazione se lo scollegamento la lascia senza processi, a meno che il chiamante sia il proprietario della pianificazione.  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono eseguite verifiche per determinare se l'utente è proprietario della pianificazione. Solo i membri del ruolo predefinito del server **sysadmin** possono scollegare le pianificazioni dai processi di proprietà di un altro utente.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene rimossa un'associazione tra una pianificazione `'NightlyJobs'` e un processo `'BackupDatabase'`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_detach_schedule  
    @job_name = 'BackupDatabase',  
    @schedule_name = 'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)  
  
  
