---
title: Impostazione dell'asse in un passaggio dell'espressione di percorso | Microsoft Docs
description: Informazioni su come specificare un passaggio dell'asse in un'espressione di percorso XQuery.
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
- attribute axis [SQL Server]
- axis step [XQuery]
- descendant axis
- self axis
- path expressions [XQuery]
- child axis
- descendant-or-self axis
- parent axis
ms.assetid: c44fb843-0626-4496-bde0-52ca0bac0a9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2207dcce2787fb6f51668e777b01ce345d1f33ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344663"
---
# <a name="path-expressions---specifying-axis"></a>Espressioni di percorso - Specifica asse
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Ogni passo dell'asse in un'espressione di percorso include i componenti seguenti:  
  
-   Un asse  
  
-   Un [test di nodo](../xquery/path-expressions-specifying-node-test.md)  
  
-   [Zero o più predicati (facoltativo)](../xquery/path-expressions-specifying-predicates.md)  
  
 Per altre informazioni, vedere [espressioni di percorso &#40;&#41;XQuery ](../xquery/path-expressions-xquery.md).  
  
 L'implementazione di XQuery in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta i passi dell'asse seguenti:  
  
|Asse|Descrizione|  
|----------|-----------------|  
|**figlio**|Restituisce gli elementi figlio del nodo di contesto.|  
|**descendant**|Restituisce tutti i discendenti del nodo di contesto.|  
|**parent**|Restituisce l'elemento padre del nodo di contesto.|  
|**attributo**|Restituisce gli attributi del nodo di contesto.|  
|**auto**|Restituisce il nodo di contesto stesso.|  
|**descendant-or-self**|Restituisce il nodo di contesto e tutti i relativi discendenti.|  
  
 Tutti questi assi, ad eccezione dell'asse **padre** , sono assi di avanzamento. L'asse **padre** è un asse inverso, perché esegue la ricerca all'indietro nella gerarchia del documento. Ad esempio, l'espressione di percorso relativo `child::ProductDescription/child::Summary` include due passi, ognuno dei quali specifica un asse `child`. Il primo passaggio recupera gli \<ProductDescription> elementi figlio del nodo di contesto. Per ogni \<ProductDescription> nodo elemento, il secondo passaggio recupera gli elementi \<Summary> figlio del nodo elemento.  
  
 L'espressione di percorso relativo `child::root/child::Location/attribute::LocationID` include tre passi. I primi due passi specificano ognuno un asse `child`, mentre il terzo passo specifica l'asse `attribute`. Quando viene eseguito sui documenti XML delle istruzioni di produzione nella tabella **Production. ProductModel** , l'espressione restituisce l' `LocationID` attributo del \<Location> nodo elemento figlio dell' \<root> elemento.  
  
## <a name="examples"></a>Esempio  
 Gli esempi di query in questo argomento vengono specificati per le colonne di tipo **XML** nel database **AdventureWorks** .  
  
### <a name="a-specifying-a-child-axis"></a>R. Definizione di un asse child  
 Per un modello di prodotto specifico, la query seguente recupera i \<Features> nodi figlio del \<ProductDescription> nodo elemento dalla descrizione del catalogo dei prodotti archiviati nella `Production.ProductModel` tabella.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il `query()` metodo del tipo di dati **XML** specifica l'espressione di percorso.  
  
-   Entrambi i passi dell'espressione di percorso specificano un asse `child` e i nomi dei nodi, `ProductDescription` e `Features`, come test di nodo. Per informazioni sui test di nodo, vedere [specifica del test di nodo in un passaggio dell'espressione di percorso](../xquery/path-expressions-specifying-node-test.md).  
  
### <a name="b-specifying-descendant-and-descendant-or-self-axes"></a>B. Definizione di assi descendant e descendant-or-self  
 Nell'esempio seguente vengono utilizzati assi descendant e descendant-or-self. La query in questo esempio viene specificata in base a una variabile di tipo **XML** . L'istanza XML è semplificata per illustrare in modo chiaro la differenza nei risultati generati.  
  
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
declare @y xml  
set @y = @x.query('  
  /child::a/child::b  
')  
select @y  
```  
  
 Nel risultato seguente, l'espressione restituisce il nodo elemento figlio `<b>` del nodo elemento `<a>`:  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
```  
  
 Se in questa espressione si specifica un asse descendant per l'espressione di percorso,  
  
 `/child::a/child::b/descendant::*`, viene richiesto l'invio di tutti i discendenti del `b` nodo <> elemento.  
  
 L'asterisco (*) nel test di nodo rappresenta il nome del nodo come test di nodo. Pertanto, il tipo di nodo primario dell'asse descendant, il nodo elemento, determina i tipi di nodi restituiti. L'espressione restituisce quindi tutti i nodi elemento. I nodi di testo non vengono restituiti. Per ulteriori informazioni sul tipo di nodo primario e la relativa relazione con il test di nodo, vedere l'argomento relativo alla [specifica del test di nodo in un passaggio dell'espressione di percorso](../xquery/path-expressions-specifying-node-test.md) .  
  
 Vengono restituiti i nodi degli elementi <`c`> e <`d`>, come illustrato nel risultato seguente:  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Se si specifica un asse descendant-or-self anziché l'asse descendant, `/child::a/child::b/descendant-or-self::*` restituisce il nodo di contesto, l'elemento <`b`> e il relativo discendente.  
  
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
  
 La query di esempio seguente sul database **AdventureWorks** recupera tutti i nodi elemento discendente del <elemento `Features` figlio> dell'elemento <`ProductDescription`>:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features/descendant::*  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
### <a name="c-specifying-a-parent-axis"></a>C. Definizione di un asse parent  
 La query seguente restituisce il <`Summary` elemento figlio> dell'elemento <`ProductDescription`> nel documento XML del catalogo prodotti archiviato nella `Production.ProductModel` tabella.  
  
 In questo esempio viene usato l'asse padre per tornare all'elemento padre dell' `Feature` elemento <> e recuperare il <`Summary` elemento figlio> dell' `ProductDescription` elemento <>.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
/child::PD:ProductDescription/child::PD:Features/parent::PD:ProductDescription/child::PD:Summary  
')  
FROM   Production.ProductModel  
WHERE  ProductModelID=19  
  
```  
  
 In questo esempio di query, l'espressione di percorso utilizza l'asse `parent`. È possibile riscrivere l'espressione senza l'asse parent, come illustrato di seguito:  
  
```  
/child::PD:ProductDescription[child::PD:Features]/child::PD:Summary  
```  
  
 Un esempio più dettagliato dell'asse parent è il seguente.  
  
 Ogni descrizione del catalogo dei modelli di prodotto archiviata nella colonna **CatalogDescription** della tabella **ProductModel** include un `<ProductDescription>` elemento con `ProductModelID` l'attributo e l' `<Features>` elemento figlio, come illustrato nel frammento seguente:  
  
```  
<ProductDescription ProductModelID="..." >  
  ...  
  <Features>  
    <Feature1>...</Feature1>  
    <Feature2>...</Feature2>  
   ...  
</ProductDescription>  
```  
  
 La query imposta la variabile iteratore `$f` nell'istruzione FLWOR per restituire gli elementi figli dell'elemento `<Features>`. Per altre informazioni, vedere [istruzione FLWOR e iterazione &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md). Per ogni caratteristica, la clausola `return` costruisce un'istanza XML nel formato seguente:  
  
```  
<Feature ProductModelID="...">...</Feature>  
<Feature ProductModelID="...">...</Feature>  
```  
  
 Per aggiungere `ProductModelID` per ogni `<Feature` elemento>, `parent` viene specificato l'asse:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  for $f in /child::PD:ProductDescription/child::PD:Features/child::*  
  return  
   <Feature  
     ProductModelID="{ ($f/parent::PD:Features/parent::PD:ProductDescription/attribute::ProductModelID)[1]}" >  
          { $f }  
   </Feature>  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Risultato parziale:  
  
```  
<Feature ProductModelID="19">  
  <wm:Warranty   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
    <wm:Description>parts and labor</wm:Description>  
  </wm:Warranty>  
</Feature>  
<Feature ProductModelID="19">  
  <wm:Maintenance   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:NoOfYears>10 years</wm:NoOfYears>  
    <wm:Description>maintenance contract available through your dealer   
                  or any AdventureWorks retail store.</wm:Description>  
  </wm:Maintenance>  
</Feature>  
<Feature ProductModelID="19">  
  <p1:wheel   
   xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">  
      High performance wheels.  
  </p1:wheel>  
</Feature>  
```  
  
 Si noti che il predicato `[1]` nell'espressione di percorso viene aggiunto per assicurare che venga restituito un valore singleton.  
  
  
