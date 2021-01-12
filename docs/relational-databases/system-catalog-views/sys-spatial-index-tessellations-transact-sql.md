---
description: sys.spatial_index_tessellations (Transact-SQL)
title: sys.spatial_index_tessellations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ddf7b9cd75999da6cc48f6e7a83bab7a733f822b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095419"
---
# <a name="sysspatial_index_tessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Rappresenta le informazioni sullo schema a mosaico e i parametri di ognuno degli indici spaziali.  
  
> [!NOTE]  
>  Per informazioni sull'implementazione dello schema a mosaico, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID dell'oggetto in cui è definito l'indice. Ogni coppia (object_id, index_id) ha una voce corrispondente in [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md).|  
|index_id|**int**|ID dell'indice spaziale in cui è definita la colonna indicizzata.|  
|tessellation_scheme|**sysname**|Nome dello schema a mosaico, uno di: GEOMETRY_GRID, GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|Coordinata x dell'angolo inferiore sinistro del rettangolo di delimitazione, uno di: NULL = non applicabile per uno schema a mosaico specificato (ad esempio GEOGRAPHY_GRID) *n* = Se tessellation_scheme è GEOMETRY_GRID, il valore della coordinata x-min.                     **Nota:** Le coordinate definite dai parametri del rettangolo di delimitazione vengono interpretate per ogni oggetto in base al relativo [identificatore SRID](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).|  
|bounding_box_ymin|**float(53)**|Coordinata y dell'angolo inferiore sinistro del rettangolo di delimitazione, uno di: NULL = non applicabile per uno schema a mosaico specificato (ad esempio GEOGRAPHY_GRID) *n* = Se tessellation_scheme è GEOMETRY_GRID, il valore della coordinata y-min|  
|bounding_box_xmax|**float(53)**|Coordinata x dell'angolo superiore destro del rettangolo di delimitazione, uno di: NULL = non applicabile per uno schema a mosaico specificato (ad esempio GEOGRAPHY_GRID) *n* = Se tessellation_scheme è GEOMETRY_GRID, il valore della coordinata x-max|  
|bounding_box_ymax|**float(53)**|Coordinata y dell'angolo superiore destro del rettangolo di delimitazione, uno di: NULL = non applicabile per uno schema a mosaico specificato (ad esempio GEOGRAPHY_GRID) *n* = Se tessellation_scheme è GEOMETRY_GRID, il valore della coordinata y-max|  
|level_1_grid|**smallint**|Densità della griglia di livello principale. Se tessellation_scheme è GEOMETRY_GRID o GEOGRAPHY_GRID, uno dei seguenti valori: 16 = 4 per 4 Grid (LOW) 64 = 8 by 8 Grid (media) 256 = 16 by 16 Grid (HIGH) NULL = not applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_1_grid_desc|**nvarchar(60)**|Densità della griglia di primo livello, ovvero uno dei seguenti valori: LOW media HIGH NULL = not applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_2_grid|**smallint**|Densità della griglia di secondo livello. Se tessellation_scheme è GEOMETRY_GRID o GEOGRAPHY_GRID, uno dei seguenti valori: 16 = 4 per 4 Grid (LOW) 64 = 8 by 8 Grid (media) 256 = 16 by 16 Grid (HIGH) NULL = not applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_2_grid_desc|**nvarchar(60)**|Densità di griglia per la griglia di secondo livello, uno dei seguenti valori: LOW media HIGH NULL = non applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_3_grid|**smallint**|Densità della griglia di terzo livello.   Se tessellation_scheme è GEOMETRY_GRID o GEOGRAPHY_GRID, uno dei seguenti valori: 16 = 4 per 4 Grid (LOW) 64 = 8 by 8 Grid (media) 256 = 16 by 16 Grid (HIGH) NULL = not applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_3_grid_desc|**nvarchar(60)**|Densità della griglia di terzo livello, ovvero uno dei seguenti valori: LOW media HIGH NULL = non applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_4_grid|**smallint**|Densità della griglia di quarto livello. Se tessellation_scheme è GEOMETRY_GRID o GEOGRAPHY_GRID, uno dei seguenti valori: 16 = 4 per 4 Grid (bassa) 64 = 8 per 8 griglia (media) 256 = 16 per 16 Grid (HIGH) NULL = non applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|level_4_grid_desc|**nvarchar(60)**|Densità della griglia di quarto livello, uno tra i valori seguenti: < valore medio basso NULL = non applicabile per il tipo di indice spaziale o lo schema a mosaico specificato.  Viene restituito NULL quando viene utilizzata la nuova suddivisione a mosaico di SQL Server 11.|  
|cells_per_object|**int**|Numero di celle per oggetto spaziale, ovvero Se tessellation_scheme è GEOMETRY_GRID o GEOGRAPHY_GRID, *n* = numero di celle per oggetto null = non applicabile per un tipo di indice spaziale o uno schema a mosaico specifico|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
