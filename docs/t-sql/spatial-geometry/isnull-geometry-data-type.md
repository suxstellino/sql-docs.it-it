---
description: IsNull (tipo di dati geometry)
title: IsNull (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IsNull (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull (geometry Data Type)
ms.assetid: f95813a5-26c0-48aa-bfb8-56d2a0980788
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 6ea6429f1776c18feaacf43508100d4c960cd7be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159423"
---
# <a name="isnull-geometry-data-type"></a>IsNull (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il tipo di un'istanza **geometry** è Null. Restituisce 0 se l'istanza non è Null.
  
## <a name="syntax"></a>Sintassi  
  
```  
.IsNull  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Tipo CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Osservazioni  
 `IsNull` può essere usato per verificare se un'istanza **geometry** è Null. `IsNull` restituisce 0 se l'istanza non è Null, ma Null se l'istanza è Null.  
  
 Questo metodo viene usato principalmente dall'infrastruttura di SQL Server. Non è consigliabile usare `IsNull` per verificare se un'istanza è Null.  
  

## <a name="see-also"></a>Vedere anche  
 [Metodi estesi sulle istanze di geometria](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

