---
title: 'Esempi: Utilizzo della modalità PATH | Microsoft Docs'
description: Visualizzazione di esempi di utilizzo della modalità PATH per generare codice XML da una query SELECT.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- PATH FOR XML mode, examples
ms.assetid: 3564e13b-9b97-49ef-8cf9-6a78677b09a3
author: rothja
ms.author: jroth
ms.openlocfilehash: c0b79a861d10c2c0997d7455eb8e7f212706a495
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490818"
---
# <a name="examples-using-path-mode"></a>Esempi: Uso della modalità PATH
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Negli esempi seguenti viene illustrato l'utilizzo della modalità PATH nella creazione di codice XML da una query SELECT. Molte di queste query vengono specificate sui documenti XML di istruzioni per la produzione di biciclette archiviate nella colonna Instructions della tabella ProductModel.  
  
## <a name="specifying-a-simple-path-mode-query"></a>Specifica di una query semplice in modalità PATH  
 La query seguente specifica una modalità FOR XML PATH.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT   
       ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML PATH;  
GO  
```  
  
 Il risultato seguente è codice XML incentrato sugli elementi in cui il valore di ogni colonna nel set di righe risultante viene inserito in un elemento. Poiché la clausola `SELECT` non specifica alcun alias per i nomi delle colonne, i nomi degli elementi figlio generati sono gli stessi nomi di colonna corrispondenti nella clausola `SELECT` . Per ogni riga del set di righe viene aggiunto un tag <`row`>.  
  
 ```
<row>  
  <ProductModelID>122</ProductModelID>  
  <Name>All-Purpose Bike Stand</Name>  
</row>  
<row>  
  <ProductModelID>119</ProductModelID>
  <Name>Bike Wash</Name>
</row>
```
  
 Il risultato seguente corrisponde a quello della query in modalità `RAW` con l'opzione `ELEMENTS` specificata. Restituisce codice XML incentrato sugli elementi con un elemento <`row`> predefinito per ogni riga del set di risultati.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML RAW, ELEMENTS;  
```  
  
 È inoltre possibile specificare che il nome dell'elemento riga sovrascriva l'elemento <`row`> predefinito. Ad esempio, la query seguente restituisce l'elemento <`ProductModel`> per ogni riga del set di righe.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML PATH ('ProductModel');  
GO  
```  
  
 Il codice XML risultante avrà il nome dell'elemento riga specificato.  
  
```
<ProductModel>
  <ProductModelID>122</ProductModelID>
  <Name>All-Purpose Bike Stand</Name>
</ProductModel>
<ProductModel>
  <ProductModelID>119</ProductModelID>
  <Name>Bike Wash</Name>
</ProductModel>
```
  
 Se viene specificata una stringa di lunghezza zero, l'elemento di wrapping non viene prodotto.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML PATH ('');  
GO  
```  
  
 Risultato:  
  
```
<ProductModelID>122</ProductModelID>
<Name>All-Purpose Bike Stand</Name>
<ProductModelID>119</ProductModelID>
<Name>Bike Wash</Name>
```
  
## <a name="specifying-xpath-like-column-names"></a>Specifica di nomi di colonna in formato XPath  
 Nella query seguente il nome di colonna `ProductModelID` specificato inizia con "\@" e non contiene una barra ("/"). Nel codice XML risultante viene pertanto creato un attributo dell'elemento <`row`> con il valore di colonna corrispondente.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID AS "@id",  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML PATH ('ProductModelData');  
GO  
```  
  
 Risultato:  
  
```
<ProductModelData id="122">
  <Name>All-Purpose Bike Stand</Name>
</ProductModelData>
<ProductModelData id="119">
  <Name>Bike Wash</Name>
</ProductModelData>
```
  
 È possibile aggiungere un singolo elemento di livello principale specificando l'opzione `root` in `FOR XML`.  
  
```  
SELECT ProductModelID AS "@id",  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML PATH ('ProductModelData'), root ('Root');  
GO  
```  
  
 Per creare una gerarchia è possibile includere una sintassi di tipo PATH. Ad esempio, modificando il nome della colonna da `Name` in "SomeChild/ModelName", si otterrà una struttura XML gerarchica, come illustrato nel risultato seguente:  
  
```
<Root>
  <ProductModelData id="122">
    <SomeChild>
      <ModelName>All-Purpose Bike Stand</ModelName>
    </SomeChild>
  </ProductModelData>
  <ProductModelData id="119">
    <SomeChild>
      <ModelName>Bike Wash</ModelName>
    </SomeChild>
  </ProductModelData>
</Root>
```
  
 Oltre all'ID del modello di prodotto e al nome, la query seguente consente di recuperare i percorsi delle relative istruzioni di produzione. Poiché la colonna Instructions è di tipo **xml** , per il recupero del percorso viene specificato il metodo **query()** del tipo di dati **xml** .  
  
```  
SELECT ProductModelID AS "@id",  
       Name,  
       Instructions.query('declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
                /MI:root/MI:Location   
              ') AS ManuInstr  
FROM Production.ProductModel  
WHERE ProductModelID = 7  
FOR XML PATH ('ProductModelData'), root ('Root');  
GO  
```  
  
 Di seguito è riportato il risultato parziale. Poiché la query specifica ManuInstr come nome di colonna, il wrapping del codice XML restituito dal metodo **query()** viene eseguito in un tag <`ManuInstr`> come illustrato di seguito:  

```
<Root>
  <ProductModelData id="7">
    <Name>HL Touring Frame</Name>
    <ManuInstr>
      <MI:Location xmlns:MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"
        <MI:step>...</MI:step>...
      </MI:Location>
      ...
    </ManuInstr>
  </ProductModelData>
</Root> 
```

Nella query FOR XML precedente può essere utile includere gli spazi dei nomi per gli elementi <`Root`> e <`ProductModelData`>. A questo scopo è necessario innanzitutto definire i prefissi per l'associazione degli spazi dei nomi tramite WITH XMLNAMESPACES e utilizzare tali prefissi nella query FOR XML. Per altre informazioni, vedere [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
```  
USE AdventureWorks2012;  
GO  
WITH XMLNAMESPACES (  
   'uri1' AS ns1,    
   'uri2' AS ns2,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' as MI)  
SELECT ProductModelID AS "ns1:ProductModelID",  
       Name           AS "ns1:Name",  
       Instructions.query('  
                /MI:root/MI:Location   
              ')   
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH ('ns2:ProductInfo'), root('ns1:root');  
GO  
```  
  
 Il prefisso `MI` è inoltre definito in `WITH XMLNAMESPACES`. Di conseguenza, il metodo **query()** del tipo **xml** specificato non definisce il prefisso nel prologo della query. Risultato:  
  
```
<ns1:root xmlns:MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" xmlns="uri2" xmlns:ns2="uri2" xmlns:ns1="uri1">
  <ns2:ProductInfo>
    <ns1:ProductModelID>7</ns1:ProductModelID>
    <ns1:Name>HL Touring Frame</ns1:Name>
    <MI:Location xmlns:MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" LaborHours="2.5" LotSize="100" MachineHours="3" SetupHours="0.5" LocationID="10" xmlns="">
    <MI:step>
      Insert <MI:material>aluminum sheet MS-2341</MI:material> into the <MI:tool>T-85A framing tool</MI:tool>.
    </MI:step>
    ...
    </MI:Location>
    ...
  </ns2:ProductInfo>
</ns1:root>
```
  
## <a name="generating-a-value-list-using-path-mode"></a>Creazione di un elenco di valori mediante la modalità PATH  
 La query successiva crea un elenco di valori di prodotti ID. Per ogni ID prodotto la query crea inoltre elementi nidificati <`ProductName`>, come illustrato in questo frammento XML:  

```
<ProductModelData ProductModelID="7" ProductModelName="..." ProductIDs="product id list in the product model">
  <ProductName>...</ProductName>
  <ProductName>...</ProductName>
  ...
</ProductModelData>
```
  
 La query che produce il codice XML desiderato è la seguente:  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID     AS "@ProductModelID",  
       Name               AS "@ProductModelName",  
      (SELECT ProductID AS "data()"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH ('')) AS "@ProductIDs",  
       (SELECT Name AS "ProductName"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
        FOR XML PATH ('')) AS "ProductNames"  
FROM   Production.ProductModel  
WHERE  ProductModelID= 7 or ProductModelID=9  
FOR XML PATH('ProductModelData');  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La prima istruzione `SELECT` nidificata restituisce un elenco di ProductID utilizzando il nome di colonna `data()` . Poiché la query specifica una stringa vuota come nome dell'elemento riga in `FOR XML PATH`, non viene generato alcun elemento. L'elenco di valori viene invece assegnato all'attributo `ProductID` .  
  
-   La seconda istruzione `SELECT` nidificata recupera i nomi dei prodotti per quelli presenti nel modello del prodotto. Genera elementi <`ProductName`> che vengono restituiti inseriti nell'elemento <`ProductNames`>, poiché la query specifica `ProductNames` come nome di colonna.  
  
 Risultato parziale:  
  
```
<ProductModelData PId="7" ProductModelName="HL Touring Frame" ProductIDs="885 887 ...">
  <ProductNames>
    <ProductName>HL Touring Frame - Yellow, 60</ProductName>
    <ProductName>HL Touring Frame - Yellow, 46</ProductName>
  </ProductNames>
  ...
</ProductModelData>
<ProductModelData PId="9" ProductModelName="LL Road Frame" ProductIDs="722 723 724 ...">
  <ProductNames>
    <ProductName>LL Road Frame - Black, 58</ProductName>
    <ProductName>LL Road Frame - Black, 60</ProductName>
    <ProductName>LL Road Frame - Black, 62</ProductName>
    ...
  </ProductNames>
</ProductModelData>
```
  
 La sottoquery che crea i nomi di prodotto restituisce il risultato sotto forma di una stringa sostituita con entità e quindi aggiunta al codice XML. Se si aggiunge la direttiva TYPE, `FOR XML PATH (''), type`, la sottoquery restituisce il risultato come tipo **xml** e non viene eseguita la sostituzione con entità.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID AS "@ProductModelID",  
      Name AS "@ProductModelName",  
      (SELECT ProductID AS "data()"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH ('')  
       ) AS "@ProductIDs",  
       (  
       SELECT Name AS "ProductName"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH (''), type  
       ) AS "ProductNames"  
  
FROM Production.ProductModel  
WHERE ProductModelID= 7 OR ProductModelID=9  
FOR XML PATH('ProductModelData');  
```  
  
## <a name="adding-namespaces-in-the-resulting-xml"></a>Aggiunta degli spazi dei nomi nel codice XML risultante  
 Come descritto in [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md), è possibile usare WITH XMLNAMESPACES per includerli nelle query della modalità PATH. Ad esempio, i nomi specificati nella clausola SELECT includono i prefissi degli spazi dei nomi. La seguente query in modalità `PATH` crea codice XML con spazi dei nomi.  
  
```  
SELECT 'en'    as "English/@xml:lang",  
       'food'  as "English",  
       'ger'   as "German/@xml:lang",  
       'Essen' as "German"  
FOR XML PATH ('Translation')  
GO  
```  
  
 L'attributo `@xml:lang` aggiunto all'elemento <`English`> viene definito nello spazio dei nomi xml predefinito.  
  
 Risultato:  

```
<Translation>
  <English xml:lang="en">food</English>
  <German xml:lang="ger">Essen</German>
</Translation>
```
  
 La query seguente è simile a quella dell'esempio C, ma utilizza `WITH XMLNAMESPACES` per includere gli spazi dei nomi nel risultato XML. Per altre informazioni, vedere [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
```  
USE AdventureWorks2012;  
GO  
WITH XMLNAMESPACES ('uri1' AS ns1,  DEFAULT 'uri2')  
SELECT ProductModelID AS "@ns1:ProductModelID",  
      Name AS "@ns1:ProductModelName",  
      (SELECT ProductID AS "data()"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH ('')  
       ) AS "@ns1:ProductIDs",  
       (  
       SELECT ProductID AS "@ns1:ProductID",   
              Name AS "@ns1:ProductName"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH , type   
       ) AS "ns1:ProductNames"  
FROM Production.ProductModel  
WHERE ProductModelID= 7 OR ProductModelID=9  
FOR XML PATH('ProductModelData'), root('root');  
```  
  
 Risultato:  
  
```
<root xmlns="uri2" 
  xmlns:ns1="uri1">
  <ProductModelData ns1:ProductModelID="7" ns1:ProductModelName="HL Touring Frame" ns1:ProductIDs="885 887 888 889 890 891 892 893">
    <ns1:ProductNames>
      <row xmlns="uri2" xmlns:ns1="uri1" ns1:ProductID="885" ns1:ProductName="HL Touring Frame - Yellow, 60" />
      <row xmlns="uri2" xmlns:ns1="uri1" ns1:ProductID="887" ns1:ProductName="HL Touring Frame - Yellow, 46" />
      ...
    </ns1:ProductNames>
  </ProductModelData>
  <ProductModelData ns1:ProductModelID="9" ns1:ProductModelName="LL Road Frame" ns1:ProductIDs="722 723 724 725 726 727 728 729 730 736 737 738">
    <ns1:ProductNames>
      <row xmlns="uri2" xmlns:ns1="uri1" ns1:ProductID="722" ns1:ProductName="LL Road Frame - Black, 58" />
      ...
    </ns1:ProductNames>
  </ProductModelData>
</root>
```
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzare la modalità PATH con FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
  
  
