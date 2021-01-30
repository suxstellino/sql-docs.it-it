---
description: BufferWithTolerance (tipo di dati geography)
title: BufferWithTolerance (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithTolerance_TSQL
- BufferWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- BefferWithTolerance method
ms.assetid: f1783e6b-0f17-464f-b1c7-1c3f7d8aa042
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 51be64effcbbb75abe7830949d8bdf83b5637307
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205052"
---
# <a name="bufferwithtolerance-geography-data-type"></a>BufferWithTolerance (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un oggetto geometrico che rappresenta l'unione dei valori di tutti i punti la cui distanza da un'istanza **geography** è minore o uguale a un valore specificato, consentendo una tolleranza specificata.  
  
Questo metodo con tipo di dati geography supporta le istanze **FullGlobe** o le istanze spaziali con dimensioni maggiori di un emisfero.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
_distance_  
Espressione **float** che specifica la distanza dall'istanza **geography** intorno alla quale calcolare il buffer.  
  
La distanza massima del buffer non può superare 0,999 \* _π_ * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 della circonferenza della Terra) o l'intero globo.  
  
_tolerance_  
Espressione **float** che specifica la tolleranza della distanza del buffer.  
  
Il valore _tolerance_ indica la variazione massima nella distanza ideale del buffer per l'approssimazione lineare restituita.  
  
La distanza ideale del buffer di un punto, ad esempio, è un cerchio, ma tale distanza deve essere approssimata da un poligono. Minore è la tolleranza, maggiore sarà il numero di punti del poligono. Ciò aumenta la complessità del risultato, ma riduce al minimo le possibilità di errore.  
  
Il limite minimo è lo 0,1 percento della distanza e qualsiasi tolleranza inferiore verrà arrotondata al limite minimo.  
  
_relative_  
Valore **bit** che specifica se il valore _tolerance_ è relativo o assoluto. Se il valore è "TRUE" o 1, la tolleranza è relativa. Il valore è il prodotto tra il parametro _tolerance_ e l'estensione angolare \* raggio equatoriale dell'ellissoide. La tolleranza è assoluta se il valore è "FALSE" o 0. Il valore _tolerance_ indica la variazione massima assoluta nella distanza ideale del buffer per l'approssimazione lineare restituita.  
  
## <a name="return-types"></a>Tipi restituiti  
Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
Tipo CLR restituito: **SqlGeography**  
  
## <a name="remarks"></a>Osservazioni  
Questo metodo genera un'eccezione **ArgumentException** se _distance_ non è un numero (NAN, Not-a-Number) o se _distance_ è un valore infinito positivo o negativo.  Questo metodo genera inoltre un'eccezione **ArgumentException** se _tolerance_ è zero (0), non un numero, un valore negativo o un valore infinito positivo o negativo.  
  
`STBuffer()` restituisce un'istanza **FullGlobe** in determinati casi; ad esempio, `STBuffer()` restituisce un'istanza **FullGlobe** su due poli quando la distanza del buffer è maggiore della distanza dall'equatore ai poli.  
  
Questo metodo genererà un'eccezione **ArgumentException** nelle istanze **FullGlobe** in cui la distanza del buffer supera il limite seguente:  
  
0,999 \* _π_ * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 della circonferenza della Terra)  
  
L'errore tra il buffer calcolato e quello teorico è max(tolerance, extents \* 1.E-7) dove tolerance è il valore del parametro _tolerance_. Per altre informazioni sulle estensioni, vedere la [Guida di riferimento ai metodi per il tipo di dati geography](./stequals-geography-data-type.md).  
  
Questo metodo non è preciso.  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene creata un'istanza `Point` e viene utilizzato `BufferWithTolerance()` per ottenere un buffer approssimato intorno all'istanza stessa.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
[STBuffer &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)   
[Metodi estesi sulle istanze geografiche](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
