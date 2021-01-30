---
description: STConvexHull (tipo di dati geography)
title: STConvexHull (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- STConvexHull method (geography)
ms.assetid: fb435db7-31bb-4243-9d8b-35379184cfb4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b97a73933a395aa868df5dfba4d281be873ca4db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185974"
---
# <a name="stconvexhull-geography-data-type"></a>STConvexHull (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce un oggetto che rappresenta la struttura convessa di un'istanza **geography**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STConvexHull ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
## <a name="remarks"></a>Osservazioni  
 Restituisce un oggetto `FullGlobe` per l'istanza **geography** che ha un angolo della busta maggiore di 90 gradi.  
  
 Restituisce una raccolta **geography** vuota per un'istanza **geography** vuota.  
  
 Restituisce **null** per un'istanza **geography** non inizializzata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-stconvexhull-on-an-uninitialized-geography-instance"></a>R. Utilizzo di STConvexHull() in un'istanza geografica non inizializzata  
 Nell'esempio seguente viene usato `STConvexHull()` in un'istanza **geography** non inizializzata.  
  
```
 DECLARE @g geography;  
 SELECT @g.STConvexHull();
 ```  
  
### <a name="b-using-stconvexhull-on-an-empty-geography-instance"></a>B. Utilizzo di STConvexHull in un'istanza geografica vuota  
 Nell'esempio seguente viene utilizzato `STConvexHull()` in un'istanza `Polygon` vuota.  
  
```
 DECLARE @g geography = 'POLYGON EMPTY';  
 SELECT @g.STConvexHull().ToString();
 ```  
  
### <a name="c-finding-the-convex-hull-of-a-non-convex-polygon-instance"></a>C. Ricerca della struttura convessa di un'istanza Polygon non convessa  
 Nell'esempio seguente viene utilizzato `STConvexHull()` per trovare la struttura convessa di un'istanza `Polygon` non convessa.  
  
```  
 DECLARE @g geography;  
 SET @g = geography::Parse('POLYGON((-120.533 46.566, -118.283 46.1, -122.3 47.45, -120.533 46.566))');  
 SELECT @g.STConvexHull().ToString();  
```  
  
### <a name="d-finding-the-convex-hull-on-a-geography-instance-with-an-envelope-angle-larger-than-90-degrees"></a>D. Ricerca della struttura convessa in un'istanza geografica con un angolo della busta maggiore di 90 gradi  
 Nell'esempio seguente viene usato `STConvexHull()` in un'istanza **geography** con un angolo della busta maggiore di 90 gradi.  
  
```
 DECLARE @g geography = 'POLYGON((20.533 46.566, -18.283 46.1, -22.3 47.45, 20.533 46.566))';  
 SELECT @g.STConvexHull().ToString();
 ```  
  
  
