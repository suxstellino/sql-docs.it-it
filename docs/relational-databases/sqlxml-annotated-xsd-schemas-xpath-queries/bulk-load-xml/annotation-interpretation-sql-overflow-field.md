---
title: sql:overflow-field (SQLXML)
description: Informazioni su come usare l'annotazione sql:overflow-field per identificare una colonna come colonna di overflow che riceverà tutti i dati non utilizzati dal documento XML.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- overflow-field annotation
- unconsumed data
- overflow data [SQLXML]
- sql:overflow-field
ms.assetid: f005182b-6151-432d-ab22-3bc025742cd3
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a79f03a282083477d0b648138c0a5ecb2c19a80
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490669"
---
# <a name="annotation-interpretation---sqloverflow-field"></a>Interpretazione delle annotazioni - sql:overflow-field
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In uno schema è possibile identificare una colonna come colonna di overflow per ricevere tutti i dati non utilizzati dal documento XML. Questa colonna viene specificata nello schema tramite l'annotazione **sql:overflow-field.** È possibile avere più colonne di overflow.  
  
 Ogni volta che un nodo XML (elemento o attributo) per cui è presente un'annotazione **sql:overflow-field** definita entra nell'ambito, la colonna di overflow viene attivata e riceve dati non consumati. Quando il nodo non è più in ambito, la colonna di overflow non è più attiva e il caricamento bulk XML rende attivo il campo di overflow precedente, se presente.  
  
 Quando archivia i dati nella colonna di overflow, il caricamento bulk XML archivia anche i tag di apertura e chiusura dell'elemento padre per cui è definito **sql:overflow-field.**  
  
 Ad esempio, lo schema seguente descrive gli **\<Customers>** elementi **\<CustOrder>** e . Ognuno di questi elementi identifica una colonna di overflow:  
  
```  
<?xml version="1.0" ?>  
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
 <xsd:element name="ROOT" sql:is-constant="1">  
  <xsd:complexType>  
    <xsd:sequence>   
      <xsd:element name="Customers"   
                   sql:relation="Cust"  
                   sql:overflow-field="OverflowColumn">  
        <xsd:complexType>  
             <xsd:sequence>   
             <xsd:element name="CustomerID" type="xsd:integer"/>  
             <xsd:element name="CompanyName"  type="xsd:string"/>  
             <xsd:element name="City" type="xsd:string"/>  
             <xsd:element name="Order"  
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder"  
                          sql:overflow-field="OverflowColumn">  
               <xsd:complexType>  
                 <xsd:attribute name="OrderID"/>  
                 <xsd:attribute name="CustomerID"/>  
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
  
 Nello schema **\<Customer>** l'elemento esegue il mapping alla tabella Cust e **\<Order>** l'elemento esegue il mapping alla tabella CustOrder.  
  
 Entrambi gli **\<Customer>** elementi **\<Order>** e identificano una colonna di overflow. Di conseguenza, il caricamento bulk XML salva tutti gli elementi figlio e gli attributi non utilizzati dell'elemento nella colonna di overflow della tabella Cust e tutti gli elementi figlio e gli attributi non utilizzati dell'elemento nella colonna di overflow della **\<Customer>** **\<Order>** tabella CustOrder.  
  
### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Salvare lo schema fornito in questo esempio come SampleSchema.xml.  
  
2.  Creare le tabelle seguenti:  
  
    ```  
    CREATE TABLE Cust (  
              CustomerID     int         PRIMARY KEY,  
              CompanyName    varchar(20) NOT NULL,  
              City           varchar(20) DEFAULT 'Seattle',  
              OverflowColumn nvarchar(200))  
    GO  
    CREATE TABLE CustOrder (  
              OrderID        int         PRIMARY KEY,  
              CustomerID     int         FOREIGN KEY REFERENCES  
                                              Cust(CustomerID),  
              OverflowColumn nvarchar(200))  
    GO  
    ```  
  
3.  Salvare i dati XML di esempio seguenti come file SampleXMLData.xml:  
  
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
       <City><![CDATA[LA]]> </City>  
       <xyz><address>111 Maple, Seattle</address></xyz>     
       <Order OrderID="3" />  
     </Customers>  
       <Customers>  
       <CustomerID>1113</CustomerID>  
       <CompanyName>Victuailles en stock</CompanyName>  
       <Order OrderID="4" />  
      </Customers>  
    </ROOT>  
    ```  
  
4.  Per eseguire il caricamento bulk XML, salvare ed eseguire questo esempio di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic, Scripting Edition (VBScript) come file Sample.vbs:  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  
