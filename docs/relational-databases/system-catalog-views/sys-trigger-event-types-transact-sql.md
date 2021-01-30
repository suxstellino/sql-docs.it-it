---
description: sys.trigger_event_types (Transact-SQL)
title: sys.trigger_event_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trigger_event_types_TSQL
- sys.trigger_event_types_TSQL
- sys.trigger_event_types
- trigger_event_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trigger_event_types catalog view
ms.assetid: 054aed54-7151-4760-934a-149fa434f1ae
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 57a1051371c79d924d1a518155dca1fd3f9a57ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208311"
---
# <a name="systrigger_event_types-transact-sql"></a>sys.trigger_event_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni evento o gruppo di eventi in corrispondenza del quale un trigger può essere attivato.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**type**|**int**|Tipo di evento o gruppo di eventi che attiva il trigger.|  
|**type_name**|**nvarchar (64)**|Nome di un evento o gruppo di eventi. Tale valore può essere specificato nella clausola FOR di un'istruzione [create trigger](../../t-sql/statements/create-trigger-transact-sql.md) .|  
|**parent_type**|**int**|Tipo del gruppo di eventi padre dell'evento o del gruppo di eventi.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
