---
description: sys.spatial_indexes (Transact-SQL)
title: sys.spatial_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.spatial_indexes_TSQL
- spatial_indexes
- spatial_indexes_TSQL
- sys.spatial_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_indexes catalog view
ms.assetid: 40e967d5-2e8d-45af-bf5e-5251493cf7cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 22d70c60c3234e57cff277e1ba298da9f55148ff
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093028"
---
# <a name="sysspatial_indexes-transact-sql"></a>sys.spatial_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rappresenta le principali informazioni per gli indici spaziali.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||Eredita le colonne da [sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).|  
|spatial_index_type|**tinyint**|Tipo di indice spaziale:<br /><br /> 1 = Indice spaziale geometrico<br /><br /> 2 = Indice spaziale geografico|  
|spatial_index_type_desc|**nvarchar(60)**|Descrizione del tipo di indice spaziale:<br /><br /> GEOMETRY = Indice spaziale geometrico<br /><br /> GEOGRAPHY = Indice spaziale geografico|  
|tessellation_scheme|**sysname**|Nome dello schema a mosaico:<br /><br /> GEOMETRY_GRID, GEOMETRY_AUTO_GRID,<br /><br /> GEOGRAPHY_GRID, GEOGRAPHY_AUTO_GRID<br /><br /> Nota: per informazioni sugli schemi a mosaico, vedere [Cenni preliminari sugli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).|  
|\<inherited columns>||Eredita le colonne da [sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).<br /><br /> Le colonne ereditate has_filter e filter_definition vengono visualizzate dopo le colonne specifiche per gli indici spaziali.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Vedere anche  
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_index_tessellations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md)  
  
  
