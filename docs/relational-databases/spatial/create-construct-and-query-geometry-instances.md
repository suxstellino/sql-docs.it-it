---
description: Creazione, costruzione e query di istanze geometry
title: Creare, costruire ed eseguire query di istanze di geometria | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- planar spatial data [SQL Server], getting started
- geometry data type [SQL Server], getting started
ms.assetid: c6b5c852-37d2-48d0-a8ad-e43bb80d6514
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ddc0df0e6949ed429d415940fe9fda4263d3190a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127714"
---
# <a name="create-construct-and-query-geometry-instances"></a>Creazione, costruzione e query di istanze geometry
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  Il tipo di dati spaziali planare **geometry** rappresenta i dati in un sistema di coordinate euclideo (piano). Questo tipo è implementato come tipo di dati CLR (Common Language Runtime) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Il tipo **geometry** è predefinito e disponibile in ogni database. È possibile creare colonne di tabella di tipo **geometry** e operare sui dati **geometry** nello stesso modo in cui si utilizzano gli altri tipi CLR.  
  
 Il tipo di dati **geometry** (planare) supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è conforme allo standard Open Geospatial Consortium (OGC) Simple Features for SQL Specification versione 1.1.0.  
  
 Per ulteriori informazioni sulle specifiche OGC, vedere quanto riportato di seguito:  
  
-   [OGC Specifications, Simple Feature Access Part 1 - Common Architecture](https://go.microsoft.com/fwlink/?LinkId=93628)  
  
-   [OGC Specifications, Simple Feature Access Part 2 - SQL Options](https://go.microsoft.com/fwlink/?LinkId=93629) (Specifiche OGC, accesso semplificato alle caratteristiche Parte 2 - Opzioni SQL)  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta un subset dello standard GML 3.1 esistente che viene definito nello schema seguente: [https://schemas.microsoft.com/sqlserver/profiles/gml/SpatialGML.xsd](https://go.microsoft.com/fwlink/?LinkId=230959).  
  
##  <a name="creating-or-constructing-a-new-geometry-instance"></a><a name="creating"></a> Creazione o costruzione di una nuova istanza geometry  
  
###  <a name="creating-a-new-geometry-instance-from-an-existing-instance"></a><a name="existing"></a> Creazione di una nuova istanza geometry da un'istanza esistente  
 Il tipo di dati **geometry** offre molti metodi predefiniti che è possibile usare per creare nuove istanze di **geometry** in base a quelle esistenti.  
  
 **Per creare un buffer relativo a una geometria**  
 [STBuffer &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stbuffer-geometry-data-type.md)  
  
 [BufferWithTolerance &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)  
  
 **Per creare una versione semplificata di una geometria**  
 [Reduce &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/reduce-geometry-data-type.md)  
  
 **Per creare la struttura convessa di una geometria**  
 [STConvexHull &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stconvexhull-geometry-data-type.md)  
  
 **Per creare una geometria dall'intersezione di due geometrie**  
 [STIntersection &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stintersection-geometry-data-type.md)  
  
 **Per creare una geometria dall'unione di due geometrie**  
 [STUnion &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stunion-geometry-data-type.md)  
  
 **Per creare una geometria dai punti in cui una non si sovrappone all'altra**  
 [STDifference &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stdifference-geometry-data-type.md)  
  
 **Per creare una geometria dai punti in cui due geometrie non si sovrappongono**  
 [STSymDifference &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stsymdifference-geometry-data-type.md)  
  
 **Per creare un'istanza Point arbitraria che giace su una geometria esistente**  
 [STPointOnSurface &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)  
  
  
###  <a name="constructing-a-geometry-instance-from-well-known-text-input"></a><a name="wkt"></a> Costruzione di un'istanza geometry da un input WKT (well-known text)  
 Il tipo di dati **geometry** fornisce molti metodi predefiniti che generano una geometria dalla rappresentazione WKT OGC (Open Geospatial Consortium). Lo standard WKT è una stringa di testo che consente lo scambio di dati geometrici in formato testuale.  
  
 **Per costruire qualsiasi tipo di istanza geometry da input WKT**  
 [STGeomFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeomfromtext-geometry-data-type.md)  
  
 [Parse &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/parse-geometry-data-type.md)  
  
 **Per costruire un'istanza di geometria Point dall'input WKT**  
 [STPointFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpointfromtext-geometry-data-type.md)  
  
 **Per costruire un'istanza MultiPoint di tipo geometry da un input WKT**  
 [STMPointFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stmpointfromtext-geometry-data-type.md)  
  
 **Per costruire un'istanza LineString di tipo geometry da un input WKT**  
 [STLineFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stlinefromtext-geometry-data-type.md)  
  
 **Per costruire un'istanza MultiLineString di tipo geometry da un input WKT**  
 [STMLineFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stmlinefromtext-geometry-data-type.md)  
  
 **Per costruire un'istanza Polygon di tipo geometry da un input WKT**  
 [STPolyFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpolyfromtext-geometry-data-type.md)  
  
 **Per costruire un'istanza MultiPolygon di tipo geometry da un input WKT**  
 [STMPolyFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stmpolyfromtext-geometry-data-type.md)  
  
 **Per costruire un'istanza GeometryCollection di tipo geometry da un input WKT**  
 [STGeomCollFromText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeomcollfromtext-geometry-data-type.md)  
  
  
###  <a name="constructing-a-geometry-instance-from-well-known-binary-input"></a><a name="wkb"></a> Costruzione di un'istanza geometry da un input WKB (well-known binary)  
 WKB è un formato binario definito da OGC (Open Geospatial Consortium) che consente lo scambio di dati **geometry** tra un'applicazione client e un database SQL. Le funzioni seguenti accettano input WKB per costruire le geometrie:  
  
 **Per costruire qualsiasi tipo di istanza geometry da un input WKB**  
 [STGeomFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeomfromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza Point di tipo geometry da un input WKB**  
 [STPointFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpointfromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza MultiPoint di tipo geometry da un input WKB**  
 [STMPointFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stmpointfromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza LineString di tipo geometry da un input WKB**  
 [STLineFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stlinefromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza MultiLineString di tipo geometry da un input WKB**  
 [STMLineFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stmlinefromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza Polygon di tipo geometry da un input WKB**  
 [STPolyFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpolyfromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza MultiPolygon di tipo geometry da un input WKB**  
 [STMPolyFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stmpolyfromwkb-geometry-data-type.md)  
  
 **Per costruire un'istanza GeometryCollection di tipo geometry da un input WKB**  
 [STGeomCollFromWKB &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeomcollfromwkb-geometry-data-type.md)  
  
  
###  <a name="constructing-a-geometry-instance-from-gml-text-input"></a><a name="gml"></a> Costruzione di un'istanza geometry da un input di testo GML  
 Il tipo di dati **geometry** include un metodo che genera un'istanza **geometry** da GML, una rappresentazione XML di oggetti geometrici. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta un subset di GML.  
  
 **Per costruire qualsiasi tipo di istanza geometry da un input GML**  
 [GeomFromGml &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/geomfromgml-geometry-data-type.md)  
  
  
##  <a name="returning-well-known-text-and-well-known-binary-from-a-geometry-instance"></a><a name="returning"></a> Restituzione di WKT e WKB da un'istanza geometry  
 È possibile utilizzare i metodi seguenti per restituire il formato WKT o WKB di un'istanza **geometry** :  
  
 **Per restituire la rappresentazione WKT di un'istanza geometry**  
 [STAsText &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stastext-geometry-data-type.md)  
  
 [ToString &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/tostring-geometry-data-type.md)  
  
 **Per restituire la rappresentazione WKT di un'istanza geometry, con qualsiasi valore Z e M.**  
 [AsTextZM &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/astextzm-geometry-data-type.md)  
  
 **Per restituire la rappresentazione WKB di un'istanza geometry**  
 [STAsBinary &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stasbinary-geometry-data-type.md)  
  
 **Per restituire la rappresentazione GML di un'istanza geometry**  
 [AsGml &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/asgml-geometry-data-type.md)  
  
  
##  <a name="querying-the-properties-and-behaviors-of-geometry-instances"></a><a name="querying"></a> Esecuzione di query sulle proprietà e i comportamenti delle istanze geometry  
 A tutte le istanze **geometry** sono associate diverse proprietà che possono essere recuperate tramite metodi disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Negli argomenti seguenti vengono definiti le proprietà e i comportamenti dei tipi di geometria, nonché i metodi per l'esecuzione di query per ognuno di essi.  
  
###  <a name="validity-instance-type-and-geometrycollection-information"></a><a name="valid"></a> Informazioni sulla validità, sul tipo di istanza e su GeometryCollection  
 Dopo aver costruito un'istanza **geometry** , è possibile usare i metodi seguenti per determinare se essa è corretta, per restituire il tipo di istanza o, se si tratta di un'istanza di raccolta, per restituire un'istanza **geometry** specifica.  
  
 **Per restituire il tipo di istanza di una geometria**  
 [STGeometryType &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)  
  
 **Per determinare se una geometria è un tipo di istanza specificato**  
 [InstanceOf &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/instanceof-geometry-data-type.md)  
  
 **Per determinare se un'istanza geometry è corretta per il tipo di istanza**  
 [STIsValid &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)  
  
 **Per convertire un'istanza geometry in un'istanza geometry corretta con un tipo di istanza**  
 [MakeValid &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)  
  
 **Per restituire il numero delle geometrie in un'istanza GeometryCollection**  
 [STNumGeometries &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stnumgeometries-geometry-data-type.md)  
  
 Per restituire una specifica geometria in un'istanza GeometryCollection  
 [STGeometryN &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stgeometryn-geometry-data-type.md)STGeometryN (tipo di dati geometry)  
  
  
###  <a name="number-of-points"></a><a name="number"></a> Numero di punti  
 Tutte le istanze **geometry** non vuote sono costituite da *punti*. Tali punti rappresentano le coordinate X e Y del piano su cui vengono tracciate le geometrie. Il tipo di dati **geometry** offre numerosi metodi predefiniti per l'esecuzione di query sui punti di un'istanza.  
  
 **Per restituire il numero di punti che comprendono un'istanza**  
 [STNumPoints &#40;geometry Data Type&#41; (STNumPoints &#40;tipo di dati geometry&#41;)](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)  
  
 **Per restituire un punto specifico in un'istanza**  
 [STPointN](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **Per restituire un punto arbitrario che si trova in un'istanza**  
 [STPointOnSurface](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)  
  
 **Per restituire il punto di inizio di un'istanza**  
 [STStartPoint](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)  
  
 **Per restituire il punto di fine di un'istanza**  
 [STEndpoint](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)  
  
 **Per restituire la coordinata X di un'istanza Point**  
 [STX &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stx-geometry-data-type.md)  
  
 **Per restituire la coordinata Y di un'istanza Point**  
 [STY](../../t-sql/spatial-geometry/sty-geometry-data-type.md)  
  
 **Per restituire il punto centrale geometrico di un'istanza Polygon, CurvePolygon o MultiPolygon**  
 [STCentroid](../../t-sql/spatial-geometry/stcentroid-geometry-data-type.md)  
  
  
###  <a name="dimension"></a><a name="dimension"></a> Dimensione  
 Un'istanza **geometry** non vuota può essere a 0, 1 o 2 dimensioni. Le istanze **geometry** senza dimensioni, come **Point** e **MultiPoint**, non hanno lunghezza o area. Gli oggetti unidimensionali come **LineString, CircularString, CompoundCurve** e **MultiLineString** hanno una lunghezza. Le istanze bidimensionali come **Polygon**, **CurvePolygon** e **MultiPolygon**. Per le istanze vuote verrà indicata una dimensione di -1 e per **GeometryCollection** verrà indicata un'area dipendente dai tipi del contenuto.  
  
 **Per restituire la dimensione di un'istanza**  
 [STDimension](../../t-sql/spatial-geometry/stdimension-geometry-data-type.md)  
  
 **Per restituire la lunghezza di un'istanza**  
 [STLength](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)  
  
 **Per restituire l'area di un'istanza**  
 [STArea](../../t-sql/spatial-geometry/starea-geometry-data-type.md)  
  
  
###  <a name="empty"></a><a name="empty"></a> Vuoto  
 Un'istanza _vuota_ di tipo **geometry** non contiene punti. La lunghezza delle istanze **LineString, CircularString**, **CompoundCurve** e **MultiLineString** vuote è pari a zero. L'area delle istanze **Polygon**, **CurvePolygon** e **MultiPolygon** vuote è pari a 0.  
  
 **Per determinare se un'istanza è vuota**  
 [STIsEmpty](../../t-sql/spatial-geometry/stisempty-geometry-data-type.md).  
  
  
###  <a name="simple"></a><a name="simple"></a> Simple  
 Affinché il tipo **geometry** dell'istanza sia *semplice*, deve soddisfare entrambi questi requisiti:  
  
-   Ogni figura dell'istanza non deve intersecarsi, salvo agli endpoint.  
  
-   Le figure dell'istanza non possono intersecarsi tra di loro in un punto non esistente in entrambi i loro limiti.  
  
> [!NOTE]  
>  Le geometrie vuote sono sempre semplici.  
  
 **Per determinare se un'istanza è semplice**  
 [STIsSimple](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md).  
  
  
###  <a name="boundary-interior-and-exterior"></a><a name="boundary"></a> Limite interno ed esterno  
 L' *interno* di un'istanza **geometry** è lo spazio occupato dall'istanza e l' *esterno* è lo spazio non occupato da essa.  
  
 Il *limite* è definito da OGC come segue:  
  
-   Le istanze **Point** e **MultiPoint** non hanno un limite.  
  
-   I limiti **LineString** e **MultiLineString** boundaries are formed by the start points e end points, removing those that occur an even number of times.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 1, 0 0, 1 0, 0 1), (1 1, 1 0))');  
SELECT @g.STBoundary().ToString();  
```  
  
 Il limite di un'istanza **Polygon** o **MultiPolygon** è il set dei suoi anelli.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0), (1 1, 1 2, 2 2, 2 1, 1 1))');  
SELECT @g.STBoundary().ToString();  
```  
  
 **Per restituire il limite di un'istanza**  
 [STBoundary](../../t-sql/spatial-geometry/stboundary-geometry-data-type.md)  
   
###  <a name="envelope"></a><a name="envelope"></a> Envelope  
 L' *envelope* di un'istanza **geometry** , nota anche come *rettangolo di selezione* è il rettangolo allineato all'asse formato dalle coordinate minime e massime (X, Y) dell'istanza.  
  
 **Per restituire l'envelope di un'istanza**  
 [STEnvelope](../../t-sql/spatial-geometry/stenvelope-geometry-data-type.md)  
  
###  <a name="closure"></a><a name="closure"></a> Chiusura  
 Un'istanza _geometry_**chiusa** è una figura i cui punti di inizio e di fine corrispondono. Le istanze **Polygon** sono considerate chiuse. Le istanze **Point** non sono considerate chiuse.  
  
 Un anello è un'istanza **LineString** semplice chiusa.  
  
 **Per determinare se un'istanza è chiusa**  
 [STIsClosed](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)  
  
 **Per determinare se un'istanza è un anello**  
 [STIsRing](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)  
  
 **Per restituire l'anello esterno di un'istanza Polygon**  
 [STExteriorRing](../../t-sql/spatial-geometry/stexteriorring-geometry-data-type.md)  
  
 **Per restituire il numero di anelli interni in un'istanza Polygon**  
 [STNumInteriorRing](../../t-sql/spatial-geometry/stnuminteriorring-geometry-data-type.md)  
  
 **Per restituire un anello interno specificato di un'istanza Polygon**  
 [STInteriorRingN](../../t-sql/spatial-geometry/stinteriorringn-geometry-data-type.md)  
  
  
###  <a name="spatial-reference-id-srid"></a><a name="srid"></a> Identificatore SRID  
 L'identificatore SRID specifica il sistema di coordinate in cui è rappresentata l'istanza **geometry** . Due istanze con identificatori SRID diversi non possono essere confrontate.  
  
 **Per impostare o restituire l'identificatore SRID di un'istanza**  
 [STSrid](../../t-sql/spatial-geometry/stsrid-geometry-data-type.md)  
  
> [!NOTE]
> Questa proprietà può essere modificata.  
  
##  <a name="determining-relationships-between-geometry-instances"></a><a name="rel"></a> Determinazione delle relazioni esistenti tra istanze geometry  
 Il tipo di dati **geometry** offre molti metodi predefiniti che è possibile usare per determinare relazioni tra due istanze **geometry** .  
  
 **Per determinare se due istanze includono lo stesso punto impostato**  
 [STEquals](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **Per determinare se due istanze sono disgiunte**  
 [STDisjoint](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **Per determinare se due istanze si intersecano**  
 [STIntersects](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **Per determinare se due istanze entrano in contatto**  
 [STTouches](../../t-sql/spatial-geometry/sttouches-geometry-data-type.md)  
  
 **Per determinare se due istanze si sovrappongono**  
 [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)  
  
 **Per determinare se due istanze si incrociano**  
 [STCrosses](../../t-sql/spatial-geometry/stcrosses-geometry-data-type.md)  
  
 **Per determinare se un'istanza è all'interno dell'altra**  
 [STWithin](../../t-sql/spatial-geometry/stwithin-geometry-data-type.md)  
  
 **Per determinare se un'istanza contiene l'altra**  
 [STContains](../../t-sql/spatial-geometry/stcontains-geometry-data-type.md)  
  
 **Per determinare se un'istanza si sovrappone all'altra**  
 [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)  
  
 **Per determinare se due istanze sono collegate a livello spaziale**  
 [STRelate](../../t-sql/spatial-geometry/strelate-geometry-data-type.md)  
  
 **Per determinare la distanza più breve tra punti in due geometrie**  
 [STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
##  <a name="geometry-instances-default-to-zero-srid"></a><a name="defaultsrid"></a> Istanze geometry con SRID zero per impostazione predefinita  
 L'identificatore SRID predefinito per le istanze **geometry** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è 0. Con i dati spaziali **geometry** lo specifico identificatore SRID dell'istanza spaziale non deve eseguire i calcoli. Di conseguenza le istanze possono risiedere nello spazio planare indefinito. Per indicare lo spazio planare indefinito nei calcoli di metodi del tipo di dati **geometry** , [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] SRID 0.  
  
##  <a name="examples"></a><a name="examples"></a> Esempi  
Nei due esempi seguenti viene illustrato come aggiungere ed eseguire query su dati di tipo geometry.  
  
### <a name="example-a"></a>Esempio A.
In questo esempio viene creata una tabella con una colonna di identità e una colonna `geometry``GeomCol1`. Una terza colonna effettua il rendering della colonna `geometry` nella rappresentazione Well-Known Text (WKT) OGC (Open Geospatial Consortium) e utilizza il metodo `STAsText()` . Vengono quindi inserite due righe: in una riga è contenuta un'istanza `LineString` di `geometry`e in una seconda è contenuta un'istanza `Polygon` .  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
DROP TABLE dbo.SpatialTable;  
GO  

CREATE TABLE SpatialTable   
  ( id int IDENTITY (1,1),  
    GeomCol1 geometry,   
    GeomCol2 AS GeomCol1.STAsText() 
  );  
GO  

INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0));  

INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('POLYGON ((0 0, 150 0, 150 150, 0 150, 0 0))', 0));  
GO  
```  
  
### <a name="example-b"></a>Esempio B.
In questo esempio viene usato il metodo `STIntersection()` per restituire i punti in cui le due istanze `geometry` inserite in precedenza si intersecano.  
  
```sql  
DECLARE @geom1 geometry;  
DECLARE @geom2 geometry;  
DECLARE @result geometry;  

SELECT @geom1 = GeomCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geom2 = GeomCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geom1.STIntersection(@geom2);  
SELECT @result.STAsText();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati spaziali &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
