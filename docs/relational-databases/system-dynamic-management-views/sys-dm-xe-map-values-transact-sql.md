---
description: sys.dm_xe_map_values (Transact-SQL)
title: sys.dm_xe_map_values (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_xe_map_values
- dm_xe_map_values
- dm_xe_map_values_TSQL
- sys.dm_xe_map_values_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_map_values dynamic management view
- xe
ms.assetid: c0c5dd7e-9cee-47e2-b65a-88194c00aa1f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10633e55f26d8399c8899061fd1aa04617b05ebf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99130043"
---
# <a name="sysdm_xe_map_values-transact-sql"></a>sys.dm_xe_map_values (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un mapping di chiavi numeriche interne in un testo leggibile.  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Nome della mappa. il nome è univoco nel sistema locale. Non ammette i valori Null.|  
|object_package_guid|**uniqueidentifier**|GUID del pacchetto che contiene la mappa. Non ammette i valori Null.|  
|map_key|**int**|Valore della chiave interna. Non ammette i valori Null.|  
|map_value|**nvarchar (3072)**|Descrizione del valore della chiave. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
### <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|dm_xe_map_values.object_package_guid<br /><br /> dm_xe_map_values.name|sys.dm_xe_objects.package_guid<br /><br /> sys.dm_xe_objects.name|Molti-a-uno| 
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

