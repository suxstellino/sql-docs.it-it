---
title: sql:relationship e la regola di ordinamento delle chiavi (SQLXML)
description: Informazioni sull'uso dell'elemento sql:relationship e delle regole di ordinamento delle chiavi in SQLXML.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:relationship
- key ordering rules [SQLXML]
- relationship annotation
ms.assetid: 914cb152-09f5-4b08-b35d-71940e4e9986
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 856f9c311c86c428713d947475dfb5f1ea57a32d
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490649"
---
# <a name="annotation-interpretation---sqlrelationship-and-key-ordering-rule"></a>Interpretazione delle annotazioni - sql:relationship e regola di ordinamento delle chiavi
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Poiché il caricamento bulk XML genera record quando i nodi entrano nell'ambito e invia tali record a Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quando i nodi abbandonano l'ambito, i dati per il record devono essere presenti nell'ambito del nodo.  
  
 Si consideri lo schema XSD seguente, in cui la relazione uno-a-molti tra gli elementi e (un cliente può eseguire molti ordini) viene specificata **\<Customer>** **\<Order>** usando **\<sql:relationship>** l'elemento :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"<>   
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
</xsd:schema>  
```  
  
 Quando il **\<Customer>** nodo dell'elemento entra nell'ambito, il caricamento bulk XML genera un record cliente. Questo record rimane fino a quando il caricamento bulk XML non legge **\</Customer>** . Durante l'elaborazione del nodo dell'elemento, il caricamento bulk XML usa per ottenere il valore della colonna chiave esterna CustomerID della tabella CustOrder dall'elemento padre, perché l'elemento non specifica **\<Order>** **\<sql:relationship>** **\<Customer>** **\<Order>** l'attributo **CustomerID.** Ciò significa che nella definizione dell'elemento è necessario **\<Customer>** specificare **l'attributo CustomerID** nello schema prima di specificare **\<sql:relationship>** . In caso contrario, quando un elemento entra nell'ambito, il caricamento bulk XML genera un record per la tabella CustOrder e quando il caricamento bulk XML raggiunge il tag di fine, invia il record a senza il valore della colonna chiave esterna **\<Order>** **\</Order>** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] CustomerID.  
  
 Salvare lo schema fornito in questo esempio come SampleSchema.xml.  
  
### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare le tabelle seguenti:  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int          PRIMARY KEY,  
               CompanyName    varchar(20)  NOT NULL,  
                  City           varchar(20)  DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID        varchar(10) PRIMARY KEY,  
               CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID))  
    GO  
    ```  
  
2.  Salvare i dati di esempio seguenti come SampleXMLData.xml:  
  
    ```  
    <ROOT>    
      <Customers>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
        <CustomerID>1111</CustomerID>  
      </Customers>  
      <Customers>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <City>LA</City>    
        <Order OrderID="3" />  
        <CustomerID>1112</CustomerID>  
      </Customers>  
      <Customers>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
        <CustomerID>1113</CustomerID>  
      </Customers>  
    </ROOT>  
    ```  
  
3.  Per eseguire il caricamento bulk XML, salvare ed eseguire l'esempio di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic, Scripting Edition (VBScript) seguente come file MySample.vbs:  

    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
     Come risultato, il caricamento bulk XML inserisce un valore NULL nella colonna chiave esterna CustomerID della tabella CustOrder. Se si rivedono i dati di esempio XML in modo che l'elemento figlio venga visualizzato prima dell'elemento figlio, si ottiene il risultato previsto: Il caricamento bulk XML inserisce il valore di chiave esterna specificato **\<CustomerID>** **\<Order>** nella colonna.  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
   <ElementType name="CustomerID"  />  
   <ElementType name="CompanyName" />  
   <ElementType name="City"        />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust" >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
                 <sql:relationship  
                        key-relation    ="Cust"  
                        key             ="CustomerID"  
                        foreign-key     ="CustomerID"  
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
  
  
