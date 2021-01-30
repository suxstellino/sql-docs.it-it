---
description: sp_delete_jobstep (Transact-SQL)
title: sp_delete_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobstep
- sp_delete_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobstep
ms.assetid: 421ede8e-ad57-474a-9fb9-92f70a3e77e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b795b6526941518e476390dba158f56e18040da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186336"
---
# <a name="sp_delete_jobstep-transact-sql"></a>sp_delete_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un passaggio di un processo.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_jobstep { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,   
     [ @step_id = ] step_id   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo da cui verrà rimosso il passaggio. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo da cui verrà rimosso il passaggio. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> **Nota:** È necessario specificare *job_id* o *job_name* . non è possibile specificare entrambi.  
  
`[ @step_id = ] step_id` Numero di identificazione del passaggio da rimuovere. *step_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Se si rimuove un passaggio di un processo, gli altri passaggi del processo che fanno riferimento al passaggio eliminato vengono aggiornati automaticamente.  
  
 Per ulteriori informazioni sui passaggi associati a un determinato processo, eseguire **sp_help_jobstep**.  
  
> **Nota:** La chiamata di **sp_delete_jobstep** con un valore *step_id* zero Elimina tutti i passaggi del processo.  
  
 Microsoft SQL Server Management Studio include uno strumento grafico di facile utilizzo per la gestione dei processi, ed è lo strumento consigliato per la creazione e la gestione dell'infrastruttura dei processi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri di **sysadmin** possono eliminare un passaggio di processo di proprietà di un altro utente.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il passaggio di processo `1` viene rimosso dal processo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare o modificare i processi](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_update_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
