---
description: STMLineFromText (tipo di dati geometry)
title: STMLineFromText (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STMLineFromText (geometry Data Type)
- STMLineFromText_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STMLineFromText (geometry Data Type)
ms.assetid: 39fe8559-c4c2-4d61-8508-86eb0a103807
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: ce19b6188657afc79dae6a98ace97fc7391424ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172484"
---
# <a name="stmlinefromtext-geometry-data-type"></a>STMLineFromText (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un'istanza **geometry** da una rappresentazione WKT (Well-Known Text) OGC (Open Geospatial Consortium) integrata con qualsiasi valore Z (innalzamento) e M (misura) appartenente all'istanza.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
STMLineFromText ( 'multilinestring_tagged_text' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *multilinestring_tagged_text*  
 Rappresentazione WKT dell'istanza **geometryMultiLineString** da restituire. *multilinestring_tagged_text* è un'espressione **nvarchar(max)**.  
  
 *SRID*  
 Espressione **int** che rappresenta l'identificatore SRID dell'istanza **geometryMultiLineString** da restituire.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
 Tipo OGC: **MultiLineString**  
  
## <a name="remarks"></a>Commenti  
 Questo metodo genererà un'eccezione **FormatException** se l'input non è formattato in modo corretto.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato il metodo `STMLineFromText()` per creare un'istanza `geometry`.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STMLineFromText('MULTILINESTRING ((100 100, 200 200), (3 4, 7 8, 10 10))', 0);  
```  
  
 `SELECT @g.ToString();`  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di geometria statici OGC](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  

