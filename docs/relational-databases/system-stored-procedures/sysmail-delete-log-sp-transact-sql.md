---
description: sysmail_delete_log_sp (Transact-SQL)
title: sysmail_delete_log_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_log_sp_TSQL
- sysmail_delete_log_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_log_sp
ms.assetid: e94b37a1-70ad-46a5-86c0-721892156f7c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f3921d220f367d15849eab080369db986d8f53e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160013"
---
# <a name="sysmail_delete_log_sp-transact-sql"></a>sysmail_delete_log_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina eventi dal log di Posta elettronica database. È possibile eliminare tutti gli eventi nel log oppure solo quelli che soddisfano un particolare criterio relativo alla data o al tipo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_delete_log_sp  [ [ @logged_before = ] 'logged_before' ]  
    [, [ @event_type = ] 'event_type' ]  
  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @logged_before = ] 'logged_before'` Elimina le voci fino alla data e all'ora specificate dall'argomento *logged_before* . *logged_before* è di tipo **DateTime** con valore predefinito. che indica tutte le date.  
  
`[ @event_type = ] 'event_type'` Elimina le voci di log del tipo specificato come *event_type*. *event_type* è di tipo **varchar (15)** e non prevede alcun valore predefinito. Le voci valide sono **esito positivo**, **avviso**, **errore** e **informativo**. NULL indica tutti i tipi di eventi.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Usare il stored procedure **sysmail_delete_log_sp** per eliminare definitivamente le voci dal log di posta elettronica database. Un argomento facoltativo consente di eliminare solo i record meno recenti tramite l'impostazione di una data e un'ora. Gli eventi con una data anteriore a quella specificata nell'argomento verranno eliminati. Un argomento facoltativo consente di eliminare solo gli eventi di un determinato tipo, specificati come argomento **event_type** .  
  
 L'eliminazione delle voci dal log di Posta elettronica database non comporta la rimozione dei messaggi di posta elettronica dalle tabelle di Posta elettronica database. Utilizzare [sysmail_delete_mailitems_sp](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) per eliminare la posta elettronica dalle tabelle posta elettronica database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono accedere a questa procedura.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-deleting-all-events"></a>R. Eliminazione di tutti gli eventi  
 Nell'esempio seguente vengono eliminati tutti gli eventi nel log di Posta elettronica database.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp ;  
GO  
```  
  
### <a name="b-deleting-the-oldest-events"></a>B. Eliminazione degli eventi meno recenti  
 Nell'esempio seguente vengono eliminati gli eventi nel log di Posta elettronica database con una data anteriore al 9 ottobre 2005.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @logged_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-events-of-a-certain-type"></a>C. Eliminazione di tutti gli eventi di un determinato tipo  
 Nell'esempio seguente vengono eliminati i messaggi di operazione riuscita nel log di Posta elettronica database.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @event_type = 'success' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sysmail_event_log &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_delete_mailitems_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md)   
 [Creare un processo di SQL Server Agent per l'archiviazione di messaggi e log eventi di Posta elettronica database](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
