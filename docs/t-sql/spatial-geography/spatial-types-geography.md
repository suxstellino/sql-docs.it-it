---
description: Tipi spaziali - geography
title: geography (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- geography
dev_langs:
- TSQL
helpviewer_keywords:
- geography data type [SQL Server], Transact-SQL
- spatial data types [SQL Server]
ms.assetid: d9e4952a-1841-4465-a64b-11e9288dba1d
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 46489eadd2c56fbccca62dfe415611e0f8f66a2a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88467478"
---
# <a name="spatial-types---geography"></a>Tipi spaziali - geography
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Il tipo di dati spaziali **geography** viene implementato come tipo di dati .NET Common Language Runtime (CLR) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo tipo rappresenta i dati in un sistema di coordinate di tipo terra rotonda. Il tipo di dati  **geography** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di archiviare dati ellissoidali (terra rotonda), ad esempio coordinate di latitudine e longitudine GPS.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è supportato un set di metodi per il tipo di dati spaziali **geography**. Sono inclusi metodi per **geography** definiti dallo standard OGC (Open Geospatial Consortium) e un set di estensioni [!INCLUDE[msCoName](../../includes/msconame-md.md)] a tale standard.  
 
 La tolleranza agli errori dei metodi **geography** può arrivare fino a 1,0e-7 * extent. Il termine extent indica la distanza massima approssimativa tra i punti dell'oggetto **geography**.
  

## <a name="registering-the-geography-type"></a>Registrazione di un tipo geography  
 Il tipo **geography** è predefinito e disponibile in ogni database. È possibile creare colonne di tabella di tipo **geography** e usare dati **geography** nello stesso modo in cui vengono usati gli altri tipi forniti dal sistema. Questo tipo può essere utilizzato in colonne calcolate persistenti e non persistenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-showing-how-to-add-and-query-geography-data"></a>R. Informazioni su come aggiungere ed eseguire una query su dati geography  
 Negli esempi seguenti viene illustrato come aggiungere ed eseguire query su dati geography. Nel primo esempio viene creata una tabella con una colonna Identity e una colonna `geography`, ovvero `GeogCol1`. Una terza colonna effettua il rendering della colonna `geography` nella rappresentazione Well-Known Text (WKT) OGC (Open Geospatial Consortium) e utilizza il metodo `STAsText()` . Vengono quindi inserite due righe: in una riga è contenuta un'istanza `LineString` di `geography`e in una seconda è contenuta un'istanza `Polygon` .  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
    ( id int IDENTITY (1,1),  
    GeogCol1 geography,   
    GeogCol2 AS GeogCol1.STAsText() );  
GO  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656 )', 4326));  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653 , -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
GO  
```  
  
### <a name="b-returning-the-intersection-of-two-geography-instances"></a>B. Restituzione dell'intersezione di due istanze geografiche  
 Nell'esempio seguente viene utilizzato il metodo `STIntersection()` per restituire i punti in cui si intersecano le due istanze `geography` inserite in precedenza.  
  
```sql  
DECLARE @geog1 geography;  
DECLARE @geog2 geography;  
DECLARE @result geography;  
  
SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geog1.STIntersection(@geog2);  
SELECT @result.STAsText();  
```  
  
### <a name="c-using-geography-in-a-computed-column"></a>C. Utilizzo del tipo geography in una colonna calcolata  
 Nell'esempio seguente viene creata una tabella con una colonna calcolata persistente usando un tipo **geography**.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable  
(  
    locationId int IDENTITY(1,1),  
    location geography,  
    deliveryArea as location.STBuffer(10) persisted  
);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati spaziali &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   

  
  
