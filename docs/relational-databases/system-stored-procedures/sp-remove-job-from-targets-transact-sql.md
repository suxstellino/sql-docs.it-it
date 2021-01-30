---
description: sp_remove_job_from_targets (Transact-SQL)
title: sp_remove_job_from_targets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_remove_job_from_targets_TSQL
- sp_remove_job_from_targets
dev_langs:
- TSQL
helpviewer_keywords:
- sp_remove_job_from_targets
ms.assetid: b8171fb1-c11d-4244-8618-a12e28a150ce
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b55575c5e57a72cb5a1d0b3260550cf9e14bc21a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185633"
---
# <a name="sp_remove_job_from_targets-transact-sql"></a>sp_remove_job_from_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove il processo specificato dai server di destinazione o gruppi di server di destinazione specificati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_remove_job_from_targets [ @job_id = ] job_id   
     | [ @job_name = ] 'job_name'   
     [ , [ @target_server_groups = ] 'target_server_groups' ]   
     [ , [ @target_servers = ] 'target_servers' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo da cui rimuovere i server di destinazione o i gruppi di server di destinazione specificati. È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo da cui rimuovere i server di destinazione o i gruppi di server di destinazione specificati. È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @target_server_groups = ] 'target_server_groups'` Elenco delimitato da virgole dei gruppi di server di destinazione da rimuovere dal processo specificato. *target_server_groups* è di **tipo nvarchar (1024)** e il valore predefinito è null.  
  
`[ @target_servers = ] 'target_servers'` Elenco delimitato da virgole dei server di destinazione da rimuovere dal processo specificato. *target_servers* è di **tipo nvarchar (1024)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il processo `Weekly Sales Backups` creato in precedenza viene rimosso dal gruppo di server di destinazione `Servers Processing Customer Orders` e dai server `SEATTLE1` e `SEATTLE2`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_remove_job_from_targets  
    @job_name = N'Weekly Sales Backups',  
    @target_server_groups = N'Servers Processing Customer Orders',   
    @target_servers = N'SEATTLE2,SEATTLE1' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_apply_job_to_targets &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
