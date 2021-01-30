---
description: CurveToLineWithTolerance (tipo di dati geometry)
title: CurveToLineWithTolerance (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CurveToLineWithTolerance method (geometry)
ms.assetid: 96871075-1998-4cd9-86b1-3fc55577aee4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d1851b51e9068c3bd0fcbe7ac559e3d770fc1b48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198266"
---
# <a name="curvetolinewithtolerance-geometry-data-type"></a>CurveToLineWithTolerance (tipo di dati geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un'approssimazione poligonale di un'istanza **geometry** contenente segmenti di arco circolare.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.CurveToLineWithTolerance ( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *tolerance*  
 Espressione **double** che definisce l'errore massimo tra il segmento di arco circolare originale e l'approssimazione lineare.  
  
 *relative*  
 Espressione **bool** che indica se usare un valore massimo relativo per la deviazione. Quando il parametro relative viene impostato su false (0), viene impostato un valore massimo assoluto per la deviazione che può presentare un'approssimazione lineare. Quando il parametro relative viene impostato su true (1), la tolleranza e viene calcolata come prodotto tra il parametro della tolleranza e il diametro del rettangolo di selezione per l'oggetto spaziale.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
## <a name="exceptions"></a>Eccezioni  
 L'impostazione della tolleranza <= 0 genera un'eccezione `ArgumentOutOfRange`.  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo può specificare la tolleranza di errore per l'istanza **LineString** risultante.  
  
 Nella tabella seguente viene illustrato il tipo di istanza restituito da `CurveToLineWithTolerance()` per vari tipi.  
  
|Tipo di istanza di chiamata|Tipo spaziale restituito|  
|----------------------------|---------------------------|  
|Istanza di geometria vuota|Istanza **GeometryCollection** vuota|  
|**Point** e **MultiPoint**|Istanza **Point**|  
|**MultiPoint**|Istanza **Point** o **MultiPoint**|  
|**CircularString**, **CompoundCurve** o **LineString**|Istanza **LineString**|  
|**MultiLineString**|Istanza **LineString** o **MultiLineString**|  
|**CurvePolygon** e **Polygon**|Istanza **Polygon**|  
|**MultiPolygon**|Istanza **Polygon** o **MultiPolygon**|  
|**GeometryCollection** con una sola istanza che non contiene un segmento di arco circolare|L'istanza contenuta in **GeometryCollection** determina il tipo di istanza restituita.|  
|**GeometryCollection** con una singola istanza di segmento di arco circolare unidimensionale (**CircularString**, **CompoundCurve**)|Istanza **LineString**|  
|**GeometryCollection** con una singola istanza di segmento di arco circolare bidimensionale (**CurvePolygon**)|Istanza **Polygon**|  
|**GeometryCollection** con più istanze unidimensionali|Istanza **MultiLineString**|  
|**GeometryCollection** con più istanze bidimensionali|Istanza **MultiPolygon**|  
|**GeometryCollection** con più istanze di dimensioni diverse|Istanza **GeometryCollection**|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>R. Utilizzo di valori di tolleranza diversi in un'istanza CircularString  
 L'esempio seguente illustra l'impatto dell'impostazione della tolleranza sull'istanza `LineString` restituita da un'istanza `CircularString`:  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
 ```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. Utilizzo del metodo in un'istanza MultiLineString che contiene un'istanza LineString  
 Nell'esempio seguente viene illustrato ciò che viene restituito da un'istanza `MultiLineString` che contiene una sola istanza `LineString`:  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('MULTILINESTRING((1 3, 4 8, 6 9))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
 ```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. Utilizzo del metodo in un'istanza MultiLineString che contiene più istanze LineString  
 Nell'esempio seguente viene illustrato ciò che viene restituito da un'istanza `MultiLineString` che contiene più di un'istanza `LineString`:  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('MULTILINESTRING((1 3, 4 8, 6 9),(4 4, 9 18))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
 ```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D. Impostazione del parametro relative su true per un'istanza CurvePolygon di chiamata  
 Nell'esempio seguente viene usata un'istanza `CurvePolygon` per chiamare `CurveToLineWithTolerance()` con *relative* impostato su true:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
 ```  
  
### <a name="e-using-the-method-on-a-geometrycollection-instance"></a>E. Utilizzo del metodo in un'istanza GeometryCollection  
 Nell'esempio seguente viene chiamato `CurveToLineWithTolerance()` on a `GeometryCollection` che contiene un'istanza `CurvePolygon` bidimensionale e un'istanza `CircularString` unidimensionale. `CurveToLineWithTolerance()` converte entrambi i tipi di segmento di arco circolare in tipi di segmento lineare e li restituisce in un tipo `GeometryCollection`.  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('GEOMETRYCOLLECTION(CURVEPOLYGON( COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 0 2))), CIRCULARSTRING(4 4, 8 6, 9 5))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.1, 0).ToString();
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [CurveToLineWithTolerance &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/curvetolinewithtolerance-geography-data-type.md)   
 [STCurveToLine &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stcurvetoline-geometry-data-type.md)  
  
  

