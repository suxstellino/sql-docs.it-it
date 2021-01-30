---
description: sys.event_notifications (Transact-SQL)
title: sys.event_notifications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- event_notifications_TSQL
- event_notifications
- sys.event_notifications_TSQL
- sys.event_notifications
dev_langs:
- TSQL
helpviewer_keywords:
- sys.event_notifications catalog view
ms.assetid: 136a76ee-2b35-4418-ab46-fda2d51f7d99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 85f93f5284b8673188d6c6dcb486d80a65d3ccec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203871"
---
# <a name="sysevent_notifications-transact-sql"></a>sys.event_notifications (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni oggetto che rappresenta una notifica degli eventi, con **sys. Objects. Type** = en.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome della notifica degli eventi.|  
|**object_id**|**int**|Numero di identificazione dell'oggetto. Valore univoco all'interno di un database.|  
|**parent_class**|**tinyint**|Classe padre.<br /><br /> 0 = Database<br /><br /> 1 = Oggetto o colonna|  
|**parent_class_desc**|**nvarchar(60)**|DATABASE<br /><br /> OBJECT_OR_COLUMN|  
|**parent_id**|**int**|ID diverso da zero dell'oggetto padre.<br /><br /> 0 = Il database rappresenta la classe padre.|  
|**create_date**|**datetime**|Data di creazione.|  
|**modify_date**|**datetime**|È sempre uguale **create_date**.|  
|**service_name**|**nvarchar(256)**|Nome del servizio di destinazione a cui viene inviata la notifica.|  
|**broker_instance**|**nvarchar(128)**|Istanza di Service Broker a cui viene inviata la notifica.|  
|**principal_id**|**int**|ID dell'entità di database proprietaria della notifica degli eventi.|  
|**creator_sid**|**varbinary(85)**|SID dell'account di accesso che ha creato la notifica degli eventi.<br /><br /> È NULL se non si specifica l'opzione FAN_IN.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
