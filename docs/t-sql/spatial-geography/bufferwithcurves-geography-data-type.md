---
description: BufferWithCurves (tipo di dati geography)
title: BufferWithCurves (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithCurves
- BufferWithCurves_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geography)
ms.assetid: abf0a11c-c99c-4faa-bf80-3ae8e04d7bfb
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: e34c83606e632657ab0e4846a6444ecb0a5050ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200233"
---
# <a name="bufferwithcurves-geography-data-type"></a>BufferWithCurves (tipo di dati geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce un'istanza **geography** che rappresenta il set di tutti i punti la cui distanza dall'istanza **geography** chiamante è minore o uguale al parametro *distance*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.BufferWithCurves ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *distance*  
 Valore **float** che indica la distanza massima a cui possono trovarsi i punti che compongono il buffer dall'istanza geography.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
## <a name="exceptions"></a>Eccezioni  
 I criteri seguenti genereranno un'eccezione **ArgumentException**.  
  
-   Nessun parametro viene passato al metodo, ad esempio `@g.BufferWithCurves()`  
  
-   Un parametro non numerico viene passato al metodo, ad esempio `@g.BufferWithCurves('a')`  
  
-   **NULL** viene passato al metodo, ad esempio `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Commenti  
 Nella tabella seguente vengono illustrati i risultati restituiti per i diversi valori della distanza.  
  
|Valore del parametro distance|Dimensioni tipo|Tipo spaziale restituito|  
|--------------------|---------------------|---------------------------|  
|distance < 0|Zero o uno|Istanza **GeometryCollection** vuota|  
|distance \< 0|Due o più|Istanza **CurvePolygon** o **GeometryCollection** con buffer negativo.<br /><br /> Nota: è possibile che un buffer negativo crei un'istanza **GeometryCollection** vuota.|
|distance = 0|Tutte le dimensioni|Copia dell'istanza **geography** di chiamata|  
|distance > 0|Tutte le dimensioni|Istanza **CurvePolygon** o **GeometryCollection**|  
  
> [!NOTE]  
>  Poiché *distance* è di tipo **float**, nei calcoli un valore estremamente ridotto può corrispondere a zero.  Quando ciò si verifica, viene restituita una copia dell'istanza **geography** chiamante.  
  
 Se un parametro **string** viene passato al metodo, verrà convertito in **float** o verrà generata un'eccezione `ArgumentException`.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geography-instance"></a>R. Chiamata a BufferWithCurves() con un valore di parametro < 0 in un'istanza di geografia unidimensionale  
 Nell'esempio seguente viene restituita un'istanza `GeometryCollection` vuota:  
  
 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(-1).ToString();
``` 
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geography-instance"></a>B. Chiamata a BufferWithCurves() con un valore di parametro < 0 in un'istanza di geografia bidimensionale  
 Nell'esempio seguente viene restituita un'istanza `CurvePolygon` con un buffer negativo:  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Chiamata a BufferWithCurves() con un valore di parametro < 0 che restituisce un'istanza GeometryCollection vuota  
 Nell'esempio seguente viene illustrato cosa accade quando il parametro *distance* è uguale a -2:  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Questa istruzione **SELECT** restituisce `GEOMETRYCOLLECTION EMPTY`  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. Chiamata a BufferWithCurves() con un valore di parametro = 0  
 Nell'esempio seguente viene restituita una copia dell'istanza **geography** chiamante:  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Chiamata a BufferWithCurves() con un valore di parametro diverso da zero ed estremamente basso  
 Anche nell'esempio seguente viene restituita una copia dell'istanza **geography** chiamante:  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Chiamata a BufferWithCurves() con un valore di parametro > 0  
 Nell'esempio seguente viene restituita un'istanza `CurvePolygon`:  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
### <a name="g-passing-a-valid-string-parameter"></a>G. Passaggio di un parametro di stringa valido  
 Nell'esempio seguente viene restituita la stessa istanza `CurvePolygon` come indicato precedentemente, ma un parametro di stringa viene passato al metodo:  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves('2').ToString();
```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Passaggio di un parametro di stringa non valido  
 Nell'esempio seguente verrà generato un errore:  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)'  
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Si noti che nei due esempi precedenti è stato passato un valore letterale stringa al metodo `BufferWithCurves()`. Il primo esempio funziona perché il valore letterale stringa può essere convertito in un valore numerico. Tuttavia, nel secondo esempio viene generata un'eccezione `ArgumentException`.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi estesi sulle istanze di geografia](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [BufferWithCurves &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
  
