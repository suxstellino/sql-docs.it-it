---
description: sp_help_jobsteplog (Transact-SQL)
title: sp_help_jobsteplog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobsteplog_TSQL
- sp_help_jobsteplog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobsteplog
ms.assetid: 1a0be7b1-8f31-4b4c-aadb-586c0e00ed04
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0e5311127f4ff825f862cb3aef2317a2c53578cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200098"
---
# <a name="sp_help_jobsteplog-transact-sql"></a>sp_help_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce i metadati relativi a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] log del passaggio del processo di Agent specifico. **sp_help_jobsteplog** non restituisce il log effettivo.  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]  
     [ , [ @step_name = ] 'step_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] 'job_id'` Numero di identificazione del processo per il quale restituire le informazioni del log dei passaggi di processo. *job_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @step_id = ] step_id` Numero di identificazione del passaggio nel processo. Se viene omesso, vengono inclusi tutti i passaggi del processo. *step_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @step_name = ] 'step_name'` Nome del passaggio nel processo. *step_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID univoco del processo.|  
|**job_name**|**sysname**|Nome del processo.|  
|**step_id**|**int**|ID del passaggio all'interno del processo. Se, ad esempio, il passaggio è il primo passaggio del processo, il *step_id* è 1.|  
|**step_name**|**sysname**|Nome del passaggio del processo.|  
|**step_uid**|**uniqueidentifier**|ID univoco generato dal sistema del passaggio nel processo.|  
|**date_created**|**datetime**|Data di creazione del passaggio.|  
|**date_modified**|**datetime**|Data dell'ultima modifica del passaggio.|  
|**log_size**|**float**|Dimensioni in megabyte (MB) del log dei passaggi del processo.|  
|**log**|**nvarchar(max)**|Output del log dei passaggi del processo.|  
  
## <a name="remarks"></a>Commenti  
 **sp_help_jobsteplog** si trova nel database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri di **SQLAgentUserRole** possono visualizzare solo i metadati del log dei passaggi di processo per i passaggi di processo di cui sono proprietari.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returns-job-step-log-information-for-all-steps-in-a-specific-job"></a>R. Restituzione delle informazioni del log su tutti i passaggi di un processo specifico  
 Nell'esempio seguente vengono restituite le informazioni del log su tutti i passaggi di un processo denominato `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-job-step-log-information-about-a-specific-job-step"></a>B. Restituzione delle informazioni del log su un passaggio specifico  
 Nell'esempio seguente vengono restituite le informazioni del log relative al primo passaggio del processo denominato `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_delete_jobsteplog &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)   
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
