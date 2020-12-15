---
title: 'Escludi elementi dello schema dal documento XML con SQL: mapping'
description: "Viene illustrato come utilizzare l'annotazione sql: mappata per creare un elemento nello schema XSD che non esegue il mapping a una tabella o a una colonna di database."
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- element does not map [SQLXML]
- annotated XSD schemas, excluding schema elements
- mapped annotation
- table mapping [SQLXML], excluding schema elements
- sql:mapped
- excluding schema elements
- element mapping [SQLXML], excluding schema elements
- column mapping [SQLXML]
- XSD schemas [SQLXML], excluding schema elements
- attribute mapping [SQLXML], excluding schema elements
- table/view mapping [SQLXML], excluding schema elements
ms.assetid: 7d2649dd-0038-4a2c-b16d-f80f7c306966
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 19c9148b0e352a1b8a07bae23525df606f34f1c8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461792"
---
# <a name="excluding-schema-elements-from-the-xml-document-using-sqlmapped"></a>Esclusione di elementi dello schema dal documento XML con sql:mapped
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  A causa del mapping predefinito, viene eseguito il mapping di ogni elemento e attributo nello schema XSD a una vista/tabella e a una colonna di database. Se si desidera creare un elemento nello schema XSD che non viene mappato ad alcuna tabella o colonna di database e che non è presente nel codice XML, è possibile specificare l'annotazione **SQL: mapping** .  
  
 L'annotazione **SQL: mapping** è particolarmente utile se lo schema non può essere modificato o se lo schema viene utilizzato per convalidare XML da altre origini e contiene ancora dati non archiviati nel database. L'annotazione **SQL: mapping** differisce da **SQL: is-constant** in quanto gli elementi e gli attributi non mappati non vengono visualizzati nel documento XML.  
  
 L'annotazione **SQL: mapping** accetta un valore booleano (0 = false, 1 = true). I valori possibili sono 0, 1, true e false.  
  
## <a name="examples"></a>Esempio  
 Per creare esempi reali utilizzando gli esempi seguenti, è necessario soddisfare alcuni requisiti. Per ulteriori informazioni, vedere [requisiti per l'esecuzione di esempi SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-the-sqlmapped-annotation"></a>R. Specifica dell'annotazione sql:mapped  
 Si supponga di disporre di uno schema XSD di un'altra origine. Questo schema XSD è costituito da un **\<Person.Contact>** elemento con gli attributi **ContactID**, **FirstName**, **LastName** e **HomeAddress** .  
  
 Per eseguire il mapping di questo schema XSD alla tabella Person. Contact nel database AdventureWorks, **SQL: mapping** viene specificato nell'attributo **HomeAddress** perché la tabella Employees non archivia gli indirizzi Home dei dipendenti. Di conseguenza, questo attributo non viene mappato al database e non viene restituito nel documento XML risultante quando viene specificata una query XPath sullo schema di mapping.  
  
 Per il resto dello schema viene eseguito il mapping predefinito. L' **\<Person.Contact>** elemento viene mappato alla tabella Person. Contact e tutti gli attributi vengono mappati alle colonne con lo stesso nome nella tabella Person. Contact.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Person.Contact">  
    <xsd:complexType>  
      <xsd:attribute name="ContactID"   type="xsd:string"/>  
      <xsd:attribute name="FirstName"    type="xsd:string" />  
      <xsd:attribute name="LastName"     type="xsd:string" />  
      <xsd:attribute name="HomeAddress" type="xsd:string"   
                     sql:mapped="false" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Per testare una query Xpath di esempio sullo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file con il nome sql-mapped.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file con il nome sql-mappedT.xml nella stessa directory in cui è stato salvato il file sql-mapped.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="sql-mapped.xml">  
            /Person.Contact[@ContactID < 10]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (MySchema.xml) è relativo alla directory in cui è salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\sql-mapped.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  

     Per ulteriori informazioni, vedere [utilizzo di ADO per eseguire query SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Set di risultati:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact ContactID="1" FirstName="Gustavo" LastName="Achong" />   
  <Person.Contact ContactID="2" FirstName="Catherine" LastName="Abel" />   
  <Person.Contact ContactID="3" FirstName="Kim" LastName="Abercrombie" />   
  <Person.Contact ContactID="4" FirstName="Humberto" LastName="Acevedo" />   
  <Person.Contact ContactID="5" FirstName="Pilar" LastName="Ackerman" />   
  <Person.Contact ContactID="6" FirstName="Frances" LastName="Adams" />   
  <Person.Contact ContactID="7" FirstName="Margaret" LastName="Smith" />   
  <Person.Contact ContactID="8" FirstName="Carla" LastName="Adams" />   
  <Person.Contact ContactID="9" FirstName="Jay" LastName="Adams" />   
</ROOT>  
```  
  
 Si noti che ContactID, FirstName e LastName sono presenti, ma HomeAddress non è perché lo schema di mapping ha specificato 0 per l'attributo **SQL: mapping** .  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping predefinito di elementi e attributi XSD a tabelle e colonne &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)  
  
  
