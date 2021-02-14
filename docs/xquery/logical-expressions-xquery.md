---
title: Espressioni logiche (XQuery) | Microsoft Docs
description: Informazioni sulle espressioni logiche supportate in XQuery.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- logical operators [SQL Server], XQuery
- effective Boolean value [XQuery]
- logical expressions [XQuery]
- EBV
- expressions [XQuery], logical
ms.assetid: de94cd2e-2d48-49fb-9ebd-a2d90c79bf62
author: rothja
ms.author: jroth
ms.openlocfilehash: 44f05374320dc2d5c329ad234e62dd0c420bf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341857"
---
# <a name="logical-expressions-xquery"></a>Espressioni logiche (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery supporta gli operatori **and** e **or** logici.  
  
```  
expression1 and expression2  
expression1 or expression2  
```  
  
 Le espressioni di test, `expression1,``expression2` , in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possono generare una sequenza vuota, una sequenza di uno o più nodi o un singolo valore booleano. In base al risultato, il valore booleano effettivo viene determinato nel modo seguente:  
  
-   Se l'espressione di prova restituisce una sequenza vuota, il risultato dell'espressione è False.  
  
-   Se l'espressione di prova restituisce un singolo valore booleano, il risultato dell'espressione è tale valore.  
  
-   Se l'espressione di prova restituisce una sequenza costituita da uno o più nodi, il risultato dell'espressione è True.  
  
-   Negli altri casi, viene restituito un errore statico.  
  
 L'operatore **and** e **or** logico viene quindi applicato ai valori booleani risultanti delle espressioni con la semantica logica standard.  
  
 La query seguente recupera dal catalogo prodotti le immagini di piccole dimensioni, ovvero l'elemento <`Picture`>, per un modello di prodotto specifico. Si noti che, per ogni documento di descrizione del prodotto, nel catalogo è possibile archiviare uno o più foto del prodotto con attributi diversi, ad esempio le dimensioni e l'angolazione.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $F in /PD:ProductDescription/PD:Picture[PD:Size="small"   
                                                 and PD:Angle="front"]  
     return   
         $F   
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Risultato:  
  
```  
<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  <PD:Angle>front</PD:Angle>  
  <PD:Size>small</PD:Size>  
  <PD:ProductPhotoID>31</PD:ProductPhotoID>  
</PD:Picture>  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni XQuery](../xquery/xquery-expressions.md)  
  
  
