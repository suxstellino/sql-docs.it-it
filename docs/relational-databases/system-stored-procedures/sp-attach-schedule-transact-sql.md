---
description: sp_attach_schedule (Transact-SQL)
title: sp_attach_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_schedule_TSQL
- sp_attach_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_schedule
ms.assetid: 80c80eaf-cf23-4ed8-b8dd-65fe59830dd1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 050436b47193c13fe36214d45bb6bd12749a5eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199234"
---
# <a name="sp_attach_schedule-transact-sql"></a>sp_attach_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta una pianificazione per un processo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_attach_schedule  
     { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,   
     { [ @schedule_id = ] schedule_id   
     | [ @schedule_name = ] 'schedule_name' }  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo a cui viene aggiunta la pianificazione. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo a cui viene aggiunta la pianificazione. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @schedule_id = ] schedule_id` Numero di identificazione della pianificazione da impostare per il processo. *schedule_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @schedule_name = ] 'schedule_name'` Nome della pianificazione da impostare per il processo. *schedule_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *schedule_id* o *schedule_name* , ma non è possibile specificarli entrambi.  
  
## <a name="remarks"></a>Commenti  
 La pianificazione e il processo devono avere lo stesso proprietario.  
  
 È possibile impostare una pianificazione per più di un processo. È possibile eseguire un processo su più di una pianificazione.  
  
 Questo stored procedure deve essere eseguito dal database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Notare che il proprietario del processo può collegare un processo a una pianificazione e può scollegare un processo da una pianificazione senza dovere essere anche il proprietario della pianificazione. Tuttavia, non è possibile eliminare una pianificazione se lo scollegamento la lascia senza processi, a meno che il chiamante sia il proprietario della pianificazione.  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verifica se l'utente è proprietario sia del processo che della pianificazione.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creata una pianificazione denominata `NightlyJobs`. I processi che utilizzano questa pianificazione vengono eseguiti ogni giorno quando l'ora indicata dal server è `01:00`. Nell'esempio la pianificazione viene collegata al processo `BackupDatabase` e al processo `RunReports`.  
  
> [!NOTE]  
>  In questo esempio si presuppone che il processo `BackupDatabase` e il processo `RunReports` esistano già.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_add_schedule  
    @schedule_name = N'NightlyJobs' ,  
    @freq_type = 4,  
    @freq_interval = 1,  
    @active_start_time = 010000 ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'BackupDatabase',  
   @schedule_name = N'NightlyJobs' ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'RunReports',  
   @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)  
  
  
