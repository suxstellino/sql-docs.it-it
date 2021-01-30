---
description: sp_delete_schedule (Transact-SQL)
title: sp_delete_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_schedule
- sp_delete_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_schedule
ms.assetid: 18b2c985-47b8-49c8-82d1-8a4af3d7d33a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b152260dca3ab9791124b9f7f1ac13e311f6e514
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199155"
---
# <a name="sp_delete_schedule-transact-sql"></a>sp_delete_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una pianificazione.  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_schedule { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @force_delete = ] force_delete  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @schedule_id = ] schedule_id` Numero di identificazione della pianificazione da eliminare. *schedule_id* è di **tipo int** e il valore predefinito è null.  
  
> **Nota:** È necessario specificare *schedule_id* o *schedule_name* , ma non è possibile specificarli entrambi.  
  
`[ @schedule_name = ] 'schedule_name'` Nome della pianificazione da eliminare. *schedule_name* è di **tipo sysname** e il valore predefinito è null.  
  
> **Nota:** È necessario specificare *schedule_id* o *schedule_name* , ma non è possibile specificarli entrambi.  
  
`[ @force_delete = ] force_delete` Specifica se la stored procedure deve avere esito negativo se la pianificazione è associata a un processo. *Force_delete* è di bit e il valore predefinito è **0**. Quando *force_delete* è **0**, l'stored procedure ha esito negativo se la pianificazione è associata a un processo. Quando *force_delete* è **1**, la pianificazione viene eliminata indipendentemente dal fatto che la pianificazione sia collegata a un processo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Per impostazione predefinita, una pianificazione non può essere eliminata se è associata a un processo. Per eliminare una pianificazione collegata a un processo, specificare il valore **1** per *force_delete*. L'eliminazione di una pianificazione non comporta l'arresto dei processi in esecuzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Notare che il proprietario del processo può collegare un processo a una pianificazione e può scollegare un processo da una pianificazione senza dovere essere anche il proprietario della pianificazione. Tuttavia, non è possibile eliminare una pianificazione se lo scollegamento la lascia senza processi, a meno che il chiamante sia il proprietario della pianificazione.  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri del ruolo **sysadmin** possono eliminare la pianificazione di un processo di proprietà di un altro utente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-deleting-a-schedule"></a>R. Eliminazione di una pianificazione  
 Nell'esempio seguente viene eliminata la pianificazione `NightlyJobs`. Se è associata a un processo qualsiasi, la pianificazione non verrà eliminata.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="b-deleting-a-schedule-attached-to-a-job"></a>B. Eliminazione di una pianificazione associata a un processo  
 Nell'esempio seguente viene eliminata la pianificazione `RunOnce` anche se è associata a un processo.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = 'RunOnce',  
    @force_delete = 1;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Implementazione di processi](../../ssms/agent/implement-jobs.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
  
