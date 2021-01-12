---
description: sysmail_mailattachments (Transact-SQL)
title: sysmail_mailattachments (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fe55316cc27c21849379afe400d2950a5bcf882
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100135"
---
# <a name="sysmail_mailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni allegato inviato a Posta elettronica database. Utilizzare questa vista quando si desidera ottenere informazioni sugli allegati di Posta elettronica database. Per esaminare tutti i messaggi di posta elettronica elaborati da Posta elettronica database usare [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**int**|Identificatore dell'allegato.|  
|**mailitem_id**|**int**|Identificatore dell'elemento di posta contenente l'allegato.|  
|**filename**|**nvarchar (520)**|Nome di file dell'allegato. Quando **attach_query_result** è 1 e **query_attachment_filename** è null, posta elettronica database crea un nome file arbitrario.|  
|**Filesize**|**int**|Dimensioni in byte dell'allegato.|  
|**allegato**|**varbinary(max)**|Contenuto dell'allegato.|  
|**last_mod_date**|**datetime**|Data e ora dell'ultima modifica della riga.|  
|**last_mod_user**|**sysname**|Autore dell'ultima modifica della riga.|  
  
## <a name="remarks"></a>Osservazioni  
 Quando si risolvono i problemi relativi a Posta elettronica database, è possibile utilizzare questa vista per visualizzare le proprietà degli allegati.  
  
 Gli allegati archiviati nelle tabelle di sistema possono causare l'aumento delle dimensioni del database **msdb** . Utilizzare **sysmail_delete_mailitems_sp** per eliminare gli elementi di posta elettronica e gli allegati associati. Per ulteriori informazioni, vedere [la pagina relativa alla creazione di un processo di SQL Server Agent per l'archiviazione di messaggi posta elettronica database e log eventi](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Concesso al ruolo predefinito del server **sysadmin** e al ruolo del database **DatabaseMailUserRole** . Se eseguita da un membro del ruolo predefinito del server **sysadmin** , questa vista Mostra tutti gli allegati. Tutti gli altri utenti vedono semplicemente gli allegati dei messaggi che hanno inviato personalmente.  
  
## <a name="see-also"></a>Vedere anche  
 [sysmail_allitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
