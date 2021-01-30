---
description: STSrid (tipo di dati geometry)
title: STSrid (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STSrid (geometry Data Type)
- STSrid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STSrid (geometry Data Type)
ms.assetid: 5e0de983-a0fe-48b7-9e08-30588d7271e2
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 15f91de88c027d96d56dfaeb80859e8433727c9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184581"
---
# <a name="stsrid-geometry-data-type"></a>STSrid (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  **STSrid** è un Integer che rappresenta l'identificatore SRID dell'istanza.  
  
Questa proprietà può essere modificata.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
STSrid  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **int**  
  
 Tipo CLR: **SqlInt32**  
  
## <a name="examples"></a>Esempi  
 Nel primo esempio viene creata un'istanza **geometry** con il valore dell'identificatore SRID uguale a 13 e viene usato `STSrid` per confermare l'identificatore SRID.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0))', 13);  
SELECT @g.STSrid;  
```  
  
 Nel secondo esempio viene utilizzato `STSrid` per impostare il valore dell'identificatore SRID dell'istanza su 23 e quindi viene confermato il valore SRID modificato.  
  
```  
SET @g.STSrid = 23;  
SELECT @g.STSrid;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [STX &#40;Tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stx-geometry-data-type.md)   
 [STY &#40;Tipo di dati geometry&#41;](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

