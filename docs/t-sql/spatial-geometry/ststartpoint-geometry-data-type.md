---
description: STStartPoint (tipo di dati geometry)
title: STStartPoint (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STStartPoint_TSQL
- STStartPoint (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STStartPoint (geometry Data Type)
ms.assetid: 049917db-3f76-4053-8cd2-bc54158e89bc
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 3fd64405d2e08038701262105675557893cdf96f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88416857"
---
# <a name="ststartpoint-geometry-data-type"></a>STStartPoint (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce il punto di inizio di un'istanza **geometry**.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STStartPoint ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
 Tipo OGC (Open Geospatial Consortium): **Point**  
  
## <a name="remarks"></a>Osservazioni  
 `STStartPoint()` equivale a [STPointN](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md) (1).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `LineString` e viene utilizzato `STStartPoint()` per recuperare il punto di inizio dell'istanza.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STStartPoint().ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [STPointN &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)   
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

