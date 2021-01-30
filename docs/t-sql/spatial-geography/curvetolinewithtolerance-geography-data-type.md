---
description: CurveToLineWithTolerance (tipo di dati geography)
title: CurveToLineWithTolerance (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CurveToLineWithTolerance_TSQL
- CurveToLineWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- CurveToLineWithTolerance method (geography)
ms.assetid: 74369c76-2cf6-42ae-b9cc-e7a051db2767
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f19dbc100f507d10a3c1c382735608b307a40ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210607"
---
# <a name="curvetolinewithtolerance-geography-data-type"></a>CurveToLineWithTolerance (tipo di dati geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un'approssimazione poligonale di un'istanza **geography** contenente segmenti di arco circolare.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.CurveToLineWithTolerance( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
_tolerance_  
Espressione **double** che definisce l'errore massimo tra il segmento di arco circolare originale e l'approssimazione lineare.  
  
_relative_  
Espressione **bool** che indica se usare un valore massimo relativo per la deviazione. Se relative è false (0), viene impostato un valore massimo assoluto per la deviazione che può presentare un'approssimazione lineare. Se relative è true (1), la tolleranza e viene calcolata come prodotto tra il parametro tolerance per il diametro del rettangolo di selezione per l'oggetto spaziale.  
  
## <a name="return-types"></a>Tipi restituiti  
Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
Tipo CLR restituito: **SqlGeography**  
  
## <a name="exceptions"></a>Eccezioni  
L'impostazione di tolerance <= 0 genera un'eccezione **ArgumentOutOfRange**.  
  
## <a name="remarks"></a>Osservazioni  
Questo metodo consente di specificare la tolleranza di errore per l'istanza **LineString** risultante.  
  
Il metodo **CurveToLineWithTolerance** restituisce un'istanza **LineString** per un'istanza **CircularString** o **CompoundCurve** e un'istanza **Polygon** per un'istanza **CurvePolygon**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>R. Utilizzo di valori di tolleranza diversi in un'istanza CircularString  
L'esempio seguente illustra l'impatto dell'impostazione della tolleranza sull'istanza `LineString` restituita da un'istanza `CircularString`:  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. Utilizzo del metodo in un'istanza MultiLineString che contiene un'istanza LineString  
Nell'esempio seguente viene illustrato ciò che viene restituito da un'istanza `MultiLineString` che contiene una sola istanza `LineString`:  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. Utilizzo del metodo in un'istanza MultiLineString che contiene più istanze LineString  
Nell'esempio seguente viene illustrato ciò che viene restituito da un'istanza `MultiLineString` che contiene più di un'istanza `LineString`:  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649),(-123.358 47.653, -123.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D. Impostazione del parametro relative su true per un'istanza CurvePolygon di chiamata  
Nell'esempio seguente viene usata un'istanza `CurvePolygon` per chiamare `CurveToLineWithTolerance()` con *relative* impostato su true:  
  
```
DECLARE @g geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658), (-122.348 47.658, -122.358 47.658, -122.358 47.653)))';  
SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
```  
  
## <a name="see-also"></a>Vedere anche  
[Metodi estesi sulle istanze geografiche](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
