---
description: BufferWithCurves (tipo di dati geometry)
title: BufferWithCurves (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geometry)
ms.assetid: 8ffaba3f-d2dd-4e57-9f41-3ced9f14b600
author: MladjoA
ms.author: mlandzic
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18691c1a3e02d93d7c98a6b290672af5c77f15c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347532"
---
# <a name="bufferwithcurves-geometry-data-type"></a>BufferWithCurves (tipo di dati geometry)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Restituisce un'istanza **geometry** che rappresenta il set di tutti i punti la cui distanza dall'istanza **geometry** chiamante è minore o uguale al parametro *distance*.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
.BufferWithCurves ( distance )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti  
 *distance*  
 Valore **float** che indica la distanza massima a cui possono trovarsi i punti che compongono il buffer dall'istanza **geometry**.  
  
## <a name="return-types"></a>Tipi restituiti
Tipo SQL Server restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
## <a name="exceptions"></a>Eccezioni  
 I criteri seguenti genereranno un'eccezione **ArgumentException**.  
  
-   Nessun parametro viene passato al metodo, ad esempio `@g.BufferWithCurves()`  
  
-   Un parametro non numerico viene passato al metodo, ad esempio `@g.BufferWithCurves('a')`  
  
-   **NULL** viene passato al metodo, ad esempio `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Commenti  
 Nell'illustrazione seguente viene mostrato un esempio di un'istanza di geometria restituita da questo metodo.  
  
 ![Diagramma che mostra un esempio di istanza di geometria restituita da questo metodo.](../../t-sql/spatial-geometry/media/bufferedcurve.gif)
  
 Nella tabella seguente vengono illustrati i risultati restituiti per i diversi valori della distanza.  
  
|Valore del parametro distance|Dimensioni tipo|Tipo spaziale restituito|  
|--------------------|---------------------|---------------------------|  
|distance < 0|Zero o uno|Istanza **GeometryCollection** vuota|  
|distance < 0|Due o più|Istanza **CurvePolygon** o **GeometryCollection** con buffer negativo. **Nota:** è possibile che un buffer negativo crei un'istanza **GeometryCollection** vuota|  
|distance = 0|Tutte le dimensioni|Copia dell'istanza **geometry** di chiamata|  
|distance > 0|Tutte le dimensioni|Istanza **CurvePolygon** o **GeometryCollection**|  
  
> [!NOTE]  
>  Poiché *distance* è di tipo **float**, nei calcoli un valore estremamente ridotto può corrispondere a zero. Quando ciò si verifica, viene restituita una copia dell'istanza **geometry** chiamante. Vedere [float e real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Un buffer negativo rimuove tutti i punti racchiusi nella distanza specificata del limite della geometria. Nell'illustrazione seguente viene mostrato un buffer negativo come area lievemente ombreggiata del cerchio. La linea punteggiata è il limite del poligono originale e la linea continua è il limite del poligono risultante.  
  
 Se un parametro **string** viene passato al metodo, verrà convertito in **float** o verrà generata un'eccezione `ArgumentException`.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geometry-instance"></a>R. Chiamata a BufferWithCurves() con un valore di parametro < 0 in un'istanza di geometria unidimensionale  
 Nell'esempio seguente viene restituita un'istanza `GeometryCollection` vuota:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(-1).ToString(); 
 ```
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geometry-instance"></a>B. Chiamata a BufferWithCurves() con un valore di parametro < 0 in un'istanza di geometria bidimensionale  
 Nell'esempio seguente viene restituita un'istanza `CurvePolygon` con un buffer negativo:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Chiamata a BufferWithCurves() con un valore di parametro < 0 che restituisce un'istanza GeometryCollection vuota  
 Nell'esempio seguente viene illustrato cosa accade quando il parametro *distance* è uguale a -2:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Questa istruzione **SELECT** restituisce `GEOMETRYCOLLECTION EMPTY`  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. Chiamata a BufferWithCurves() con un valore di parametro = 0  
 Nell'esempio seguente viene restituita una copia dell'istanza **geometry** chiamante:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Chiamata a BufferWithCurves() con un valore di parametro diverso da zero ed estremamente basso  
 Anche nell'esempio seguente viene restituita una copia dell'istanza **geometry** chiamante:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 DECLARE @distance float = 1e-20; 
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Chiamata a BufferWithCurves() con un valore di parametro > 0  
 Nell'esempio seguente viene restituita un'istanza `CurvePolygon`:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
  
### <a name="g-passing-a-valid-string-parameter"></a>G. Passaggio di un parametro di stringa valido  
 Nell'esempio seguente viene restituita la stessa istanza `CurvePolygon` come indicato precedentemente, ma un parametro di stringa viene passato al metodo:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves('2').ToString();
 ```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Passaggio di un parametro di stringa non valido  
 Nell'esempio seguente verrà generato un errore:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)' 
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Si noti che nei due esempi precedenti è stato passato un valore letterale stringa al metodo `BufferWithCurves()`. Il primo esempio funziona perché il valore letterale stringa può essere convertito in un valore numerico. Tuttavia, nel secondo esempio viene generata un'eccezione `ArgumentException`.  
  
### <a name="i-calling-bufferwithcurves-on-multipoint-instance"></a>I. Chiamata a BufferWithCurves() in un'istanza MultiPoint  
 Nell'esempio seguente vengono restituite due istanze `GeometryCollection` e un'istanza `CurvePolygon`:  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.BufferWithCurves(1).ToString(); 
 SELECT @g.BufferWithCurves(1.5).ToString(); 
 SELECT @g.BufferWithCurves(1.6).ToString();
 ```  
  
 Le prime due istruzioni **SELECT** restituiscono un'istanza `GeometryCollection` perché il parametro *DISTANCE* è minore o uguale a 1/2 della distanza tra i due punti (1 1) e (1 4). La terza istruzione **select** restituisce un'istanza `CurvePolygon` perché le istanze dei due punti (1 1) e (1 4) memorizzate nel buffer si sovrappongono.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi estesi sulle istanze di geometria](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
 
