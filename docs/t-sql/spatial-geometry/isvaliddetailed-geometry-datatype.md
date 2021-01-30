---
description: IsValidDetailed (tipo di dati geometry)
title: IsValidDetailed (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- IsValidDetailed geometry
ms.assetid: 5a31e88a-ad7b-4ef7-b773-e2571f1cb3aa
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 812819918da17184029d3b28d2971f9cd85cd7a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159411"
---
# <a name="isvaliddetailed-geometry-datatype"></a>IsValidDetailed (tipo di dati geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un messaggio che aiuta a identificare i problemi con un oggetto spaziale non valido. Quando l'oggetto non è valido, viene restituito solo il primo errore. Quando l'oggetto è valido, viene restituito il valore 24400.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.IsValidDetailed()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **nvarchar(max)**  
  
 Tipo CLR restituito: **string**  
  
## <a name="remarks"></a>Osservazioni  
 Nella tabella seguente sono elencati i valori restituiti possibili:  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|24400|Valido|  
|24401|Non valido, motivo sconosciuto.|  
|24402|Non valido perché il punto {0} è un punto isolato, che non è valido in questo tipo di oggetto.|  
|24403|Non valido perché vi sono coppie di bordi del poligono che si sovrappongono.|  
|24404|Non valido perché l'anello del poligono {0} interseca se stesso o un altro anello.|  
|24405|Non valido perché una parte dell'anello del poligono interseca se stessa o un altro anello.|  
|24406|Non valido perché la curva {0} degenera in un punto.|  
|24407|Non valido perché l'anello del poligono {0} si comprime in una linea nel punto {1}.|  
|24408|Non valido perché l'anello del poligono {0} non è chiuso.|  
|24409|Non valido perché una parte dell'anello del poligono {0} si trova all'interno di un poligono.|  
|24410|Non valido perché l'anello {0} è il primo anello in un poligono di cui non costituisce l'anello esterno.|  
|24411|Non valido perché l'anello {0} si trova al di fuori dell'anello esterno {1} del relativo poligono.|  
|24412|Non valido perché l'interno di un poligono con gli anelli {0} e {1} non è connesso.|  
|24413|Non valido a causa di due bordi sovrapposti nella curva {0}.|  
|24414|Non valido perché un bordo della curva {0} si sovrappone a un bordo della curva {1}.|  
|24415|Non valido perché in qualche poligono è presente una struttura dell'anello non valida.|  
|24416|Non valido perché nella curva {0} il bordo che inizia al punto {1} è una linea o un arco degenerato con estremità diametralmente opposte.|  
  
## <a name="examples"></a>Esempi  
 L'esempio seguente di oggetto spaziale non valido illustra il comportamento del metodo **IsValidDetailed()**.  
  
```sql  
DECLARE @p GEOMETRY = 'Polygon((2 2, 4 4, 4 2, 2 4, 2 2))'  
SELECT @p.IsValidDetailed()  
--Returns: 24404: Not valid because polygon ring (1) intersects itself or some other ring.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi estesi sulle istanze di geometria](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

