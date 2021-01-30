---
description: sp_help_jobserver (Transact-SQL)
title: sp_help_jobserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobserver
- sp_help_jobserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobserver
ms.assetid: 57971787-f9f5-4199-9f64-c2b61a308906
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4c5e5f8a4c7ec027c72452ea725ec6def715309a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200122"
---
# <a name="sp_help_jobserver-transact-sql"></a>sp_help_jobserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sul server per un determinato processo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobserver  
     { [ @job_id = ] job_id   
     | [ @job_name = ] 'job_name' }  
     [ , [ @show_last_run_details = ] show_last_run_details ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo per cui restituire informazioni. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo per cui restituire informazioni. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @show_last_run_details = ] show_last_run_details` Indica se le informazioni sull'esecuzione dell'ultima esecuzione fanno parte del set di risultati. *show_last_run_details* è di **tinyint** e il valore predefinito è **0**. **0** non include informazioni sull'ultima esecuzione e **1** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Numero di identificazione del server di destinazione.|  
|**server_name**|**nvarchar(30)**|Nome di computer del server di destinazione.|  
|**enlist_date**|**datetime**|Data di integrazione del server di destinazione nel server master.|  
|**last_poll_date**|**datetime**|Data dell'ultimo polling del server master eseguito dal server di destinazione.|  
  
 Se **sp_help_jobserver** viene eseguita con *show_last_run_details* impostato su **1**, il set di risultati include le colonne aggiuntive.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**last_run_date**|**int**|Data di inizio dell'ultima esecuzione del processo nel server di destinazione.|  
|**last_run_time**|**int**|Ora di inizio dell'ultima esecuzione del processo nel server corrente|  
|**last_run_duration**|**int**|Durata in secondi dell'ultima esecuzione del processo nel server di destinazione corrente.|  
|**last_outcome_message**|**nvarchar(1024)**|Descrive l'ultimo risultato del processo.|  
|**last_run_outcome**|**int**|Risultato dell'ultima esecuzione del processo nel server specificato:<br /><br /> **0** = non riuscito<br /><br /> **1** = operazione completata<br /><br /> **3** = annullato<br /><br /> **5** = sconosciuto|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri di **SQLAgentUserRole** possono visualizzare solo le informazioni per i processi di cui sono proprietari.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sul processo `NightlyBackups`, comprese le informazioni relative all'ultima esecuzione.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobserver  
    @job_name = N'NightlyBackups',  
    @show_last_run_details = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
