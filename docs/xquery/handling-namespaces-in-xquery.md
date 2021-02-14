---
title: Gestione degli spazi dei nomi in XQuery | Microsoft Docs
description: Visualizzazione di esempi di gestione degli spazi dei nomi in un'espressione XQuery che include come dichiarare spazi dei nomi nuovi e predefiniti.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- declaring namespaces
- namespaces [XQuery]
- XQuery, namespaces
ms.assetid: 542b63da-4d3d-4ad5-acea-f577730688f1
author: rothja
ms.author: jroth
ms.openlocfilehash: acb1595ca2e7032d0363df3a1dd81740b7bcbf2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341881"
---
# <a name="handling-namespaces-in-xquery"></a>Gestione degli spazi dei nomi in XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono forniti esempi della gestione dello spazio dei nomi nelle query.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-declaring-a-namespace"></a>R. Dichiarazione di uno spazio dei nomi  
 La query seguente recupera le fasi di produzione di un modello di prodotto specifico.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /AWMI:root/AWMI:Location[1]/AWMI:step  
    ') as x  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Risultato parziale:  
  
```  
<AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>. </AWMI:step>  
...  
```  
  
 Si noti che la parola chiave **namespace** viene utilizzata per definire un nuovo prefisso dello spazio dei nomi, "AWMI:". Questo prefisso dovrà pertanto essere utilizzato nella query per tutti gli elementi che rientrano nell'ambito dello spazio dei nomi specifico.  
  
### <a name="b-declaring-a-default-namespace"></a>B. Dichiarazione di uno spazio dei nomi predefinito  
 Nella query precedente è stato definito un nuovo prefisso dello spazio dei nomi. È stato quindi necessario utilizzare tale prefisso nella query per selezionare le strutture XML desiderate. In alternativa, è possibile dichiarare uno spazio dei nomi predefinito, come illustrato nella query modificata seguente:  
  
```  
SELECT Instructions.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /root/Location[1]/step  
    ') as x  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Risultato:  
  
```  
<step xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <material>aluminum sheet MS-2341</material> into the <tool>T-85A framing tool</tool>. </step>  
...  
```  
  
 Si noti che lo spazio dei nomi definito, `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"`, ignora lo spazio dei nomi predefinito o vuoto. Per questo motivo, non è più disponibile un prefisso dello spazio dei nomi nell'espressione di percorso utilizzata per la query. Non è inoltre più disponibile un prefisso dello spazio dei nomi nei nomi degli elementi visualizzati nei risultati. Inoltre, lo spazio dei nomi predefinito viene applicato a tutti gli elementi, ma non ai relativi attributi.  
  
### <a name="c-using-namespaces-in-xml-construction"></a>C. Utilizzo degli spazi dei nomi nella costruzione di codice XML  
 Quando si definiscono nuovi spazi dei nomi, tali spazi vengono inseriti non solo nell'ambito della query ma anche nell'ambito della costruzione. Ad esempio, per costruire codice XML è possibile definire un nuovo spazio dei nomi utilizzando la dichiarazione "`declare namespace ...`" e quindi utilizzare tale spazio dei nomi con gli elementi e gli attributi che verranno visualizzati nei risultati della query.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace myNS="uri:SomeNamespace";<myNS:Result>  
          { /ProductDescription/Summary }  
       </myNS:Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Risultato:  
  
```  
  
      <myNS:Result xmlns:myNS="uri:SomeNamespace">  
  <Summary xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
     Our top-of-the-line competition mountain bike. Performance-enhancing   
     options include the innovative HL Frame, super-smooth front   
     suspension, and traction for all terrain.</p1:p>  
  </Summary>  
</myNS:Result>  
```  
  
 In alternativa, è inoltre possibile definire lo spazio dei nomi in modo esplicito in ogni punto in cui viene utilizzato come parte della costruzione XML, come illustrato nella query seguente:  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <myNS:Result xmlns:myNS="uri:SomeNamespace">  
          { /ProductDescription/Summary }  
       </myNS:Result>  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
### <a name="d-construction-using-default-namespaces"></a>D. Costruzione tramite gli spazi dei nomi predefiniti  
 È inoltre possibile definire uno spazio dei nomi predefinito da utilizzare nel codice XML costruito. Ad esempio, la query seguente mostra come è possibile specificare uno spazio dei nomi predefinito, "URI: SomeNamespace" \\ , da usare come predefinito per gli elementi denominati localmente costruiti, ad esempio l' `<Result>` elemento.  
  
```  
SELECT CatalogDescription.query('  
      declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      declare default element namespace "uri:SomeNamespace";<Result>  
          { /PD:ProductDescription/PD:Summary }  
       </Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Risultato:  
  
```  
  
      <Result xmlns="uri:SomeNamespace">  
  <PD:Summary xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         Our top-of-the-line competition mountain bike. Performance-  
         enhancing options include the innovative HL Frame, super-smooth   
         front suspension, and traction for all terrain.</p1:p>  
  </PD:Summary>  
</Result>  
```  
  
 Si noti che, dato che lo spazio dei nomi predefinito o lo spazio dei nomi vuoto vengono ignorati, tutti gli elementi denominati a livello locale nel codice XML costruito vengono associati allo spazio dei nomi predefinito sostitutivo. Se la costruzione del codice XML richiede flessibilità per poter sfruttare lo spazio dei nomi vuoto, è pertanto consigliabile non ignorare lo spazio dei nomi predefinito.  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
