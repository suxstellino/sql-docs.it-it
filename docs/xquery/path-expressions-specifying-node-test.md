---
title: Specifica del test di nodo in un passaggio dell'espressione di percorso | Microsoft Docs
description: Informazioni su come specificare un test di nodo nel passaggio dell'asse di un'espressione di percorso XQuery.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- node test [XQuery]
ms.assetid: ffe27a4c-fdf3-4c66-94f1-7e955a36cadd
author: rothja
ms.author: jroth
ms.openlocfilehash: 578d10bc078edf886eb7980c3b7a099c99ef59a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338545"
---
# <a name="path-expressions---specifying-node-test"></a>Espressioni di percorso - Specifica test di nodo
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Ogni passo dell'asse in un'espressione di percorso include i componenti seguenti:  
  
-   [Un asse](../xquery/path-expressions-specifying-axis.md)  
  
-   Un test di nodo  
  
-   [Zero o più predicati (facoltativo)](../xquery/path-expressions-specifying-predicates.md)  
  
 Per altre informazioni, vedere [espressioni di percorso &#40;&#41;XQuery ](../xquery/path-expressions-xquery.md).  
  
 Un test di nodo è una condizione e rappresenta il secondo componente del passo dell'asse in un'espressione di percorso. È necessario che tutti i nodi selezionati da un passo soddisfino questa condizione. Per l'espressione di percorso `/child::ProductDescription`, il test di nodo è `ProductDescription`. Questo passo recupera i nodi elemento figlio con nome ProductDescription.  
  
 Una condizione del test di nodo include:  
  
-   Un nome di nodo. Vengono restituiti solo nodi del tipo di nodo principale con il nome specificato.  
  
-   Un tipo di nodo. Vengono restituiti solo nodi del tipo specificato.  
  
> [!NOTE]  
>  I nomi di nodo specificati nelle espressioni di percorso XQuery non sono soggetti alle stesse regole di confronto applicate alle query Transact-SQL e rispettano sempre la distinzione tra maiuscole e minuscole.  
  
## <a name="node-name-as-node-test"></a>Nome di nodo specificato come test di nodo  
 Per specificare un nome di nodo come test di nodo in un passo dell'espressione di percorso, è necessario comprendere il concetto di tipo di nodo principale. Ogni asse child, element o attribute dispone di un tipo di nodo principale. Ad esempio:  
  
-   Un asse attribute può contenere solo attributi. Pertanto, il nodo attributo è il tipo di nodo principale dell'asse attribute.  
  
-   Per altri assi, se i nodi selezionati in base all'asse specifico possono contenere nodi elemento, il tipo di nodo principale per tale asse è element.  
  
 Quando si specifica un nome di nodo come test di nodo, il passo restituisce i tipi di nodi seguenti:  
  
-   Nodi del tipo di nodo principale dell'asse.  
  
-   Nodi con lo stesso nome specificato nel test di nodo.  
  
 Si consideri ad esempio l'espressione di percorso seguente:  
  
```  
child::ProductDescription   
```  
  
 Questa espressione costituita da un singolo passo specifica un asse `child` e il nome di nodo `ProductDescription` come test di nodo. L'espressione restituisce solo i nodi del tipo di nodo principale dell'asse child, ovvero nodi elemento, e con nome ProductDescription.  
  
 L'espressione di percorso `/child::PD:ProductDescription/child::PD:Features/descendant::*,` include tre passi. Tali passi specificano gli assi child e descendant. In ogni passo, il nome di nodo viene specificato come test di nodo. Il carattere jolly (`*`) nel terzo passo indica tutti i nodi del tipo di nodo principale per l'asse descendant. Il tipo di nodo principale dell'asse determina il tipo di nodi selezionati, che vengono filtrati in base al nome di nodo.  
  
 Di conseguenza, quando questa espressione viene eseguita sui documenti XML del catalogo dei prodotti nella tabella **ProductModel** , recupera tutti gli elementi figlio del nodo dell'elemento \<Features> figlio dell' \<ProductDescription> elemento.  
  
 L'espressione di percorso, `/child::PD:ProductDescription/attribute::ProductModelID` , è costituita da due passaggi. Entrambi i passi specificano un nome di nodo come test di nodo. Inoltre, il secondo passo utilizza l'asse attribute. Pertanto, ogni passo seleziona i nodi del tipo di nodo principale dell'asse corrispondente con il nome specificato come test di nodo. Pertanto, l'espressione restituisce il nodo dell'attributo **ProductModelID** del \<ProductDescription> nodo dell'elemento.  
  
 Quando si specificano i nomi dei nodi per i test di nodo, è inoltre possibile utilizzare il carattere jolly (*) per specificare il nome locale di un nodo o il relativo prefisso dello spazio dei nomi, come illustrato nell'esempio seguente:  
  
```  
declare @x xml  
set @x = '  
<greeting xmlns="ns1">  
   <salutation>hello</salutation>  
</greeting>  
<greeting xmlns="ns2">  
   <salutation>welcome</salutation>  
</greeting>  
<farewell xmlns="ns1" />'  
select @x.query('//*:greeting')  
select @x.query('declare namespace ns="ns1"; /ns:*')  
```  
  
## <a name="node-type-as-node-test"></a>Tipo di nodo specificato come test di nodo  
 Per le query su tipi di nodo diversi dai nodi elemento, è necessario utilizzare un test del tipo di nodo. Come illustrato nella tabella seguente, sono disponibili quattro test del tipo di nodo.  
  
|Tipo di nodo|Restituisce|Esempio|  
|---------------|-------------|-------------|  
|`comment()`|True per un nodo di commento.|`following::comment()` seleziona tutti i nodi di commento che seguono il nodo di contesto.|  
|`node()`|True per un nodo di qualsiasi tipo.|`preceding::node()` seleziona tutti nodi che precedono il nodo di contesto.|  
|`processing-instruction()`|True per un nodo di istruzioni di elaborazione.|`self::processing instruction()` seleziona tutti i nodi di istruzioni di elaborazione all'interno del nodo di contesto.|  
|`text()`|True per un nodo di testo.|`child::text()` seleziona tutti i nodi di testo figlio del nodo di contesto.|  
  
 Se si specifica il tipo di nodo, ad esempio text() o comment() ..., come test di nodo, il passo restituisce solo i nodi del tipo specificato, indipendentemente dal tipo di nodo principale dell'asse. Ad esempio, l'espressione di percorso seguente restituisce solo i nodi di commento figlio del nodo di contesto:  
  
```  
child::comment()  
```  
  
 Analogamente, `/child::ProductDescription/child::Features/child::comment()` Recupera i nodi di commento figlio del nodo \<Features> elemento figlio del \<ProductDescription> nodo elemento.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti vengono confrontati il nome di nodo e il tipo di nodo.  
  
### <a name="a-results-of-specifying-the-node-name-and-the-node-type-as-node-tests-in-a-path-expression"></a>R. Risultati della definizione del nome di nodo e del tipo di nodo come test di nodo in un'espressione di percorso  
 Nell'esempio seguente viene assegnato un semplice documento XML a una variabile di tipo **XML** . Vengono eseguite query sul documento tramite espressioni di percorso diverse. Successivamente, vengono confrontati i risultati.  
  
```  
declare @x xml  
set @x='  
<a>  
 <b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
 </b>  
</a>'  
select @x.query('  
/child::a/child::b/descendant::*  
')  
```  
  
 Questa espressione chiede i nodi elemento discendenti del nodo elemento `<b>`.  
  
 L'asterisco (`*`) nel test di nodo indica un carattere jolly per il nome di nodo. Il tipo di nodo primario per l'asse descendant è il nodo elemento. Pertanto, l'espressione restituisce tutti i nodi elemento discendenti del nodo elemento `<b>`, ovvero vengono restituiti `<c>` e `<d>`, come illustrato nel risultato seguente:  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Se si specifica un asse descendent-or-self anziché un asse descendant, vengono restituiti sia il nodo di contesto che i relativi discendenti:  
  
```  
/child::a/child::b/descendant-or-self::*  
```  
  
 Questa espressione restituisce il nodo elemento `<b>` e i relativi nodi elemento discendenti. Il tipo di nodi restituiti è determinato dal tipo di nodo primario element dell'asse descendant-or-self, che restituisce infatti i nodi discendenti.  
  
 Risultato:  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
  
<c>text2  
     <d>text3</d>  
</c>  
  
<d>text3</d>   
```  
  
 Nell'espressione precedente viene utilizzato un carattere jolly come nome di nodo. In alternativa, è possibile utilizzare la funzione `node()`, come illustrato nell'espressione seguente:  
  
```  
/child::a/child::b/descendant::node()  
```  
  
 Poiché `node()` è un tipo di nodo, si riceveranno tutti i nodi dell'asse discendente. Risultato:  
  
```  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
 Analogamente, se si specifica un asse descendant-or-self e `node()` come test di nodo, verranno restituiti tutti i discendenti, gli elementi e i nodi di testo, nonché il nodo di contesto, ovvero l'elemento `<b>`.  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
### <a name="b-specifying-a-node-name-in-the-node-test"></a>B. Definizione di un nome di nodo nel test di nodo  
 Nell'esempio seguente viene specificato un nome di nodo come test di nodo in tutte le espressioni di percorso. Di conseguenza, tutte le espressioni restituiscono nodi del tipo di nodo principale dell'asse che include il nome di nodo specificato nel test di nodo.  
  
 L'espressione di query seguente restituisce l' `Warranty` elemento <> dal documento XML del catalogo prodotti archiviato nella `Production.ProductModel` tabella:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:Warranty  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La parola chiave `namespace` nel prologo XQuery definisce un prefisso utilizzato nel corpo della query. Per ulteriori informazioni sul prologo XQuery, vedere [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md) .  
  
-   Tutti e tre i passi nell'espressione di percorso specificano l'asse child e un nome di nodo come test di nodo.  
  
-   La parte facoltativa relativa al qualificatore di passo del passo dell'asse non viene specificata nei passi dell'espressione.  
  
 La query restituisce gli `Warranty` elementi figlio di <> dell'elemento figlio dell'elemento <`Features`> dell' `ProductDescription` elemento <>.  
  
 Risultato:  
  
```  
<wm:Warranty xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
  <wm:Description>parts and labor</wm:Description>  
</wm:Warranty>     
```  
  
 Nella query seguente, l'espressione di percorso specifica un carattere jolly (`*`) in un test di nodo.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Il carattere jolly viene specificato per il nome di nodo. Quindi, la query restituisce tutti gli elementi figlio del nodo elemento dell'<`Features`> nodo elemento figlio del `ProductDescription` nodo elemento <>.  
  
 La query seguente è simile alla precedente tranne per il fatto che, insieme al carattere jolly, viene specificato uno spazio dei nomi. Di conseguenza, vengono restituiti tutti i nodi elemento figlio in tale spazio dei nomi. Si noti che l' `Features` elemento <> può contenere elementi di spazi dei nomi diversi.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 È possibile utilizzare il carattere jolly come prefisso dello spazio dei nomi, come illustrato nella query seguente:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*:Maintenance  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Questa query restituisce il <`Maintenance`> nodo elementi figlio in tutti gli spazi dei nomi dal documento XML del catalogo prodotti.  
  
### <a name="c-specifying-node-kind-in-the-node-test"></a>C. Definizione del tipo di nodo nel test di nodo  
 Nell'esempio seguente viene specificato il tipo di nodo come test di nodo in tutte le espressioni di percorso. Di conseguenza, tutte le espressioni restituiscono nodi del tipo specificato nel test di nodo.  
  
 Nella query seguente, l'espressione di percorso specifica un tipo di nodo nel terzo passo:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::text()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Nella query successiva, viene specificato quanto segue:  
  
-   L'espressione di percorso include tre passi separati da una barra (`/`).  
  
-   Ognuno di questi passi specifica un asse child.  
  
-   I primi due passi specificano un nome di nodo come test di nodo, mentre il terzo passo specifica un tipo di nodo come test di nodo.  
  
-   L'espressione restituisce gli elementi figlio del nodo di testo della <`Features` elemento figlio> del `ProductDescription` nodo elemento> <.  
  
 Viene restituito solo un nodo di testo. Risultato:  
  
```  
These are the product highlights.   
```  
  
 La query seguente restituisce gli elementi figlio del nodo di commento dell' `ProductDescription` elemento <>:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::comment()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il secondo passo specifica un tipo di nodo come test di nodo.  
  
-   Di conseguenza, l'espressione restituisce gli elementi figlio del nodo di commento dell'<`ProductDescription`> nodi dell'elemento.  
  
 Risultato:  
  
```  
<!-- add one or more of these elements... one for each specific product in this product model -->  
<!-- add any tags in <specifications> -->      
```  
  
 La query seguente recupera i nodi di istruzioni di elaborazione di livello principale:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Risultato:  
  
```  
<?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>   
```  
  
 È possibile passare un parametro letterale stringa al test di nodo `processing-instruction()`. In tal caso, la query restituisce le istruzioni di elaborazione per le quali il valore dell'attributo name è il valore letterale stringa specificato nell'argomento.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction("xml-stylesheet")  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
## <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   Non sono supportati i test di nodo SequenceType estesi.  
  
-   Non è supportata la sintassi processing-instruction(name). In alternativa, è possibile inserire il nome tra virgolette.  
  
  
