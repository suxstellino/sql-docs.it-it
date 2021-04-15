---
title: Generare uno schema XSD inline | Microsoft Docs
description: Informazioni su come generare uno schema XSD inline usando l'opzione XMLSCHEMA nella clausola FOR XML di una query SQL.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XSD schemas [SQL Server]
- XMLSCHEMA option
- schemas [SQL Server], XML
- XDR schemas
- FOR XML clause, inline XSD schema generation
- inline XSD schema generation [SQL Server]
- XMLDATA option
ms.assetid: 04b35145-1cca-45f4-9eb7-990abf2e647d
author: rothja
ms.author: jroth
ms.openlocfilehash: d6f4282d9c134caf25052affc3132a2212e1fa49
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488211"
---
# <a name="generate-an-inline-xsd-schema"></a>Generazione di uno schema XSD inline
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In una clausola FOR XML è possibile richiedere che la query restituisca uno schema inline oltre ai risultati della query. Per ottenere uno schema XDR, utilizzare la parola chiave XMLDATA nella clausola FOR XML. Per ottenere uno schema XSD, utilizzare invece la parola chiave XMLSCHEMA.  
  
 In questo argomento viene descritta la parola chiave XMLSCHEMA e viene illustrata la struttura dello schema XSD risultante. Di seguito sono illustrate le limitazioni previste per la richiesta di schemi inline:  
  
-   È possibile specificare la parola chiave XMLSCHEMA unicamente nelle modalità RAW e AUTO e non nella modalità EXPLICIT.  
  
-   Se in una query FOR XML annidata è specificata la direttiva TYPE, il risultato della query sarà di tipo **xml** e verrà considerato come istanza di dati XML non tipizzati. Per altre informazioni, vedere [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md).  
  
 Se si specifica la parola chiave XMLSCHEMA in una query FOR XML, si otterrà sia uno schema che i dati XML, ovvero il risultato della query. Ogni elemento di livello principale dei dati fa riferimento allo schema precedente tramite una dichiarazione dello spazio dei nomi predefinito che, a sua volta, fa riferimento allo spazio dei nomi di destinazione dello schema inline.  
  
 Ad esempio:  
  
```  
<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">  
  <xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />  
  <xsd:element name="Production.ProductModel">  
    <xsd:complexType>  
      <xsd:attribute name="ProductModelID" type="sqltypes:int" use="required" />  
      <xsd:attribute name="Name" use="required">  
        <xsd:simpleType sqltypes:sqlTypeAlias="[AdventureWorks2012].[dbo].[Name]">  
          <xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033" sqltypes:sqlCompareOptions="IgnoreCase IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">  
            <xsd:maxLength value="50" />  
          </xsd:restriction>  
        </xsd:simpleType>  
      </xsd:attribute>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
<Production.ProductModel xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" ProductModelID="1" Name="Classic Vest" />  
```  
  
 Il risultato include XML Schema e il risultato XML. L'elemento di livello principale <`ProductModel`> nel risultato fa riferimento allo schema tramite la dichiarazione dello spazio dei nomi predefinito, xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1".  
  
 La parte del risultato relativa allo schema può contenere più documenti dello schema che descrivono più spazi dei nomi. Verranno restituiti almeno i due documenti di schema seguenti:  
  
-   Un documento di schema per lo spazio dei nomi **Sqltypes** e per il quale vengono restituiti i tipi SQL di base.  
  
-   Un altro documento di schema che descrive la forma del risultato della query FOR XML.  
  
 Se il risultato della query contiene tipi di dati **xml** tipizzati, verranno inclusi anche gli schemi associati a tali tipi di dati **xml** tipizzati.  
  
 Lo spazio dei nomi di destinazione del documento di schema che descrive la forma del risultato della query FOR XML contiene una parte fissa e una parte numerica che viene incrementata automaticamente. Di seguito è illustrato il formato di questo spazio dei nomi, dove *n* è un numero intero positivo. Ad esempio, nella query precedente urn:schemas-microsoft-com:sql:SqlRowSet1 rappresenta lo spazio dei nomi di destinazione.  
  
```  
urn:schemas-microsoft-com:sql:SqlRowSetn  
```  
  
 La modifica degli spazi dei nomi di destinazione nel risultato tra un'esecuzione e l'altra non è consigliabile. Ad esempio, se si esegue una query sul codice XML risultante, tale modifica richiederà l'aggiornamento della query. Se nella clausola FOR XML viene aggiunta l'opzione XMLSCHEMA, è possibile specificare facoltativamente uno spazio dei nomi di destinazione. Il codice XML risultante includerà lo spazio dei nomi specificato e rimarrà invariato, indipendentemente dal numero di esecuzioni della query.  
  
```  
SELECT ProductModelID, Name  
FROM   Production.ProductModel  
WHERE ProductModelID=1  
FOR XML AUTO, XMLSCHEMA ('MyURI')  
```  
  
## <a name="entity-elements"></a>Elementi entità  
 Prima di fornire informazioni dettagliate sulla struttura dello schema XSD generato per il risultato della query, è necessario descrivere l'elemento entità.  
  
 Un elemento entità nel codice XML restituito dalla query FOR XML è un elemento generato da una tabella e non da una colonna. La query FOR XML seguente, ad esempio, restituisce informazioni relative ai contatti presenti nella tabella `Person` del database `AdventureWorks2012` .  
  
```  
SELECT BusinessEntityID, FirstName  
FROM Person.Person  
WHERE BusinessEntityID = 1  
FOR XML AUTO, ELEMENTS  
```  
  
 Risultato:  
  
 `<Person>`  
  
 `<BusinessEntityID>1</BusinessEntityID>`  
  
 `<FirstName>Ken</FirstName>`  
  
 `</Person>`  
  
 In questo risultato <`Person`> è l'elemento entità. Nel risultato XML possono essere presenti più elementi entità, per ognuno dei quali è disponibile una dichiarazione globale nello schema XSD inline. Ad esempio, la query seguente recupera l'intestazione e i dettagli relativi a un ordine di vendita specifico.  
  
```  
SELECT  SalesOrderHeader.SalesOrderID, ProductID, OrderQty  
FROM    Sales.SalesOrderHeader, Sales.SalesOrderDetail  
WHERE   SalesOrderHeader.SalesOrderID = SalesOrderDetail.SalesOrderID  
AND     SalesOrderHeader.SalesOrderID=5001  
FOR XML AUTO, ELEMENTS, XMLSCHEMA  
```  
  
 Nella query è specificata la direttiva ELEMENTS e pertanto il codice XML risultante è incentrato sugli elementi. Nella query è inoltre specificata la direttiva XMLSCHEMA e pertanto viene restituito uno schema XSD inline. Risultato:  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />`  
  
 `<xsd:element name="Sales.SalesOrderHeader">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="SalesOrderID" type="sqltypes:int" />`  
  
 `<xsd:element ref="schema:Sales.SalesOrderDetail" minOccurs="0" maxOccurs="unbounded" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `<xsd:element name="Sales.SalesOrderDetail">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" />`  
  
 `<xsd:element name="OrderQty" type="sqltypes:smallint" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 Dalla query precedente si noti quanto segue:  
  
-   Nel risultato <`SalesOrderHeader`> e <`SalesOrderDetail`> sono elementi entità e pertanto vengono dichiarati a livello globale nello schema. Ciò significa che la dichiarazione è presente nel livello principale dell'elemento <`Schema`>.  
  
-   <`SalesOrderID`>, <`ProductID`> e <`OrderQty`> non sono elementi entità perché è stato eseguito il mapping a colonne. I dati di colonna vengono restituiti come elementi nel codice XML e ciò è dovuto alla direttiva ELEMENTS. Per tali elementi viene eseguito il mapping a elementi locali del tipo complesso dell'elemento entità. Si noti che se non viene specificata la direttiva ELEMENTS, per i valori `SalesOrderID`, `ProductID` e `OrderQty` viene eseguito il mapping ad attributi locali del tipo complesso dell'elemento entità corrispondente.  
  
## <a name="attribute-name-clashes"></a>Conflitti dei nomi di attributi  
 Le informazioni seguenti si basano sulle tabelle `CustOrder` e `CustOrderDetail` : Per testare gli esempi seguenti, creare le tabelle e inserire dati di esempio personalizzati:  
  
```  
CREATE TABLE CustOrder (OrderID int primary key, CustomerID int)  
GO  
CREATE TABLE CustOrderDetail (OrderID int, ProductID int, Qty int)  
GO  
```  
  
 Nelle query FOR XML a volte viene utilizzato lo stesso nome per indicare proprietà o attributi diversi. Ad esempio, la query in modalità RAW incentrata sugli attributi seguente crea due attributi con lo stesso nome, OrderID e viene quindi generato un errore.  
  
```  
SELECT CustOrder.OrderID,   
       CustOrderDetail.ProductID,   
       CustOrderDetail.OrderID  
FROM   dbo.CustOrder, dbo.CustOrderDetail  
WHERE  CustOrder.OrderID = CustOrderDetail.OrderID  
FOR XML RAW, XMLSCHEMA  
```  
  
 Poiché è tuttavia consentito che due elementi abbiano lo stesso nome, è possibile risolvere il problema aggiungendo la direttiva ELEMENTS:  
  
```  
SELECT CustOrder.OrderID,  
       CustOrderDetail.ProductID,   
       CustOrderDetail.OrderID  
from   dbo.CustOrder, dbo.CustOrderDetail  
where  CustOrder.OrderID = CustOrderDetail.OrderID  
FOR XML RAW, XMLSCHEMA, ELEMENTS  
```  
  
 Di seguito è riportato il risultato. Si noti che nello schema XSD inline, l'elemento OrderID è definito due volte. Per una dichiarazione minOccurs è impostato su 0 e corrisponde all'elemento OrderID della tabella CustOrderDetail e la seconda dichiarazione esegue il mapping alla colonna chiave primaria OrderID della tabella `CustOrder` , nella quale minOccurs è 1 per impostazione predefinita.  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="OrderID" type="sqltypes:int" />`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" minOccurs="0" />`  
  
 `<xsd:element name="OrderID" type="sqltypes:int" minOccurs="0" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
## <a name="element-name-clashes"></a>Conflitti dei nomi di elementi  
 Nelle query FOR XML è possibile utilizzare lo stesso nome per indicare due sottoelementi. Ad esempio, la query seguente recupera i valori ListPrice e DealerPrice dei prodotti, ma specifica lo stesso alias per le due colonne, ovvero Price. Nel set di righe risultante saranno pertanto presenti due colonne con lo stesso nome.  
  
### <a name="case-1-both-subelements-are-nonkey-columns-of-the-same-type-and-can-be-null"></a>Caso 1: entrambi i sottoelementi sono colonne non chiave dello stesso tipo e possono essere NULL  
 Nella query seguente, entrambi i sottoelementi sono colonne non chiave dello stesso tipo e possono essere NULL.  
  
```  
DROP TABLE T  
go  
CREATE TABLE T (ProductID int primary key, ListPrice money, DealerPrice money)  
go  
INSERT INTO T values (1, 1.25, null)  
go  
  
SELECT ProductID, ListPrice Price, DealerPrice Price  
FROM   T  
for    XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Di seguito è riportato il codice XML generato, nel quale è visualizzata solo una frazione dello schema XSD inline:  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" minOccurs="0" maxOccurs="2" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<ProductID>1</ProductID>`  
  
 `<Price>1.2500</Price>`  
  
 `</row>`  
  
 Nello schema XSD si noti quanto segue:  
  
-   ListPrice e DealerPrice sono dello stesso tipo, `money`ed entrambi possono essere NULL nella tabella. Dato che non possono essere restituiti nel codice XML risultante, nella dichiarazione del tipo complesso dell'elemento <`row`> esiste pertanto un solo elemento figlio <`Price`> con minOccurs=0 e maxOccurs=2.  
  
-   Poiché il valore `DealerPrice` è NULL nella tabella, nel risultato viene restituito solo `ListPrice` come elemento <`Price`>. Se si aggiunge il parametro `XSINIL` alla direttiva ELEMENTS, si otterranno entrambi gli elementi con il valore `xsi:nil` impostato su TRUE per l'elemento <`Price`> corrispondente a DealerPrice. Si otterranno inoltre due elementi figlio <`Price`> nella definizione del tipo complesso <`row`> dello schema XSD inline, con l'attributo `nillable` impostato su TRUE per entrambi. Di seguito è riportato un frammento che rappresenta un risultato parziale:  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" nillable="1" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" nillable="1" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" nillable="1" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`  
  
 `<ProductID>1</ProductID>`  
  
 `<Price>1.2500</Price>`  
  
 `<Price xsi:nil="true" />`  
  
 `</row>`  
  
### <a name="case-2-one-key-and-one-nonkey-column-of-the-same-type"></a>Caso 2: una colonna chiave e una colonna non chiave dello stesso tipo  
 La query seguente illustra una colonna chiave e una colonna non chiave dello stesso tipo.  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 int, Col3 nvarchar(20))  
go  
INSERT INTO T VALUES (1, 1, 'test')  
go   
```  
  
 La query seguente alla tabella **T** specifica lo stesso alias per Col1 e Col2, dove Col1 è una chiave primaria e non può essere Null e Col2 può essere Null. Vengono pertanto generati due elementi di pari livello che sono figli dell'elemento <`row`>.  
  
```  
SELECT Col1 as Col, Col2 as Col, Col3  
FROM T  
FOR XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Di seguito è riportato il risultato. nel quale è visualizzato solo un frammento dello schema XSD inline.  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="Col" type="sqltypes:int" />`  
  
 `<xsd:element name="Col" type="sqltypes:int" minOccurs="0" />`  
  
 `<xsd:element name="Col3" minOccurs="0">`  
  
 `<xsd:simpleType>`  
  
 `<xsd:restriction base="sqltypes:nvarchar"`  
  
 `sqltypes:localeId="1033"`  
  
 `sqltypes:sqlCompareOptions="IgnoreCase`  
  
 `IgnoreKanaType IgnoreWidth"`  
  
 `sqltypes:sqlSortId="52">`  
  
 `<xsd:maxLength value="20" />`  
  
 `</xsd:restriction>`  
  
 `</xsd:simpleType>`  
  
 `</xsd:element>`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<Col>1</Col>`  
  
 `<Col>1</Col>`  
  
 `<Col3>test</Col3>`  
  
 `</row>`  
  
 Si noti che per l'elemento <`Col`> dello schema XSD inline, minOccurs è impostato su 0.  
  
### <a name="case-3-both-elements-of-different-types-and-corresponding-columns-can-be-null"></a>Caso 3: entrambi gli elementi sono di tipo diverso e le colonne corrispondenti possono essere NULL  
 La query seguente viene eseguita sulla tabella di esempio illustrata nel caso 2:  
  
```  
SELECT Col1, Col2 as Col, Col3 as Col  
FROM T  
FOR XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Nella query seguente, a Col2 e Col3 sono assegnati gli stessi alias. Nel risultato vengono pertanto generati due elementi di pari livello con lo stesso nome ed entrambi figli dell'elemento <`raw`>. Entrambe le colonne sono di tipo diverso e possono essere NULL. Di seguito è riportato il risultato. nel quale è visualizzato solo uno schema XSD inline parziale.  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:simpleType name="Col1">`  
  
 `<xsd:restriction base="sqltypes:int" />`  
  
 `</xsd:simpleType>`  
  
 `<xsd:simpleType name="Col2">`  
  
 `<xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033"`  
  
 `sqltypes:sqlCompareOptions="IgnoreCase`  
  
 `IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">`  
  
 `<xsd:maxLength value="20" />`  
  
 `</xsd:restriction>`  
  
 `</xsd:simpleType>`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="Col1" type="sqltypes:int" />`  
  
 `<xsd:element name="Col" minOccurs="0" maxOccurs="2" type="xsd:anySimpleType" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<Col1>1</Col1>`  
  
 `<Col xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`  
  
 `xsi:type="Col1">1</Col>`  
  
 `<Col xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`  
  
 `xsi:type="Col2">test</Col>`  
  
 `</row>`  
  
 Nello schema XSD si noti quanto segue:  
  
-   Col2 e Col3 possono entrambe essere NULL e pertanto nella dichiarazione dell'elemento <`Col`> è specificato che minOccurs è 0 e maxOccurs è 2.  
  
-   Poiché gli elementi <`Col`> sono di pari livello, nello schema è presente una singola dichiarazione di elemento. Inoltre, dato che entrambi gli elementi sono anche di tipo diverso, benché di tipo semplice, il tipo dell'elemento nello schema è `xsd:anySimpleType`. Nel risultato, ogni tipo di istanza è identificato dall'attributo `xsi:type` .  
  
-   Nel risultato, ogni istanza dell'elemento <`Col`> fa riferimento al proprio tipo di istanza con l'attributo `xsi:type`.  
  
  
