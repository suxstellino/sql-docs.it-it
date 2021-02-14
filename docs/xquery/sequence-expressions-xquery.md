---
title: Espressioni Sequence (XQuery) | Microsoft Docs
description: Informazioni sulle espressioni di sequenza XQuery che creano, filtrano e combinano una sequenza di elementi.
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
- sequence [XQuery]
- expressions [XQuery], sequence
- filtering sequences [XQuery]
ms.assetid: 41e18b20-526b-45d2-9bd9-e3b7d7fbce4e
author: rothja
ms.author: jroth
ms.openlocfilehash: ea654a45894791fa22ecfa843c44cdd6214a5181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351065"
---
# <a name="sequence-expressions-xquery"></a>Espressioni di sequenze (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta gli operatori XQuery utilizzati per la costruzione, l'applicazione di filtri e la combinazione di una sequenza di elementi. Un elemento può essere un valore atomico o un nodo.  
  
## <a name="constructing-sequences"></a>Costruzione delle sequenze  
 Per costruire una sequenza in cui vengono concatenati elementi è possibile utilizzare l'operatore virgola.  
  
 Una sequenza può contenere valori duplicati. Le sequenze nidificate, ovvero le sequenze che includono un'altra sequenza, vengono compresse. Ad esempio, la sequenza (1, 2, (3, 4, (5))) diventa (1, 2, 3, 4, 5). Di seguito vengono riportati esempi di costruzione delle sequenze.  
  
### <a name="example-a"></a>Esempio A  
 La query seguente restituisce una sequenza di cinque valori atomici:  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3,4,5)')  
go  
-- result 1 2 3 4 5  
```  
  
 La query seguente restituisce una sequenza di due nodi:  
  
```  
-- sequence of 2 nodes  
declare @x xml  
set @x=''  
select @x.query('(<a/>, <b/>)')  
go  
-- result  
<a />  
<b />  
```  
  
 La query seguente restituisce un errore, perché si sta costruendo una sequenza di valori atomici e di nodi, ovvero una sequenza eterogenea, che non è supportata.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1, 2, <a/>, <b/>)')  
go  
```  
  
### <a name="example-b"></a>Esempio B  
 La query seguente costruisce una sequenza di valori atomici tramite la combinazione di quattro sequenze di lunghezza diversa in un'unica sequenza.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2),10,(),(4, 5, 6)')  
go  
-- result = 1 2 10 4 5 6  
```  
  
 È possibile ordinare la sequenza utilizzando FLOWR e ORDER BY:  
  
```  
declare @x xml  
set @x=''  
select @x.query('for $i in ((1,2),10,(),(4, 5, 6))  
                  order by $i  
                  return $i')  
go  
```  
  
 È possibile contare gli elementi nella sequenza usando la funzione **FN: Count ()** .  
  
```  
declare @x xml  
set @x=''  
select @x.query('count( (1,2,3,(),4) )')  
go  
-- result = 4  
```  
  
### <a name="example-c"></a>Esempio C  
 La query seguente viene specificata sulla colonna AdditionalContactInfo del tipo **XML** nella tabella Contact. In questa colonna sono archiviate le informazioni aggiuntive sul contatto, ad esempio numeri di telefono aggiuntivi, numeri di cercapersone e indirizzi. Gli \<telephoneNumber> \<pager> altri nodi, e possono essere visualizzati in qualsiasi punto del documento. La query crea una sequenza che contiene tutti gli \<telephoneNumber> elementi figlio del nodo di contesto, seguiti dagli \<pager> elementi figlio. Si noti che nell'espressione restituita, `($a//act:telephoneNumber, $a//act:pager)`, viene utilizzato l'operatore di sequenza virgola.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('  
   for $a in /aci:AdditionalContactInfo   
   return ($a//act:telephoneNumber, $a//act:pager)  
') As Result  
FROM Person.Contact  
WHERE ContactID=3  
```  
  
 Risultato:  
  
```  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3333</act:number>  
</act:telephoneNumber>  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3334</act:number>  
</act:telephoneNumber>  
<act:pager xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>999-555-1244</act:number>  
  <act:SpecialInstructions>  
Page only in case of emergencies.  
</act:SpecialInstructions>  
</act:pager>  
```  
  
## <a name="filtering-sequences"></a>Applicazione di filtri alle sequenze  
 È possibile applicare filtri alla sequenza restituita da un'espressione, aggiungendo un predicato all'espressione. Per altre informazioni, vedere [espressioni di percorso &#40;&#41;XQuery ](../xquery/path-expressions-xquery.md). Ad esempio, la query seguente restituisce una sequenza di tre <`a`> nodi elemento:  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a')  
```  
  
 Risultato:  
  
```  
<a attrA="1">111</a>  
<a />  
<a />  
```  
  
 Per recuperare solo <`a` elementi> con l'attributo attra, è possibile specificare un filtro nel predicato. La sequenza risultante avrà un solo elemento <`a`>.  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a[@attrA]')  
```  
  
 Risultato:  
  
```  
<a attrA="1">111</a>  
```  
  
 Per ulteriori informazioni su come specificare i predicati in un'espressione di percorso, vedere [specifica di predicati in un passaggio dell'espressione di percorso](../xquery/path-expressions-specifying-predicates.md).  
  
 Nell'esempio seguente viene compilata un'espressione di sequenza dei sottoalberi e quindi viene applicato un filtro alla sequenza.  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
<c>top level c</c>  
<d></d>  
'  
```  
  
 L'espressione in `(/a, /b)` costruisce una sequenza con i sottoalberi `/a` e `/b` e quindi l'espressione filtra l'elemento `<c>` dalla sequenza risultante.  
  
```  
SELECT @x.query('  
  (/a, /b)/c  
')  
```  
  
 Risultato:  
  
```  
<c>C under a</c>  
<c>C under b</c>  
```  
  
 Nell'esempio seguente viene applicato un filtro del predicato. L'espressione trova gli elementi <`a`> e <`b`> che contengono l'elemento <`c`>.  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
  
<c>top level c</c>  
<d></d>  
'  
SELECT @x.query('  
  (/a, /b)[c]  
')  
```  
  
 Risultato:  
  
```  
<a>  
  <c>C under a</c>  
</a>  
<b>  
  <c>C under b</c>  
</b>  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   Le espressioni intervallo di XQuery non sono supportate.  
  
-   Le sequenze devono essere omogenee. In particolare, tutti gli elementi di una sequenza devono essere nodi o valori atomici. Per verificarlo, viene eseguito il controllo statico.  
  
-   Non è supportata la combinazione di sequenze di nodi tramite gli operatori UNION, INTERSECT o EXCEPT.  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni XQuery](../xquery/xquery-expressions.md)  
  
  
