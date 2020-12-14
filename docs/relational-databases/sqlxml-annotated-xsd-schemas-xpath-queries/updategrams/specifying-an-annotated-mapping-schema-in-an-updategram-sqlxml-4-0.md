---
title: Schemi di mapping con annotazioni per un updategram (SQLXML)
description: Viene illustrato come viene utilizzato uno schema di mapping XSD o XDR con annotazioni specificato in un updategram SQLXML 4,0 per elaborare gli aggiornamenti in un database.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, updategrams
- data types [SQLXML], mapping schema in updategrams
- updategrams [SQLXML], annotated mapping schemas
- annotated XDR schemas, updategrams
- inverse attribute
- parent-child relationships [SQLXML]
- mapping-schema attribute
- mapping schema [SQLXML], updategrams
- sql:inverse
ms.assetid: 2e266ed9-4cfb-434a-af55-d0839f64bb9a
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8c8643c40f7b62c0a0fdc3d85d32111d05ee955f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97405112"
---
# <a name="specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-40"></a>Specifica di uno schema di mapping con annotazioni in un updategram (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene illustrata la modalità di utilizzo dello schema di mapping (XSD o XDR) specificato in un updategram per l'elaborazione degli aggiornamenti. In un updategram è possibile specificare il nome di uno schema di mapping con annotazioni da utilizzare per eseguire il mapping degli elementi e degli attributi nell'updategram alle tabelle e alle colonne in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Quando si specifica uno schema di mapping in un updategram, è necessario eseguire il mapping dei nomi di elemento e di attributo specificati nell'updategram agli elementi e agli attributi dello schema di mapping.  
  
 Per specificare uno schema di mapping, utilizzare l'attributo **mapping-schema** dell' **\<sync>** elemento. Negli esempi seguenti sono illustrati due updategram, uno che utilizza uno schema di mapping semplice e uno che utilizza uno schema più complesso.  
  
> [!NOTE]  
>  In questa documentazione si presuppone che l'utente disponga di una certa familiarità con i modelli e il supporto dello schema di mapping in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [Introduzione agli schemi XSD con Annotazioni &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Per le applicazioni legacy che usano XDR, vedere la pagina relativa agli [schemi XDR con Annotazioni &#40;deprecati in SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="dealing-with-data-types"></a>Gestione dei tipi di dati  
 Se lo schema specifica il tipo di dati **Image**, **Binary** o **varbinary** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (tramite **SQL: DataType**) e non specifica un tipo di dati XML, l'updategram presuppone che il tipo di dati XML sia **binario di base 64**. Se i dati sono di tipo **bin. base** , è necessario specificare in modo esplicito il tipo (**DT: type = bin. base** o **Type = "xsd: hexBinary"**).  
  
 Se lo schema specifica il tipo di dati XSD **DateTime**, **date** o **Time** , è necessario specificare anche il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tipo di dati corrispondente utilizzando **SQL: DataType = "DateTime"**.  
  
 Quando si gestiscono parametri di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tipo **Money** , è necessario specificare in modo esplicito **SQL: DataType = "Money"** sul nodo appropriato nello schema di mapping.  
  
## <a name="examples"></a>Esempio  
 Per creare esempi funzionanti utilizzando gli esempi seguenti, è necessario soddisfare i requisiti specificati nei [requisiti per l'esecuzione di esempi SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-creating-an-updategram-with-a-simple-mapping-schema"></a>R. Creazione di un updategram con uno schema di mapping semplice  
 Lo schema XSD seguente (SampleSchema.xml) è uno schema di mapping che esegue il mapping dell' **\<Customer>** elemento alla tabella Sales. Customer:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
        <xsd:attribute name="CustID"    
                       sql:field="CustomerID"   
                       type="xsd:string" />  
        <xsd:attribute name="RegionID"    
                       sql:field="TerritoryID"    
                       type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 L'updategram seguente inserisce un record nella tabella Sales.Customer e si basa sullo schema di mapping precedente per eseguire il mapping di questi dati alla tabella correttamente. Si noti che l'updategram utilizza lo stesso nome di elemento, **\<Customer>** , come definito nello schema. Questa condizione è obbligatoria perché l'updategram specifica uno schema particolare.  
  
##### <a name="to-test-the-updategram"></a>Per testare l'updategram  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file come SampleUpdateSchema.xml.  
  
2.  Copiare il modello di updategram seguente e incollarlo in un file di testo. Salvare il file come SampleUpdategram.xml nella stessa directory nella quale è stato salvato SampleUpdateSchema.xml.  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
      <updg:sync mapping-schema="SampleUpdateSchema.xml">  
        <updg:before>  
          <Customer CustID="1" RegionID="1"  />  
        </updg:before>  
        <updg:after>  
          <Customer CustID="1" RegionID="2" />  
        </updg:after>  
      </updg:sync>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (SampleUpdateSchema.xml) è relativo alla directory nella quale viene salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per ulteriori informazioni, vedere [utilizzo di ADO per eseguire query SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
   <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
         xmlns:dt="urn:schemas-microsoft-com:datatypes"   
         xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
     <ElementType name="Customer" sql:relation="Sales.Customer" >  
       <AttributeType name="CustID" />  
       <AttributeType name="RegionID" />  
  
       <attribute type="CustID" sql:field="CustomerID" />  
       <attribute type="RegionID" sql:field="TerritoryID" />  
     </ElementType>  
   </Schema>   
```  
  
### <a name="b-inserting-a-record-by-using-the-parent-child-relationship-specified-in-the-mapping-schema"></a>B. Inserimento di un record tramite la relazione padre-figlio specificata nello schema di mapping  
 Gli elementi dello schema possono essere correlati. L' **\<sql:relationship>** elemento specifica la relazione padre-figlio tra gli elementi dello schema. Queste informazioni vengono utilizzate per aggiornare le tabelle corrispondenti che presentano una relazione chiave primaria/chiave esterna.  
  
 Lo schema di mapping seguente (SampleSchema.xml) è costituito da due elementi **\<Order>** **\<OD>** :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrderOD"  
          parent="Sales.SalesOrderHeader"  
          parent-key="SalesOrderID"  
          child="Sales.SalesOrderDetail"  
          child-key="SalesOrderID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="OD"   
                     sql:relation="Sales.SalesOrderDetail"  
                     sql:relationship="OrderOD" >  
           <xsd:complexType>  
              <xsd:attribute name="SalesOrderID"   type="xsd:integer" />  
              <xsd:attribute name="ProductID" type="xsd:integer" />  
             <xsd:attribute name="UnitPrice"  type="xsd:decimal" />  
             <xsd:attribute name="OrderQty"   type="xsd:integer" />  
             <xsd:attribute name="UnitPriceDiscount"   type="xsd:decimal" />  
  
           </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
        <xsd:attribute name="SalesOrderID"  type="xsd:integer" />  
        <xsd:attribute name="OrderDate"  type="xsd:date" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 L'updategram seguente utilizza questo schema XSD per aggiungere un nuovo record di dettagli dell'ordine (un **\<OD>** elemento nel **\<after>** blocco) per l'ordine 43860. L'attributo **mapping-schema** viene utilizzato per specificare lo schema di mapping nell'updategram.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="SampleUpdateSchema.xml" >  
    <updg:before>  
       <Order SalesOrderID="43860" />  
    </updg:before>  
    <updg:after>  
      <Order SalesOrderID="43860" >  
           <OD ProductID="753" UnitPrice="$10.00"  
               Quantity="5" Discount="0.0" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Per testare l'updategram  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file come SampleUpdateSchema.xml.  
  
2.  Copiare il modello di updategram precedente e incollarlo in un file di testo. Salvare il file come SampleUpdategram.xml nella stessa directory nella quale è stato salvato SampleUpdateSchema.xml.  
  
     Il percorso di directory specificato per lo schema di mapping (SampleUpdateSchema.xml) è relativo alla directory nella quale viene salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per ulteriori informazioni, vedere [utilizzo di ADO per eseguire query SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="OD" sql:relation="Sales.SalesOrderDetail" >  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="ProductID" />  
    <AttributeType name="UnitPrice"  dt:type="fixed.14.4" />  
    <AttributeType name="OrderQty" />  
    <AttributeType name="UnitPriceDiscount" />  
  
    <attribute type="SalesOrderID" />  
    <attribute type="ProductID" />  
    <attribute type="UnitPrice" />  
    <attribute type="OrderQty" />  
    <attribute type="UnitPriceDiscount" />  
</ElementType>  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader" >  
    <AttributeType name="CustomerID" />  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="OrderDate" />  
  
    <attribute type="CustomerID" />  
    <attribute type="SalesOrderID" />  
    <attribute type="OrderDate" />  
    <element type="OD" >  
             <sql:relationship   
                   key-relation="Sales.SalesOrderHeader"  
                   key="SalesOrderID"  
                   foreign-key="SalesOrderID"  
                   foreign-relation="Sales.SalesOrderDetail" />  
    </element>  
</ElementType>  
</Schema>  
```  
  
### <a name="c-inserting-a-record-by-using-the-parent-child-relationship-and-inverse-annotation-specified-in-the-xsd-schema"></a>C. Inserimento di un record tramite la relazione padre-figlio e l'annotazione inverse specificata nello schema XSD  
 In questo esempio viene illustrato il modo in cui la logica dell'updategram utilizza la relazione padre-figlio specificata nell'XSD per elaborare gli aggiornamenti e come viene utilizzata l'annotazione **inversa** . Per ulteriori informazioni sull'annotazione **inversa** , vedere [specifica dell'attributo SQL: inverse in sql: relationship &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md).  
  
 In questo esempio si presuppone che le tabelle seguenti si trovino nel database **tempdb** :  
  
-   `Cust (CustomerID, CompanyName)`, dove `CustomerID` è la chiave primaria  
  
-   `Ord (OrderID, CustomerID)`, dove `CustomerID` è una chiave esterna che fa riferimento alla chiave primaria `CustomerID` nella tabella `Cust`.  
  
 L'updategram utilizza lo schema XSD seguente per inserire i record nelle tabelle Cust e Ord:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
       <sql:relationship name="OrdCust" inverse="true"  
                  parent="Ord"  
                  parent-key="CustomerID"  
                  child-key="CustomerID"  
                  child="Cust"/>  
  </xsd:appinfo>  
</xsd:annotation>  
  
<xsd:element name="Order" sql:relation="Ord">  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element ref="Customer" sql:relationship="OrdCust"/>  
    </xsd:sequence>  
    <xsd:attribute name="OrderID"   type="xsd:int"/>  
    <xsd:attribute name="CustomerID" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:element>  
  
<xsd:element name="Customer" sql:relation="Cust">  
  <xsd:complexType>  
     <xsd:attribute name="CustomerID"  type="xsd:string"/>  
    <xsd:attribute name="CompanyName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:element>  
  
</xsd:schema>  
```  
  
 Lo schema XSD in questo esempio include **\<Customer>** **\<Order>** gli elementi e e specifica una relazione padre-figlio tra i due elementi. Identifica **\<Order>** come elemento padre e **\<Customer>** come elemento figlio.  
  
 La logica di elaborazione dell'updategram utilizza le informazioni sulla relazione padre-figlio per determinare l'ordine in cui i record vengono inseriti nelle tabelle. In questo esempio, la logica dell'updategram tenta innanzitutto di inserire un record nella tabella Ord (perché **\<Order>** è l'elemento padre), quindi tenta di inserire un record nella tabella Cust (perché **\<Customer>** è l'elemento figlio). A causa delle informazioni su chiave primaria/chiave esterna contenute nello schema della tabella di database, questa operazione di inserimento genera tuttavia una violazione di chiave esterna nel database e pertanto l'inserimento ha esito negativo.  
  
 Per indicare alla logica dell'updategram di invertire la relazione padre-figlio durante l'operazione di aggiornamento, l'annotazione **inversa** viene specificata nell' **\<relationship>** elemento. Di conseguenza, i record vengono aggiunti prima nella tabella Cust e successivamente nella tabella Ord e l'operazione riesce.  
  
 Nell'updategram seguente viene inserito un ordine (OrderID=2) nella tabella Ord e un cliente (CustomerID='AAAAA) nella tabella Cust tramite lo schema XSD specificato:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="SampleUpdateSchema.xml" >  
    <updg:before/>  
    <updg:after>  
      <Order OrderID="2" CustomerID="AAAAA" >  
        <Customer CustomerID="AAAAA" CompanyName="AAAAA Company" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Per testare l'updategram  
  
1.  Creare le tabelle seguenti nel database **tempdb** :  
  
    ```  
    USE tempdb  
    CREATE TABLE Cust(CustomerID varchar(5) primary key,   
                      CompanyName varchar(20))  
    GO  
    CREATE TABLE Ord (OrderID int primary key,   
                      CustomerID varchar(5) references Cust(CustomerID))  
    GO  
    ```  
  
2.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file come SampleUpdateSchema.xml.  
  
3.  Copiare il modello di updategram precedente e incollarlo in un file di testo. Salvare il file come SampleUpdategram.xml nella stessa directory nella quale è stato salvato SampleUpdateSchema.xml.  
  
     Il percorso di directory specificato per lo schema di mapping (SampleUpdateSchema.xml) è relativo alla directory nella quale viene salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
4.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per ulteriori informazioni, vedere [utilizzo di ADO per eseguire query SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza degli updategram &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
