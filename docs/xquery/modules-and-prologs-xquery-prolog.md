---
title: Prologo XQuery | Microsoft Docs
description: Informazioni sul prologo XQuery che contiene una serie di dichiarazioni e definizioni che creano l'ambiente necessario per l'elaborazione delle query.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
- namespaces [XQuery]
- default namespace declarations
ms.assetid: 03924684-c5fd-44dc-8d73-c6ab90f5e069
author: rothja
ms.author: jroth
ms.openlocfilehash: 2b460f6da448e45d688ebcc08f418a9acbae85b8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341843"
---
# <a name="modules-and-prologs---xquery-prolog"></a>Moduli e prologhi - Prologo XQuery
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Una query XQuery è composta da un prologo e da un corpo. Il prologo include una serie di dichiarazioni e di definizioni che contribuiscono a creare l'ambiente necessario per l'elaborazione della query. In SQL Server, il prologo della query XQuery può contenere dichiarazioni dello spazio dei nomi. Il corpo della query XQuery è composto da una sequenza di espressioni che specificano il risultato della query desiderato.  
  
 Ad esempio, l'espressione XQuery seguente viene specificata sulla colonna Instructions di tipo **XML** in cui vengono archiviate le istruzioni di produzione come XML. La query recupera le istruzioni di produzione per il centro di lavorazione `10`. Il `query()` metodo del tipo di dati **XML** viene utilizzato per specificare l'espressione XQuery.  
  
```  
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') AS Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il prologo XQuery include una dichiarazione di prefisso dello spazio dei nomi (AWMI), `(namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";` .  
  
-   La parola chiave `declare namespace` definisce un prefisso dello spazio dei nomi che viene utilizzato successivamente nel corpo della query.  
  
-   `/AWMI:root/AWMI:Location[@LocationID="10"]` è il corpo della query.  
  
## <a name="namespace-declarations"></a>Dichiarazioni dello spazio dei nomi  
 Una dichiarazione dello spazio dei nomi definisce un prefisso e lo associa a un URI (Uniform Resource Identifier) dello spazio dei nomi, come illustrato nella query seguente. Nella query, `CatalogDescription` è una colonna di tipo **XML** .  
  
 Quando si esegue la query XQuery sulla colonna, il prologo della query specifica la dichiarazione `declare namespace` tramite la quale il prefisso `PD`, ovvero la descrizione del prodotto, viene associato all'URI dello spazio dei nomi. Questo prefisso viene quindi utilizzato nel corpo della query in sostituzione dell'URI dello spazio dei nomi. I nodi del codice XML risultante sono nello spazio dei nomi associato all'URI dello spazio dei nomi.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Per migliorare la leggibilità della query è possibile dichiarare gli spazi dei nomi utilizzando WITH XMLNAMESPACES anziché dichiarare l'associazione tra il prefisso e lo spazio dei nomi nel prologo della query utilizzando `declare namespace`.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
  
SELECT CatalogDescription.query('  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Per altre informazioni, vedere [aggiungere spazi dei nomi alle query con with XMLnamespaces](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
### <a name="default-namespace-declaration"></a>Dichiarazione dello spazio dei nomi predefinito  
 Anziché dichiarare un prefisso dello spazio dei nomi utilizzando la dichiarazione `declare namespace`, è possibile utilizzare la dichiarazione `declare default element namespace` per associare uno spazio dei nomi predefinito ai nomi degli elementi. In questo caso, non è necessario specificare un prefisso.  
  
 Nell'esempio seguente, l'espressione di percorso nel corpo della query non specifica un prefisso dello spazio dei nomi. Per impostazione predefinita, tutti i nomi degli elementi appartengono allo spazio dei nomi predefinito specificato nel prologo.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace  "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
 Per dichiarare uno spazio dei nomi predefinito è possibile utilizzare WITH XMLNAMESPACES:  
  
```  
WITH XMLNAMESPACES (DEFAULT 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription')  
SELECT CatalogDescription.query('  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)  
  
  
