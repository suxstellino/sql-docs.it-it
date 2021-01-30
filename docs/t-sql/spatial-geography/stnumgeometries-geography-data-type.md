---
description: STNumGeometries (tipo di dati geography)
title: STNumGeometries (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumGeometries (geography Data Type)
- STNumGeometries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STNumGeometries method
ms.assetid: 6ae7fac2-62f1-420f-9fc9-a09606be9605
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 21dfb949f48d8717062cc2df6abb7ac3d30c7d23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202115"
---
# <a name="stnumgeometries-geography-data-type"></a>STNumGeometries (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il numero di **geometrie** che costituiscono un'istanza **geography**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STNumGeometries ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **int**  
  
 Tipo CLR restituito: **SqlInt32**  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo restituisce 1 se l'istanza **geography** non è un'istanza **MultiPoint**, **MultiLineString**, **MultiPolygon** o **GeometryCollection** oppure 0 se l'istanza **geography** è vuota.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `MultiPoint` e viene usato `STNumGeometries()` per trovare il numero di **geometrie** contenute nell'istanza.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('MULTIPOINT((-122.360 47.656), (-122.343 47.656))', 4326);  
SELECT @g.STNumGeometries();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
