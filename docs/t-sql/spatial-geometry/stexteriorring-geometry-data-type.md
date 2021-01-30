---
description: STExteriorRing (tipo di dati geometry)
title: STExteriorRing (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STExteriorRing_TSQL
- STExteriorRing (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STExteriorRing (geometry Data Type)
ms.assetid: b402b36f-05bf-4c6d-8cd6-76c0fff19db2
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 7d73818f16a059de5b6f23171bc9cd4dec026d10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159374"
---
# <a name="stexteriorring-geometry-data-type"></a>STExteriorRing (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce l'anello esterno di un'istanza **geometry** che è un poligono.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STExteriorRing ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
 Tipo OGC (Open Geospatial Consortium): **LineString**  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo restituisce **null** se l'istanza **geometry** non è un poligono.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `Polygon` e viene usato `STExteriorRing()` per restituire l'anello esterno del poligono come **LineString**.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STExteriorRing().ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

