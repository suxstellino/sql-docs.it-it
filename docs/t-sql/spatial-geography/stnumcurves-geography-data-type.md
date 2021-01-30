---
description: STNumCurves (tipo di dati geography)
title: STNumCurves (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumCurves
- STNumCurves_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STNumCurves method (geography)
ms.assetid: e98a56c2-8496-4dfd-9b37-7f3c4ca9b2b5
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 91a95d05edfdf55ea5fda38bfda078607cd142c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180132"
---
# <a name="stnumcurves-geography-data-type"></a>STNumCurves (tipo di dati geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il numero di curve in un'istanza **geography** unidimensionale.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STNumCurves()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
## <a name="remarks"></a>Osservazioni  
 I tipi di dati spaziali unidimensionali includono **LineString**, **CircularString** e **CompoundCurve**. Un'istanza **geography** unidimensionale vuota restituisce 0.  
  
 `STNumCurves`() funziona solo su tipi semplici; non funziona con raccolte **geography** come **MultiLineString**. **NULL** viene restituito quando l'istanza **geography** non è un tipo di dati unidimensionale.  
  
 **Null** viene restituito per le istanze **geography** non inizializzate.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-stnumcurves-on-a-circularstring-instance"></a>R. Utilizzo di STNumCurves() in un'istanza CircularString  
 Nell'esempio seguente viene illustrato come ottenere il numero di curve in un'istanza `CircularString`:  
  
```
 DECLARE @g geography; 
 SET @g = geography::Parse('CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
 SELECT @g.STNumCurves();
 ```  
  
### <a name="b-using-stnumcurves-on-a-compoundcurve-instance"></a>B. Utilizzo di STNumCurves() in un'istanza CompoundCurve  
 Nell'esempio seguente viene utilizzato `STNumCurves()` per restituire il numero di curve in un'istanza `CompoundCurve`.  
  
```
 DECLARE @g geography;  
 SET @g = geography::Parse('COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))');  
 SELECT @g.STNumCurves();
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica dei tipi di dati spaziali](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
