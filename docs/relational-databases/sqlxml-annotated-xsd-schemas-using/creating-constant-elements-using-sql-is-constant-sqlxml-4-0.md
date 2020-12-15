---
title: 'Rendere gli elementi costanti con SQL: is-constant (SQLXML)'
description: "Informazioni su come utilizzare l'annotazione sql: is-constant in SQLXML 4,0 per creare elementi costanti in uno schema XSD che non eseguono il mapping ad alcuna tabella o colonna di database."
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- element does not map [SQLXML]
- sql:is-constant
- XSD schemas [SQLXML], constant elements
- element mapping [SQLXML], constant elements
- is-constant annotation
- constant elements [SQLXML]
- annotated XSD schemas, constant elements
ms.assetid: 940eea1b-54f5-445f-b844-c894d9f3941b
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 952c7e905e40e869d459f1ada3225d5524b3d799
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461822"
---
# <a name="creating-constant-elements-using-sqlis-constant-sqlxml-40"></a>Creazione di elementi costanti tramite sql:is-constant (SQLXML 4.0)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Per specificare un elemento costante, ovvero un elemento nello schema XSD che non esegue il mapping ad alcuna tabella o colonna di database, è possibile utilizzare l'annotazione **SQL: is-constant** . Questa annotazione accetta un valore booleano (0=false, 1=true). I valori possibili sono 0, 1, true e false. È possibile specificare l'annotazione **SQL: is-constant** su un elemento che non ha attributi. Se viene specificata in un elemento con valore true (o 1), l'elemento non viene mappato al database ma viene comunque visualizzato nel documento XML.  
  
 È possibile utilizzare l'annotazione **SQL: is-constant** per:  
  
-   Aggiunta di un elemento di livello principale al documento XML. XML richiede un singolo elemento di livello principale (elemento radice) per il documento.  
  
-   Creazione di elementi contenitore, ad esempio un **\<Orders>** elemento che esegue il wrapping di tutti gli ordini.  
  
 È possibile aggiungere l'annotazione **SQL: is-constant** a un **\<complexType>** elemento.  
  
## <a name="examples"></a>Esempio  
 Per creare esempi reali utilizzando gli esempi seguenti, è necessario soddisfare alcuni requisiti. Per ulteriori informazioni, vedere [requisiti per l'esecuzione di esempi SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-sqlis-constant-to-add-a-container-element"></a>R. Definizione di sql:is-constant per aggiungere un elemento contenitore  
 In questo schema XSD con annotazioni, **\<CustomerOrders>** viene definito come elemento costante specificando l'attributo **SQL: is-constant** con un valore pari a 1. Pertanto, **\<CustomerOrders>** non è mappato ad alcuna tabella o colonna di database. Questo elemento costante è costituito dagli **\<Order>** elementi figlio.  
  
 Sebbene non sia **\<CustomerOrders>** mappato ad alcuna tabella o colonna di database, viene comunque visualizzato nel codice XML risultante come elemento contenitore contenente gli **\<Order>** elementi figlio.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
        parent="Sales.Customer"  
        parent-key="CustomerID"  
        child="Sales.SalesOrderHeader"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="CustomerOrders" sql:is-constant="1" >  
          <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"  
                           sql:relationship="CustOrders"   
                           maxOccurs="unbounded" >  
                <xsd:complexType>  
                   <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
                   <xsd:attribute name="OrderDate" type="xsd:date" />  
                   <xsd:attribute name="CustomerID" type="xsd:string" />  
                </xsd:complexType>  
              </xsd:element>  
            </xsd:sequence>  
           </xsd:complexType>  
          </xsd:element>  
         </xsd:sequence>  
          <xsd:attribute name="CustomerID" type="xsd:string" />  
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Per testare una query Xpath di esempio sullo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file con il nome isConstant.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file con il nome isConstantT.xml nella stessa directory in cui è stato salvato il file isConstant.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="isConstant.xml">  
            Customer[@CustomerID=1]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (isConstant.xml) è relativo alla directory in cui è salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\isConstant.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  

     Per ulteriori informazioni, vedere [utilizzo di ADO per eseguire query SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Di seguito è riportato il set di risultati parziale:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
<Customer CustomerID="1">   
  <CustomerOrders>   
    <Order SalesOrderID="43860" OrderDate="2001-08-01" CustomerID="1" />   
    <Order SalesOrderID="44501" OrderDate="2001-11-01" CustomerID="1" />   
    <Order SalesOrderID="45283" OrderDate="2002-02-01" CustomerID="1" />   
    <Order SalesOrderID="46042" OrderDate="2002-05-01" CustomerID="1" />   
    ...  
  </CustomerOrders>   
</Customer>   
</ROOT>  
```  
  
  
