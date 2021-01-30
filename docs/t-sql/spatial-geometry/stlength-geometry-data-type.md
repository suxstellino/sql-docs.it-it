---
description: STLength (tipo di dati geometry)
title: STLength (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STLength_TSQL
- STLength (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STLength (geometry Data Type)
ms.assetid: e34dc620-2a65-4248-b099-fff91830ab98
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fa53274ed371828b0e124ef362dadfce2754de7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158314"
---
# <a name="stlength-geometry-data-type"></a>STLength (tipo di dati geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce la lunghezza totale degli elementi in un'istanza **geometry**.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STLength ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **float**  
  
 Tipo CLR restituito: **SqlDouble**  
  
## <a name="remarks"></a>Commenti  
 Se un'istanza **geometry** è chiusa, la lunghezza viene calcolata come lunghezza totale lungo l'istanza. La lunghezza di qualsiasi poligono è il relativo perimetro e la lunghezza di un punto è 0. La lunghezza di qualsiasi tipo **geometrycollection** è la somma delle lunghezze delle istanze **geometry** contenute.  
  
 STLength() può essere utilizzato con oggetti LineString validi e non validi. In genere, un oggetto LineString non è valido a causa di segmenti sovrapposti dovuti ad anomalie, ad esempio tracce GPS imprecise. STLength() non rimuove i segmenti sovrapposti o non validi, i quali vengono inclusi nel valore di lunghezza restituito. È possibile rimuovere i segmenti sovrapposti da un oggetto LineString con il metodo MakeValid().  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `LineString` e viene utilizzato `STLength()` per trovare la lunghezza dell'istanza.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STLength();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geometria](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

