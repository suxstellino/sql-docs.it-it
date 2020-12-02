---
description: STRelate (tipo di dati geometry)
title: STRelate (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STRelate (geometry Data Type)
- STRelate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRelate (geometry Data Type)
ms.assetid: 9dcb5f58-35ab-4bb3-86ee-2d29eefba6d3
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: e427ca859bf6e1e861ed58bb7fdc4ab2dc869fc1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88444985"
---
# <a name="strelate-geometry-data-type"></a>STRelate (tipo di dati geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce 1 se un'istanza **geometry** è correlata a un'altra istanza **geometry**. La relazione è definita da un valore della matrice del modello DE-9IM (Dimensionally Extended 9 Intersection Model). In caso contrario, restituisce 0.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STRelate ( other_geometry, intersection_pattern_matrix )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *other_geometry*  
 Altra istanza **geometry** da confrontare con l'istanza sulla quale viene chiamato `STRelate()`.  
  
 *intersection_pattern_matrix*  
 Stringa di tipo **nchar(9)** che codifica valori accettabili per il dispositivo a matrice del modello DE-9IM tra le due istanze **geometry**.  
  
## <a name="remarks"></a>Commenti  
 Questo metodo restituisce sempre Null se gli identificatori SRID delle istanze **geometry** non corrispondono. Questo metodo genererà un'eccezione **ArgumentException** se la matrice non è formattata in modo corretto.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **bit**  
  
 Tipo CLR restituito: **SqlBoolean**  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene usato `STRelate()` per verificare se due istanze **geometry** sono disgiunte a livello spaziale usando un modello DE-9IM esplicito.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);  
SET @h = geometry::STGeomFromText('POINT(5 5)', 0);  
SELECT @g.STRelate(@h, 'FF*FF****');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
