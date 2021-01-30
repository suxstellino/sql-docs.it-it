---
description: STPointN (tipo di dati geography)
title: STPointN (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STPointN_TSQL
- STPointN (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STPointN method
ms.assetid: 47670feb-b9e0-4b4b-af83-b9bba7da66ac
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b871061f6e2d90471ee006184fed2bf438fb8fa2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190546"
---
# <a name="stpointn-geography-data-type"></a>STPointN (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il punto specificato in un'istanza **geography**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.STPointN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Espressione **int** compresa tra 1 e il numero di punti nell'istanza **geography**.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
 Tipo CLR restituito: **SqlGeography**  
  
 Tipo OGC (Open Geospatial Consortium): **Point**  
  
## <a name="remarks"></a>Osservazioni  
 Se un'istanza **geography** è creata dall'utente, restituisce il punto specificato da *expression* ordinando i punti con l'ordine di immissione originale.  
  
 Se un'istanza **geography** è costruita dal sistema, STPointN() restituisce il punto specificato da *expression* ordinando tutti i punti nello stesso ordine di restituzione, ovvero in primo luogo in base all'istanza **geography**, quindi in base all'anello all'interno dell'istanza (se appropriato), infine in base ai punti all'interno dell'anello. Questo ordine è deterministico.  
  
 Se questo metodo viene chiamato con un valore minore di 1, genera un'eccezione **ArgumentOutOfRangeException**.  
  
 Se questo metodo viene chiamato con un valore maggiore del numero di punti nell'istanza, restituisce Null.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata un'istanza `LineString` e viene utilizzato `STPointN()` per recuperare il secondo punto della descrizione dell'istanza.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STPointN(2).ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi OGC sulle istanze di geografia](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
