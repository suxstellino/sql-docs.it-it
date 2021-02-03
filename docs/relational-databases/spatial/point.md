---
description: Point
title: Punto | Microsoft Docs
ms.date: 02/02/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Point geometry subtype [SQL Server]
- geometry data type [SQL Server], spatial data
ms.assetid: 2a596ec4-8b2f-4962-bcb4-e5c8f77edad5
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bcbdda427a587e06bf1ad678939b8164178112dd
ms.sourcegitcommit: 05fc736e6b6b3a08f503ab124c3151f615e6faab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478576"
---
# <a name="point"></a>Point
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  Nei dati spaziali [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un **punto** è un oggetto senza dimensioni che rappresenta una sola posizione e può contenere valori Z (innalzamento) e M (misura).  
  
## <a name="geography-data-type"></a>Tipo di dati geography  
 Il tipo Punto del tipo di dati geography rappresenta una singola posizione in cui *Lat* indica la latitudine e *Long* la longitudine. I valori di latitudine e longitudine vengono misurati in gradi. I valori della latitudine sono compresi sempre nell'intervallo [-90, 90], quelli al di fuori genereranno un'eccezione. I valori della longitudine sono compresi sempre nell'intervallo [-180, 180], quelli al fuori, per rientrare in tale intervallo, vengono arrotondati. Ad esempio, se il valore immesso per la longitudine è 190, verrà arrotondato a -170. *SRID* rappresenta l'ID di riferimento spaziale dell'istanza **geography** da restituire.  
  
## <a name="geometry-data-type"></a>Tipo di dati geometry  
 Il tipo Punto per il tipo di dati geometry rappresenta una singola posizione in cui *X* e *Y* rappresentano rispettivamente le coordinate X e Y del punto generato. *SRID* rappresenta l'ID di riferimento spaziale dell'istanza **geometry** da restituire.  
  
## <a name="examples"></a>Esempi  
### <a name="example-a"></a>Esempio A.
Nell'esempio seguente viene creata un'istanza del punto di geometria che rappresenta il punto (3, 4) con un SRID pari a 0.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT (3 4)', 0);  
```  
  
### <a name="example-b"></a>Esempio B.
Nell'esempio seguente viene creata un'istanza del punto di geometria che rappresenta il punto (3, 4) con un valore Z (innalzamento) pari a 7, un valore M (misura) di 2,5 e l'identificatore SRID predefinito 0.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POINT(3 4 7 2.5)');  
```  
  
### <a name="example-c"></a>Esempio C.
Nell'esempio seguente vengono restituiti i valori X, Y, Z e M per l'istanza del punto di geometria.  
  
```  
SELECT @g.STX;  
SELECT @g.STY;  
SELECT @g.Z;  
SELECT @g.M;  
```  
  
### <a name="example-d"></a>Esempio D.
I valori Z e M possono essere specificati in modo esplicito come NULL, così come mostrato nell'esempio seguente.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POINT(3 4 NULL NULL)');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [MultiPoint](../../relational-databases/spatial/multipoint.md)   
 [STX &#40;Tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stx-geometry-data-type.md)   
 [STY &#40;Tipo di dati geometry&#41;](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [Dati spaziali &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
