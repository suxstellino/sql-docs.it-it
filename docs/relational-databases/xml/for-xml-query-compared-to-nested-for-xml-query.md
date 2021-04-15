---
title: Query FOR XML e query FOR XML annidate a confronto | Microsoft Docs
description: Per informazioni sui vantaggi delle query FOR XML nidificate, vedere un esempio in cui viene confrontata una query FOR XML a un solo livello con una query FOR XML nidificata.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML query
- queries [XML in SQL Server], comparing query types
ms.assetid: 19225b4a-ee3f-47cf-8bcc-52699eeda32c
author: rothja
ms.author: jroth
ms.openlocfilehash: ebc5eb7fdc2e4ebe1e5424e3f7df1ac52a2b193e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488349"
---
# <a name="for-xml-query-compared-to-nested-for-xml-query"></a>Query FOR XML e query nidificata FOR XML a confronto
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene messa a confronto una query FOR XML con un solo livello con una query FOR XML nidificata. Uno dei vantaggi dell'utilizzo di query FOR XML consiste nella possibilità di specificare una combinazione di XML incentrati sia sugli attributi che sugli elementi per i risultati della query. Tutto ciò è dimostrato nell'esempio riportato di seguito.  
  
## <a name="example"></a>Esempio  
 La query seguente `SELECT` recupera informazioni sulla categoria e sulla sottocategoria di un prodotto dal database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La query non include istruzioni FOR XML nidificate.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT   ProductCategory.ProductCategoryID,   
         ProductCategory.Name as CategoryName,  
         ProductSubCategory.ProductSubCategoryID,   
         ProductSubCategory.Name  
FROM     Production.ProductCategory, Production.ProductSubCategory  
WHERE    ProductCategory.ProductCategoryID = ProductSubCategory.ProductCategoryID  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
GO  
```  
  
 Risultato parziale:  
  
```  
<ProductCategory ProductCategoryID="1" CategoryName="Bike">  
  <ProductSubCategory ProductSubCategoryID="1" Name="Mountain Bike"/>  
  <ProductSubCategory ProductSubCategoryID="2" Name="Road Bike"/>  
  <ProductSubCategory ProductSubCategoryID="3" Name="Touring Bike"/>  
</ProductCategory>  
...  
```  
  
 Se nella query si specifica la direttiva `ELEMENTS` , verrà restituito un risultato incentrato sugli elementi, come illustrato nel frammento di risultato seguente:  
  
```  
<ProductCategory>  
  <ProductCategoryID>1</ProductCategoryID>  
  <CategoryName>Bike</CategoryName>  
  <ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <Name>Mountain Bike</Name>  
  </ProductSubCategory>  
  <ProductSubCategory>  
     ...  
  </ProductSubCategory>  
</ProductCategory>  
```  
  
 Si supponga quindi di voler generare una gerarchia XML costituita da una combinazione di valori XML incentrati sugli elementi e incentrati sugli attributi, come illustrato nel frammento seguente:  
  
```  
<ProductCategory ProductCategoryID="1" CategoryName="Bike">  
  <ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <SubCategoryName>Mountain Bike</SubCategoryName></ProductSubCategory>  
  <ProductSubCategory>  
     ...  
  <ProductSubCategory>  
     ...  
</ProductCategory>  
```  
  
 Nel frammento precedente le informazioni sulla categoria del prodotto, ad esempio il nome e l'ID della categoria, sono attributi. Tuttavia, le informazioni sulla sottocategoria sono incentrate sugli elementi. Per costruire l'elemento <`ProductCategory`> è possibile creare una query `FOR XML`, come illustrato di seguito:  
  
```  
SELECT ProductCategoryID, Name as CategoryName  
FROM Production.ProductCategory ProdCat  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
```  
  
 Risultato:  
  
```  
< ProdCat ProductCategoryID="1" CategoryName="Bikes" />  
< ProdCat ProductCategoryID="2" CategoryName="Components" />  
< ProdCat ProductCategoryID="3" CategoryName="Clothing" />  
< ProdCat ProductCategoryID="4" CategoryName="Accessories" />  
```  
  
 Per costruire gli elementi <`ProductSubCategory`> nidificati nel valore XML, è quindi necessario aggiungere una query `FOR XML` nidificata, come illustrato di seguito:  
  
```  
SELECT ProductCategoryID, Name as CategoryName,  
       (SELECT ProductSubCategoryID, Name SubCategoryName  
        FROM   Production.ProductSubCategory  
        WHERE ProductSubCategory.ProductCategoryID =   
              ProductCategory.ProductCategoryID  
        FOR XML AUTO, TYPE, ELEMENTS  
       )  
FROM Production.ProductCategory  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La query `FOR XML` interna recupera informazioni sulla sottocategoria del prodotto. Nella query `ELEMENTS` interna viene aggiunta la direttiva `FOR XML` per generare un valore XML incentrato sugli elementi, che verrà aggiunto al valore XML generato dalla query esterna. Per impostazione predefinita la query esterna genera un valore XML incentrato sugli attributi.  
  
-   Nella query interna è specificata la direttiva `TYPE` in modo che venga restituito un risultato di tipo **xml** . Se la direttiva `TYPE` non viene specificata, verrà restituito un risultato di tipo **nvarchar(max)** e i dati XML verranno restituiti come entità.  
  
-   Poiché la direttiva `TYPE` è specificata anche nella query esterna, il risultato della query verrà restituito al client come tipo **xml** .  
  
 Risultato parziale:  
  
```  
<ProductCategory ProductCategoryID="1" CategoryName="Bike">  
  <ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <SubCategoryName>Mountain Bike</SubCategoryName></ProductSubCategory>  
  <ProductSubCategory>  
     ...  
  <ProductSubCategory>  
     ...  
</ProductCategory>  
```  
  
 La query seguente è semplicemente un'estensione della precedente. Visualizza la gerarchia dei prodotti completa nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , Sono inclusi gli elementi seguenti:  
  
-   Categorie di prodotti  
  
-   Sottocategorie di prodotti in ogni categoria  
  
-   Modelli di prodotti in ogni sottocategoria  
  
-   Prodotti per ogni modello  
  
 Per comprendere l'organizzazione del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , è possibile utilizzare la query seguente:  
  
```  
SELECT ProductCategoryID, Name as CategoryName,  
       (SELECT ProductSubCategoryID, Name SubCategoryName,  
               (SELECT ProductModel.ProductModelID,   
                       ProductModel.Name as ModelName,  
                       (SELECT ProductID, Name as ProductName, Color  
                        FROM   Production.Product  
                        WHERE  Product.ProductModelID =   
                               ProductModel.ProductModelID  
                        FOR XML AUTO, TYPE)  
                FROM   (SELECT distinct ProductModel.ProductModelID,   
                               ProductModel.Name  
                        FROM   Production.ProductModel,   
                               Production.Product  
                        WHERE  ProductModel.ProductModelID =   
                               Product.ProductModelID  
                        AND    Product.ProductSubCategoryID =   
                               ProductSubCategory.ProductSubCategoryID)   
                                  ProductModel  
                FOR XML AUTO, type  
               )  
        FROM Production.ProductSubCategory  
        WHERE ProductSubCategory.ProductCategoryID =   
              ProductCategory.ProductCategoryID  
        FOR XML AUTO, TYPE, ELEMENTS  
       )  
FROM Production.ProductCategory  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
```  
  
 Risultato parziale:  
  
```  
<Production.ProductCategory ProductCategoryID="1" CategoryName="Bikes">  
  <Production.ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <SubCategoryName>Mountain Bikes</SubCategoryName>  
    <ProductModel ProductModelID="19" ModelName="Mountain-100">  
      <Production.Product ProductID="771"   
                ProductName="Mountain-100 Silver, 38" Color="Silver" />  
      <Production.Product ProductID="772"   
                ProductName="Mountain-100 Silver, 42" Color="Silver" />  
      <Production.Product ProductID="773"   
                ProductName="Mountain-100 Silver, 44" Color="Silver" />  
        ...  
    </ProductModel>  
     ...  
```  
  
 Se si rimuove la direttiva `ELEMENTS` dalla query `FOR XML` nidificata che genera le sottocategorie di prodotti, il risultato sarà interamente incentrato sugli attributi. È pertanto possibile scrivere questa query senza nidificazione. L'aggiunta della direttiva `ELEMENTS` consente di ottenere un valore XML incentrato in parte sugli attributi ed in parte sugli elementi. Questo tipo di risultato non può essere generato da una query FOR XML con un solo livello.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di query FOR XML nidificate](../../relational-databases/xml/use-nested-for-xml-queries.md)  
  
  
