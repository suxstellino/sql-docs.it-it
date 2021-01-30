---
description: STIsRing (tipo di dati geometry)
title: STIsRing (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsRing_TSQL
- STIsRing (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIsRing (geometry Data Type)
ms.assetid: ea0063be-1c74-4cc4-ac6f-b65321ddfa54
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b0f0cb68a9923491e64fa63f52b6d481abb73c14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205006"
---
# <a name="stisring-geometry-data-type"></a>STIsRing (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce 1 se un'istanza **geometry** soddisfa i requisiti seguenti:
-   Si tratta di un'istanza **LineString**.  
-   È chiusa.  
-   È semplice.  
-   Restituisce 0 se l'istanza **LineString** non soddisfa i requisiti.  

 Affinché un'istanza **geometry** sia chiusa e semplice, sia [STIsClosed()](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md) che [STIsSimple()](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md) devono restituire 1 quando sono richiamati sull'istanza. Per determinare il tipo di istanza di un tipo **geometry**, usare [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STIsRing ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **bit**  
  
 Tipo CLR restituito: **SqlBoolean**  
  
## <a name="remarks"></a>Commenti  
 Questo metodo restituisce Null se l'istanza non è **LineString**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `LineString` e viene utilizzato `STIsRing()` per verificare se l'istanza è un anello.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0, 0 0)', 0);  
SELECT @g.STIsRing();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [STIsClosed &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STGeometryType &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [STIsSimple &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md)   
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

