---
description: STAsText (tipo di dati geometry)
title: STAsText (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STAsText_TSQL
- STAsText (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STAsText (geometry Data Type)
ms.assetid: e0decf5e-2858-4c56-b61a-6123f47fb51c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 81f22df1e2cb88f17bb0a23a09442ab6ada5341e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207894"
---
# <a name="stastext-geometry-data-type"></a>STAsText (tipo di dati geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Restituisce la rappresentazione Open Geospatial Consortium (OGC) Well-Known Text (WKT) di un'istanza **Geometry** . Il testo non conterrà alcun valore Z (innalzamento) o M (misura) appartenente all'istanza.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STAsText ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **nvarchar(max)**  
  
 Tipo CLR restituito: **SqlChars**  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza di geometria `LineString` da (0,0) a (2,3) dal testo. Tramite `STAsText()` viene restituito il risultato in formato testo.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 3)', 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

