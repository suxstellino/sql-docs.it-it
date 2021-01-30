---
description: STUnion (tipo di dati geography)
title: STUnion (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STUnion (geography Data Type)
- STUnion_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STUnion method
ms.assetid: 9bf87691-efd8-4c53-bd2f-eefe0acd19ca
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1990e524b3816c20c374dca2f01e604411530303
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186448"
---
# <a name="stunion-geography-data-type"></a>STUnion (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce un oggetto che rappresenta l'unione di un'istanza **geography** con un'altra istanza **geography**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STUnion ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *other_geography*  
 Altra istanza **geography** per formare un'unione con l'istanza sulla quale viene chiamato STUnion().  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
## <a name="exceptions"></a>Eccezioni  
 Questo metodo genera un'eccezione **ArgumentException** se l'istanza contiene un bordo opposto.  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo restituisce sempre Null se gli identificatori SRID delle istanze **geography** non corrispondono.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta le istanze spaziali di dimensioni maggiori di un emisfero. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il set di possibili risultati restituito nel server è stato esteso alle istanze **FullGlobe**.  
  
 Il risultato può contenere segmenti di arco circolare solo se le istanze di input contengono segmenti di arco circolare.  
  
 Il metodo non è preciso.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-computing-the-union-of-two-polygons"></a>R. Calcolo dell'unione di due poligoni  
 Nell'esempio seguente viene utilizzato `STUnion()` per calcolare l'unione di due istanze `Polygon`.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('POLYGON((-122.351 47.656, -122.341 47.656, -122.341 47.661, -122.351 47.661, -122.351 47.656))', 4326);  
SELECT @g.STUnion(@h).ToString();  
```  
  
### <a name="b-producing-a-fullglobe-result"></a>B. Generazione di un risultato FullGlobe  
 Nell'esempio seguente viene generato `FullGlobe` quando `STUnion()` combina due istanze `Polygon`.  
  
```
 DECLARE @g geography = 'POLYGON ((-122.358 47.653, -122.358 47.658,-122.348 47.658, -122.348 47.649, -122.358 47.653))';  
 DECLARE @h geography = 'POLYGON ((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
### <a name="c-producing-a-triagonal-hole-from-a-union-of-a-curvepolygon-and-a-triagonal-hole"></a>C. Generazione di un foro triangolare da un'unione di CurvePolygon e un foro triangolare.  
 Nell'esempio seguente viene generato un foro triangolare dall'unione di `CurvePolygon` con un'istanza `Polygon`.  
  
```
 DECLARE @g geography = 'POLYGON ((-0.5 0, 0 1, 0.5 0.5, -0.5 0))';  
 DECLARE @h geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 0, 0.7 0.7, 0 1), (0 1, 0 0)))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
