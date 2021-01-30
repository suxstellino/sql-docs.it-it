---
description: STNumPoints (tipo di dati geography)
title: STNumPoints (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumPoints (geography Data Type)
- STNumPoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STNumPoints method
ms.assetid: 25ff7ad1-ba5f-4cfb-816a-59255ac1591d
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 220e2e9149c3e153080fd044996d01a08c22db89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206507"
---
# <a name="stnumpoints-geography-data-type"></a>STNumPoints (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il numero totale di punti in ognuna delle figure di un'istanza **geography**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STNumPoints ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **int**  
  
 Tipo CLR restituito: **SqlInt32**  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo conta i punti nella descrizione di un'istanza **geography**. Vengono contati anche i punti duplicati; tuttavia, i punti di connessione tra i segmenti vengono contati una sola volta. Se questa istanza è una raccolta, il metodo restituisce il numero totale di punti nella raccolta.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-retrieving-the-total-number-of-points-in-a-linestring"></a>R. Recupero del numero complessivo di punti in LineString  
 Nell'esempio seguente viene creata un'istanza `LineString` e viene utilizzato `STNumPoints()` per determinare il numero di punti utilizzati nella descrizione dell'istanza.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STNumPoints();  
```  
  
### <a name="b-retrieving-the-total-number-of-points-in-a-geometrycollection"></a>B. Recupero del numero complessivo di punti in GeometryCollection  
 Nell'esempio seguente viene restituita la somma dei punti di tutti gli elementi in `GeometryCollection`.  
  
```  
DECLARE @g geography = 'GEOMETRYCOLLECTION(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)  
    ,CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)))';  
SELECT @g.STNumPoints();  
```  
  
### <a name="c-returning-the-number-of-points-in-a-compoundcurve"></a>C. Restituzione del numero di punti in CompoundCurve  
 Nell'esempio seguente viene restituito il numero di punti in un'istanza CompoundCurve. La query restituisce 5 anziché 6 perché in STNumPoints() i punti di connessione tra i segmenti vengono contati una sola volta.  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658),( -122.348 47.658, -121.56 48.12, -122.358 47.653))'  
 SELECT @g.STNumPoints();
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
