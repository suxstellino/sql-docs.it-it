---
description: STDistance (tipo di dati geography)
title: STDistance (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STDistance_TSQL
- STDistance (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STDistance method
ms.assetid: 063d8722-e019-4d3d-8fcf-dbf5325823e7
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: cd4f7fd8fb9f0918fa419847ff0ecda02306d3ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99156804"
---
# <a name="stdistance-geography-data-type"></a>STDistance (tipo di dati geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce la distanza più breve tra un punto in un'istanza **geography** e un punto in un'altra istanza **geography**.  
  
> [!NOTE]  
>  `STDistance()` restituisce la **LineString** più breve tra due tipi di geografia. È un'approssimazione della distanza geodetica. La deviazione di `STDistance()` dalla distanza geodetica esatta sui modelli di terra comuni non è superiore allo 0,25%. In tal modo si evitano confusioni sulle differenze minime tra lunghezza e distanza nei tipi geodetici.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STDistance ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *other_geography*  
 Altra istanza **geography** da cui misurare la distanza rispetto all'istanza sulla quale viene chiamato STDistance(). Se *other_geography* è un set vuoto, STDistance() restituisce un valore Null.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **float**  
  
 Tipo CLR restituito: **SqlDouble**  
  
## <a name="remarks"></a>Commenti  
 Il risultato viene espresso nell'unità di misura definita dall'[identificatore SRID &#40;Spatial Reference Identifier&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md) dei dati spaziali.
STDistance() restituisce sempre *Null* se gli identificatori SRID delle istanze **geography** non corrispondono.  
  
> [!NOTE]  
>  I metodi nel tipo di dati **geography** che calcolano un'area o una distanza restituiscono risultati diversi a seconda dell'identificatore SRID dell'istanza usato nel metodo. Per altre informazioni sugli identificatori SRID, vedere [Spatial Reference Identifiers &#40;SRIDs&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md) (Identificatori SRID).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene trovata la distanza tra due istanze **geography**.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SET @h = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.STDistance(@h);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
