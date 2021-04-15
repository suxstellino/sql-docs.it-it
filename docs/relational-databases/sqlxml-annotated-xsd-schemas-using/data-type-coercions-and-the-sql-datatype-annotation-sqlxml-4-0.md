---
title: Convertire tipi di dati con sql:datatype (SQLXML)
description: Informazioni su come usare gli attributi xsd:type e sql:datatype in SQLXML 4.0 per controllare il mapping tra i tipi di dati XSD e SQL Server tipi di dati.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping data types [SQLXML]
- type attribute
- sql:datatype
- data types [SQLXML], converting
- annotated XSD schemas, mapping data types
- xsd:type
- datatype annotation
- converting data types [SQLXML]
- data types [SQLXML], mapping data types
- XSD schemas [SQLXML], mapping data types
ms.assetid: db192105-e8aa-4392-b812-9d727918c005
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6027201a9af3064d1cf34b91c79885b46d9bbcdb
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492122"
---
# <a name="data-type-conversions-and-the-sqldatatype-annotation-sqlxml-40"></a>Conversioni dei tipi di dati e annotazione sql:datatype (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In uno schema XSD **l'attributo xsd:type** specifica il tipo di dati XSD di un elemento o di un attributo. Quando viene utilizzato uno schema XSD per estrarre dati dal database, il tipo di dati specificato viene utilizzato per formattare i dati.  
  
 Oltre a specificare un tipo XSD in uno schema, è anche possibile specificare un tipo di dati Microsoft usando l'annotazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sql:datatype.** Gli **attributi xsd:type e** **sql:datatype** controllano il mapping tra i tipi di dati XSD e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i tipi di dati.  
  
## <a name="xsdtype-attribute"></a>Attributo xsd:type  
 È possibile usare **l'attributo xsd:type** per specificare il tipo di dati XML di un attributo o di un elemento mappato a una colonna. **Xsd:type influisce** sul documento restituito dal server e anche sulla query XPath eseguita. Quando viene eseguita una query XPath su uno schema di mapping che contiene **xsd:type**, XPath usa il tipo di dati specificato durante l'elaborazione della query. Per altre informazioni sull'uso di **xsd:type** da parte di XPath, vedere Mapping dei tipi di dati XSD ai tipi di dati [XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md).  
  
 In un documento restituito tutti i tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono convertiti in rappresentazioni di stringa. Alcuni tipi di dati richiedono conversioni aggiuntive. Nella tabella seguente sono elencate le conversioni usate per vari **valori xsd:type.**  
  
|Tipo di dati XSD|Conversione SQL Server|  
|-------------------|---------------------------|  
|Boolean|CONVERT(bit, COLUMN)|  
|Data|LEFT(CONVERT(nvarchar(4000), COLUMN, 126), 10)|  
|decimal|CONVERT(money, COLUMN)|  
|id/idref/idrefs|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|nmtoken/nmtokens|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|Tempo|SUBSTRING(CONVERT(nvarchar(4000), COLUMN, 126), 1+CHARINDEX(N'T', CONVERT(nvarchar(4000), COLUMN, 126)), 24)|  
|Tutti gli altri|Nessuna conversione aggiuntiva|  
  
> [!NOTE]  
>  Alcuni dei valori restituiti da potrebbero non essere compatibili con i tipi di dati XML specificati tramite xsd:type , perché la conversione non è possibile (ad esempio, la conversione di "XYZ" in un tipo di dati decimal) o perché il valore supera l'intervallo del tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ad esempio, -100000 convertito in un tipo XSD **UnsignedShort).**  Conversioni di tipi incompatibili possono restituire documenti XML non validi o errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="mapping-from-sql-server-data-types-to-xsd-data-types"></a>Mapping dai tipi di dati di SQL Server a tipi di dati XSD  
 Nella tabella seguente viene illustrato un mapping evidente dai tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ai tipi di dati XSD. Se il tipo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è noto, nella tabella è disponibile il tipo XSD corrispondente che è possibile specificare nello schema XSD.  
  
|Tipo di dati di SQL Server|Tipo di dati XSD|  
|--------------------------|-------------------|  
|**bigint**|**long**|  
|**binary**|**base64Binary**|  
|**bit**|**boolean**|  
|**char**|**string**|  
|**datetime**|**dateTime**|  
|**decimal**|**decimal**|  
|**float**|**double**|  
|**image**|**base64Binary**|  
|**int**|**int**|  
|**money**|**decimal**|  
|**nchar**|**string**|  
|**ntext**|**string**|  
|**nvarchar**|**string**|  
|**numeric**|**decimal**|  
|**real**|**float**|  
|**smalldatetime**|**dateTime**|  
|**smallint**|**short**|  
|**smallmoney**|**decimal**|  
|**sql_variant**|**string**|  
|**sysname**|**string**|  
|**text**|**string**|  
|**timestamp**|**dateTime**|  
|**tinyint**|**unsignedByte**|  
|**varbinary**|**base64Binary**|  
|**varchar**|**string**|  
|**uniqueidentifier**|**string**|  
  
## <a name="sqldatatype-annotation"></a>Annotazione sql:datatype  
 **L'annotazione sql:datatype** viene usata per specificare il tipo di dati. Questa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] annotazione deve essere specificata quando:  
  
-   Si sta caricando in blocco in **una colonna dateTime** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un tipo **dateTime**, **date** o **time** XSD. In questo caso, è necessario identificare il tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] della colonna usando **sql:datatype="dateTime"**. Questa regola si applica anche agli updategram.  
  
-   Si sta caricando in blocco in una colonna di tipo uniqueidentifier e il valore XSD è un GUID che include [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  parentesi graffe ({ e }). Quando si specifica **sql:datatype="uniqueidentifier",** le parentesi graffe vengono rimosse dal valore prima che venga inserito nella colonna. Se **sql:datatype non** viene specificato, il valore viene inviato con le parentesi graffe e l'inserimento o l'aggiornamento non riesce.  
  
-   Il tipo di dati XML **base64Binary** esegue il mapping a vari tipi di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (**binary**, **image** o **varbinary**). Per eseguire il mapping del tipo di dati XML **base64Binary** a un tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specifico, usare l'annotazione **sql:datatype** . Questa annotazione specifica il tipo di dati esplicito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] della colonna a cui viene mappato l'attributo. Ciò risulta utile durante l'archiviazione dei dati nei database. Specificando **l'annotazione sql:datatype,** è possibile identificare il tipo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dati esplicito.  
  
 È in genere consigliabile specificare **sql:datatype** nello schema.  
  
## <a name="examples"></a>Esempio  
 Per creare esempi reali utilizzando gli esempi seguenti, è necessario soddisfare alcuni requisiti. Per altre informazioni, vedere [Requisiti per l'esecuzione di esempi SQLXML.](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
  
### <a name="a-specifying-xsdtype"></a>R. Definizione dell'attributo xsd:type  
 In questo esempio viene illustrato **in** che modo un tipo di data XSD specificato utilizzando l'attributo **xsd:type** nello schema influisce sul documento XML risultante. Lo schema fornisce una vista XML della tabella Sales.SalesOrderHeader nel database AdventureWorks.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader">  
     <xsd:complexType>  
       <xsd:attribute name="SalesOrderID" type="xsd:string" />   
       <xsd:attribute name="CustomerID"   type="xsd:string" />   
       <xsd:attribute name="OrderDate"    type="xsd:date" />   
       <xsd:attribute name="DueDate"  />   
       <xsd:attribute name="ShipDate"  type="xsd:time" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 In questo schema XSD sono inclusi tre attributi che restituiscono un valore di data da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esaminare i casi seguenti per lo schema:  
  
-   Specifica **xsd:type=date** **nell'attributo OrderDate.** Viene visualizzata la parte della data del valore restituito da per l'attributo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **OrderDate.**  
  
-   Specifica **xsd:type=time** **nell'attributo ShipDate.** Viene visualizzata la parte dell'ora del valore restituito da per l'attributo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **ShipDate.**  
  
-   Non specifica **xsd:type** **nell'attributo DueDate.** Viene visualizzato lo stesso valore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito da .  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Per testare una query Xpath di esempio sullo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file con il nome xsdType.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file con il nome xsdTypeT.xml nella stessa directory in cui è stato salvato xsdType.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="xsdType.xml">  
        /Order  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (xsdType.xml) è relativo alla directory in cui è salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\xsdType.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  

     Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML 4.0.](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Di seguito è riportato il set di risultati parziale:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Order SalesOrderID="43659"   
         CustomerID="676"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
  <Order SalesOrderID="43660"   
         CustomerID="117"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
 ...  
</ROOT>  
```  
  
 Di seguito viene indicato lo schema XDR equivalente:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader">  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="CustomerID"  />  
    <AttributeType name="OrderDate" dt:type="date" />  
    <AttributeType name="DueDate" />  
    <AttributeType name="ShipDate" dt:type="time" />  
  
    <attribute type="SalesOrderID" sql:field="OrderID" />  
    <attribute type="CustomerID" sql:field="CustomerID" />  
    <attribute type="OrderDate" sql:field="OrderDate" />  
    <attribute type="DueDate" sql:field="DueDate" />  
    <attribute type="ShipDate" sql:field="ShipDate" />  
</ElementType>  
</Schema>  
```  
  
### <a name="b-specifying-sql-data-type-using-sqldatatype"></a>B. Definizione del tipo di dati SQL tramite sql:datatype  
 Per un esempio funzionante, vedere l'esempio G negli esempi di caricamento bulk [XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md). In questo esempio viene eseguito il caricamento bulk di un valore GUID che include"{" e "}". Lo schema in questo esempio specifica **sql:datatype per** identificare il tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di dati come **uniqueidentifier**. Questo esempio illustra quando **è necessario specificare sql:datatype** nello schema.  
  
  
