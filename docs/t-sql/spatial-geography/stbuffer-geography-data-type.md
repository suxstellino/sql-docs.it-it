---
description: STBuffer (tipo di dati geography)
title: STBuffer (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STBuffer (geography Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geography Data Type)
ms.assetid: cb4deab8-642b-44d9-b3d9-85114d64021e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5b8b15017c53da5fe6020742fe5d3b5cdc5be1da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186007"
---
# <a name="stbuffer-geography-data-type"></a>STBuffer (tipo di dati geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce un oggetto geografico che rappresenta l'unione di tutti i punti la cui distanza da un'istanza **geography** è minore o uguale a un valore specificato.  
  
 Questo metodo con tipo di dati geography supporta le istanze **FullGlobe** o le istanze spaziali con dimensioni maggiori di un emisfero.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *distance*  
 Valore di tipo **float** (**double** in .NET Framework) che specifica la distanza dall'istanza **geography** intorno alla quale calcolare il buffer.  
  
 La distanza massima del buffer non può superare 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 della circonferenza della Terra) o l'intero globo.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
## <a name="remarks"></a>Osservazioni  
 STBuffer() calcola un buffer in modo analogo a [BufferWithTolerance](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md), specificando *tolerance* = abs(distance) \* 0,001 e *relative* = **false**.  
  
 Un buffer negativo consente di rimuovere tutti i punti all'interno della distanza specificata del limite dell'istanza **geography**.  
  
 `STBuffer()` restituisce un'istanza **FullGlobe** in determinati casi; ad esempio, `STBuffer()` restituisce un'istanza **FullGlobe** quando la distanza del buffer è maggiore della distanza dall'equatore ai poli. Un buffer non può superare l'intero globo.  
  
 Questo metodo genererà un'eccezione **ArgumentException** nelle istanze **FullGlobe** in cui la distanza del buffer supera il limite seguente:  
  
 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 della circonferenza della Terra)  
  
 Il limite massimo della distanza consente la massima flessibilità per la costruzione del buffer.  
  
 L'errore tra il buffer teorico e quello calcolato è max(tolerance, extents * 1.E-7) dove tolerance = distance \* 0,001. Per altre informazioni sulle estensioni, vedere la [Guida di riferimento ai metodi per il tipo di dati geography](./stequals-geography-data-type.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `LineString``geography`. e viene utilizzato `STBuffer()` per restituire l'area all'interno di 1 metro dell'istanza.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STBuffer(1).ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BufferWithTolerance &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)   
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
