---
description: InstanceOf (tipo di dati geography)
title: InstanceOf (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- InstanceOf
- InstanceOf_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- InstanceOf method
ms.assetid: 1eaed0e4-1c72-45a9-9926-5b513335cf33
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 87255f6a6a170a252ea37bbb9bf21e53da778727
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186421"
---
# <a name="instanceof-geography-data-type"></a>InstanceOf (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Verifica se l'istanza **geography** corrisponde al tipo specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
  
.InstanceOf ( 'geography_type')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*geography_type*  
La stringa **nvarchar(4000)** specifica uno dei 16 tipi esposti nella gerarchia del tipo **geography**.  
  
## <a name="return-types"></a>Tipi restituiti  
Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **bit**  
  
Tipo CLR restituito: **SqlBoolean**  
  
## <a name="remarks"></a>Commenti  
Restituisce 1 se il tipo di un'istanza **geography** corrisponde al tipo specificato o se il tipo specificato è un predecessore del tipo di istanza. In caso contrario, restituisce 0.  
  
Questo metodo con tipo di dati **geography** supporta le istanze **FullGlobe** o le istanze spaziali con dimensioni maggiori di un emisfero.  
  
L'input per il metodo deve essere uno dei seguenti: Geometry, Point, Curve, LineString, CircularString, Surface, Polygon, CurvePolygon, **GeometryCollection**, **MultiSurface**, **MultiPolygon, MultiCurve, MultiLineString**, **MultiPoint** o **FullGlobe**.  
  
Questo metodo genera un'eccezione `ArgumentException` se per l'input si usa qualsiasi altra stringa.  
  
Questo metodo non è preciso.  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene creata un'istanza `MultiPoint` e viene utilizzato `InstanceOf()` per verificare se l'istanza è di tipo `GeometryCollection`.  
  
```sql  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('MULTIPOINT(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.InstanceOf('GEOMETRYCOLLECTION');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi estesi sulle istanze geografiche](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
