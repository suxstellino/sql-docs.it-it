---
description: sp_delete_jobsteplog (Transact-SQL)
title: sp_delete_jobsteplog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobsteplog
- sp_delete_jobsteplog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobsteplog
ms.assetid: e9ef4c99-abde-4038-b6a3-a25dcbaf0958
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17cdcb1ba86e214ce707ff084a0d921b3817cbe0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211144"
---
# <a name="sp_delete_jobsteplog-transact-sql"></a>sp_delete_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove tutti i log dei passaggi di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent specificati con gli argomenti. Utilizzare questo stored procedure per gestire la tabella **nella sysjobstepslogs** nel database **msdb** .  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
       [ , [ @step_id = ] step_id | [ @step_name = ] 'step_name' ]  
       [ , [ @older_than = ] 'date' ]  
       [ , [ @larger_than = ] 'size_in_bytes' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] 'job_id'` Numero di identificazione del processo che contiene il log dei passaggi del processo da rimuovere. *job_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> **Nota:** È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @step_id = ] step_id` Numero di identificazione del passaggio del processo per il quale è necessario eliminare il log dei passaggi del processo. Se non è incluso, tutti i log dei passaggi di processo nel processo vengono eliminati a meno che non vengano specificati **\@ older_than** o **\@ larger_than** . *step_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @step_name = ] 'step_name'` Nome del passaggio del processo per cui deve essere eliminato il log dei passaggi del processo. *step_name* è di **tipo sysname** e il valore predefinito è null.  
  
> **Nota:** È possibile specificare *step_id* o *step_name* , ma non è possibile specificarli entrambi.  
  
`[ @older_than = ] 'date'` Data e ora del log del passaggio di processo meno recente che si desidera memorizzare. Verranno rimossi tutti i log dei passaggi di processo antecedenti questa data e ora. *date* è di tipo **DateTime** e il valore predefinito è null. È possibile specificare sia **\@ older_than** che **\@ larger_than** .  
  
`[ @larger_than = ] 'size_in_bytes'` Dimensioni in byte del log dei passaggi di processo più grande che si desidera memorizzare. Vengono rimossi tutti i log dei passaggi di processo la cui dimensione è maggiore rispetto a quella indicata. È possibile specificare sia **\@ larger_than** che **\@ older_than** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_delete_jobsteplog** si trova nel database **msdb** .  
  
 Se non viene specificato alcun argomento eccetto **\@ job_id** o **\@ job_name** , vengono eliminati tutti i log dei passaggi di processo per il processo specificato.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri di **sysadmin** possono eliminare un log dei passaggi di processo di proprietà di un altro utente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-removing-all-job-step-logs-from-a-job"></a>R. Rimozione di tutti i log dei passaggi di processo da un processo  
 Nell'esempio seguente vengono rimossi tutti i log dei passaggi di processo del processo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup';  
GO  
```  
  
### <a name="b-removing-the-job-step-log-for-a-particular-job-step"></a>B. Rimozione del log dei passaggi di processo per un passaggio di processo specifico  
 Nell'esempio seguente viene rimosso il log relativo al passaggio 2 del processo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 2;  
GO  
```  
  
### <a name="c-removing-all-job-step-logs-based-on-age-and-size"></a>C. Rimozione di tutti i log dei passaggi di processo in base alla data di creazione e alle dimensioni  
 Nell'esempio seguente tutti i log dei passaggi di processo antecedenti le ore 12.00 del 25 ottobre 2005 e di dimensioni maggiori di 100 MB vengono rimossi dal processo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @older_than = '10/25/2005 12:00:00',  
    @larger_than = 104857600;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_help_jobsteplog &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
