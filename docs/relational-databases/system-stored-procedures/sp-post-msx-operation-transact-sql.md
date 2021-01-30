---
description: sp_post_msx_operation (Transact-SQL)
title: sp_post_msx_operation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_post_msx_operation
- sp_post_msx_operation_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_post_msx_operation
ms.assetid: 085deef8-2709-4da9-bb97-9ab32effdacf
author: markingmyname
ms.author: maghan
ms.openlocfilehash: be9ef622f686044800f634837819b4cba92821a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196033"
---
# <a name="sp_post_msx_operation-transact-sql"></a>sp_post_msx_operation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inserisce le operazioni (righe) nella tabella di sistema **sysdownloadlist** per il download e l'esecuzione dei server di destinazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_post_msx_operation  
     [ @operation = ] 'operation'  
     [ , [ @object_type = ] 'object' ]   
     { , [ @job_id = ] job_id }   
     [ , [ @specific_target_server = ] 'target_server' ]   
     [ , [ @value = ] value ]  
     [ , [ @schedule_uid = ] schedule_uid ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @operation = ] 'operation'` Tipo di operazione per l'operazione pubblicata. *Operation* è di tipo **varchar (64)** e non prevede alcun valore predefinito. Le operazioni valide dipendono dalla *object_type*.  
  
|Tipo di oggetto|Operazione|  
|-----------------|---------------|  
|**PROCESSO**|INSERT<br /><br /> UPDATE<br /><br /> DELETE<br /><br /> START<br /><br /> STOP|  
|**SERVER**|RE-ENLIST<br /><br /> DEFECT<br /><br /> SYNC-TIME<br /><br /> SET-POLL|  
|**PIANIFICAZIONE**|INSERT<br /><br /> UPDATE<br /><br /> DELETE|  
  
`[ @object_type = ] 'object'` Tipo di oggetto per il quale pubblicare un'operazione. I tipi validi sono **Job**, **Server** e **Schedule**. l' *oggetto* è di tipo **varchar (64)** e il valore predefinito è **Job**.  
  
`[ @job_id = ] job_id` Numero di identificazione del processo a cui si applica l'operazione. *job_id* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito. **0x00** indica tutti i processi. Se l' *oggetto* è **Server**, *job_id* non è obbligatorio.  
  
`[ @specific_target_server = ] 'target_server'` Nome del server di destinazione per cui si applica l'operazione specificata. Se *job_id* viene specificato, ma non viene specificato *target_server* , le operazioni vengono inviate per tutti i server di processo del processo. *target_server* è di **tipo nvarchar (30)** e il valore predefinito è null.  
  
`[ @value = ] value` Intervallo di polling, in secondi. *value* è **int** e il valore predefinito è NULL. Specificare questo parametro solo se *Operation* è **set-poll**.  
  
`[ @schedule_uid = ] schedule_uid` Identificatore univoco per la pianificazione a cui si applica l'operazione. *schedule_uid* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_post_msx_operation** deve essere eseguito dal database **msdb** .  
  
 **sp_post_msx_operation** può essere sempre chiamato in modo sicuro perché determina innanzitutto se il server corrente è un agente di Microsoft SQL Server multiserver e, in caso affermativo, se l' *oggetto* è un processo multiserver.  
  
 Una volta inviata, un'operazione viene visualizzata nella tabella **sysdownloadlist** . Dopo la creazione e l'inserimento di un processo, è necessario comunicare ai server di destinazione (TSX) tutte le successive modifiche apportate al processo. A tale scopo è possibile utilizzare l'elenco di download.  
  
 È consigliabile gestire l'elenco di download in SQL Server Management Studio. Per altre informazioni, vedere [visualizzare o modificare i processi](../../ssms/agent/view-or-modify-jobs.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, è necessario che agli utenti venga concesso il ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_delete_targetserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-targetserver-transact-sql.md)   
 [sp_resync_targetserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md)   
 [sp_start_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_stop_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-stop-job-transact-sql.md)   
 [sp_update_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [sp_update_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
