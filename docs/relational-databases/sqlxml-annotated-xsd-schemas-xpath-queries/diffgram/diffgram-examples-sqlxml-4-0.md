---
title: Esempi di diffGram (SQLXML)
description: Visualizzare esempi di diffgram in SQLXML 4.0 che eseguono operazioni di inserimento, aggiornamento ed eliminazione in un database.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- DiffGrams [SQLXML], examples
- examples [SQLXML], DiffGram
- diffgr:parentID
- parentID annotation
ms.assetid: fc148583-dfd3-4efb-a413-f47b150b0975
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 02bdf0338e4393bd08add1ab1c8dcd4c0735b14a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491859"
---
# <a name="diffgram-examples-sqlxml-40"></a>Esempi di DiffGram (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Negli esempi presenti in questo argomento viene illustrato l'utilizzo di DiffGram per eseguire operazioni di inserimento, aggiornamento ed eliminazione nel database. Prima di utilizzare gli esempi, si tenga presente quanto segue:  
  
-   Negli esempi vengono utilizzate due tabelle (Cust e Ord) che devono essere create se si desidera testare gli esempi di DiffGram:  
  
    ```  
    Cust(CustomerID, CompanyName, ContactName)  
    Ord(OrderID, CustomerID)  
    ```  
  
-   La maggior parte degli esempi presenti in questo argomento utilizza lo schema XSD seguente:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
    <xsd:annotation>  
      <xsd:documentation>  
        Diffgram Customers/Orders Schema.  
      </xsd:documentation>  
      <xsd:appinfo>  
           <sql:relationship name="CustomersOrders"   
                      parent="Cust"  
                      parent-key="CustomerID"  
                      child-key="CustomerID"  
                      child="Ord"/>  
      </xsd:appinfo>  
    </xsd:annotation>  
  
    <xsd:element name="Customer" sql:relation="Cust">  
      <xsd:complexType>  
        <xsd:sequence>  
          <xsd:element name="CompanyName"    type="xsd:string"/>  
          <xsd:element name="ContactName"    type="xsd:string"/>  
           <xsd:element name="Order" sql:relation="Ord" sql:relationship="CustomersOrders">  
            <xsd:complexType>  
              <xsd:attribute name="OrderID" type="xsd:int" sql:field="OrderID"/>  
              <xsd:attribute name="CustomerID" type="xsd:string"/>  
            </xsd:complexType>  
          </xsd:element>  
        </xsd:sequence>  
        <xsd:attribute name="CustomerID" type="xsd:string" sql:field="CustomerID"/>  
      </xsd:complexType>  
    </xsd:element>  
  
    </xsd:schema>     
    ```  
  
     Salvare questo schema con il nome DiffGramSchema.xml nella stessa cartella in cui vengono salvati gli altri file utilizzati negli esempi.  
  
## <a name="a-deleting-a-record-by-using-a-diffgram"></a>R. Eliminazione di un record mediante DiffGram  
 In questo esempio DiffGram viene utilizzato per eliminare un record del cliente (il cui CustomerID è ALFKI) dalla tabella Cust e per eliminare il record dell'ordine corrispondente (il cui OrderID è 1) dalla tabella Ord.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
           xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
           xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance/>  
  
    <diffgr:before>  
        <Order diffgr:id="Order1"   
               msdata:rowOrder="0"   
               CustomerID="ALFKI"   
               OrderID="1"/>  
        <Customer diffgr:id="Customer1"   
                  msdata:rowOrder="0"   
                  CustomerID="ALFKI">  
           <CompanyName>Alfreds Futterkiste</CompanyName>  
           <ContactName>Maria Anders</ContactName>  
        </Customer>  
    </diffgr:before>  
    <msdata:errors/>  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 Nel blocco sono presenti un elemento **\<before>** **\<Order>** (**diffgr:id="Order1"**) e un elemento **\<Customer>** (**diffgr:id="Customer1"**). Questi elementi rappresentano i record esistenti nel database. **\<DataInstance>** L'elemento non ha i record corrispondenti (con lo stesso **diffgr:id**). indicando un'operazione di eliminazione.  
  
#### <a name="to-test-the-diffgram"></a>Per testare DiffGram  
  
1.  Creare queste tabelle nel database **tempdb.**  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Aggiungere i dati di esempio seguenti:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiare il DiffGram sopra riportato e incollarlo in un file di testo. Salvare il file con il nome MyDiffGram.xml nella stessa cartella utilizzata nel passaggio precedente.  
  
4.  Copiare il DiffGramSchema fornito precedentemente in questo argomento e incollarlo in un file di testo. Salvare il file con il nome DiffGramSchema.xml.  
  
5.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il DiffGram.  
  
     Per altre informazioni, vedere [Uso di ADO per eseguire query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
## <a name="b-inserting-a-record-by-using-a-diffgram"></a>B. Inserimento di un record mediante DiffGram  
 In questo esempio DiffGram viene utilizzato per inserire un record nella tabella Cust e un record nella tabella Ord.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"   
      sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
          xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
          xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
       <Customer diffgr:id="Customer1" msdata:rowOrder="0"    
                 diffgr:hasChanges="inserted" CustomerID="ALFKI">  
        <CompanyName>C3Company</CompanyName>  
        <ContactName>C3Contact</ContactName>  
        <Order diffgr:id="Order1"   
               msdata:rowOrder="0"  
               diffgr:hasChanges="inserted"   
               CustomerID="ALFKI" OrderID="1"/>  
      </Customer>  
    </DataInstance>  
  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 In questo DiffGram il **\<before>** blocco non è specificato (nessun record di database esistente identificato). Esistono due istanze di record (identificate dagli elementi e nel blocco ) che eseere mappate rispettivamente alle tabelle Cust e **\<Customer>** **\<Order>** **\<DataInstance>** Ord. Entrambi questi elementi specificano **l'attributo diffgr:hasChanges** (**hasChanges="inserted"**). indicando un'operazione di inserimento. In questo DiffGram, se si specifica **hasChanges="modified",** si indica che si vuole modificare un record che non esiste e viene generato un errore.  
  
#### <a name="to-test-the-diffgram"></a>Per testare DiffGram  
  
1.  Creare queste tabelle nel database **tempdb.**  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Aggiungere i dati di esempio seguenti:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiare il DiffGram sopra riportato e incollarlo in un file di testo. Salvare il file con il nome MyDiffGram.xml nella stessa cartella utilizzata nel passaggio precedente.  
  
4.  Copiare il DiffGramSchema fornito precedentemente in questo argomento e incollarlo in un file di testo. Salvare il file con il nome DiffGramSchema.xml.  
  
5.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il DiffGram.  
  
     Per altre informazioni, vedere [Uso di ADO per eseguire query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
## <a name="c-updating-an-existing-record-by-using-a-diffgram"></a>C. Aggiornamento di un record esistente mediante DiffGram  
 In questo esempio DiffGram viene utilizzato per aggiornare le informazioni per il cliente ALFKI (CompanyName e ContactName).  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
           xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
           xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
      <Customer diffgr:id="Customer1"   
                msdata:rowOrder="0" diffgr:hasChanges="modified"   
                CustomerID="ALFKI">  
          <CompanyName>Bottom Dollar Markets</CompanyName>  
          <ContactName>Antonio Moreno</ContactName>  
      </Customer>  
    </DataInstance>  
  
    <diffgr:before>  
     <Customer diffgr:id="Customer1"   
               msdata:rowOrder="0"   
               CustomerID="ALFKI">  
        <CompanyName>Alfreds Futterkiste</CompanyName>  
        <ContactName>Maria Anders</ContactName>  
      </Customer>  
    </diffgr:before>  
  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 Il **\<before>** blocco include un elemento ( **\<Customer>** **diffgr:id="Customer1"**). Il **\<DataInstance>** blocco include l'elemento **\<Customer>** corrispondente con lo stesso **ID**. **\<customer>** L'elemento in specifica anche **\<NewDataSet>** **diffgr:hasChanges="modified"**. Ciò indica un'operazione di aggiornamento e il record del cliente nella **tabella Cust** viene aggiornato di conseguenza. Si noti che se **l'attributo diffgr:hasChanges** non è specificato, la logica di elaborazione DiffGram ignora questo elemento e non viene eseguito alcun aggiornamento.  
  
#### <a name="to-test-the-diffgram"></a>Per testare DiffGram  
  
1.  Creare queste tabelle nel database **tempdb.**  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Aggiungere i dati di esempio seguenti:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiare il DiffGram sopra riportato e incollarlo in un file di testo. Salvare il file con il nome MyDiffGram.xml nella stessa cartella utilizzata nel passaggio precedente.  
  
4.  Copiare il DiffGramSchema fornito precedentemente in questo argomento e incollarlo in un file di testo. Salvare il file con il nome DiffGramSchema.xml.  
  
5.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il DiffGram.  
  
     Per altre informazioni, vedere [Uso di ADO per eseguire query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
## <a name="d-inserting-updating-and-deleting-records-by-using-a-diffgram"></a>D. Inserimento, aggiornamento ed eliminazione di record mediante DiffGram  
 In questo esempio viene utilizzato un DiffGram relativamente complesso per eseguire le operazioni di inserimento, aggiornamento ed eliminazione.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
      <Customer diffgr:id="Customer2" msdata:rowOrder="1"   
                diffgr:hasChanges="modified"   
                CustomerID="ANATR">  
          <CompanyName>Bottom Dollar Markets</CompanyName>  
          <ContactName>Elizabeth Lincoln</ContactName>  
          <Order diffgr:id="Order2" msdata:rowOrder="1"   
               msdata:hiddenCustomerID="ANATR"   
               CustomerID="ANATR" OrderID="2"/>  
      </Customer>  
  
      <Customer diffgr:id="Customer3" msdata:rowOrder="2"   
                CustomerID="ANTON">  
         <CompanyName>Chop-suey Chinese</CompanyName>  
         <ContactName>Yang Wang</ContactName>  
         <Order diffgr:id="Order3" msdata:rowOrder="2"   
               msdata:hiddenCustomerID="ANTON"   
               CustomerID="ANTON" OrderID="3"/>  
      </Customer>  
      <Customer diffgr:id="Customer4" msdata:rowOrder="3"   
                diffgr:hasChanges="inserted"   
                CustomerID="AROUT">  
         <CompanyName>Around the Horn</CompanyName>  
         <ContactName>Thomas Hardy</ContactName>  
         <Order diffgr:id="Order4" msdata:rowOrder="3"   
                diffgr:hasChanges="inserted"   
                msdata:hiddenCustomerID="AROUT"   
               CustomerID="AROUT" OrderID="4"/>  
      </Customer>  
    </DataInstance>  
    <diffgr:before>  
      <Order diffgr:id="Order1" msdata:rowOrder="0"   
             msdata:hiddenCustomerID="ALFKI"   
             CustomerID="ALFKI" OrderID="1"/>  
      <Customer diffgr:id="Customer1" msdata:rowOrder="0"   
                CustomerID="ALFKI">  
        <CompanyName>Alfreds Futterkiste</CompanyName>  
        <ContactName>Maria Anders</ContactName>  
      </Customer>  
      <Customer diffgr:id="Customer2" msdata:rowOrder="1"   
                CustomerID="ANATR">  
        <CompanyName>Ana Trujillo Emparedados y helados</CompanyName>  
        <ContactName>Ana Trujillo</ContactName>  
      </Customer>  
    </diffgr:before>  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 La logica DiffGram elabora questo DiffGram nel modo seguente:  
  
-   In base alla logica di elaborazione di DiffGram, tutti gli elementi di primo livello nel blocco vengono mappati alle tabelle corrispondenti, come **\<before>** descritto nello schema di mapping.  
  
-   Il blocco ha un elemento **\<before>** **\<Order>** (**dffgr:id="Order1"**) e un elemento **\<Customer>** (**diffgr:id="Customer1"**) per il quale non esiste alcun elemento corrispondente nel blocco (con lo stesso **\<DataInstance>** ID). indicando un'operazione di eliminazione e i record vengono eliminati dalle tabelle Cust e Ord.  
  
-   Il **\<before>** blocco ha un elemento ( **\<Customer>** **diffgr:id="Customer2"**) per il quale è presente un elemento corrispondente nel blocco **\<Customer>** **\<DataInstance>** (con lo stesso ID). L'elemento **\<DataInstance>** nel blocco specifica **diffgr:hasChanges="modified"**. Si tratta di un'operazione di aggiornamento in cui, per il cliente ANATR, le informazioni CompanyName e ContactName vengono aggiornate nella tabella Cust usando i valori specificati nel **\<DataInstance>** blocco .  
  
-   Il **\<DataInstance>** blocco ha un elemento ( **\<Customer>** **diffgr:id="Customer3"**) e un elemento **\<Order>** (**diffgr:id="Order3"**). Nessuno di questi elementi specifica **l'attributo diffgr:hasChanges.** Pertanto, la logica di elaborazione di DiffGram ignora tali elementi.  
  
-   Il blocco ha un elemento **\<DataInstance>** **\<Customer>** (**diffgr:id="Customer4"**) e un elemento **\<Order>** (**diffgr:id="Order4"**) per il quale non sono presenti elementi corrispondenti nel \<before> blocco. Questi elementi nel **\<DataInstance>** blocco specificano **diffgr:hasChanges="inserted".** Pertanto, viene aggiunto un nuovo record nella tabella Cust e nella tabella Ord.  
  
#### <a name="to-test-the-diffgram"></a>Per testare DiffGram  
  
1.  Creare le tabelle seguenti nel database **tempdb.**  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Aggiungere i dati di esempio seguenti:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiare il DiffGram sopra riportato e incollarlo in un file di testo. Salvare il file con il nome MyDiffGram.xml nella stessa cartella utilizzata nel passaggio precedente.  
  
4.  Copiare il DiffGramSchema fornito precedentemente in questo argomento e incollarlo in un file di testo. Salvare il file con il nome DiffGramSchema.xml.  
  
5.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il DiffGram.  
  
     Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
## <a name="e-applying-updates-by-using-a-diffgram-with-the-diffgrparentid-annotation"></a>E. Applicazione degli aggiornamenti mediante DiffGram con l'annotazione diffgr:parentID  
 In questo esempio viene illustrato **l'utilizzo** dell'annotazione parentID specificata nel blocco di **\<before>** DiffGram per l'applicazione degli aggiornamenti.  
  
```  
<NewDataSet />  
<diffgr:before>  
   <Order diffgr:id="Order1" msdata:rowOrder="0" OrderID="2" />  
   <Order diffgr:id="Order3" msdata:rowOrder="2" OrderID="4" />  
  
   <OrderDetail diffgr:id="OrderDetail1"   
                diffgr:parentId="Order1"   
                msdata:rowOrder="0"   
                ProductID="13"   
                OrderID="2" />  
   <OrderDetail diffgr:id="OrderDetail3"   
                diffgr:parentId="Order3"  
                ProductID="77"  
                OrderID="4"/>  
</diffgr:before>  
</diffgr:diffgram>  
```  
  
 Questo DiffGram specifica un'operazione di eliminazione perché è presente solo un **\<before>** blocco . In DiffGram l'annotazione **parentID** viene usata per specificare una relazione padre-figlio tra gli ordini e i dettagli dell'ordine. Quando vengono eliminati i record con SQLXML, questi vengono eliminati prima dalla tabella figlio identificata da questa relazione e poi dalla tabella padre corrispondente.  
  
  
