---
description: STBuffer (tipo di dati geometry)
title: STBuffer (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STBuffer (geometry Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geometry Data Type)
ms.assetid: ca6bf2dc-1d38-4503-b87e-f2ea033d36ba
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 787612ae8cdb2cc5751818c31f111831e4013cbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187221"
---
# <a name="stbuffer-geometry-data-type"></a>STBuffer (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un oggetto geometrico che rappresenta l'unione di tutti i punti la cui distanza da un'istanza **geometry** è minore o uguale a un valore specificato.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *distance*  
 Valore di tipo **float** (**double** in .NET Framework) che specifica la distanza dall'istanza di geometria intorno alla quale calcolare il buffer.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
## <a name="remarks"></a>Commenti  
 `STBuffer()` calcola un buffer in modo analogo a [BufferWithTolerance](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md), specificando *tolerance* = distance \* 0,001 e *relative* = **false**.  
  
 Se *distance* > 0 viene restituita un'istanza **Polygon** o **MultiPolygon**.  
  
> [!NOTE]  
>  Poiché distance è di tipo **float**, nei calcoli un valore estremamente ridotto può corrispondere a zero.  Quando ciò si verifica, viene restituita una copia dell'istanza **geometry** chiamante.  Vedere [float e real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Quando *distance* = 0, viene restituita una copia dell'istanza **geometry** chiamante.  
  
 Quando *distance* < 0,  
  
-   viene restituita un'istanza **GeometryCollection** vuota se le dimensioni dell'istanza sono 0 o 1.  
  
-   viene restituito un buffer negativo quando le dimensioni dell'istanza sono 2 o maggiori di 2.  
  
    > [!NOTE]  
    >  È possibile che un buffer negativo crei un'istanza **GeometryCollection** vuota.  
  
 Un buffer negativo rimuove tutti i punti racchiusi nella distanza specificata del limite della geometria.  
  
 L'errore tra il buffer teorico e quello calcolato è max(tolerance, extents * 1.E-7) dove tolerance = distance \* 0,001. Per altre informazioni sulle estensioni, vedere la [Guida di riferimento ai metodi per il tipo di dati geometry](./spatial-types-geometry-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-calling-stbuffer-with-parameter_value--0-on-one-dimensional-geometry-instance"></a>R. Chiamata di STBuffer() con parameter_value < 0 in un'istanza di geometria unidimensionale  
 Nell'esempio seguente viene restituita un'istanza `GeometryCollection` vuota:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="b-calling-stbuffer-with-parameter_value--0-on-a-polygon-instance"></a>B. Chiamata di STBuffer() con parameter_value < 0 in un'istanza Polygon  
 Nell'esempio seguente viene restituita un'istanza `Polygon` con un buffer negativo:  
  
```
 DECLARE @g geometry = 'POLYGON((1 1, 1 5, 5 5, 5 1, 1 1))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="c-calling-stbuffer-with-parameter_value--0-on-a-curvepolygon-instance"></a>C. Chiamata di STBuffer() con parameter_value < 0 in un'istanza CurvePolygon  
 Nell'esempio seguente viene restituita un'istanza `Polygon` con un buffer negativo da un'istanza `CurvePolygon`:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
> [!NOTE]  
>  Viene restituita un'istanza `Polygon` anziché `CurvePolygon`.  Per restituire un'istanza `CurvePolygon`, vedere [BufferWithCurves &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
### <a name="d-calling-stbuffer-with-a-negative-parameter-value-that-returns-an-empty-instance"></a>D. Chiamata a STBuffer () con un valore di parametro negativo mediante il quale viene restituita un'istanza vuota  
 Nell'esempio seguente viene illustrato cosa accade quando nell'esempio precedente il parametro *distance* è uguale a -2.  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-2).ToString();
 ```  
  
 Questa istruzione **SELECT** restituisce `GEOMETRYCOLLECTION EMPTY.`  
  
### <a name="e-calling-stbuffer-with-parameter_value--0"></a>E. Chiamata a STBuffer() con parameter_value = 0  
 Nell'esempio seguente viene restituita una copia dell'istanza `geometry` chiamante:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(0).ToString();
 ```  
  
### <a name="f-calling-stbuffer-with-a-non-zero-parameter-value-that-is-extremely-small"></a>F. Chiamata a STBuffer() con un valore di parametro diverso da zero ed estremamente basso  
 Nell'esempio seguente viene inoltre restituita una copia dell'istanza `geometry` chiamante:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.STBuffer(@distance).ToString();
 ```  
  
### <a name="g-calling-stbuffer-with-parameter_value--0"></a>G. Chiamata a STBuffer() con parameter_value > 0  
 Nell'esempio seguente viene restituita un'istanza `Polygon`:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(2).ToString();
 ```  
  
### <a name="h-calling-stbuffer-with-a-string-parameter-value"></a>H. Chiamata a STBuffer() con un valore di parametro stringa  
 Nell'esempio seguente viene restituita la stessa istanza `Polygon` come indicato precedentemente, ma un parametro di stringa viene passato al metodo:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('2').ToString();
 ```  
  
 Nell'esempio seguente verrà generato un errore:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('a').ToString();
 ```  
  
> [!NOTE]  
>  Nei due esempi precedenti è stato passato un valore letterale stringa a `STBuffer()`.  Il primo esempio funziona perché il valore letterale stringa può essere convertito in un valore numerico. Tuttavia, nel secondo esempio viene generata un'eccezione `ArgumentException`.  
  
### <a name="i-calling-stbuffer-on-a-multipoint-instance"></a>I. Chiamata a STBuffer() in un'istanza MultiPoint  
 Nell'esempio seguente vengono restituite due istanze `MultiPolygon` e un'istanza `Polygon`:  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.STBuffer(1).ToString(); 
 SELECT @g.STBuffer(1.5).ToString(); 
 SELECT @g.STBuffer(1.6).ToString();
 ```  
  
 Le prime due istruzioni **SELECT** restituiscono un'istanza `MultiPolygon` perché il parametro *DISTANCE* è minore o uguale a 1/2 della distanza tra i due punti (1 1) e (1 4). La terza istruzione **select** restituisce un'istanza `Polygon` perché le istanze dei due punti (1 1) e (1 4) memorizzate nel buffer si sovrappongono.  
  
## <a name="see-also"></a>Vedere anche  
 [BufferWithTolerance &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)   
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
