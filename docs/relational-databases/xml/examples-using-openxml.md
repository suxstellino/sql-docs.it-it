---
title: 'Esempi: Utilizzo di OPENXML | Microsoft Docs'
description: Esempi di come usare OPENXML per creare una vista del set di righe di un documento XML.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- ColPattern [XML in SQL Server]
- XML [SQL Server], mapping data
- OPENXML statement, about OPENXML statement
- overflow in XML document [SQL Server]
- mapping XML data [SQL Server]
- combining attribute-centric and element centric mapping
- unconsumed data
- attribute-centric mapping
- column patterns [XML in SQL Server]
- XML [SQL Server], overflow handling
- row patterns [XML in SQL Server]
- rowpattern [XML in SQL Server]
- flags parameter
- element-centric mapping [SQL Server]
- edge tables
ms.assetid: 689297f3-adb0-4d8d-bf62-cfda26210164
author: rothja
ms.author: jroth
ms.openlocfilehash: 6b16bd6ed6fd7499109e9891725766d6934811a1
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488462"
---
# <a name="examples-using-openxml"></a>Esempi: Uso di OPENXML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Negli esempi presentati in questo argomento viene illustrato come utilizzare l'istruzione OPENXML per visualizzare un documento XML come set di righe. Per informazioni sulla sintassi di OPENXML, vedere [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md). Negli esempi vengono illustrati tutti gli aspetti dell'istruzione OPENXML, ma non ne vengono specificate le metaproprietà. Per altre informazioni su come specificare le metaproprietà in OPENXML, vedere [Specificare metaproprietà in OPENXML](../../relational-databases/xml/specify-metaproperties-in-openxml.md).  
  
## <a name="examples"></a>Esempi  
 Durante il recupero dei dati è possibile usare il parametro *rowpattern* per identificare i nodi del documento XML che definiscono le righe. *rowpattern* viene espresso anche nel linguaggio del modello XPath usato nell'implementazione di XPath di MSXML. Se ad esempio il modello termina con un elemento o con un attributo, viene creata una riga per ogni nodo di elemento o di attributo selezionato da *rowpattern*.  
  
 Il mapping predefinito è determinato dal valore di *flags* . Se in *SchemaDeclaration* non è specificato *ColPattern*, viene usato il mapping specificato in *flags* . Se invece in *SchemaDeclaration* è specificato *ColPattern* , il valore *flags* viene ignorato. Il valore specificato per *ColPattern* determina il tipo di mapping, che può essere incentrato sugli attributi o sugli elementi, nonché la modalità di gestione dei dati di overflow e di quelli non utilizzati.  
  
### <a name="a-executing-a-simple-select-statement-with-openxml"></a>R. Esecuzione di una semplice istruzione SELECT con OPENXML  
 Il documento XML utilizzato nell'esempio è costituito da elementi <`Customer`>, <`Order`> e <`OrderDetail`>. L'istruzione OPENXML recupera dal documento XML le informazioni sui clienti in un set di righe con due colonne, **CustomerID** e **ContactName**.  
  
 Prima di tutto, viene chiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento. L'handle del documento viene quindi passato a OPENXML.  
  
 Nell'istruzione OPENXML si noti quanto segue:  
  
-   Il parametro *rowpattern* (/ROOT/Customer) identifica i nodi <`Customer`> da elaborare.  
  
-   Il valore del parametro *flags* è impostato su **1** , per indicare che il mapping è incentrato sugli attributi. Per gli attributi XML viene eseguito il mapping alle colonne del set di righe definite in *SchemaDeclaration*.  
  
-   In *SchemaDeclaration*, nella clausola WITH, i valori specificati per *ColName* corrispondono ai nomi degli attributi XML associati. Quindi, in *SchemaDeclaration* non è specificato il parametro *ColPattern*.  
  
 L'istruzione SELECT recupera quindi tutte le colonne nel set di righe specificato da OPENXML.  
  
```  
DECLARE @DocHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @DocHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@DocHandle, '/ROOT/Customer',1)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @DocHandle  
```  
  
 Risultato:  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Gli elementi <`Customer`> non hanno sottoelementi, quindi, se si esegue la stessa istruzione SELECT con il parametro *flags* impostato su **2** per indicare il mapping incentrato sugli elementi, i valori **CustomerID** e **ContactName** verranno restituiti come NULL per entrambi i clienti.  
  
 La variabile \@xmlDocument può essere anche di tipo **xml** o di tipo **(n)varchar(max)** .  
  
 Se nel documento XML <`CustomerID`> e <`ContactName`> sono sottoelementi, il mapping incentrato sugli elementi ne recupererà i valori.  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer>  
   <CustomerID>VINET</CustomerID>  
   <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer>     
   <CustomerID>LILAS</CustomerID>  
   <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT    *  
FROM      OPENXML (@XmlDocumentHandle, '/ROOT/Customer',2)  
           WITH (CustomerID  varchar(10),  
                 ContactName varchar(20))  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 Risultato:  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 L'handle del documento restituito da **sp_xml_preparedocument** è valido solo per la durata del batch e non per tutta la sessione.  
  
### <a name="b-specifying-colpattern-for-mapping-between-rowset-columns-and-the-xml-attributes-and-elements"></a>B. Impostazione del parametro ColPattern per il mapping tra le colonne del set di righe e attributi o elementi XML  
 Questo esempio mostra l'impostazione del modello XPath nel parametro facoltativo *ColPattern* per specificare il mapping tra le colonne del set di righe e gli attributi o elementi XML.  
  
 Il documento XML utilizzato nell'esempio è costituito da elementi <`Customer`>, <`Order`> e <`OrderDetail`>. L'istruzione OPENXML recupera dal documento XML le informazioni sui clienti e sugli ordini in un set di righe che include le colonne **CustomerID**, **OrderDate**, **ProdID** e **Qty**.  
  
 Prima di tutto, viene chiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento. L'handle del documento viene quindi passato a OPENXML.  
  
 Nell'istruzione OPENXML si noti quanto segue:  
  
-   Il parametro *rowpattern* (/ROOT/Customer/Order/OrderDetail) identifica i nodi <`OrderDetail`> da elaborare.  
  
 Nell'esempio il valore del parametro *flags* è impostato su **2** , per indicare che il mapping è incentrato sugli elementi, ma tale mapping viene sovrascritto da quello specificato in *ColPattern* . In altre parole, il modello XPath specificato in *ColPattern* esegue il mapping delle colonne del set di righe agli attributi, il che determina un mapping incentrato sugli attributi.  
  
 Nella clausola WITH di *SchemaDeclaration* il parametro *ColPattern* è specificato anche con i parametri *ColName* e *ColType* . Il parametro *ColPattern* facoltativo è il modello XPath specificato e indica quanto segue:  
  
-   Il mapping delle colonne **OrderID**, **CustomerID** e **OrderDate** del set di righe viene eseguito agli attributi dell'elemento padre dei nodi identificati da *rowpattern* e *rowpattern* identifica i nodi <`OrderDetail`>. Viene quindi eseguito il mapping delle colonne **CustomerID** e **OrderDate** agli attributi **CustomerID** e **OrderDate** dell'elemento <`Order`>.  
  
-   Il mapping delle colonne **ProdID** e **Qty** del set di righe viene eseguito agli attributi **ProductID** e **Quantity** dei nodi identificati in *rowpattern*.  
  
 L'istruzione SELECT recupera quindi tutte le colonne nel set di righe specificato da OPENXML.  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT stmt using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@XmlDocumentHandle, '/ROOT/Customer/Order/OrderDetail',2)  
WITH (OrderID     int         '../@OrderID',  
      CustomerID  varchar(10) '../@CustomerID',  
      OrderDate   datetime    '../@OrderDate',  
      ProdID      int         '@ProductID',  
      Qty         int         '@Quantity')  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 Risultato:  
  
```  
OrderID CustomerID        OrderDate          ProdID    Qty  
-------------------------------------------------------------  
10248    VINET     1996-07-04 00:00:00.000     11       12  
10248    VINET     1996-07-04 00:00:00.000     42       10  
10283    LILAS     1996-08-16 00:00:00.000     72        3  
```  
  
 Il modello XPath specificato in *ColPattern* può anche specificare il mapping degli elementi XML alle colonne del set di righe, che determina un mapping incentrato sugli elementi. Nell'esempio seguente gli elementi <`CustomerID`> e <`OrderDate`> del documento XML sono sottoelementi dell'elemento <`Orders`>. *ColPattern* sovrascrive il mapping specificato nel parametro *flags* , quindi *flags* non viene specificato nell'istruzione OPENXML.  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order EmployeeID="5" >  
      <OrderID>10248</OrderID>  
      <CustomerID>VINET</CustomerID>  
      <OrderDate>1996-07-04T00:00:00</OrderDate>  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order  EmployeeID="3" >  
      <OrderID>10283</OrderID>  
      <CustomerID>LILAS</CustomerID>  
      <OrderDate>1996-08-16T00:00:00</OrderDate>  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail')  
WITH (CustomerID  varchar(10)   '../CustomerID',  
      OrderDate   datetime      '../OrderDate',  
      ProdID      int           '@ProductID',  
      Qty         int           '@Quantity')  
EXEC sp_xml_removedocument @docHandle  
```  
  
### <a name="c-combining-attribute-centric-and-element-centric-mapping"></a>C. Combinazione di mapping incentrato sugli attributi e mapping incentrato sugli elementi  
 Nell'esempio seguente il parametro *flags* è impostato su **3** , per indicare che verrà applicato sia il mapping incentrato sugli attributi che quello incentrato sugli elementi. In questo caso verrà applicato per primo il mapping incentrato sugli attributi, mentre il mapping incentrato sugli elementi verrà applicato successivamente a tutte le colonne non ancora sottoposte a mapping.  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument =N'<ROOT>  
<Customer CustomerID="VINET"  >  
     <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" >   
     <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer',3)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Risultato:  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 A **CustomerID** viene applicato il mapping incentrato sugli attributi. Nell'elemento <`Customer`> non è presente l'attributo **ContactName**, quindi verrà applicato il mapping incentrato sugli elementi.  
  
### <a name="d-specifying-the-text-xpath-function-as-colpattern"></a>D. Impostazione della funzione XPath text() come parametro ColPattern  
 Il documento XML utilizzato nell'esempio è costituito da elementi <`Customer`> e <`Order`>. L'istruzione OPENXML recupera un set di righe composto dall'attributo **oid** dell'elemento <`Order`>, dall'ID dell'elemento padre del nodo identificato da *rowpattern* e dalla stringa del valore foglia del contenuto dell'elemento.  
  
 Prima di tutto, viene chiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento. L'handle del documento viene quindi passato a OPENXML.  
  
 Nell'istruzione OPENXML si noti quanto segue:  
  
-   Il parametro *rowpattern* (/root/Customer/Order) identifica i nodi <`Order`> da elaborare.  
  
-   Il valore del parametro *flags* è impostato su **1** , per indicare che il mapping è incentrato sugli attributi. Per gli attributi XML viene eseguito il mapping alle colonne del set di righe definite in *SchemaDeclaration*.  
  
-   In *SchemaDeclaration* , nella clausola WITH, i nomi delle colonne del set di righe **oid** e **amount** corrispondono ai nomi degli attributi XML associati. Di conseguenza, il parametro *ColPattern* non viene specificato. Per la colonna **comment** del set di righe, la funzione XPath **text()** viene specificata come parametro *ColPattern*. Questo parametro sovrascrive il mapping incentrato sugli attributi specificato nel parametro *flags* e la colonna contiene la stringa del valore foglia del contenuto dell'elemento.  
  
 L'istruzione SELECT recupera quindi tutte le colonne nel set di righe specificato da OPENXML.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied  
      </Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
            <Urgency>Important</Urgency>  
            Happy Customer.  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH (oid     char(5),   
           amount  float,   
           comment ntext 'text()')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Risultato:  
  
```  
oid   amount        comment  
----- -----------   -----------------------------  
O1    3.5           NULL  
O2    13.4          Customer was very satisfied  
O3    100.0         Happy Customer.  
O4    10000.0       NULL  
```  
  
### <a name="e-specifying-tablename-in-the-with-clause"></a>E. Impostazione del parametro TableName nella clausola WITH  
 Questo esempio specifica *TableName* con la clausola WITH invece di *SchemaDeclaration*. Questo risulta utile se è disponibile una tabella con la struttura desiderata e se non sono necessari i modelli di colonna definiti dal parametro *ColPattern* .  
  
 Il documento XML utilizzato nell'esempio è costituito da elementi <`Customer`> e <`Order`>. L'istruzione OPENXML recupera dal documento XML le informazioni sugli ordini in un set di righe con tre colonne,**oid**, **date** e **amount**.  
  
 Prima di tutto, viene chiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento. L'handle del documento viene quindi passato a OPENXML.  
  
 Nell'istruzione OPENXML si noti quanto segue:  
  
-   Il parametro *rowpattern* (/root/Customer/Order) identifica i nodi <`Order`> da elaborare.  
  
-   Nella clausola WITH non è presente *SchemaDeclaration* , ma viene specificato un nome di tabella. Come schema del set di righe viene pertanto utilizzato lo schema della tabella.  
  
-   Il valore del parametro *flags* è impostato su **1** , per indicare che il mapping è incentrato sugli attributi. Per gli attributi degli elementi identificati da *rowpattern* viene quindi eseguito il mapping alle colonne del set di righe con lo stesso nome.  
  
 L'istruzione SELECT recupera quindi tutte le colonne nel set di righe specificato da OPENXML.  
  
```  
-- Create a test table. This table schema is used by OPENXML as the  
-- rowset schema.  
CREATE TABLE T1(oid char(5), date datetime, amount float)  
GO  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
-- Sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH T1  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Risultato:  
  
```  
oid   date                        amount  
----- --------------------------- ----------  
O1    1996-01-20 00:00:00.000     3.5  
O2    1997-04-30 00:00:00.000     13.4  
O3    1999-07-14 00:00:00.000     100.0  
O4    1996-01-20 00:00:00.000     10000.0  
```  
  
### <a name="f-obtaining-the-result-in-an-edge-table-format"></a>F. Recupero di risultati in formato tabella edge  
 In questo esempio nell'istruzione OPENXML non viene specificata la clausola WITH. Il set di righe generato dall'istruzione OPENXML ha pertanto un formato tabella edge. L'istruzione SELECT restituisce tutte le colonne della tabella edge.  
  
 Il documento XML utilizzato nell'esempio è costituito da elementi <`Customer`>, <`Order`> e <`OrderDetail`>.  
  
 Prima di tutto, viene chiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento. L'handle del documento viene quindi passato a OPENXML.  
  
 Nell'istruzione OPENXML si noti quanto segue:  
  
-   Il parametro *rowpattern* (/ROOT/Customer) identifica i nodi <`Customer`> da elaborare.  
  
-   La clausola WITH è stata omessa e OPENXML restituisce pertanto un set di righe in formato tabella edge.  
  
 L'istruzione SELECT recupera quindi tutte le colonne della tabella edge.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
SET @xmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer')  
  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Il risultato viene restituito sotto forma di tabella edge. È possibile creare query da eseguire sulla tabella edge per recuperare informazioni specifiche. Ad esempio:  
  
-   La query seguente restituisce il numero di nodi **Customer** presenti nel documento. Poiché non è stata specificata la clausola WITH, l'istruzione OPENXML restituisce una tabella edge. L'istruzione SELECT esegue la query sulla tabella edge.  
  
    ```  
    SELECT count(*)  
    FROM OPENXML(@docHandle, '/')  
    WHERE localname = 'Customer'  
    ```  
  
-   La query seguente restituisce i nomi locali dei nodi XML di tipo elemento.  
  
    ```  
    SELECT distinct localname   
    FROM OPENXML(@docHandle, '/')   
    WHERE nodetype = 1   
    ORDER BY localname  
    ```  
  
### <a name="g-specifying-rowpattern-ending-with-an-attribute"></a>G. Impostazione di un parametro rowpattern che termina con un attributo  
 Il documento XML utilizzato nell'esempio è costituito da elementi <`Customer`>, <`Order`> e <`OrderDetail`>. L'istruzione OPENXML recupera dal documento XML le informazioni sui dettagli degli ordini in un set di righe con tre colonne,**ProductID**, **Quantity** e **OrderID**.  
  
 Prima di tutto, viene chiamata la stored procedure **sp_xml_preparedocument** per ottenere un handle di documento. L'handle del documento viene quindi passato a OPENXML.  
  
 Nell'istruzione OPENXML si noti quanto segue:  
  
-   Il parametro *rowpattern* (/ROOT/Customer/Order/OrderDetail/\@ProductID) termina con l'attributo XML, **ProductID**. Nel set di righe risultante viene creata una riga per ogni nodo di attributo selezionato nel documento XML.  
  
-   In questo esempio il parametro *flags* non è specificato e i mapping vengono definiti dal parametro *ColPattern* .  
  
 Nella clausola WITH di *SchemaDeclaration* il parametro *ColPattern* è specificato anche con i parametri *ColName* e *ColType* . Il parametro *ColPattern* facoltativo è il modello XPath specificato e indica quanto segue:  
  
-   Il modello XPath ( **.** ) specificato come *ColPattern* per la colonna **ProdID** nel set di righe identifica il nodo di contesto, ovvero il nodo corrente. Il valore specificato per *rowpattern* è l'attributo **ProductID** dell'elemento <`OrderDetail`>.  
  
-   Il valore di *ColPattern*, **../\@Quantity**, specificato per la colonna **Qty** nel set di righe identifica l'attributo **Quantity** del nodo padre, <`OrderDetail`>, del nodo di contesto, \<ProductID>.  
  
-   Analogamente, il valore di *ColPattern*, **../../\@OrderID**, specificato per la colonna **OID** nel set di righe identifica l'attributo **OrderID** dell'elemento padre, <`Order`>, del nodo padre del nodo di contesto. Il nodo padre è <`OrderDetail`>, mentre il nodo di contesto è <`ProductID`>.  
  
 L'istruzione SELECT recupera quindi tutte le colonne nel set di righe specificato da OPENXML.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--Sample XML document  
SET @xmlDocument =N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail/@ProductID')  
       WITH ( ProdID  int '.',  
              Qty     int '../@Quantity',  
              OID     int '../../@OrderID')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Risultato:  
  
```  
ProdID      Qty         OID  
----------- ----------- -------   
11          12          10248  
42          10          10248  
72          3           10283  
```  
  
### <a name="h-specifying-an-xml-document-that-has-multiple-text-nodes"></a>H. Impostazione di un documento XML con più nodi di testo  
 Se in un documento XML sono presenti più nodi di testo, un'istruzione SELECT con un parametro *ColPattern* di tipo **text()** restituirà solo il primo, invece di tutti i nodi di testo. Ad esempio:  
  
```  
DECLARE @h int  
EXEC sp_xml_preparedocument @h OUTPUT,  
         N'<root xmlns:a="urn:1">  
           <a:Elem abar="asdf">  
             T<a>a</a>U  
           </a:Elem>  
         </root>',  
         '<ns xmlns:b="urn:1" />'  
  
SELECT * FROM openxml(@h, '/root/b:Elem')  
      WITH (Col1 varchar(20) 'text()')  
EXEC sp_xml_removedocument @h  
```  
  
 L'istruzione SELECT restituisce **T** , invece di **TaU**.  
  
### <a name="i-specifying-the-xml-data-type-in-the-with-clause"></a>I. Impostazione del tipo di dati xml nella clausola WITH  
 Nella clausola WITH un modello di colonna con mapping a una colonna con tipo di dati **xml** tipizzato o non tipizzato deve restituire una sequenza vuota oppure una sequenza di elementi, istruzioni di elaborazione, nodi di testo e commenti. Viene eseguito il cast dei dati a un tipo di dati **xml** .  
  
 Nell'esempio seguente la dichiarazione dello schema di tabella nella clausola WITH include colonne di tipo **xml** .  
  
```  
DECLARE @h int  
DECLARE @x xml  
set @x = '<Root>  
  <row id="1"><lname>Duffy</lname>  
   <Address>  
            <Street>111 Maple</Street>  
            <City>Seattle</City>  
   </Address>  
  </row>  
  <row id="2"><lname>Wang</lname>  
   <Address>  
            <Street>222 Pine</Street>  
            <City>Bothell</City>  
   </Address>  
  </row>  
</Root>'  
  
EXEC sp_xml_preparedocument @h output, @x  
SELECT *  
FROM   OPENXML (@h, '/Root/row', 10)  
      WITH (id int '@id',  
  
            lname    varchar(30),  
            xmlname  xml 'lname',  
            OverFlow xml '@mp:xmltext')  
EXEC sp_xml_removedocument @h  
```  
  
 In particolare, viene passata una variabile di tipo **xml**, \@x, alla funzione **sp_xml_preparedocument()** .  
  
 Risultato:  
  
```  
id  lname   xmlname                   OverFlow  
--- ------- ------------------------------ -------------------------------  
1   Duffy   <lname>Duffy</lname>  <row><Address>  
                                   <Street>111 Maple</Street>  
                                   <City>Seattle</City>  
                                  </Address></row>  
2   Wang    <lname>Wang</lname>   <row><Address>  
                                    <Street>222 Pine</Street>  
                                    <City>Bothell</City>  
                                   </Address></row>  
```  
  
 Dal risultato si noti quanto segue:  
  
-   Il valore della colonna **lname** di tipo **varchar(30)** viene recuperato dall'elemento <`lname`> corrispondente.  
  
-   Come valore della colonna **xmlname** di tipo **xml** , viene restituito l'elemento con lo stesso nome.  
  
-   Il flag è impostato su 10, ovvero 2 + 8, dove 2 indica il mapping incentrato sugli elementi e 8 indica che solo i dati XML non utilizzati devono essere aggiunti alla colonna OverFlow definita nella clausola WITH. Se si imposta il flag su 2, nella colonna OverFlow specificata nella clausola WITH verrà copiato l'intero documento XML.  
  
-   Se la colonna specificata nella clausola WITH è una colonna XML tipizzata e l'istanza XML non è conforme allo schema, verrà restituito un errore.  
  
### <a name="j-retrieving-individual-values-from-multivalued-attributes"></a>J. Recupero di singoli valori da attributi multivalore  
 Un documento XML può includere attributi multivalore. L'attributo **IDREFS** , ad esempio, può essere multivalore. In un documento XML gli attributi multivalore vengono specificati come stringa, con i valori separati da spazi. Nel documento XML seguente l'attributo **attends** dell'elemento \<Student> e l'attributo **attendedBy** dell'elemento \<Class> sono multivalore. Per recuperare i singoli valori da un attributo XML multivalore e archiviare ogni valore in una riga distinta del database, sono necessarie ulteriori operazioni, illustrate in questo esempio.  
  
 Questo documento XML di esempio è costituito dagli elementi seguenti:  
  
-   \<Student>  
  
     Attributi **id** (ID dello studente), **name** e **attends** . L'attributo **attends** è multivalore.  
  
-   \<Class>  
  
     Attributi **id** (ID della classe), **name** e **attendedBy** . L'attributo **attendedBy** è multivalore.  
  
 L'attributo **attends** dell'elemento \<Student> e l'attributo **attendedBy** dell'elemento \<Class> rappresentano una relazione **m:n** tra le tabelle Student e Class. Uno studente può frequentare più classi e una classe può essere frequentata da più studenti.  
  
 Si supponga che sia necessario suddividere il documento e salvarlo nel database, come illustrato di seguito:  
  
-   Salvare i dati dell'elemento \<Student> nella tabella Students.  
  
-   Salvare i dati dell'elemento \<Class> nella tabella Courses.  
  
-   Salvare nella tabella CourseAttendence i dati della relazione **m:n** tra le tabelle Student e Class. Per estrarre i valori sono necessarie ulteriori operazioni. Per recuperare le informazioni e archiviarle nella tabella, utilizzare le stored procedure seguenti:  
  
    -   **Insert_Idrefs_Values**  
  
         Inserisce gli ID di corsi e studenti nella tabella CourseAttendence.  
  
    -   **Extract_idrefs_values**  
  
         Estrae gli ID dei singoli studenti da ogni elemento \<Course>. Per recuperare questi valori viene utilizzata una tabella edge.  
  
 I passaggi necessari sono i seguenti:  
  
```  
-- Create these tables:  
DROP TABLE CourseAttendance  
DROP TABLE Students  
DROP TABLE Courses  
GO  
CREATE TABLE Students(  
                id   varchar(5) primary key,  
                name varchar(30)  
                )  
GO  
CREATE TABLE Courses(  
               id       varchar(5) primary key,  
               name     varchar(30),  
               taughtBy varchar(5)  
)  
GO  
CREATE TABLE CourseAttendance(  
             id         varchar(5) references Courses(id),  
             attendedBy varchar(5) references Students(id),  
             constraint CourseAttendance_PK primary key (id, attendedBy)  
)  
go  
-- Create these stored procedures:  
DROP PROCEDURE f_idrefs  
GO  
CREATE PROCEDURE f_idrefs  
    @t      varchar(500),  
    @idtab  varchar(50),  
    @id     varchar(5)  
AS  
DECLARE @sp int  
DECLARE @att varchar(5)  
SET @sp = 0  
WHILE (LEN(@t) > 0)  
BEGIN   
    SET @sp = CHARINDEX(' ', @t+ ' ')  
    SET @att = LEFT(@t, @sp-1)  
    EXEC('INSERT INTO '+@idtab+' VALUES ('''+@id+''', '''+@att+''')')  
    SET @t = SUBSTRING(@t+ ' ', @sp+1, LEN(@t)+1-@sp)  
END  
Go  
  
DROP PROCEDURE fill_idrefs  
GO  
CREATE PROCEDURE fill_idrefs   
    @xmldoc     int,  
    @xpath      varchar(100),  
    @from       varchar(50),  
    @to         varchar(50),  
    @idtable    varchar(100)  
AS  
DECLARE @t varchar(500)  
DECLARE @id varchar(5)  
  
/* Temporary Edge table */  
SELECT *   
INTO #TempEdge   
FROM OPENXML(@xmldoc, @xpath)  
  
DECLARE fillidrefs_cursor CURSOR FOR  
    SELECT CAST(iv.text AS nvarchar(200)) AS id,  
           CAST(av.text AS nvarchar(4000)) AS refs  
    FROM   #TempEdge c, #TempEdge i,  
           #TempEdge iv, #TempEdge a, #TempEdge av  
    WHERE  c.id = i.parentid  
    AND    UPPER(i.localname) = UPPER(@from)  
    AND    i.id = iv.parentid  
    AND    c.id = a.parentid  
    AND    UPPER(a.localname) = UPPER(@to)  
    AND    a.id = av.parentid  
  
OPEN fillidrefs_cursor  
FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    IF (@@FETCH_STATUS <> -2)  
    BEGIN  
        execute f_idrefs @t, @idtable, @id  
    END  
    FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
END  
CLOSE fillidrefs_cursor  
DEALLOCATE fillidrefs_cursor  
Go  
-- This is the sample document that is shredded and the data is stored in the preceding tables.  
DECLARE @h int  
EXECUTE sp_xml_preparedocument @h OUTPUT, N'<Data>  
  <Student id = "s1" name = "Student1"  attends = "c1 c3 c6"  />  
  <Student id = "s2" name = "Student2"  attends = "c2 c4" />  
  <Student id = "s3" name = "Student3"  attends = "c2 c4 c6" />  
  <Student id = "s4" name = "Student4"  attends = "c1 c3 c5" />  
  <Student id = "s5" name = "Student5"  attends = "c1 c3 c5 c6" />  
  <Student id = "s6" name = "Student6" />  
  
  <Class id = "c1" name = "Intro to Programming"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c2" name = "Databases"   
         attendedBy = "s2 s3" />  
  <Class id = "c3" name = "Operating Systems"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c4" name = "Networks" attendedBy = "s2 s3" />  
  <Class id = "c5" name = "Algorithms and Graphs"   
         attendedBy =  "s4 s5"/>  
  <Class id = "c6" name = "Power and Pragmatism"   
         attendedBy = "s1 s3 s5" />  
</Data>'  
  
INSERT INTO Students SELECT * FROM OPENXML(@h, '//Student') WITH Students  
  
INSERT INTO Courses SELECT * FROM OPENXML(@h, '//Class') WITH Courses  
/* Using the edge table */  
EXECUTE fill_idrefs @h, '//Class', 'id', 'attendedby', 'CourseAttendance'  
  
SELECT * FROM Students  
SELECT * FROM Courses  
SELECT * FROM CourseAttendance  
  
EXECUTE sp_xml_removedocument @h  
```  
  
### <a name="k-retrieving-binary-from-base64-encoded-data-in-xml"></a>K. Recupero di dati binari da dati con codifica Base64 in un valore XML  
 Nei valori XML sono spesso inclusi dati binari con codifica Base64. Quando si suddivide un valore XML di questo tipo tramite l'istruzione OPENXML, vengono restituiti dati con codifica Base64. In questo esempio viene illustrato come convertire in formato binario i dati con codifica Base64.  
  
-   Creare una tabella con dati binari di esempio.  
  
-   Utilizzare una query FOR XML e l'opzione BINARY BASE64 per costruire il valore XML che include dati binari con codifica Base64.  
  
-   Suddividere il valore XML tramite un'istruzione OPENXML. L'istruzione OPENXML restituirà dati con codifica Base64. Chiamare quindi la funzione .value per convertire i dati in formato binario.  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 varbinary(100))  
go  
-- Insert sample binary data  
INSERT T VALUES(1, 0x1234567890)   
go  
 -- Create test XML document that has base64 encoded binary data (use FOR XML query and specify BINARY BASE64 option)  
SELECT * FROM T  
FOR XML AUTO, BINARY BASE64  
go  
-- result  
-- <T Col1="1" Col2="EjRWeJA="/>  
  
-- Now shredd the sample XML using OPENXML.   
-- Call the .value function to convert   
-- the base64 encoded data returned by OPENXML to binary.  
DECLARE @h int ;  
EXEC sp_xml_preparedocument @h OUTPUT, '<T Col1="1" Col2="EjRWeJA="/>' ;  
SELECT Col1,   
CAST('<binary>' + Col2 + '</binary>' AS XML).value('.', 'varbinary(max)') AS BinaryCol   
FROM openxml(@h, '/T')   
WITH (Col1 integer, Col2 varchar(max)) ;  
EXEC sp_xml_removedocument @h ;  
GO  
```  
  
 Di seguito è riportato il risultato. I dati binari restituiti sono i dati binari originali della tabella T.  
  
```  
Col1        BinaryCol  
----------- ---------------------  
1           0x1234567890  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_xml_preparedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)   
 [sp_xml_removedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)   
 [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML &#40;SQL Server&#41;](../../relational-databases/xml/openxml-sql-server.md)  
  
  
