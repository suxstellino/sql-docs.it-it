---
title: Funzione data (XQuery) | Microsoft Docs
description: Informazioni su come usare la funzione XQuery data () per restituire il valore tipizzato per ogni elemento in una sequenza specificata di elementi.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:data function
- data function [XQuery]
ms.assetid: 511b5d7d-c679-4cb2-a3dd-170cc126f49d
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e20c00d93082994cd5dc230bfe1b4356b3ed031
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338502"
---
# <a name="data-accessor-functions---data-xquery"></a>Funzioni di accesso dati - data (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce il valore tipizzato per ogni elemento specificato da *$arg*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:data ($arg as item()*) as xdt:untypedAtomic*  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Sequenza di elementi per i quali verranno restituiti i valori tipizzati.  
  
## <a name="remarks"></a>Commenti  
 Per i valori tipizzati sono valide le osservazioni seguenti:  
  
-   Il valore tipizzato di un valore atomico è il valore atomico.  
  
-   Il valore tipizzato di un nodo di testo è il valore stringa del nodo di testo.  
  
-   Il valore tipizzato di un commento è il valore stringa del commento.  
  
-   Il valore tipizzato di un'istruzione di elaborazione è il contenuto dell'istruzione, senza il relativo nome di destinazione.  
  
-   Il valore tipizzato di un nodo documento è il relativo valore stringa.  
  
 Per i nodi attributo ed elemento sono valide le osservazioni seguenti:  
  
-   Se un nodo attributo è tipizzato con un tipo di XML Schema, il relativo valore tipizzato è il valore tipizzato corrispondente.  
  
-   Se il nodo attributo non è tipizzato, il relativo valore tipizzato è uguale al relativo valore stringa restituito come un'istanza di **xdt: untypedAtomic**.  
  
-   Se il nodo dell'elemento non è stato tipizzato, il relativo valore tipizzato è uguale al relativo valore stringa restituito come un'istanza di **xdt: untypedAtomic**.  
  
 Per i nodi elemento tipizzati sono valide le osservazioni seguenti:  
  
-   Se l'elemento ha un tipo di contenuto semplice, **Data ()** restituisce il valore tipizzato dell'elemento.  
  
-   Se il nodo è di tipo complesso, incluso XS: anyType, **Data ()** restituisce un errore statico.  
  
 Sebbene l'utilizzo della funzione **Data ()** sia spesso facoltativo, come illustrato negli esempi seguenti, la specifica della funzione **Data ()** aumenta in modo esplicito la leggibilità delle query. Per altre informazioni, vedere [nozioni di base su XQuery](../xquery/xquery-basics.md).  
  
 Non è possibile specificare **i dati ()** nel codice XML costruito, come illustrato di seguito:  
  
```  
declare @x xml  
set @x = ''  
select @x.query('data(<SomeNode>value</SomeNode>)')  
```  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-data-xquery-function-to-extract-typed-value-of-a-node"></a>R. Utilizzo della funzione XQuery data() per estrarre il valore tipizzato di un nodo  
 Nella query seguente viene illustrato il modo in cui viene utilizzata la funzione **Data ()** per recuperare i valori di un attributo, di un elemento e di un nodo di testo:  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query(N'  
 for $pd in //p1:ProductDescription  
 return   
    <Root   
      ProductID = "{ data( ($pd//@ProductModelID)[1] ) }"   
      Feature =   "{ data( ($pd/p1:Features/wm:Warranty/wm:Description)[1] ) }" >  
    </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Risultato:  
  
```  
<Root ProductID="19" Feature="parts and labor"/>  
```  
  
 Come indicato in precedenza, la funzione **Data ()** è facoltativa quando si costruiscono attributi. Se non si specifica la funzione **Data ()** , si presuppone in modo implicito. La query seguente genera gli stessi risultati della query precedente:  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for $pd in //p1:ProductDescription  
         return   
          <Root    
                ProductID = "{ ($pd/@ProductModelID)[1] }"    
                Feature =   "{ ($pd/p1:Features/wm:Warranty/wm:Description)[1] }" >  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Negli esempi seguenti vengono illustrate le istanze in cui è richiesta la funzione **Data ()** .  
  
 Nella query seguente **$PD/P1: Specifications/Material** restituisce l'elemento <`Material`>. Inoltre, **i dati ($PD/P1: Specifications/Material)** restituiscono dati di tipo carattere tipizzati come xdt: untypedAtomic, perché <> non è tipizzato `Material` . Quando l'input non è tipizzato, il risultato di **Data ()** viene tipizzato come **xdt: untypedAtomic**.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in //p1:ProductDescription  
         return   
          <Root>  
             { $pd/p1:Specifications/Material }  
             { data($pd/p1:Specifications/Material) }  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Risultato:  
  
```  
<Root>  
  <Material>Almuminum Alloy</Material>Almuminum Alloy  
</Root>  
```  
  
 Nella query seguente, **Data ($PD/P1: features/WM: warranty)** restituisce un errore statico, perché <`Warranty`> è un elemento di tipo complesso.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
 <Root>  
   {     /p1:ProductDescription/p1:Features/wm:Warranty }  
   { data(/p1:ProductDescription/p1:Features/wm:Warranty) }  
 </Root>  
 ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID = 23  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
