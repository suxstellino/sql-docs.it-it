---
title: Esempi di caricamento bulk XML (SQLXML)
description: Visualizzare esempi dettagliati della funzionalità di caricamento bulk XML in SQKXML 4.0 con schemi XSD e XDR per ogni esempio.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- overflow-field annotation
- ConnectionCommand property
- XMLFragment property
- relationship chains [SQLXML]
- multiple table bulk loading
- examples [SQLXML], XML Bulk Load
- overflow data [SQLXML]
- sql:datatype
- transaction mode [SQLXML]
- CheckConstraints property
- sql:overflow-field
- XML Bulk Load [SQLXML], examples
- TempFilePath property
- datatype annotation
- Transaction property
- KeepIdentity property
- Execute method
- streaming XML data
- xml data type [SQL Server], SQLXML
- bulk load [SQLXML], examples
ms.assetid: 970e4553-b41d-4a12-ad50-0ee65d1f305d
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 136d5e2650faa9f092f331fee2b4fb4bafd86570
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491996"
---
# <a name="xml-bulk-load-examples-sqlxml-40"></a>Esempi di caricamento bulk XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Negli esempi seguenti viene illustrata la funzionalità di caricamento bulk XML in Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. In ogni esempio vengono forniti uno schema XSD e lo schema XDR equivalente.  
  
## <a name="bulk-loader-script-validateandbulkloadvbs"></a>Script per il caricamento bulk (ValidateAndBulkload.vbs)  
 Lo script seguente, scritto in Visual Basic Scripting Edition (VBScript), carica un documento XML nel DOM XML, lo convalida in base a uno schema e, se il documento è valido, esegue un caricamento bulk XML per caricare il codice XML in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] una [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tabella. Lo script può essere utilizzato con ognuno dei singoli esempi che vi fanno riferimento più avanti in questo argomento.  
  
> [!NOTE]  
>  Il caricamento bulk XML non genera un avviso o un errore se non viene caricato alcun contenuto dal file di dati. È pertanto consigliabile convalidare il file di dati XML prima di eseguire un'operazione di caricamento bulk.  
  
```vbs  
Dim FileValid  
  
set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=MyServer;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
  
'Validate the data file prior to bulkload  
Dim sOutput   
sOutput = ValidateFile("SampleXMLData.xml", "", "SampleSchema.xml")  
WScript.Echo sOutput  
  
If FileValid Then  
   ' Check constraints and initiate transaction (if needed)  
   ' objBL.CheckConstraints = True  
   ' objBL.Transaction=True  
  'Execute XML bulkload using file.  
  objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
  set objBL=Nothing  
End If  
  
Function ValidateFile(strXmlFile,strUrn,strXsdFile)  
  
   ' Create a schema cache and add SampleSchema.xml to it.  
   Dim xs, fso, sAppPath  
   Set fso = CreateObject("Scripting.FileSystemObject")   
   Set xs = CreateObject("MSXML2.XMLSchemaCache.6.0")  
   sAppPath = fso.GetFolder(".")   
   xs.Add strUrn, sAppPath & "\" & strXsdFile  
  
   ' Create an XML DOMDocument object.  
   Dim xd   
   Set xd = CreateObject("MSXML2.DOMDocument.6.0")  
  
   ' Assign the schema cache to the DOM document.  
   ' schemas collection.  
   Set xd.schemas = xs  
  
   ' Load XML document as DOM document.  
   xd.async = False  
   xd.Load sAppPath & "\" & strXmlFile  
  
   ' Return validation results in message to the user.  
   If xd.parseError.errorCode <> 0 Then  
        ValidateFile = "Validation failed on " & _  
             strXmlFile & vbCrLf & _  
             "=======" & vbCrLf & _  
             "Reason: " & xd.parseError.reason & _  
             vbCrLf & "Source: " & _  
             xd.parseError.srcText & _  
             vbCrLf & "Line: " & _  
             xd.parseError.Line & vbCrLf  
             FileValid = False  
    Else  
        ValidateFile = "Validation succeeded for " & _  
             strXmlFile & vbCrLf & _  
             "========" & _  
             vbCrLf & "Contents to be bulkloaded" & vbCrLf  
             FileValid = True  
    End If  
End Function  
```  
  
## <a name="a-bulk-loading-xml-in-a-table"></a>R. Caricamento bulk di un file XML in una tabella  
 In questo esempio viene stabilita una connessione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] all'istanza di specificata nella proprietà ConnectionString (MyServer). L'esempio specifica anche la proprietà ErrorLogFile. L'output degli errori viene pertanto salvato nel file specificato ("C:\error.log"), che può essere anche spostato in un percorso diverso. Si noti anche che il metodo Execute ha come parametri sia il file dello schema di mapping (SampleSchema.xml) che il file di dati XML (SampleXMLData.xml). Quando viene eseguito il caricamento bulk, la tabella Cust creata nel database **tempdb** conterrà nuovi record basati sul contenuto del file di dati XML.  
  
#### <a name="to-test-a-sample-bulk-load"></a>Per testare un caricamento bulk di esempio  
  
1.  Creare la tabella seguente:  
  
    ```sql  
    CREATE TABLE Cust(CustomerID  int PRIMARY KEY,  
                      CompanyName varchar(20),  
                      City        varchar(20));  
    GO  
    ```  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere lo schema XSD seguente al file:  
  
    ```xml  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
       <xsd:element name="ROOT" sql:is-constant="1" >  
         <xsd:complexType>  
           <xsd:sequence>  
             <xsd:element name="Customers" sql:relation="Cust" maxOccurs="unbounded">  
               <xsd:complexType>  
                 <xsd:sequence>  
                   <xsd:element name="CustomerID"  type="xsd:integer" />  
                   <xsd:element name="CompanyName" type="xsd:string" />  
                   <xsd:element name="City"        type="xsd:string" />  
                 </xsd:sequence>  
               </xsd:complexType>  
             </xsd:element>  
           </xsd:sequence>  
          </xsd:complexType>  
         </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Aggiungere il documento XML seguente al file:  
  
    ```xml  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Sean Chai</CompanyName>  
        <City>New York</City>  
      </Customers>  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Tom Johnston</CompanyName>  
         <City>Los Angeles</City>  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Institute of Art</CompanyName>  
        <City>Chicago</City>  
      </Customers>  
    </ROOT>  
    ```  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere al file il codice VBScript fornito all'inizio di questo argomento. Modificare la stringa di connessione per specificare il nome del server appropriato. Specificare il percorso appropriato per i file specificati come parametri per il metodo Execute.  
  
5.  Eseguire il codice VBScript. Il caricamento bulk XML carica il file XML nella tabella Cust.  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```xml  
  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers"  sql:relation="Cust" >  
      <element type="CustomerID"  sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City"        sql:field="City" />  
  
   </ElementType>  
</Schema>  
```  
  
## <a name="b-bulk-loading-xml-data-in-multiple-tables"></a>B. Caricamento bulk di dati XML in più tabelle  
 In questo esempio il documento XML è costituito da **\<Customer>** elementi **\<Order>** e .  
  
```xml  
<ROOT>  
  <Customers>  
    <CustomerID>1111</CustomerID>  
    <CompanyName>Sean Chai</CompanyName>  
    <City>NY</City>  
    <Order OrderID="1" />  
    <Order OrderID="2" />  
  </Customers>  
  <Customers>  
    <CustomerID>1112</CustomerID>  
    <CompanyName>Tom Johnston</CompanyName>  
     <City>LA</City>    
    <Order OrderID="3" />  
  </Customers>  
  <Customers>  
    <CustomerID>1113</CustomerID>  
    <CompanyName>Institute of Art</CompanyName>  
    <Order OrderID="4" />  
  </Customers>  
</ROOT>  
```  
  
 In questo esempio i dati XML vengono caricati in blocco in due tabelle, **Cust** e **CustOrder:**  
  
-   Cust(CustomerID, CompanyName, City)  
  
-   CustOrder(OrderID, CustomerID)  
  
 Nello schema XSD seguente viene definita la vista XML delle tabelle. Lo schema specifica la relazione padre-figlio tra gli **\<Customer>** elementi **\<Order>** e .  
  
```xml  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="ROOT" sql:is-constant="1" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Customers" sql:relation="Cust" >  
          <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="CustomerID"  type="xsd:integer" />  
              <xsd:element name="CompanyName" type="xsd:string" />  
              <xsd:element name="City"        type="xsd:string" />  
              <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
                <xsd:complexType>  
                  <xsd:attribute name="OrderID" type="xsd:integer" />  
                </xsd:complexType>  
              </xsd:element>  
             </xsd:sequence>  
          </xsd:complexType>  
        </xsd:element>  
      </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Il caricamento bulk XML usa la relazione chiave primaria/chiave esterna specificata in precedenza tra gli elementi e per eseguire il **\<Cust>** caricamento bulk dei dati in **\<CustOrder>** entrambe le tabelle.  
  
#### <a name="to-test-a-sample-bulk-load"></a>Per testare un caricamento bulk di esempio  
  
1.  Creare due tabelle nel database **tempdb:**  
  
    ```sql  
    USE tempdb;  
    CREATE TABLE Cust(  
           CustomerID  int PRIMARY KEY,  
           CompanyName varchar(20),  
           City        varchar(20));  
    CREATE TABLE CustOrder(        OrderID     int PRIMARY KEY,   
            CustomerID int FOREIGN KEY REFERENCES Cust(CustomerID));  
    ```  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere lo schema XSD fornito in questo esempio al file.  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleData.xml. Aggiungere al file il documento XML fornito in precedenza in questo esempio.  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere al file il codice VBScript fornito all'inizio di questo argomento. Modificare la stringa di connessione per specificare i nomi del server e del database appropriati. Specificare il percorso appropriato per i file specificati come parametri per il metodo Execute.  
  
5.  Eseguire il codice VBScript indicato in precedenza. Il caricamento bulk XML carica il documento XML nelle tabelle Cust e CustOrder.  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```xml  
  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust" >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
<sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
</Schema>  
```  
  
## <a name="c-using-chain-relationships-in-the-schema-to-bulk-load-xml"></a>C. Utilizzo di relazioni a catena nello schema per il caricamento bulk XML  
 In questo esempio viene illustrata la modalità di utilizzo della relazione M:N specificata nello schema di mapping dal caricamento bulk XML per caricare dati in una tabella che rappresenta una relazione M:N.  
  
 Si consideri, ad esempio, lo schema XSD seguente:  
  
```xml  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrderOD"  
          parent="Ord"  
          parent-key="OrderID"  
          child="OrderDetail"  
          child-key="OrderID" />  
  
    <sql:relationship name="ODProduct"  
          parent="OrderDetail"  
          parent-key="ProductID"  
          child="Product"  
          child-key="ProductID"   
          inverse="true"/>  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="ROOT" sql:is-constant="1" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Ord"   
                     sql:key-fields="OrderID" >  
          <xsd:complexType>  
            <xsd:sequence>  
             <xsd:element name="Product"  
                          sql:relation="Product"   
                          sql:key-fields="ProductID"  
                          sql:relationship="OrderOD ODProduct">  
               <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
               </xsd:complexType>  
             </xsd:element>  
           </xsd:sequence>  
           <xsd:attribute name="OrderID"   type="xsd:integer" />   
           <xsd:attribute name="CustomerID"   type="xsd:string" />  
         </xsd:complexType>  
       </xsd:element>  
      </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Lo schema specifica un elemento **\<Order>** con un **\<Product>** elemento figlio. **\<Order>** L'elemento esegue il mapping alla tabella Ord e **\<Product>** l'elemento viene mappato alla tabella Product nel database. La relazione a catena specificata **\<Product>** nell'elemento identifica una relazione M:N rappresentata dalla tabella OrderDetail. Un ordine può includere molti prodotti e un prodotto può essere incluso in molti ordini.  
  
 Quando si esegue il caricamento bulk di un documento XML con questo schema, vengono aggiunti record alle tabelle Ord, Product e OrderDetail.  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare tre tabelle:  
  
    ```sql  
    CREATE TABLE Ord (  
             OrderID     int  PRIMARY KEY,  
             CustomerID  varchar(5));  
    GO  
    CREATE TABLE Product (  
             ProductID   int PRIMARY KEY,  
             ProductName varchar(20));  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID));  
    GO  
    ```  
  
2.  Salvare lo schema fornito in precedenza in questo esempio come file SampleSchema.xml.  
  
3.  Salvare i dati XML di esempio seguenti come file SampleXMLData.xml:  
  
    ```xml  
    <ROOT>    
      <Order OrderID="1" CustomerID="ALFKI">  
        <Product ProductID="1" ProductName="Chai" />  
        <Product ProductID="2" ProductName="Chang" />  
      </Order>  
      <Order OrderID="2" CustomerID="ANATR">  
        <Product ProductID="3" ProductName="Aniseed Syrup" />  
        <Product ProductID="4" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere al file il codice VBScript fornito all'inizio di questo argomento. Modificare la stringa di connessione per specificare i nomi del server e del database appropriati. Rimuovere il commento dalle righe seguenti nel codice sorgente per questo esempio.  
  
    ```vbs  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    ```  
  
5.  Eseguire il codice VBScript. Il caricamento bulk XML carica il documento XML nelle tabelle Ord e Product.  
  
## <a name="d-bulk-loading-in-identity-type-columns"></a>D. Caricamento bulk in colonne di tipo Identity  
 In questo esempio viene illustrata la modalità di gestione delle colonne di tipo Identity da parte del caricamento bulk. Nell'esempio viene eseguito il caricamento bulk dei dati in tre tabelle, Ord, Product e OrderDetail.  
  
 In tali tabelle:  
  
-   OrderID nella tabella Ord è una colonna di tipo Identity.  
  
-   ProductID nella tabella Product è una colonna di tipo Identity.  
  
-   Le colonne OrderID e ProductID in OrderDetail sono colonne chiavi esterne che fanno riferimento alle colonne chiavi primarie corrispondenti nelle tabelle Ord e Product.  
  
 Di seguito vengono indicati gli schemi di tabella per l'esempio:  
  
```  
Ord (OrderID, CustomerID)  
Product (ProductID, ProductName)  
OrderDetail (OrderID, ProductID)  
```  
  
 In questo esempio di caricamento bulk XML la proprietà KeepIdentity del modello a oggetti BulkLoad è impostata su false. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] genera pertanto valori Identity per le colonne ProductID e OrderID rispettivamente nelle tabelle Product e Ord. Qualsiasi valore fornito nei documenti di cui eseguire un caricamento bulk viene ignorato.  
  
 In questo caso, il caricamento bulk XML identifica la relazione di chiave primaria/chiave esterna nelle tabelle. Il caricamento bulk inserisce innanzitutto record nelle tabelle con la chiave primaria, quindi propaga il valore Identity generato in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nelle tabelle con le colonne chiavi esterne. Nell'esempio seguente il caricamento bulk XML inserisce dati nelle tabelle in base all'ordine seguente:  
  
1.  Prodotto  
  
2.  Ord  
  
3.  OrderDetail  
  
    > [!NOTE]  
    >  Per propagare i valori Identity generati nelle tabelle Products e Orders, la logica di elaborazione richiede che il caricamento bulk XML tenga traccia di tali valori per l'inserimento successivo nella tabella OrderDetails. A tale scopo, il caricamento bulk XML crea tabelle intermedie, popola i dati in tali tabelle e successivamente li rimuove.  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare le tabelle seguenti:  
  
    ```  
    CREATE TABLE Ord (  
             OrderID     int identity(1,1)  PRIMARY KEY,  
             CustomerID  varchar(5));  
    GO  
    CREATE TABLE Product (  
             ProductID   int identity(1,1) PRIMARY KEY,  
             ProductName varchar(20));  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID));  
    GO  
    ```  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere lo schema XSD seguente al file.  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
     <xsd:annotation>  
       <xsd:appinfo>  
        <sql:relationship name="OrderOD"  
              parent="Ord"  
              parent-key="OrderID"  
              child="OrderDetail"  
              child-key="OrderID" />  
        <sql:relationship name="ODProduct"  
              parent="OrderDetail"  
              parent-key="ProductID"  
              child="Product"  
              child-key="ProductID"   
              inverse="true"/>  
       </xsd:appinfo>  
     </xsd:annotation>  
  
      <xsd:element name="Order" sql:relation="Ord"   
                                sql:key-fields="OrderID" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="Product" sql:relation="Product"   
                         sql:key-fields="ProductID"  
                         sql:relationship="OrderOD ODProduct">  
              <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
              </xsd:complexType>  
            </xsd:element>  
         </xsd:sequence>  
            <xsd:attribute name="OrderID"   type="xsd:integer" />   
            <xsd:attribute name="CustomerID"   type="xsd:string" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Aggiungere il documento XML seguente.  
  
    ```  
    <ROOT>    
      <Order OrderID="11" CustomerID="ALFKI">  
        <Product ProductID="11" ProductName="Chai" />  
        <Product ProductID="22" ProductName="Chang" />  
      </Order>  
      <Order OrderID="22" CustomerID="ANATR">  
         <Product ProductID="33" ProductName="Aniseed Syrup" />  
        <Product ProductID="44" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere al file il codice VBScript seguente: Modificare la stringa di connessione per specificare i nomi del server e del database appropriati. Specificare il percorso appropriato per i file che fungono da parametri per il **metodo Execute.**  
  
    ```  
    Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "C:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction = False  
    objBL.KeepIdentity = False  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    Set objBL = Nothing  
    MsgBox "Done."  
    ```  
  
5.  Eseguire il codice VBScript. Il caricamento bulk XML caricherà i dati nelle tabelle appropriate.  
  
## <a name="e-generating-table-schemas-before-bulk-loading"></a>E. Generazione di schemi di tabella prima del caricamento bulk  
 Il caricamento bulk XML può eventualmente generare le tabelle se queste non sono già presenti. L'impostazione della proprietà SchemaGen dell'oggetto SQLXMLBulkLoad su TRUE esegue questa operazione. Facoltativamente, è anche possibile richiedere il caricamento bulk XML per eliminare le tabelle esistenti e crearle di nuovo impostando la proprietà SGDropTables su TRUE. Nell'esempio di codice VBScript seguente viene illustrato l'utilizzo di tali proprietà.  
  
 Nell'esempio vengono inoltre impostate altre due proprietà su TRUE:  
  
-   CheckConstraints. Impostando questa proprietà su TRUE, è possibile garantire che i dati inseriti nelle tabelle non violino eventuali vincoli specificati nelle tabelle, in questo caso i vincoli PRIMARY KEY/FOREIGN KEY specificati tra le tabelle Cust e CustOrder. In caso di violazione di vincoli, il caricamento bulk non riesce.  
  
-   XmlFragment. Questa proprietà deve essere impostata su TRUE perché il documento XML di esempio (origine dati) non contiene singoli elementi di livello principale ed è pertanto un frammento.  
  
 Di seguito viene fornito il codice VBScript:  
  
```  
Dim objBL   
Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
  
objBL.CheckConstraints=true  
objBL.XMLFragment = True  
objBL.SchemaGen = True  
objBL.SGDropTables = True  
  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
Set objBL = Nothing  
```  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere al file lo schema XSD fornito nell'esempio precedente "Utilizzo di relazioni a catena nello schema per il caricamento bulk XML".  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Aggiungere al file il documento XML fornito nell'esempio precedente "Utilizzo di relazioni a catena nello schema per il caricamento bulk XML". Rimuovere \<ROOT> l'elemento dal documento (per renderlo un frammento).  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere al file il codice VBScript fornito in questo esempio. Modificare la stringa di connessione per specificare i nomi del server e del database appropriati. Specificare il percorso appropriato per i file specificati come parametri per il metodo Execute.  
  
4.  Eseguire il codice VBScript. Il caricamento bulk XML crea le tabelle necessarie in base allo schema di mapping fornito e vi esegue il caricamento bulk dei dati.  
  
## <a name="f-bulk-loading-from-a-stream"></a>F. Caricamento bulk da un flusso  
 Il metodo Execute del modello a oggetti con caricamento bulk XML accetta due parametri. Il primo parametro corrisponde al file dello schema di mapping. Il secondo parametro fornisce i dati XML da caricare nel database. Esistono due modi per passare i dati XML al metodo Execute del caricamento bulk XML:  
  
-   Specificare il nome di file come parametro.  
  
-   Passare un flusso contenente i dati XML.  
  
 In questo esempio viene illustrato come eseguire un caricamento bulk da un flusso.  
  
 VBScript esegue innanzitutto un'istruzione SELECT per recuperare informazioni sui clienti dalla tabella Customers del database Northwind. Poiché nell'istruzione SELECT è specificata la clausola FOR XML (con l'opzione ELEMENTS), la query restituisce un documento XML incentrato sugli elementi nel formato seguente:  
  
```  
<Customer>  
  <CustomerID>..</CustomerID>  
  <CompanyName>..</CompanyName>  
  <City>..</City>  
</Customer>  
...  
```  
  
 Lo script passa quindi il codice XML come flusso al metodo Execute come secondo parametro. Il metodo Execute carica in blocco i dati nella tabella Cust.  
  
 Poiché questo script imposta la proprietà SchemaGen su TRUE e la proprietà SGDropTables su TRUE, il caricamento bulk XML crea la tabella Cust nel database specificato. Se la tabella è già presente, questa viene innanzitutto eliminata e quindi ricreata.  
  
 Di seguito viene fornito l'esempio di codice di VBScript.  
  
```  
Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
Set objCmd = CreateObject("ADODB.Command")  
Set objConn = CreateObject("ADODB.Connection")  
Set objStrmOut = CreateObject ("ADODB.Stream")  
  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile     = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.SchemaGen        = True  
objBL.SGDropTables     = True  
objBL.XMLFragment      = True  
' Open a connection to the instance of SQL Server to get the source data.  
  
objConn.Open "provider=SQLOLEDB;server=(local);database=tempdb;integrated security=SSPI"  
Set objCmd.ActiveConnection = objConn  
objCmd.CommandText = "SELECT CustomerID, CompanyName, City FROM Customers FOR XML AUTO, ELEMENTS"  
  
' Open the return stream and execute the command.  
Const adCRLF = -1  
Const adExecuteStream = 1024  
objStrmOut.Open  
objStrmOut.LineSeparator = adCRLF  
objCmd.Properties("Output Stream").Value = objStrmOut  
objCmd.Execute , , adExecuteStream  
objStrmOut.Position = 0  
  
' Execute bulk load. Read source XML data from the stream.  
objBL.Execute "SampleSchema.xml", objStrmOut  
  
Set objBL = Nothing  
```  
  
 Nello schema di mapping XSD seguente vengono fornite le informazioni necessarie per creare la tabella:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:element name="ROOT" sql:is-constant="true" >  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element ref="Customers"/>  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
<xsd:element name="Customers" sql:relation="Cust" >  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element name="CustomerID"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(5)"/>  
      <xsd:element name="CompanyName"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(40)"/>  
      <xsd:element name="City"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(40)"/>  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
</xsd:schema>  
```  
  
 Di seguito viene fornito lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
    </ElementType>  
</Schema>  
```  
  
### <a name="opening-a-stream-on-an-existing-file"></a>Apertura di un flusso in un file esistente  
 È anche possibile aprire un flusso in un file di dati XML esistente e passare il flusso come parametro al metodo Execute (anziché passare il nome del file come parametro).  
  
 Di seguito viene fornito un esempio di passaggio di un flusso come parametro in Visual Basic:  
  
```  
Private Sub Form_Load()  
Dim objBL As New SQLXMLBulkLoad  
Dim objStrm As New ADODB.Stream  
Dim objFileSystem As New Scripting.FileSystemObject  
Dim objFile As Scripting.TextStream  
  
MsgBox "Begin BulkLoad..."  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.SchemaGen = True  
objBL.SGDropTables = True  
' Here again a stream is specified that contains the source data   
' (instead of the file name). But this is just an illustration.  
' Usually this is useful if you have an XML data   
' stream that is created by some other means that you want to bulk   
' load. This example starts with an XML text file, so it may not be the   
' best to use a stream (you can specify the file name directly).  
' Here you could have specified the file name itself.   
Set objFile = objFileSystem.OpenTextFile("c:\SampleData.xml")  
objStrm.Open  
objStrm.WriteText objFile.ReadAll  
objStrm.Position = 0  
objBL.Execute "c:\SampleSchema.xml", objStrm  
  
Set objBL = Nothing  
MsgBox "Done."  
End Sub  
```  
  
 Per testare l'applicazione, utilizzare il documento XML seguente in un file (SampleData.xml) e lo schema XSD fornito in questo esempio:  
  
 Di seguito viene fornita l'origine dati XML (SampleData.xml):  
  
```  
<ROOT>  
  <Customers>  
    <CustomerID>1111</CustomerID>  
    <CompanyName>Hanari Carnes</CompanyName>  
    <City>NY</City>  
    <Order OrderID="1" />  
    <Order OrderID="2" />  
  </Customers>  
  
  <Customers>  
    <CustomerID>1112</CustomerID>  
    <CompanyName>Toms Spezialitten</CompanyName>  
     <City>LA</City>  
    <Order OrderID="3" />  
  </Customers>  
  <Customers>  
    <CustomerID>1113</CustomerID>  
    <CompanyName>Victuailles en stock</CompanyName>  
    <Order CustomerID= "4444" OrderID="4" />  
</Customers>  
</ROOT>  
```  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
             <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
</Schema>  
```  
  
## <a name="g-bulk-loading-in-overflow-columns"></a>G. Caricamento bulk in colonne di overflow  
 Se lo schema di mapping specifica una colonna di overflow usando l'annotazione **sql:overflow-field,** il caricamento bulk XML copia tutti i dati non consumati dal documento di origine in questa colonna.  
  
 Si consideri lo schema XSD seguente:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Customers" sql:relation="Cust"  
                                sql:overflow-field="OverflowColumn" >  
   <xsd:complexType>  
     <xsd:sequence>  
       <xsd:element name="CustomerID"  type="xsd:integer" />  
       <xsd:element name="CompanyName" type="xsd:string" />  
       <xsd:element name="City"        type="xsd:string" />  
       <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
         <xsd:complexType>  
          <xsd:attribute name="OrderID" type="xsd:integer" />  
          <xsd:attribute name="CustomerID" type="xsd:integer" />  
         </xsd:complexType>  
       </xsd:element>  
     </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Lo schema identifica una colonna di overflow (OverflowColumn) per la tabella Cust. Di conseguenza, tutti i dati XML non consumati per ogni **\<Customer>** elemento vengono aggiunti a questa colonna.  
  
> [!NOTE]  
>  Tutti gli elementi astratti (elementi per cui è specificato **abstract="true")** e tutti gli attributi non consentiti (attributi per cui **è specificato prohibited="true")** vengono considerati overflow dal caricamento bulk XML e vengono aggiunti alla colonna di overflow, se specificato. In caso contrario, vengono ignorati.  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare due tabelle nel database **tempdb:**  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust (  
                  CustomerID     int         PRIMARY KEY,  
                  CompanyName    varchar(20) NOT NULL,  
                  City           varchar(20) DEFAULT 'Seattle',  
                  OverflowColumn nvarchar(200));  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID    int PRIMARY KEY,  
                  CustomerID int FOREIGN KEY   
                                 REFERENCES Cust(CustomerID));  
    GO  
    ```  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere lo schema XSD fornito in questo esempio al file.  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Aggiungere il seguente documento XML al file:  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City><![CDATA[NY]]> </City>  
        <Junk>garbage in overflow</Junk>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <![CDATA[LA]]>   
        <!-- <xyz><address>111 Maple, Seattle</address></xyz>   -->  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere a questo file il codice Microsoft Visual Basic, Scripting Edition (VBScript) seguente. Modificare la stringa di connessione per specificare i nomi del server e del database appropriati. Specificare il percorso appropriato per i file specificati come parametri per il metodo Execute.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Eseguire il codice VBScript.  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"   
                       sql:overflow-field="OverflowColumn"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
             <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
</Schema>  
```  
  
## <a name="h-specifying-the-file-path-for-temp-files-in-transaction-mode"></a>H. Impostazione del percorso dei file temporanei in modalità transazione  
 Quando si esegue il caricamento bulk in modalità transazione, ovvero quando la proprietà Transaction è impostata su TRUE, è necessario impostare anche la proprietà TempFilePath quando si verifica una delle condizioni seguenti:  
  
-   Viene eseguito un caricamento bulk in un server remoto.  
  
-   Si desidera utilizzare un'unità locale o una cartella alternativa, diversa dal percorso specificato dalla variabile di ambiente TEMP, per archiviare i file temporanei creati in modalità transazione.  
  
 Il codice VBScript seguente, ad esempio, esegue il caricamento bulk dei dati dal file SampleXMLData.xml nelle tabelle di database in modalità transazione. La proprietà TempFilePath viene specificata per impostare il percorso dei file temporanei generati in modalità transazione.  
  
```  
set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.Transaction=True  
objBL.TempFilePath="\\Server\MyDir"  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
set objBL=Nothing  
```  
  
> [!NOTE]  
>  Il percorso dei file temporanei deve essere un percorso condiviso a cui l'account del servizio dell'istanza di destinazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e l'account che esegue l'applicazione di caricamento bulk possano accedere. A meno che non si eserviti il caricamento bulk in un server locale, il percorso del file temporaneo deve essere un percorso UNC, ad esempio \\ \nomeserver\nomecondizione.  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare questa tabella nel database **tempdb:**  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust (     CustomerID uniqueidentifier,   
          LastName  varchar(20));  
    GO  
    ```  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere lo schema XSD seguente al file:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:attribute name="CustomerID"  type="xsd:string" />  
         <xsd:attribute name="LastName" type="xsd:string" />  
       </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Aggiungere il seguente documento XML al file:  
  
    ```  
    <ROOT>  
    <Customers CustomerID="6F9619FF-8B86-D011-B42D-00C04FC964FF"   
               LastName="Smith" />  
    </ROOT>  
    ```  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome ValidateAndBulkload.vbs. Aggiungere al file il codice VBScript seguente: Modificare la stringa di connessione per specificare i nomi del server e del database appropriati. Specificare il percorso appropriato per i file specificati come parametri per il metodo Execute. Specificare anche il percorso appropriato per la proprietà TempFilePath.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    objBL.TempFilePath="\\server\folder"  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Eseguire il codice VBScript.  
  
     Lo schema deve specificare il tipo di dati **sql:datatype** corrispondente per l'attributo **CustomerID** quando il valore di **CustomerID** viene specificato come GUID che include parentesi graffe ({ e }), ad esempio:  
  
    ```  
    <ROOT>  
    <Customers CustomerID="{6F9619FF-8B86-D011-B42D-00C04FC964FF}"   
               LastName="Smith" />  
    </ROOT>  
    ```  
  
     Di seguito viene fornito lo schema aggiornato:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:attribute name="CustomerID"  type="xsd:string"   
                        sql:datatype="uniqueidentifier" />  
         <xsd:attribute name="LastName" type="xsd:string" />  
       </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
     Quando viene specificato **sql:datatype** identificando il tipo di colonna come **uniqueidentifier,** l'operazione di caricamento bulk rimuove le parentesi graffe ({ e }) dal valore **CustomerID** prima di inserirlo nella colonna.  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
<ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
</ElementType>  
<ElementType name="Customers" sql:relation="Cust" >  
  <AttributeType name="CustomerID"  sql:datatype="uniqueidentifier" />  
  <AttributeType name="LastName"   />  
  
  <attribute type="CustomerID" />  
  <attribute type="LastName"   />  
</ElementType>  
</Schema>  
```  
  
## <a name="i-using-an-existing-database-connection-with-the-connectioncommand-property"></a>I. Utilizzo di una connessione al database esistente con la proprietà ConnectionCommand  
 È possibile utilizzare una connessione ADO esistente per eseguire il caricamento bulk XML. Si tratta di una scelta utile se il caricamento bulk XML è solo una delle numerose operazioni che verranno eseguite su un'origine dati.  
  
 La proprietà ConnectionCommand consente di usare una connessione ADO esistente usando un oggetto comando ADO. Questo comportamento viene illustrato nell'esempio di Visual Basic seguente:  
  
```  
Private Sub Form_Load()  
Dim objBL As New SQLXMLBulkLoad4  
Dim objCmd As New ADODB.Command  
Dim objConn As New ADODB.Connection  
  
'Open a connection to an instance of SQL Server.  
objConn.Open "provider=SQLOLEDB;data source=(local);database=tempdb;integrated security=SSPI"  
'Ask the Command object to use the connection just established.  
Set objCmd.ActiveConnection = objConn  
  
'Tell Bulk Load to use the active command object that is using the Connection obj.  
objBL.ConnectionCommand = objCmd  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
'The Transaction property must be set to True if you use ConnectionCommand.  
objBL.Transaction = True  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
Set objBL = Nothing  
End Sub  
```  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare due tabelle nel database **tempdb:**  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust(  
                   CustomerID   varchar(5) PRIMARY KEY,  
                   CompanyName  varchar(30),  
                   City         varchar(20));  
    GO  
    CREATE TABLE CustOrder(  
                   CustomerID  varchar(5) references Cust (CustomerID),  
                   OrderID     varchar(5) PRIMARY KEY);  
    GO  
    ```  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Aggiungere lo schema XSD seguente al file:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
    <xsd:annotation>  
      <xsd:appinfo>  
        <sql:relationship name="CustCustOrder"  
              parent="Cust"  
              parent-key="CustomerID"  
              child="CustOrder"  
              child-key="CustomerID" />  
      </xsd:appinfo>  
    </xsd:annotation>  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:sequence>  
           <xsd:element name="CustomerID"  type="xsd:integer" />  
           <xsd:element name="CompanyName" type="xsd:string" />  
           <xsd:element name="City"        type="xsd:string" />  
           <xsd:element name="Order"   
                              sql:relation="CustOrder"  
                              sql:relationship="CustCustOrder" >  
             <xsd:complexType>  
              <xsd:attribute name="OrderID" type="xsd:integer" />  
              <xsd:attribute name="CustomerID" type="xsd:integer" />  
             </xsd:complexType>  
           </xsd:element>  
         </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Aggiungere il seguente documento XML al file:  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <City>LA</City>  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  Creare un'applicazione Visual Basic (Standard EXE) e il codice precedente. Aggiungere i riferimenti seguenti al progetto:  
  
    ```  
    Microsoft XML BulkLoad for SQL Server 4.0 Type Library  
    Microsoft ActiveX Data objects 2.6 Library  
    ```  
  
5.  Eseguire l'applicazione.  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
         <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
</Schema>  
```  
  
## <a name="j-bulk-loading-in-xml-data-type-columns"></a>J. Caricamento bulk in colonne con tipo di dati xml  
 Se lo schema di mapping specifica una colonna con tipo di dati [xml](../../../t-sql/xml/xml-transact-sql.md) tramite l'annotazione **sql:datatype="xml",** il caricamento bulk XML può copiare gli elementi figlio XML per il campo mappato dal documento di origine in questa colonna.  
  
 Si consideri lo schema XSD seguente, che esegue il mapping di una vista della tabella Production.ProductModel nel database di esempio AdventureWorks. In questa tabella viene eseguito il mapping del campo CatalogDescription del tipo di dati **xml** a un elemento usando le annotazioni **\<Desc>** **sql:field** e **sql:datatype="xml".**  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
           xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
           xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">   
  <xsd:element name="ProductModel"  sql:relation="Production.ProductModel" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Name" type="xs:string"></xsd:element>  
        <xsd:element name="Desc" sql:field="CatalogDescription" sql:datatype="xml">  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element name="ProductDescription">  
              <xsd:complexType>  
                <xsd:sequence>  
                  <xsd:element name="Summary" type="xs:anyType"/>  
                </xsd:sequence>  
              </xsd:complexType>  
            </xsd:element>  
          </xsd:sequence>  
        </xsd:complexType>  
        </xsd:element>   
     </xsd:sequence>  
     <xsd:attribute name="ProductModelID" sql:field="ProductModelID" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Verificare che il database di esempio AdventureWorks sia installato.  
  
2.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleSchema.xml. Copiare lo schema XSD precedente, incollarlo nel file e salvarlo.  
  
3.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome SampleXMLData.xml. Copiare il documento XML seguente, incollarlo nel file e salvarlo nella stessa cartella utilizzata per il passaggio precedente.  
  
    ```  
    <ProductModel ProductModelID="2005">  
        <Name>Mountain-100 (2005 model)</Name>  
        <Desc><?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>  
            <p1:ProductDescription xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
                  xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
                  xmlns:wf="https://www.adventure-works.com/schemas/OtherFeatures"   
                  xmlns:html="http://www.w3.org/1999/xhtml"   
                  xmlns="">  
                <p1:Summary>  
                    <html:p>Our top-of-the-line competition mountain bike.   
          Performance-enhancing options include the innovative HL Frame,   
          super-smooth front suspension, and traction for all terrain.  
                            </html:p>  
                </p1:Summary>  
                <p1:Manufacturer>  
                    <p1:Name>AdventureWorks</p1:Name>  
                    <p1:Copyright>2002-2005</p1:Copyright>  
                    <p1:ProductURL>HTTP://www.Adventure-works.com</p1:ProductURL>  
                </p1:Manufacturer>  
                <p1:Features>These are the product highlights.   
                     <wm:Warranty>  
                        <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
                        <wm:Description>parts and labor</wm:Description>  
                    </wm:Warranty><wm:Maintenance>  
                        <wm:NoOfYears>10 years</wm:NoOfYears>  
                        <wm:Description>maintenance contract available through your dealer or any AdventureWorks retail store.</wm:Description>  
                    </wm:Maintenance><wf:wheel>High performance wheels.</wf:wheel><wf:saddle>  
                        <html:i>Anatomic design</html:i> and made from durable leather for a full-day of riding in comfort.</wf:saddle><wf:pedal>  
                        <html:b>Top-of-the-line</html:b> clipless pedals with adjustable tension.</wf:pedal><wf:BikeFrame>Each frame is hand-crafted in our Bothell facility to the optimum diameter   
          and wall-thickness required of a premium mountain frame.   
          The heat-treated welded aluminum frame has a larger diameter tube that absorbs the bumps.</wf:BikeFrame><wf:crankset> Triple crankset; alumunim crank arm; flawless shifting. </wf:crankset></p1:Features>  
                <!-- add one or more of these elements... one for each specific product in this product model -->  
                <p1:Picture>  
                    <p1:Angle>front</p1:Angle>  
                    <p1:Size>small</p1:Size>  
                    <p1:ProductPhotoID>118</p1:ProductPhotoID>  
                </p1:Picture>  
                <!-- add any tags in <specifications> -->  
                <p1:Specifications> These are the product specifications.  
                       <Material>Almuminum Alloy</Material><Color>Available in most colors</Color><ProductLine>Mountain bike</ProductLine><Style>Unisex</Style><RiderExperience>Advanced to Professional riders</RiderExperience></p1:Specifications>  
            </p1:ProductDescription>  
        </Desc>  
    </ProductModel>  
    ```  
  
4.  Creare un file nell'editor di testo o XML preferito e salvarlo con il nome BulkloadXml.vbs. Copiare il codice VBScript seguente e incollarlo nel file. Salvarlo nella stessa cartella utilizzata per i dati e i file di XML Schema precedenti.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=MyServer;database=AdventureWorks;integrated security=SSPI"  
  
    Dim fso, sAppPath  
    Set fso = CreateObject("Scripting.FileSystemObject")   
    sAppPath = fso.GetFolder(".")   
  
    objBL.ErrorLogFile = sAppPath & "\error.log"  
  
    'Execute XML bulkload using file.  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Eseguire lo script BulkloadXml.vbs.  
  
  
