---
title: Utilizzo di schemi XSD con annotazioni nelle query (SQLXML)
description: Informazioni su come specificare query XPath su uno schema XSD con annotazioni in SQLXML 4.0 per recuperare dati dal database.
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- queries [SQLXML]
- inline schemas [SQLXML]
- XPath queries [SQLXML], annotated XSD schemas in queries
- queries [SQLXML], annotated XSD schemas
- retrieving data
- mapping schema [SQLXML], queries
- multiple inline schemas
- annotated XSD schemas, queries
- XSD schemas [SQLXML], queries
- templates [SQLXML], annotated XSD schemas in queries
ms.assetid: 927a30a2-eae8-420d-851d-551c5f884f3c
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2a2d4fca9cd7df8d35bfa90d679aa14b3bb9412b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490419"
---
# <a name="using-annotated-xsd-schemas-in-queries-sqlxml-40"></a>Utilizzo di schemi XSD con annotazioni in query (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  È possibile specificare query su uno schema con annotazioni per recuperare dati dal database specificando in un modello query XPath sullo schema XSD.  
  
 **\<sql:xpath-query>** L'elemento consente di specificare una query XPath sulla vista XML definita dallo schema con annotazioni. Lo schema annotato sul quale deve essere eseguita la query XPath viene identificato tramite l'attributo **dello schema di mapping** dell'elemento **\<sql:xpath-query>** .  
  
 I modelli sono documenti XML validi che contengono una o più query. Le query FOR XML e XPath restituiscono un frammento del documento. I modelli fungono da contenitori per i frammenti del documento e offrono in tal modo un metodo per specificare un singolo elemento di livello principale.  
  
 Negli esempi inclusi in questo argomento vengono utilizzati modelli per specificare una query XPath su uno schema con annotazioni per recuperare dati dal database.  
  
 Si consideri, ad esempio, lo schema con annotazioni seguente:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Person.Contact" >  
     <xsd:complexType>  
       <xsd:attribute name="ContactID" type="xsd:string" />   
       <xsd:attribute name="FirstName" type="xsd:string" />   
       <xsd:attribute name="LastName"  type="xsd:string" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 A scopo illustrativo, lo schema XSD viene archiviato in un file denominato Schema2.xml. È quindi possibile specificare una query XPath sullo schema con annotazioni nel file di modello seguente (Schema2T.xml):  
  
```  
<sql:xpath-query   
     xmlns:sql="urn:schemas-microsoft-com:xmlsql"  
     >  
          Person.Contact[@ContactID="1"]  
</sql:xpath-query>  
```  
  
 È infine possibile creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire la query come parte di un file di modello. Per altre informazioni, vedere [Annotated XDR Schemas &#40;Deprecated in SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="using-inline-mapping-schemas"></a>Utilizzo di schemi di mapping inline  
 È possibile includere uno schema con annotazioni direttamente in un modello e quindi specificare nel modello una query XPath sullo schema inline. Il modello può essere anche un updategram.  
  
 Un modello può includere più schemi inline. Per usare uno schema inline incluso in un modello, specificare l'attributo **id** con un valore univoco nell'elemento e quindi usare #idvalue per fare riferimento allo **\<xsd:schema>** schema  inline. Il comportamento dell'attributo **id** è identico a quello di **sql:id** ({urn:schemas-microsoft-com:xml-sql}id) usato negli schemi XDR.  
  
 Nel modello seguente, ad esempio, vengono specificati due schemi con annotazioni inline:  
  
```  
<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<xsd:schema xmlns:xsd='http://www.w3.org/2001/XMLSchema'  
        xmlns:ms='urn:schemas-microsoft-com:mapping-schema'  
        id='InLineSchema1' sql:is-mapping-schema='1'>  
  <xsd:element name='Employees' ms:relation='HumanResources.Employee'>  
    <xsd:complexType>  
      <xsd:attribute name='LoginID'   
                     type='xsd:string'/>  
      <xsd:attribute name='Title'   
                     type='xsd:string'/>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
  
<xsd:schema xmlns:xsd='http://www.w3.org/2001/XMLSchema'  
        xmlns:ms='urn:schemas-microsoft-com:mapping-schema'  
        id='InLineSchema2' sql:is-mapping-schema='1'>  
  <xsd:element name='Contacts' ms:relation='Person.Contact'>  
    <xsd:complexType>  
  
      <xsd:attribute name='ContactID'   
                     type='xsd:string' />  
      <xsd:attribute name='FirstName'   
                     type='xsd:string' />  
      <xsd:attribute name='LastName'   
                     type='xsd:string' />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
  
<sql:xpath-query xmlns:sql='urn:schemas-microsoft-com:xml-sql'   
        mapping-schema='#InLineSchema1'>  
    /Employees[@LoginID='adventure-works\guy1']  
</sql:xpath-query>  
  
<sql:xpath-query xmlns:sql='urn:schemas-microsoft-com:xml-sql'   
        mapping-schema='#InLineSchema2'>  
    /Contacts[@ContactID='1']  
</sql:xpath-query>  
</ROOT>  
```  
  
 Il modello specifica inoltre due query XPath. Ogni elemento identifica **\<xpath-query>** in modo univoco lo schema di mapping specificando l'attributo **dello schema di mapping.**  
  
 Quando si specifica uno schema inline nel modello, è necessario specificare anche l'annotazione **sql:is-mapping-schema** **\<xsd:schema>** nell'elemento . **sql:is-mapping-schema** accetta un valore booleano (0=false, 1=true). Uno schema inline con **sql:is-mapping-schema="1"** viene considerato come schema con annotazioni inline e non viene restituito nel documento XML.  
  
 **L'annotazione sql:is-mapping-schema** appartiene allo spazio dei nomi del modello **urn:schemas-microsoft-com:xml-sql**.  
  
 Per testare questo esempio, salvare il modello (InlineSchemaTemplate.xml) in una directory locale, quindi creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello. Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Oltre a specificare l'attributo **dello schema** di mapping sull'elemento di un modello (quando è presente una query XPath) o sull'elemento in un updategram, è possibile **\<sql:xpath-query>** **\<updg:sync>** eseguire le operazioni seguenti:  
  
-   Specificare **l'attributo dello schema** di mapping **\<ROOT>** nell'elemento (dichiarazione globale) nel modello. Questo schema di mapping diventa quindi lo schema predefinito che verrà usato da tutti i nodi XPath e updategram senza **annotazione esplicita dello schema di mapping.**  
  
-   Specificare **l'attributo dello schema** di mapping usando l'oggetto  COMMAND DI ADO.  
  
 **L'attributo mapping-schema** specificato nell'elemento o ha la precedenza più alta. L'oggetto **\<xpath-query>** COMMAND **\<updg:sync>** ADO ha la precedenza più bassa.   
  
 Si noti che se si specifica una query XPath in un modello e non si specifica uno schema di mapping su cui viene eseguita la query XPath, la query XPath viene considerata come una query di tipo **dbobject.** Considerare ad esempio il modello seguente:  
  
```  
<sql:xpath-query   
     xmlns:sql="urn:schemas-microsoft-com:xmlsql">  
          Production.ProductPhoto[@ProductPhotoID='100']/@LargePhoto  
</sql:xpath-query>  
```  
  
 Il modello specifica una query XPath ma non specifica uno schema di mapping. Pertanto, questa query viene considerata come una query di tipo **dbobject** in cui Production.ProductPhoto è il nome della tabella e ='100' è un predicato che trova una foto di prodotto con valore @ProductPhotoID ID pari a 100. @LargePhoto è la colonna da cui recuperare il valore.  
  
  
