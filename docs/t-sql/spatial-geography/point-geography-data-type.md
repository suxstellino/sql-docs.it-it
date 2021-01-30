---
description: Point (tipo di dati geography)
title: Point (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Point
- Point_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Point method
- Point (geography Data Type)
ms.assetid: 0dc6f422-7aae-4016-b7f4-3289fa8f989c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 207b4c8fb9c9d171e7caad003e9b398c97dc4152
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210143"
---
# <a name="point-geography-data-type"></a>Point (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Costruisce un'istanza **geography** che rappresenta un'istanza **Point** dai relativi valori di latitudine e longitudine e un identificatore SRID.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Point ( Lat, Long, SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Lat*  
 Espressione **float** che rappresenta la coordinata Y dell'istanza **Point** da generare.  
  
 *Long*  
 Espressione **float** che rappresenta la coordinata X dell'istanza **Point** da generare. Per altre informazioni sui valori validi di latitudine e longitudine, vedere [Point](../../relational-databases/spatial/point.md).  
  
 *SRID*  
 Espressione **int** che rappresenta l'[identificatore SRID (Spatial Reference Identifier)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md) dell'istanza **geography** da restituire.  
  
> [!NOTE]  
>  Gli argomenti del metodo Point (tipo di dati geography) hanno coordinate inverse rispetto a WKT.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato il metodo `Point()` per creare un'istanza `geography`.  
  
```  
DECLARE @g geography;   
SET @g = geography::Point(47.65100, -122.34900, 4326)  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi geography statici estesi](../../t-sql/spatial-geography/extended-static-geography-methods.md)