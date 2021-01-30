---
description: sp_apply_job_to_targets (Transact-SQL)
title: sp_apply_job_to_targets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_apply_job_to_targets
- sp_apply_job_to_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_apply_job_to_targets
ms.assetid: 4a3e9173-7e3c-4100-a9ac-2f5d2c60a8b0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9fdc4cffdbe21d1c6c502aa813db55e0444d696
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203241"
---
# <a name="sp_apply_job_to_targets-transact-sql"></a>sp_apply_job_to_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Applica un processo a uno o più server di destinazione o ai server appartenenti a uno o più gruppi di server di destinazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_apply_job_to_targets { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @target_server_groups = ] 'target_server_groups' ]   
     [ , [ @target_servers = ] 'target_servers' ]   
     [ , [ @operation = ] 'operation' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo da applicare ai server di destinazione o ai gruppi di server di destinazione specificati. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo da applicare al server di destinazione o ai gruppi di server di destinazione associati specificati. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @target_server_groups = ] 'target_server_groups'` Elenco delimitato da virgole dei gruppi di server di destinazione a cui deve essere applicato il processo specificato. *target_server_groups* è di **tipo nvarchar (2048)** e il valore predefinito è null.  
  
`[ @target_servers = ] 'target_servers'` Elenco delimitato da virgole dei server di destinazione a cui deve essere applicato il processo specificato. *target_servers* è di **tipo nvarchar (2048)** e il valore predefinito è null.  
  
`[ @operation = ] 'operation'` Indica se il processo specificato deve essere applicato o rimosso dai server di destinazione o dai gruppi di server di destinazione specificati. *Operation* è di tipo **varchar (7)** e il valore predefinito è Apply. Le operazioni valide sono **Apply** e **Remove**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_apply_job_to_targets** fornisce un modo semplice per applicare (o rimuovere) un processo da più server di destinazione e rappresenta un'alternativa alla chiamata di **sp_add_jobserver** (o **sp_delete_jobserver**) una volta per ogni server di destinazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il processo `Backup Customer Information` creato in precedenza viene applicato a tutti i server di destinazione nel gruppo `Servers Maintaining Customer Information`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_apply_job_to_targets  
    @job_name = N'Backup Customer Information',  
    @target_server_groups = N'Servers Maintaining Customer Information',   
    @operation = N'APPLY' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
