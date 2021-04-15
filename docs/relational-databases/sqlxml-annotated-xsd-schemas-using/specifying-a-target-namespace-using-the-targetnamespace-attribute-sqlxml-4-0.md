---
title: Specificare uno spazio dei nomi di destinazione con targetNamespace (SQLXML)
description: Informazioni su come specificare uno spazio dei nomi di destinazione in uno schema XSD usando l'attributo targetNamespace in SQLXML 4.0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- namespaces [SQLXML], annotated XSD schemas
- targetNamespace attribute
- XSD schemas [SQLXML], target namespaces
- annotated XSD schemas, target namespaces
- xsd:targetNamespace
- attributeFormDefault attribute
- elementFormDefault attribute
- target namespaces [SQLXML]
ms.assetid: f3df9877-6672-4444-8245-2670063c9310
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 011b8e15caae5ea805aedb97223fe62c7640de3c
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490769"
---
# <a name="specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-40"></a>Specifica di uno spazio dei nomi di destinazione mediante l'attributo targetNamespace (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Quando si scrivono schemi XSD, è possibile usare l'attributo **targetNamespace** XSD per specificare uno spazio dei nomi di destinazione. Questo argomento descrive il funzionamento degli attributi XSD **targetNamespace**, **elementFormDefault** e **attributeFormDefault,** il modo in cui influiscono sull'istanza XML generata e il modo in cui le query XPath vengono specificate con gli spazi dei nomi.  
  
 È possibile usare **l'attributo xsd:targetNamespace** per inserire elementi e attributi dello spazio dei nomi predefinito in uno spazio dei nomi diverso. È inoltre possibile specificare se gli elementi e gli attributi dello schema dichiarati localmente devono essere qualificati da uno spazio dei nomi, sia in modo esplicito mediante un prefisso sia in modo implicito per impostazione predefinita. È possibile usare gli attributi **elementFormDefault** e **attributeFormDefault** nell'elemento per specificare a livello globale la qualifica di elementi e attributi locali oppure è possibile usare l'attributo form per specificare i singoli elementi e attributi **\<xsd:schema>** separatamente.   
  
## <a name="examples"></a>Esempio  
 Per creare esempi reali utilizzando gli esempi seguenti, è necessario soddisfare alcuni requisiti. Per altre informazioni, vedere [Requisiti per l'esecuzione di esempi SQLXML.](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
  
### <a name="a-specifying-a-target-namespace"></a>R. Specificare uno spazio dei nomi di destinazione  
 Lo schema XSD seguente specifica uno spazio dei nomi di destinazione usando **l'attributo xsd:targetNamespace.** Lo schema imposta anche i valori degli attributi **elementFormDefault** e **attributeFormDefault** su **"unqualified"** (valore predefinito per questi attributi). Si tratta di una dichiarazione globale e influisce su tutti gli elementi locali ( nello schema) e gli attributi **\<Order>** (**CustomerID**, **ContactName** e **OrderID** nello schema).  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:CO="urn:MyNamespace"   
            targetNamespace="urn:MyNamespace" >  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
          parent="Sales.Customer"  
          parent-key="CustomerID"  
          child="Sales.SalesOrderHeader"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer"   
               sql:relation="Sales.Customer"   
               type="CO:CustomerType" />  
  
  <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader"  
                     sql:relationship="CustOrders"  
                     type="CO:OrderType" />  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
        <xsd:attribute name="SalesPersonID"  type="xsd:string" />  
  </xsd:complexType>  
  <xsd:complexType name="OrderType" >  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name="CustomerID" type="xsd:string" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 Nello schema:  
  
-   Le **dichiarazioni di tipo CustomerType** e **OrderType** sono globali e, pertanto, sono incluse nello spazio dei nomi di destinazione dello schema. Di conseguenza, quando si fa riferimento a questi tipi nella dichiarazione dell'elemento e del relativo elemento figlio, viene specificato un prefisso associato allo spazio dei **\<Customer>** **\<Order>** nomi di destinazione.  
  
-   **\<Customer>** L'elemento è incluso anche nello spazio dei nomi di destinazione dello schema perché è un elemento globale nello schema.  
  
 Eseguire sullo schema la query Xpath seguente:  
  
```  
(/CO:Customer[@CustomerID=1)   
```  
  
 La query XPath genera questo documento dell'istanza (sono mostrati solo alcuni ordini):  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <y0:Customer xmlns:y0="urn:MyNamespace"   
      CustomerID="ALFKI" ContactName="Maria Anders">  
        <Order CustomerID="ALFKI" OrderID="10643" />   
        <Order CustomerID="ALFKI" OrderID="10692" />   
        ...  
  </y0:Customer>  
  </ROOT>  
```  
  
 Questo documento di istanza definisce lo spazio dei nomi urn:MyNamespace e associa un prefisso (y0). Il prefisso viene applicato solo **\<Customer>** all'elemento globale. L'elemento è globale perché è dichiarato come figlio **\<xsd:schema>** dell'elemento nello schema.  
  
 Il prefisso non viene applicato agli elementi e agli attributi locali perché il valore degli attributi **elementFormDefault** e **attributeFormDefault** è impostato su **"unqualified"** nello schema. Si noti che **\<Order>** l'elemento è locale perché la relativa dichiarazione viene visualizzata come figlio **\<complexType>** dell'elemento che definisce **\<CustomerType>** l'elemento. Analogamente, gli attributi (**CustomerID**, **OrderID** e **ContactName**) sono locali, non globali.  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Per creare un esempio reale di questo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file con il nome targetNameSpace.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file come targetNameSpaceT.xml nella stessa directory nella quale è stato salvato targetNamespace.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="targetNamespace.xml"  
                       xmlns:CO="urn:MyNamespace" >  
        /CO:Customer[@CustomerID=1]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     La query XPath nel modello restituisce **\<Customer>** l'elemento per il cliente con CustomerID 1. Notare che la query XPath specifica il prefisso dello spazio dei nomi per l'elemento nella query e non per l'attributo. Gli attributi locali non sono qualificati, come specificato nello schema.  
  
     Il percorso di directory specificato per lo schema di mapping (targetNamespace.xml) è relativo alla directory in cui è salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\targetNamepsace.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML.](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Se lo schema specifica gli attributi **elementFormDefault** e **attributeFormDefault** con valore **"qualified",** il documento di istanza avrà tutti gli elementi e gli attributi locali qualificati. È possibile modificare lo schema precedente per includere questi attributi **\<xsd:schema>** nell'elemento ed eseguire nuovamente il modello. Poiché ora anche gli attributi sono qualificati nell'istanza, la query XPath verrà modificata per includere il prefisso dello spazio dei nomi.  
  
 La query XPath modificata è:  
  
```  
/CO:Customer[@CO:CustomerID=1]  
```  
  
 Di seguito è riportato il documento XML restituito:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <y0:Customer xmlns:y0="urn:MyNamespace" CustomerID="1" SalesPersonID="280">  
      <Order SalesOrderID="43860" CustomerID="1" />   
      <Order SalesOrderID="44501" CustomerID="1" />   
      <Order SalesOrderID="45283" CustomerID="1" />   
      <Order SalesOrderID="46042" CustomerID="1" />   
   </y0:Customer>  
</ROOT>  
```  
  
  
