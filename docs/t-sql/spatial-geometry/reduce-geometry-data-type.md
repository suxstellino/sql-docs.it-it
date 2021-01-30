---
description: Reduce (tipo di dati geometry)
title: Reduce (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reduce_TSQL
- Reduce
dev_langs:
- TSQL
helpviewer_keywords:
- Reduce method
ms.assetid: 132184bf-c4d2-4a27-900d-8373445dce2a
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: dc6fbf1f3952e7ab59688c928eff3770776c308a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187718"
---
# <a name="reduce-geometry-data-type"></a>Reduce (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un'approssimazione dell'istanza **geometry** specificata. L'approssimazione viene prodotta eseguendo un'estensione dell'algoritmo Douglas-Peucker sull'istanza con la tolleranza specificata.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.Reduce ( tolerance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *tolerance*  
 Valore di tipo **float**. *tolerance* è la tolleranza da immettere per l'algoritmo di approssimazione.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
## <a name="remarks"></a>Osservazioni  
 Per i tipi di raccolta, questo algoritmo opera in modo indipendente su ogni elemento **geometry** contenuto nell'istanza.  
  
 Questo algoritmo non modifica le istanze **Point**.  
  
 Sulle istanze **LineString**, **CircularString** e **CompoundCurve** l'algoritmo di approssimazione mantiene i punti di inizio e fine dell'istanza originali. In seguito l'algoritmo aggiunge nuovamente in modo iterativo il punto dell'istanza originale che si discosta maggiormente dal risultato. Questo processo continua fino a quando nessun punto si discosta più della tolleranza specificata.  
  
 `Reduce()` restituisce un'istanza **LineString**, **CircularString** o **CompoundCurve** per le istanze **CircularString**.  `Reduce()` restituisce un'istanza **CompoundCurve** o **LineString** per le istanze **CompoundCurve**.  
  
 Nelle istanze **Polygon** l'algoritmo di approssimazione viene applicato in modo indipendente a ogni anello. Il metodo genera un'eccezione `FormatException` se l'istanza **Polygon** restituita non è valida. Se ad esempio `Reduce()` viene applicato per semplificare ogni anello nell'istanza e gli anelli risultanti si sovrappongono, viene creata un'istanza **MultiPolygon** non valida.  Nelle istanze **CurvePolygon** con un anello esterno e nessun anello interno, `Reduce()` restituisce un'istanza **CurvePolygon**, **LineString** o **Point**.  Se **CurvePolygon** include anelli interni viene restituita un'istanza **CurvePolygon** o **MultiPoint**.  
  
 Quando viene rilevato un segmento di arco circolare, l'algoritmo di approssimazione controlla se è possibile approssimare l'arco secondo la corda in metà della tolleranza specificata. Se la corda soddisfa questi criteri, l'arco circolare viene sostituito nei calcoli dalla corda. Se non soddisfa questi criteri, l'arco circolare viene mantenuto e l'algoritmo di approssimazione viene applicato ai segmenti rimanenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-reduce-to-simplify-a-linestring"></a>R. Utilizzo di Riduce() per semplificare LineString  
 Nell'esempio seguente viene creata un'istanza `LineString` e viene utilizzato il metodo `Reduce()` per semplificarla.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0, 2 1, 3 0, 4 1)', 0);  
SELECT @g.Reduce(.75).ToString();  
```  
  
### <a name="b-using-reduce-with-varying-tolerance-levels-on-a-circularstring"></a>B. Utilizzo di Riduce() con livelli di tolleranza diversi su CircularString  
 Nell'esempio seguente `Reduce()` viene usato con tre livelli di tolleranza in un'istanza **CircularString**:  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0)'; 
 SELECT @g.Reduce(5).ToString(); 
 SELECT @g.Reduce(15).ToString(); 
 SELECT @g.Reduce(16).ToString();
 ```  
  
 Nell'esempio viene prodotto l'output seguente:  
  
 ```
 CIRCULARSTRING (0 0, 8 8, 16 0, 20 -4, 24 0) 
 COMPOUNDCURVE (CIRCULARSTRING (0 0, 8 8, 16 0), (16 0, 24 0)) 
 LINESTRING (0 0, 24 0)
 ```  
  
 Ogni istanza restituita contiene i punti finali (0 0) e (24 0).  
  
### <a name="c-using-reduce-with-varying-tolerance-levels-on-a-compoundcurve"></a>C. Utilizzo di Riduce() con livelli di tolleranza diversi su CompoundCurve  
 Nell'esempio seguente `Reduce()` viene usato con due livelli di tolleranza in un'istanza **CompoundCurve**:  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0),(24 0, 20 4, 16 0))';  
 SELECT @g.Reduce(15).ToString();  
 SELECT @g.Reduce(16).ToString();
 ```  
  
 In questo esempio si noti che la seconda istruzione **SELECT** restituisce l'istanza **LineString**: `LineString(0 0, 16 0)`.  
  
### <a name="showing-an-example-where-the-original-start-and-end-points-are-lost"></a>Visualizzazione di un esempio in cui i punti di inizio e fine originali vengono persi  
 Nell'esempio seguente viene illustrato come i punti di inizio e fine originali potrebbero non essere mantenuti dall'istanza risultante. Questo comportamento si verifica perché il mantenimento dei punti di inizio e fine originali produrrebbe un'istanza **LineString** non valida.  
  
```  
DECLARE @g geometry = 'LINESTRING(0 0, 4 0, 2 .01, 1 0)';  
DECLARE @h geometry = @g.Reduce(1);  
SELECT @g.STIsValid() AS Valid  
SELECT @g.ToString() AS Original, @h.ToString() AS Reduced;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di geometria statici estesi](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
