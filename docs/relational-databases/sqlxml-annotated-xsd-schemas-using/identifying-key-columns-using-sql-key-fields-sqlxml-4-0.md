---
title: Identificare le colonne chiave usando sql:key-fields (SQLXML)
description: Informazioni su come garantire un annidamento corretto nei risultati di una query SQLXML 4.0 specificando l'annotazione sql:key-fields in una query XPath per identificare le colonne chiave.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nesting XML results
- proper nesting in results [SQLXML]
- sql:key-fields
- XSD schemas [SQLXML], key columns
- identifying key columns [SQLXML]
- annotated XSD schemas, key columns
- key columns [SQLXML]
- relationships [SQLXML], key columns
- hierarchical relationships [SQLXML]
- key-fields annotation
ms.assetid: 1a5ad868-8602-45c4-913d-6fbb837eebb0
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d5267f0ad5fdf26b256ec78028e1bbe6f9b07328
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492028"
---
# <a name="identifying-key-columns-using-sqlkey-fields-sqlxml-40"></a>Identificazione delle colonne chiave mediante sql:key-fields (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Quando si specifica una query XPath su uno schema XSD, nella maggior parte dei casi sono necessarie informazioni chiave per ottenere la nidificazione appropriata nel risultato. Specificare **l'annotazione sql:key-fields** è un modo per garantire che la gerarchia appropriata sia generata.  
  
> [!NOTE]  
>  Per garantire un annidamento corretto, è consigliabile specificare **sql:key-fields** per gli elementi mappati alle tabelle. Il codice XML prodotto riconosce l'ordinamento del set di risultati sottostante. Se **sql:key-fields** non viene specificato, il formato del codice XML generato potrebbe non essere corretto.  
  
 Il valore **di sql:key-fields** identifica le colonne che identificano in modo univoco le righe nella relazione. Se sono necessarie più colonne per identificare in modo univoco una riga, i valori delle colonne vengono delimitati da spazi.  
  
 È necessario usare l'annotazione **sql:key-fields** quando un elemento contiene un elemento definito tra l'elemento e un elemento figlio, ma non fornisce la chiave primaria della tabella specificata **\<sql:relationship>** nell'elemento padre.  
  
## <a name="examples"></a>Esempio  
 Per creare esempi reali utilizzando gli esempi seguenti, è necessario soddisfare alcuni requisiti. Per altre informazioni, vedere [Requisiti per l'esecuzione di esempi SQLXML.](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
  
### <a name="a-producing-the-appropriate-nesting-when-sqlrelationship-does-not-provide-sufficient-information"></a>R. Generazione dell'annidamento appropriato quando \<sql:relationship> non fornisce informazioni sufficienti  
 Questo esempio mostra dove **è necessario specificare sql:key-fields.**  
  
 Si consideri lo schema seguente: Lo schema specifica una gerarchia tra gli **\<Order>** elementi e in cui l'elemento è **\<Customer>** l'elemento padre e **\<Order>** **\<Customer>** l'elemento è un elemento figlio.  
  
 Il **\<sql:relationship>** tag viene usato per specificare la relazione padre-figlio. che identifica CustomerID nella tabella Sales.SalesOrderHeader come chiave padre che fa riferimento alla chiave figlio CustomerID nella tabella Sales.Customer. Le informazioni fornite in **\<sql:relationship>** non sono sufficienti per identificare in modo univoco le righe nella tabella padre (Sales.SalesOrderHeader). Pertanto, senza **l'annotazione sql:key-fields,** la gerarchia generata non è accurata.  
  
 Con **sql:key-fields** specificato in , l'annotazione identifica in modo univoco le righe nell'elemento padre **\<Order>** (tabella Sales.SalesOrderHeader) e i relativi elementi figlio vengono visualizzati sotto il relativo elemento padre.  
  
 Lo schema è il seguente:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrdCust"  
        parent="Sales.SalesOrderHeader"  
        parent-key="CustomerID"  
        child="Sales.Customer"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"   
               sql:key-fields="SalesOrderID">  
   <xsd:complexType>  
     <xsd:sequence>  
     <xsd:element name="Customer" sql:relation="Sales.Customer"   
                       sql:relationship="OrdCust"  >  
       <xsd:complexType>  
         <xsd:attribute name="CustID" sql:field="CustomerID" />  
         <xsd:attribute name="SoldBy" sql:field="SalesPersonID" />  
       </xsd:complexType>  
     </xsd:element>  
     </xsd:sequence>  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name= "CustomerID" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Per creare un esempio reale di questo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file come KeyFields1.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file come KeyFields1T.xml nella stessa directory nella quale è stato salvato KeyFields1.xml. La query XPath nel modello restituisce tutti gli **\<Order>** elementi con CustomerID minore di 3.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="KeyFields1.xml">  
            /Order[@CustomerID < 3]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (KeyFields1.xml) è relativo alla directory nella quale viene salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields1.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  

     Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML.](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Di seguito è riportato il set di risultati parziale:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
    <Order SalesOrderID="43860" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="44501" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="45283" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    .....  
</ROOT>  
```  
  
### <a name="b-specifying-sqlkey-fields-to-produce-proper-nesting-in-the-result"></a>B. Specifica di sql:key-fields per produrre una nidificazione appropriata nel risultato  
 Nello schema seguente non è specificata alcuna gerarchia tramite **\<sql:relationship>** . Lo schema richiede comunque di specificare **l'annotazione sql:key-fields** per identificare in modo univoco i dipendenti nella tabella HumanResources.Employee.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="HumanResources.Employee" sql:key-fields="EmployeeID" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Title">  
          <xsd:complexType>  
            <xsd:simpleContent>  
              <xsd:extension base="xsd:string">  
                 <xsd:attribute name="EmployeeID" type="xsd:integer" />  
              </xsd:extension>  
            </xsd:simpleContent>  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
   </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Per creare un esempio reale di questo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file come KeyFields2.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file come KeyField21T.xml nella stessa directory nella quale è stato salvato KeyFields2.xml. La query XPath nel modello restituisce tutti gli **\<HumanResources.Employee>** elementi:  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="KeyFields2.xml">  
        /HumanResources.Employee  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (KeyFields2.xml) è relativo alla directory nella quale viene salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields2.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per altre informazioni, vedere [Uso di ADO per eseguire query SQLXML.](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Risultato:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <HumanResources.Employee>  
    <Title EmployeeID="1">Production Technician - WC60</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="2">Marketing Assistant</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="3">Engineering Manager</Title>   
  </HumanResources.Employee>  
  ...  
</ROOT>  
```  
  
  
