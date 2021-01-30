---
description: Tipi spaziali - geometry (Transact-SQL)
title: geometry (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- geometry
dev_langs:
- TSQL
helpviewer_keywords:
- spatial data types [SQL Server]
- geometry data type [SQL Server], Transact-SQL
ms.assetid: 3fefdf7b-f931-404c-821c-82c0375eaf51
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 4889317b7492a5f51657d38cf2e71001f890e048
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180150"
---
# <a name="spatial-types---geometry-transact-sql"></a>Tipi spaziali - geometry (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Il tipo di dati spaziali planari **geometry** è implementato come tipo di dati CLR (Common Language Runtime) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo tipo rappresenta i dati in un sistema di coordinate euclideo (piano).  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è supportato un set di metodi per il tipo di dati spaziali **geometry**. Questi metodi includono metodi su **geometry** definiti dallo standard OGC (Open Geospatial Consortium) e da un set di estensioni [!INCLUDE[msCoName](../../includes/msconame-md.md)] di tale standard.  
 
 La tolleranza agli errori dei metodi geometry può arrivare fino a 1,0e-7 * extent. Il termine extent indica la distanza massima approssimativa tra i punti dell'oggetto **geometry**.
  
## <a name="registering-the-geometry-type"></a>Registrazione del tipo geometry  
 Il tipo **geometry** è predefinito e disponibile in ogni database. È possibile creare colonne di tabella di tipo **geometry** e operare sui dati **geometry** nello stesso modo in cui si utilizzano gli altri tipi CLR. Questo tipo può essere utilizzato in colonne calcolate persistenti e non persistenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-showing-how-to-add-and-query-geometry-data"></a>R. Informazioni su come aggiungere ed eseguire una query su dati geometry  
 Nei due esempi seguenti viene illustrato come aggiungere ed eseguire query su dati di tipo geometry. Nel primo esempio viene creata una tabella con una colonna Identity e una colonna `geometry`, ovvero `GeomCol1`. Una terza colonna effettua il rendering della colonna `geometry` nella rappresentazione Well-Known Text (WKT) OGC (Open Geospatial Consortium) e utilizza il metodo `STAsText()` . Vengono quindi inserite due righe: in una riga è contenuta un'istanza `LineString` di `geometry`e in una seconda è contenuta un'istanza `Polygon` .  
  
```sql 
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
    ( id int IDENTITY (1,1),  
    GeomCol1 geometry,   
    GeomCol2 AS GeomCol1.STAsText() );  
GO  
  
INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0));  
  
INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('POLYGON ((0 0, 150 0, 150 150, 0 150, 0 0))', 0));  
GO  
```  
  
### <a name="b-returning-the-intersection-of-two-geometry-instances"></a>B. Restituzione dell'intersezione di due istanze geometry  
 Nel secondo esempio viene utilizzato il metodo `STIntersection()` per restituire i punti in cui si intersecano le due istanze `geometry` inserite in precedenza.  
  
```sql  
DECLARE @geom1 geometry;  
DECLARE @geom2 geometry;  
DECLARE @result geometry;  
  
SELECT @geom1 = GeomCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geom2 = GeomCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geom1.STIntersection(@geom2);  
SELECT @result.STAsText();  
```  
  
### <a name="c-using-geometry-in-a-computed-column"></a>C. Utilizzo del tipo geometry in una colonna calcolata  
 Nell'esempio seguente viene creata una tabella con una colonna calcolata persistente usando un tipo **geometry**.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable  
(  
    locationId int IDENTITY(1,1),  
    location geometry,  
    deliveryArea as location.STBuffer(10) persisted  
)  
```  
  
## <a name="see-also"></a>Vedere anche  
  [Dati spaziali &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
