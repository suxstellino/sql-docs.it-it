---
description: Creare, Costruire e Istanze geografiche di Query
title: Creare, costruire ed eseguire query di istanze geografiche | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- geography data type [SQL Server]
- geodetic data type [SQL Server]
- geography data type [SQL Server], about geography data type
ms.assetid: b585851e-d15b-411f-adeb-aeabeb777c0b
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b6bbe58d7e21c5ef44c8e830daf4c69f754dcdfb
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107584075"
---
# <a name="create-construct-and-query-geography-instances"></a>Creare, Costruire e Istanze geografiche di Query
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   Il tipo di dati spaziali geografici, **geography**, rappresenta i dati in un sistema di coordinate terrestri. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]è implementato come tipo di dati CLR (Common Language Runtime) .NET. Il tipo di dati  **geography** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di archiviare dati ellissoidali (terra rotonda), ad esempio coordinate di latitudine e longitudine GPS.  
  
 Il tipo **geography** è predefinito e disponibile in ogni database. È possibile creare colonne di tabella di tipo **geography** e usare dati **geography** nello stesso modo in cui vengono usati gli altri tipi forniti dal sistema.  
  
##  <a name="creating-or-constructing-a-new-geography-instance"></a><a name="creating"></a> Creazione o costruzione di una nuova istanza geografica  
  
###  <a name="creating-a-new-geography-instance-from-an-existing-instance"></a><a name="existing"></a> Creazione di una nuova istanza geografica da un'istanza esistente  
 Il tipo di dati **geography** offre molti metodi predefiniti che è possibile usare per creare nuove istanze di **geography** in base a quelle esistenti.  
  
 **Per creare un buffer relativo a una geografia**  
 [STBuffer &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)  
  
 **Per creare un buffer relativo a una geografia, consentendo una certa tolleranza**  
 [BufferWithTolerance &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)  
  
 **Per creare una geografia dall'intersezione di due istanze di geografia**  
 [STIntersection &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **Per creare una geografia dall'unione di due istanze di geografia**  
 [STUnion &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stunion-geography-data-type.md)  
  
 **Per creare una geografia partendo dai punti in cui una non si sovrappone all'altra**  
 [STDifference &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
###  <a name="constructing-a-geography-instance-from-well-known-text-input"></a><a name="wkt"></a> Costruzione di un'istanza geografica dall'input WKT (well-known text)  
 Il tipo di dati **geography** fornisce molti metodi predefiniti che generano una geometria dalla rappresentazione WKT OGC (Open Geospatial Consortium). Lo standard WKT è una stringa di testo che consente ai dati geografici di essere scambiati in formato testuale.  
  
 **Per costruire qualsiasi tipo di istanza geografica dall'input WKT**  
 [STGeomFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)  
  
 [Parse &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/parse-geography-data-type.md)  
  
 **Per costruire un'istanza geografica Point dall'input WKT**  
 [STPointFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stpointfromtext-geography-data-type.md)  
  
 **Per costruire un'istanza geografica MultiPoint dall'input WKT**  
 [STMPointFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stmpointfromtext-geography-data-type.md)  
  
 **Per costruire un'istanza geografica LineString dall'input WKT**  
 [STLineFromText &#40;dati geography&#41;](../../t-sql/spatial-geography/stlinefromtext-geography-data-type.md) 
  
 **Per costruire un'istanza geografica MultiLineString dall'input WKT**  
 [STMLineFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stmlinefromtext-geography-data-type.md)  
  
 **Per costruire un'istanza geografica Polygon dall'input WKT**  
 [STPolyFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stpolyfromtext-geography-data-type.md)  
  
 **Per costruire un'istanza geografica MultiPolygon dall'input WKT**  
 [STMPolyFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stmpolyfromtext-geography-data-type.md)  
  
 **Per costruire un'istanza geografica GeometryCollection dall'input WKT**  
 [STGeomCollFromText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stgeomcollfromtext-geography-data-type.md)  
  
###  <a name="constructing-a-geography-instance-from-well-known-binary-input"></a><a name="wkb"></a> Costruzione di un'istanza geografica dall'input WKB (well-known binary)  
 WKB è un formato binario definito da OGC che consente lo scambio di dati **Geography** tra un'applicazione client e un database SQL. Le seguenti funzioni accettano l'input WKB per costruire le istanze geografiche:  
  
 **Per costruire qualsiasi tipo di istanza geografica dall'input WKB**  
 [STGeomFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stgeomfromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica Point dall'input WKB**  
 [STPointFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stpointfromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica MultiPoint dall'input WKB**  
 [STMPointFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stmpointfromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica LineString dall'input WKB**  
 [STLineFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stlinefromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica MultiLineString dall'input WKB**  
 [STMLineFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stmlinefromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica Polygon dall'input WKB**  
 [STPolyFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stpolyfromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica MultiPolygon dall'input WKB**  
 [STMPolyFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stmpolyfromwkb-geography-data-type.md)  
  
 **Per costruire un'istanza geografica GeometryCollection dall'input WKB**  
 [STGeomCollFromWKB &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stgeomcollfromwkb-geography-data-type.md)STGeomCollFromWKB (tipo di dati geography)  
  
###  <a name="constructing-a-geography-instance-from-gml-text-input"></a><a name="gml"></a> Costruzione di un'istanza geografica dall'input di testo GML  
 Il tipo di dati **geography** include un metodo che genera un'istanza **geography** da GML, una rappresentazione XML di un'istanza **geography** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta un subset di GML.  
  
 Per altre informazioni su Geography Markup Language (GML), vedere la specifica OGC [OGC Specifications, Geography Markup Language](https://go.microsoft.com/fwlink/?LinkId=93629)(Specifiche OGC, Geography Markup Language).  
  
 **Per costruire qualsiasi tipo di istanza geografica dall'input GML**  
 [GeomFromGML &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/geomfromgml-geography-data-type.md)  
  
##  <a name="returning-well-known-text-and-well-known-binary-from-a-geography-instance"></a><a name="returning"></a> Restituzione di WKT e WKB da un'istanza geografica  
 È possibile usare i metodi seguenti per restituire il formato WKT o WKB di un'istanza **geography** :  
  
 **Per restituire la rappresentazione WKT di un'istanza di geografia**  
 [STAsText &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stastext-geography-data-type.md)  
  
 [ToString &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/tostring-geography-data-type.md)  
  
 **Per restituire la rappresentazione WKT di un'istanza di geografia, con qualsiasi valore Z e M.**  
 [AsTextZM &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
 **Per restituire la rappresentazione WKB di un'istanza geografica**  
 [STAsBinary &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stasbinary-geography-data-type.md)  
  
 **Per restituire la rappresentazione GML di un'istanza geografica**  
 [AsGml &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/asgml-geography-data-type.md)  
  
##  <a name="querying-the-properties-and-behaviors-of-geography-instances"></a><a name="query"></a> Esecuzione di query sulle proprietà e i comportamenti delle istanze geografiche  
 A tutte le istanze **geography** sono associate diverse proprietà che possono essere recuperate tramite metodi disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Negli argomenti seguenti vengono definite le proprietà e i comportamenti dei tipi di geografia, nonché i metodi per l'esecuzione di query per ognuno di essi.  
  
###  <a name="validity-instance-type-and-geometrycollection-information"></a><a name="valid"></a> Informazioni sulla validità, sul tipo di istanza e su GeometryCollection  
 Dopo aver costruito un'istanza **geography** , è possibile usare i metodi seguenti per restituire il tipo di istanza o, se si tratta di un'istanza **GeometryCollection** , per restituire un'istanza **geography** .  
  
 **Per restituire il tipo di istanza di una geografia**  
 [STGeometryType &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stgeometrytype-geography-data-type.md)  
  
 **Per determinare se una geografia è un tipo di istanza specificato**  
 [InstanceOf &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/instanceof-geography-data-type.md)  
  
 **Per determinare se il formato di un'istanza di geografia è corretto per il tipo di istanza**  
 [STNumGeometries &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stnumgeometries-geography-data-type.md)  
  
 **Per restituire una specifica geografia in un'istanza GeometryCollection**  
 [STGeometryN &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stgeometryn-geography-data-type.md)STGeometryN (tipo di dati geography)  
  
###  <a name="number-of-points"></a><a name="number"></a> Numero di punti  
 Tutte le istanze **geography** non vuote sono costituite da *punti*. Questi punti rappresentano le coordinate di latitudine e longitudine terrestri sulle quali vengono tracciate le istanze **geography** . Il tipo di dati **geography** offre numerosi metodi predefiniti per l'esecuzione di query sui punti di un'istanza.  
  
 **Per restituire il numero di punti che comprendono un'istanza**  
 [STNumPoints &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)  
  
 **Per restituire un punto specifico in un'istanza**  
 [STPointN &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **Per restituire il punto di inizio di un'istanza**  
 [STStartPoint &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/ststartpoint-geography-data-type.md)  
  
 **Per restituire il punto di fine di un'istanza**  
 [STEndpoint &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stendpoint-geography-data-type.md)  
  
###  <a name="dimension"></a><a name="dimension"></a> Dimensione  
 Un'istanza **geography** non vuota può essere a 0, 1 o 2 dimensioni. Le istanze **geography** senza dimensioni, come **Point** e **MultiPoint**, non hanno lunghezza o area. Gli oggetti unidimensionali come **LineString, CircularString**, **CompoundCurve** e **MultiLineString** hanno una lunghezza. Le istanze bidimensionali come **Polygon, CurvePolygon** e **MultiPolygon**. Le istanze vuote indicano una dimensione pari a -1 e **GeometryCollection** indica le dimensioni massime del contenuto.  
  
 **Per restituire la dimensione di un'istanza**  
 [STDimension &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stdimension-geography-data-type.md)  
  
 **Per restituire la lunghezza di un'istanza**  
 [STLength &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stlength-geography-data-type.md)  
  
 **Per restituire l'area di un'istanza**  
 [STArea &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/starea-geography-data-type.md)  
  
###  <a name="empty"></a><a name="empty"></a> Vuoto  
 Un'istanza _vuota_ di tipo **geography** non contiene punti. La lunghezza delle istanze **LineString, CircularString**, **CompoundCurve** e **MultiLineString** vuote è pari a 0. L'area delle istanze **Polygon, CurvePolygon** e **MultiPolygon** vuote è pari a 0.  
  
 **Per determinare se un'istanza è vuota**  
 [STIsEmpty &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stisempty-geography-data-type.md)  
  
###  <a name="closure"></a><a name="closure"></a> Chiusura  
 Un'istanza **geography**_chiusa_ è una figura i cui punti di inizio e di fine corrispondono. Le istanze **Polygon** sono considerate chiuse. Le istanze **Point** non sono considerate chiuse.  
  
 Un anello è un'istanza **LineString** semplice chiusa.  
  
 **Per determinare se un'istanza è chiusa**  
 [STIsClosed &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stisclosed-geography-data-type.md)  
  
 **Per restituire il numero di anelli in un'istanza Polygon**  
 [NumRings &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/numrings-geography-data-type.md)  
  
 **Per restituire un anello specificato di un'istanza di geografia**  
 [RingN &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/ringn-geography-data-type.md)  
  
###  <a name="spatial-reference-id-srid"></a><a name="srid"></a> Identificatore SRID  
 L'identificatore SRID specifica in quale sistema di coordinate ellissoidale è rappresentata l'istanza **geography** . Non è possibile confrontare due istanze **geography** con identificatori SRID diversi.  
  
 **Per impostare o restituire l'identificatore SRID di un'istanza**  
 [STSrid &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stsrid-geography-data-type.md)  
  
 Questa proprietà può essere modificata.  
  
##  <a name="determining-relationships-between-geography-instances"></a><a name="rel"></a> Determinazione delle relazioni esistenti tra istanze geografiche  
 Il tipo di dati **geography** offre molti metodi predefiniti che è possibile usare per determinare relazioni tra due istanze **geography** .  
  
 **Per determinare se due istanze includono lo stesso punto impostato**  
 [STEquals &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **Per determinare se due istanze sono disgiunte**  
 [STDisjoint &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **Per determinare se due istanze si intersecano**  
 [STIntersects &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **Per determinare il punto o i punti in cui due istanze si intersecano**  
 [STIntersection &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **Per determinare la distanza più breve tra punti in due istanze di geografia**  
 [STDistance &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
 **Per determinare la differenza in termini di punti tra due istanze di geografia**  
 [STDifference &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
 **Per dedurre la differenza simmetrica o i punti univoci di un'istanza geografica a confronto con un'altra**  
 [STSymDifference &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/stsymdifference-geography-data-type.md)  
  
##  <a name="geography-instances-must-use-supported-srid"></a><a name="supportedsrid"></a> Le istanze geografiche devono utilizzare l'identificatore SRID supportato  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta gli identificatori SRID basati sugli standard EPSG. È necessario usare un identificatore SRID supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]per le istanze **geography** se si eseguono calcoli o si usano metodi con dati spaziali geografici. L'identificatore SRID deve corrispondere a uno di quelli visualizzati nella vista del catalogo **sys.spatial_reference_systems** Come indicato in precedenza, quando si eseguono i calcoli sui dati spaziali usando il tipo di dati **geography** i risultati dipenderanno dal tipo di ellissoide usato nella creazione dei dati perché a ogni ellissoide è assegnato un identificatore SRID specifico.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa l'identificatore predefinito SRID 4326, che esegue il mapping al sistema di riferimento spaziale WGS 84 quando si usano metodi nelle istanze **geography** . Se si utilizzano dati da un sistema di riferimento spaziale diverso da WGS 84 (o SRID 4326), sarà necessario determinare lo specifico identificatore SRID per i dati spaziali geografici.  
  
##  <a name="examples"></a><a name="examples"></a> Esempi  
Negli esempi seguenti viene illustrato come aggiungere ed eseguire query su dati geography.  
  
### <a name="example-a"></a>Esempio A. 
In questo esempio viene creata una tabella con una colonna di identità e una colonna `geography``GeogCol1`. Una terza colonna effettua il rendering della colonna `geography` nella rappresentazione Well-Known Text (WKT) OGC (Open Geospatial Consortium) e utilizza il metodo `STAsText()` . Vengono quindi inserite due righe: in una riga è contenuta un'istanza `LineString` di `geography`e in una seconda è contenuta un'istanza `Polygon` .  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
  ( id int IDENTITY (1,1),  
    GeogCol1 geography,   
    GeogCol2 AS GeogCol1.STAsText()
   );  
GO  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326));  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
GO  
```  
  
### <a name="example-b"></a>Esempio B.
In questo esempio viene usato il metodo `STIntersection()` per restituire i punti in cui le due istanze `geography` inserite in precedenza si intersecano.  
  
```sql  
DECLARE @geog1 geography;  
DECLARE @geog2 geography;  
DECLARE @result geography;  
  
SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geog1.STIntersection(@geog2);  
SELECT @result.STAsText();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati spaziali &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
