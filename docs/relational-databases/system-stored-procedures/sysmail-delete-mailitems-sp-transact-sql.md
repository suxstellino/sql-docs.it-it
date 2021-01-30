---
description: sysmail_delete_mailitems_sp (Transact-SQL)
title: sysmail_delete_mailitems_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_mailitems_sp_TSQL
- sysmail_delete_mailitems_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_mailitems_sp
ms.assetid: f87c9f4a-bda1-4bce-84b2-a055a3229ecd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 14e9d1fa942fc41f4865e70460465691e4aef6ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182039"
---
# <a name="sysmail_delete_mailitems_sp-transact-sql"></a>sysmail_delete_mailitems_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina definitivamente messaggi di posta elettronica dalle tabelle interne di Posta elettronica database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_delete_mailitems_sp  [ [ @sent_before = ] 'sent_before' ]  
    [ , [ @sent_status = ] 'sent_status' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ \@sent_before = ] 'sent_before'` Elimina i messaggi di posta elettronica fino alla data e all'ora fornite come argomento *sent_before* . *sent_before* è di tipo **DateTime** con valore predefinito. che indica tutte le date.  
  
`[ \@sent_status = ] 'sent_status'` Elimina i messaggi di posta elettronica del tipo specificato da *sent_status*. *sent_status* è di tipo **varchar (8)** e non prevede alcun valore predefinito. Le voci valide vengono **inviate**, non **inviate**, **riprovate** e **non riuscite**. NULL indica tutti gli stati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Posta elettronica database messaggi e i relativi allegati vengono archiviati nel database **msdb** . I messaggi devono essere eliminati periodicamente per evitare che il **database msdb** cresca più grande del previsto e sia conforme al programma di conservazione dei documenti delle organizzazioni. Utilizzare la stored procedure **sysmail_delete_mailitems_sp** per eliminare definitivamente i messaggi di posta elettronica dalle tabelle posta elettronica database. Un argomento facoltativo consente di eliminare solo i messaggi di posta elettronica meno recenti tramite l'impostazione di una data e un'ora. I messaggi di posta elettronica con una data anteriore a quella specificata nell'argomento verranno eliminati. Un altro argomento facoltativo consente di eliminare solo i messaggi di posta elettronica di un determinato tipo, specificati come argomento **sent_status** . È necessario fornire un argomento per **\@ sent_before** o **\@ sent_status**. Per eliminare tutti i messaggi, usare **\@ sent_before = getdate ()**.  
  
 L'eliminazione di un messaggio di posta elettronica comporta la rimozione degli allegati correlati a tale messaggio, L'eliminazione della posta elettronica non comporta l'eliminazione delle voci corrispondenti in **sysmail_event_log**. Utilizzare [sysmail_delete_log_sp](../../relational-databases/system-stored-procedures/sysmail-delete-log-sp-transact-sql.md) per eliminare elementi dal log.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questo stored procedure viene concesso per l'esecuzione ai membri del ruolo predefinito del server **sysadmin** e **DatabaseMailUserRole**. I membri del ruolo predefinito del server **sysadmin** possono eseguire questa procedura per eliminare i messaggi di posta elettronica inviati da tutti gli utenti. I membri di **DatabaseMailUserRole** possono eliminare solo i messaggi di posta elettronica inviati da tale utente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-deleting-all-e-mails"></a>R. Eliminazione di tutti i messaggi di posta elettronica  
 Nell'esempio seguente vengono eliminati tutti i messaggi di posta elettronica nel sistema di Posta elettronica database.  
  
```  
DECLARE @GETDATE datetime  
SET @GETDATE = GETDATE();  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp @sent_before = @GETDATE;  
GO  
```  
  
### <a name="b-deleting-the-oldest-e-mails"></a>B. Eliminazione dei messaggi di posta elettronica meno recenti  
 Nell'esempio seguente vengono eliminati i messaggi di posta elettronica nel log di Posta elettronica database con una data anteriore a `October 9, 2005`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-e-mails-of-a-certain-type"></a>C. Eliminazione di tutti i messaggi di posta elettronica di un determinato tipo  
 Nell'esempio seguente vengono eliminati dal log di Posta elettronica database tutti i messaggi di posta elettronica che non è stato possibile inviare.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_status = 'failed' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sysmail_allitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_event_log &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_mailattachments &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md)   
 [Creare un processo di SQL Server Agent per l'archiviazione di messaggi e log eventi di Posta elettronica database](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
