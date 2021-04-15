---
title: Ottenere riferimenti URL ai dati BLOB con sql:encode (SQLXML)
description: Informazioni su come richiedere un riferimento URL ai dati BLOB specificando l'annotazione sql:encode in SQLXML 4.0.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:encode
- encode annotation
- URL references to BLOB data [SQLXML]
- references to BLOB data [SQLXML]
- annotated XSD schemas, URL references to BLOB data
- requesting URL references to BLOB data
- BLOBs, URL references
- Base 64-encoded format
ms.assetid: 2f8cd93b-c636-462b-8291-167197233ee0
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 81f5d833a18d3dbb5709715c47c506900f491a4a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492039"
---
# <a name="requesting-url-references-to-blob-data-using-sqlencode-sqlxml-40"></a>Richiesta di riferimenti URL a dati BLOB utilizzando sql:encode (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In uno schema XSD con annotazioni quando viene eseguito il mapping di un attributo o elemento a una colonna BLOB in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i dati vengono restituiti in formato con codifica Base 64 in XML.  
  
 Se si vuole che sia restituito un riferimento ai dati (un URI) che può essere usato in un secondo momento per recuperare i dati BLOB in formato binario, specificare l'annotazione **sql:encode.** È possibile specificare **sql:encode** per un attributo o un elemento di tipo semplice.  
  
 Specificare **l'annotazione sql:encode** per indicare che deve essere restituito un URL al campo anziché il valore del campo. **sql:encode** dipende dalla chiave primaria per generare una selezione singleton nell'URL. La chiave primaria può essere specificata usando **l'annotazione sql:key-fields.**  
  
 **All'annotazione sql:encode** è possibile assegnare il valore "url" o "default". Il valore "default" restituisce dati in formato con codifica Base 64.  
  
 L'annotazione **sql:encode** non può essere usata con **sql:use-cdata** o nei tipi di attributo ID, IDREF, IDREFS, NMTOKEN o NMTOKENS. Non può inoltre essere usato con l'attributo **fisso** XSD.  
  
> [!NOTE]  
>  Non è possibile utilizzare le colonne di tipo BLOB come parte di una chiave o di una chiave esterna.  
  
## <a name="examples"></a>Esempio  
 Per creare esempi reali utilizzando gli esempi seguenti, è necessario soddisfare alcuni requisiti. Per altre informazioni, vedere [Requisiti per l'esecuzione di esempi SQLXML.](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
  
### <a name="a-specifying-sqlencode-to-obtain-a-url-reference-to-blob-data"></a>R. Specifica di sql:encode per ottenere un riferimento URL ai dati BLOB  
 In questo esempio, lo schema di mapping specifica **sql:encode** sull'attributo **LargePhoto** per recuperare il riferimento URI a una foto di prodotto specifica anziché recuperare i dati binari in formato con codifica Base 64.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
  <xsd:element name="ProductPhoto" sql:relation="Production.ProductPhoto"   
               sql:key-fields="ProductPhotoID" >  
   <xsd:complexType>  
      <xsd:attribute name="ProductPhotoID"  type="xsd:int"  />  
     <xsd:attribute name="LargePhoto" type="xsd:string" sql:encode="url" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Per testare una query Xpath di esempio sullo schema  
  
1.  Copiare il codice dello schema precedente e incollarlo in un file di testo. Salvare il file come sqlEncode.xml.  
  
2.  Copiare il modello seguente e incollarlo in un file di testo. Salvare il file come sqlEncodeT.xml nella stessa directory nella quale è stato salvato sqlEncode.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="sqlEncode.xml">  
            /ProductPhoto[@ProductPhotoID=100]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping (sqlEncode.xml) è relativo alla directory nella quale viene salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\sqlEncode.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per altre informazioni, vedere [Uso di ADO per eseguire query SQLXML 4.0.](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Risultato:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <ProductPhoto ProductPhotoID="100"  
                 LargePhoto="dbobject/Production.ProductPhoto[@ProductPhotoID="100"]/@LargePhoto" />   
</ROOT>  
```  
  
  
