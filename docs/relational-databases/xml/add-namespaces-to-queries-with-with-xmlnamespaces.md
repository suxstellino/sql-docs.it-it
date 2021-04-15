---
title: Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES | Microsoft Docs
description: Informazioni su come aggiungere spazi dei nomi alle query usando la clausola WITH XMLNAMESPACES.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- ELEMENTS XSINIL directive
- adding namespaces
- XSINIL directive
- default namespaces
- queries [XML in SQL Server], WITH XMLNAMESPACES clause
- predefined namespaces [XML in SQL Server]
- FOR XML clause, WITH XMLNAMESPACES clause
- namespaces [XML in SQL Server]
- xml data type [SQL Server], WITH XMLNAMESPACES clause
- WITH XMLNAMESPACES clause
ms.assetid: 2189cb5e-4460-46c5-a254-20c833ebbfec
author: rothja
ms.author: jroth
ms.openlocfilehash: f321511106d37539c4ce26c64fe98f8413d97845
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489039"
---
# <a name="add-namespaces-to-queries-with-with-xmlnamespaces"></a>Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  La clausola[WITH XMLNAMESPACES (Transact-SQL)](../../t-sql/xml/with-xmlnamespaces.md) offre il supporto dell'URI dello spazio dei nomi nel modo seguente:  
  
-   Rende disponibile il mapping tra il prefisso dello spazio dei nomi e l'URI durante le query di [costruzione di codice XML tramite la clausola FOR XML](../../relational-databases/xml/for-xml-sql-server.md) .  
  
-   Rende disponibile il mapping tra lo spazio dei nomi e l'URI nel contesto dello spazio dei nomi statico dei [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md).  
  
## <a name="using-with-xmlnamespaces-in-the-for-xml-queries"></a>Utilizzo di WITH XMLNAMESPACES nelle query FOR XML  
 WITH XMLNAMESPACES consente di includere gli spazi dei nomi XML nelle query FOR XML. Si consideri, ad esempio, la query FOR XML seguente:  
  
```  
SELECT ProductID, Name, Color  
FROM   Production.Product  
WHERE  ProductID=316 or ProductID=317  
FOR XML RAW  
```  
  
 Risultato:  
  
```  
<row ProductID="316" Name="Blade" />  
<row ProductID="317" Name="LL Crankarm" Color="Black" />  
  
```  
  
 Per aggiungere spazi dei nomi al codice XML creato dalla query FOR XML, specificare innanzitutto il mapping tra il prefisso dello spazio dei nomi e l'URI utilizzando la clausola WITH NAMESPACES. Utilizzare quindi i prefissi dello spazio dei nomi per specificare i nomi nella query, come illustrato nella query modificata seguente. Si noti che la clausola WITH XMLNAMESPACES specifica il mapping tra il prefisso dello spazio dei nomi (`ns1`) e l'URI (`uri`). Il prefisso `ns1` viene quindi utilizzato per specificare i nomi degli elementi e degli attributi che devono essere creati per la query FOR XML.  
  
```  
WITH XMLNAMESPACES ('uri' as ns1)  
SELECT ProductID as 'ns1:ProductID',  
       Name      as 'ns1:Name',   
       Color     as 'ns1:Color'  
FROM Production.Product  
WHERE ProductID=316 or ProductID=317  
FOR XML RAW ('ns1:Prod'), ELEMENTS  
  
```  
  
 Il risultato XML include i prefissi dello spazio dei nomi seguenti:  
  
```  
<ns1:Prod xmlns:ns1="uri">  
  <ns1:ProductID>316</ns1:ProductID>  
  <ns1:Name>Blade</ns1:Name>  
</ns1:Prod>  
<ns1:Prod xmlns:ns1="uri">  
  <ns1:ProductID>317</ns1:ProductID>  
  <ns1:Name>LL Crankarm</ns1:Name>  
  <ns1:Color>Black</ns1:Color>  
</ns1:Prod>  
  
```  
  
 Per la clausola WITH XMLNAMESPACES sono valide le osservazioni seguenti:  
  
-   La clausola è supportata solo nelle modalità RAW, AUTO e PATH delle query FOR XML. La modalità EXPLICIT non è supportata.  
  
-   La clausola influisce solo sui prefissi dello spazio dei nomi delle query FOR XML e sui metodi con tipo di dati **xml** , ma non sul parser XML. Ad esempio, la query seguente restituisce un errore perché nel documento XML non è disponibile una dichiarazione dello spazio dei nomi per il prefisso myNS.  
  
-   Se si utilizza una clausola WITH XMLNAMESPACES, non è possibile utilizzare le direttive FOR XML, ovvero XMLSCHEMA e XMLDATA.  
  
    ```  
    CREATE TABLE T (x xml)  
    go  
    WITH XMLNAMESPACES ('https://abc' as myNS )  
    INSERT INTO T VALUES('<myNS:root/>')  
    ```  
  
## <a name="using-the-xsinil-directive"></a>Utilizzo della direttiva XSINIL  
 Se si utilizza la direttiva ELEMENTS XSINIL, non è possibile definire il prefisso xsi nella clausola WITH XMLNAMESPACES. Questo prefisso viene infatti aggiunto automaticamente quando si utilizza ELEMENTS XSINIL. La query seguente usa ELEMENTS XSINIL che genera codice XML incentrato sugli elementi nel quale viene eseguito il mapping dei valori Null a elementi con l'attributo **xsi:nil** impostato su True.  
  
```  
WITH XMLNAMESPACES ('uri' as ns1)  
SELECT ProductID as 'ns1:ProductID',  
       Name      as 'ns1:Name',   
       Color     as 'ns1:Color'  
FROM Production.Product  
WHERE ProductID=316   
FOR XML RAW, ELEMENTS XSINIL  
```  
  
 Risultato:  
  
```  
<row xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="uri">  
  <ns1:ProductID>316</ns1:ProductID>  
  <ns1:Name>Blade</ns1:Name>  
  <ns1:Color xsi:nil="true" />  
</row>  
```  
  
## <a name="specifying-default-namespaces"></a>Impostazione degli spazi dei nomi predefiniti  
 Anziché dichiarare un prefisso dello spazio dei nomi, è possibile dichiarare uno spazio dei nomi predefinito utilizzando la parola chiave DEFAULT. Nella query FOR XML, questa parola chiave associa lo spazio dei nomi predefinito ai nodi XML nel codice XML risultante. Nell'esempio seguente, la clausola WITH XMLNAMESPACES definisce due prefissi di spazio dei nomi che vengono definiti insieme a uno spazio dei nomi predefinito.  
  
```  
WITH XMLNAMESPACES ('uri1' as ns1,   
                    'uri2' as ns2,  
                    DEFAULT 'uri2')  
SELECT ProductID,   
      Name,  
      Color  
FROM Production.Product   
WHERE ProductID=316 or ProductID=317  
FOR XML RAW ('ns1:Product'), ROOT('ns2:root'), ELEMENTS  
```  
  
 La query FOR XML genera codice XML incentrato sugli elementi. Si noti che nella query entrambi i prefissi dello spazio dei nomi vengono utilizzati per la denominazione dei nodi. Nella clausola SELECT, per l'ID prodotto, il nome e il colore non è specificato un nome con un prefisso e pertanto gli elementi corrispondenti nel codice XML risultante appartengono allo spazio dei nomi predefinito.  
  
```  
<ns2:root xmlns="uri2" xmlns:ns2="uri2" xmlns:ns1="uri1">  
  <ns1:Product>  
    <ProductID>316</ProductID>  
    <Name>Blade</Name>  
  </ns1:Product>  
  <ns1:Product>  
    <ProductID>317</ProductID>  
    <Name>LL Crankarm</Name>  
    <Color>Black</Color>  
  </ns1:Product>  
</ns2:root>  
```  
  
 La query seguente è simile alla precedente, eccetto che per il fatto che è specificata la modalità FOR XML AUTO.  
  
```  
WITH XMLNAMESPACES ('uri1' as ns1,  'uri2' as ns2,DEFAULT 'uri2')  
SELECT ProductID,   
      Name,  
      Color  
FROM Production.Product as "ns1:Product"  
WHERE ProductID=316 or ProductID=317  
FOR XML AUTO, ROOT('ns2:root'), ELEMENTS  
```  
  
## <a name="using-predefined-namespaces"></a>Utilizzo degli spazi dei nomi predefiniti  
 Quando si utilizzano gli spazi dei nomi predefiniti, è necessario specificare in modo esplicito l'associazione degli spazi dei nomi utilizzando WITH XMLNAMESPACES. Questa osservazione non è valida per lo spazio dei nomi xml e per lo spazio dei nomi xsi se si utilizza ELEMENTS XSINIL La query seguente definisce in modo esplicito l'associazione tra il prefisso dello spazio dei nomi e l'URI per lo spazio dei nomi predefinito (`urn:schemas-microsoft-com:xml-sql`).  
  
```  
WITH XMLNAMESPACES ('urn:schemas-microsoft-com:xml-sql' as sql)  
SELECT 'SELECT * FROM Customers FOR XML AUTO, ROOT("a")' AS "sql:query"  
FOR XML PATH('sql:root')  
```  
  
 Di seguito è riportato il risultato. Gli utenti di SQLXML hanno familiarità con questo modello XML. Per altre informazioni, vedere [Concetti relativi alla programmazione SQLXML 4.0](../../relational-databases/sqlxml/sqlxml-4-0-programming-concepts.md).  
  
```  
<sql:root xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query>SELECT * FROM Customers FOR XML AUTO, ROOT("a")</sql:query>  
</sql:root>  
```  
  
 Il prefisso dello spazio dei nomi xml è l'unico che può essere utilizzato senza che sia necessario definirlo in modo esplicito in WITH XMLNAMESPACES, come illustrato nella query seguente in modalità PATH. Se si dichiara questo prefisso, è anche necessario associarlo allo spazio dei nomi http://www.w3.org/XML/1998/namespace. I nomi specificati nella clausola SELECT fanno riferimento al prefisso dello spazio dei nomi xml che non viene definito in modo esplicito tramite WITH XMLNAMESPACES.  
  
```  
SELECT 'en'    as "English/@xml:lang",  
       'food'  as "English",  
       'ger'   as "German/@xml:lang",  
       'Essen' as "German"  
FOR XML PATH ('Translation')  
go  
```  
  
 Gli attributi @xml:lang usano lo spazio dei nomi xml predefinito. Poiché nella versione XML 1.0 non è necessario dichiarare in modo esplicito l'associazione dello spazio dei nomi xml, il risultato non includerà una dichiarazione esplicita di tale associazione.  
  
 Risultato:  
  
```  
<Translation>  
  <English xml:lang="en">food</English>  
  <German xml:lang="ger">Essen</German>  
</Translation>  
```  
  
## <a name="using-with-xmlnamespaces-with-the-xml-data-type-methods"></a>Utilizzo di WITH XMLNAMESPACES con i metodi con tipo di dati xml  
 Tutti i [metodi con tipo di dati xml](../../t-sql/xml/xml-data-type-methods.md) specificati in una query SELECT o in UPDATE quando si tratta del metodo **modify()** , devono ripetere la dichiarazione dello spazio dei nomi nel prologo. e possono richiedere pertanto molto tempo. Ad esempio, la query seguente recupera gli ID dei modelli di prodotto per i quali esistono specifiche nelle descrizioni del catalogo, ovvero per i quali esiste l'elemento <`Specifications`>.  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[(pd:Specifications)]'  
    ) = 1  
```  
  
 Nella query precedente, entrambi i metodi **query()** e **exist()** dichiarano lo stesso spazio dei nomi nel rispettivo prologo. Ad esempio:  
  
```  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
```  
  
 In alternativa, è possibile dichiarare innanzitutto WITH XMLNAMESPACES e utilizzare i prefissi dello spazio dei nomi nella query. In questo caso, non è necessario che i metodi **query()** e **exist()** includano le dichiarazioni di spazio dei nomi nel prologo.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[(pd:Specifications)]'  
    ) = 1  
Go  
```  
  
 Si noti che una dichiarazione esplicita nel prologo della query XQuery ignora il prefisso e lo spazio dei nomi predefinito definiti nella clausola WITH.  
  
## <a name="see-also"></a>Vedere anche  
 [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)   
 [WITH XMLNAMESPACES &#40;Transact-SQL&#41;](../../t-sql/xml/with-xmlnamespaces.md)   
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
