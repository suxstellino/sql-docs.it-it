---
description: STDisjoint (tipo di dati geography)
title: STDisjoint (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STDisjoint (geography Data Type)
- STDisjoint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STDisjoint
ms.assetid: 98328a02-e018-47d6-aa93-de162b8aef62
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 96cde2c956d745ab27c64118874c033b76670959
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157324"
---
# <a name="stdisjoint-geography-data-type"></a>STDisjoint (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce 1 se un'istanza **geography** è disgiunta a livello spaziale da un'altra istanza **geography**. In caso contrario, restituisce 0.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STDisjoint ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *other_geography*  
 Altra istanza **geography** da confrontare con l'istanza sulla quale viene chiamato STDisjoint().  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **bit**  
  
 Tipo CLR restituito: **SqlBoolean**  
  
## <a name="remarks"></a>Commenti  
 Due istanze **geography** sono disgiunte se l'intersezione dei relativi set di punti è vuota.  
  
 Questo metodo restituisce sempre Null se gli identificatori SRID delle istanze **geography** non corrispondono.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato `STDisjoint()` per verificare se due istanze `geography` sono disgiunte a livello spaziale.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SET @h = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.STDisjoint(@h);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
