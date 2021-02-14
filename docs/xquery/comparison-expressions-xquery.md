---
title: Espressioni di confronto (XQuery) | Microsoft Docs
description: Informazioni su come usare le espressioni di confronto XQuery che contengono gli operatori generale, valore, nodo e ordine del nodo.
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
- node comparison operators [XQuery]
- comparison expressions [XQuery]
- node order comparison operators [XQuery]
- expressions [XQuery], comparison
- comparison operators [XQuery]
- value comparison operators
ms.assetid: dc671348-306f-48ef-9e6e-81fc3c7260a6
author: rothja
ms.author: jroth
ms.openlocfilehash: 4824cd52001305a05c00e197e8c4140598267d4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349339"
---
# <a name="comparison-expressions-xquery"></a>Espressioni di confronto (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery include i tipi di operatori di confronto seguenti:  
  
-   Operatori di confronto generali  
  
-   Operatori di confronto dei valori  
  
-   Operatore di confronto dei nodi  
  
-   Operatori di confronto dell'ordine dei nodi  
  
## <a name="general-comparison-operators"></a>Operatori di confronto generali  
 È possibile utilizzare gli operatori di confronto generali per confrontare valori atomici, sequenze o una combinazione dei due.  
  
 Gli operatori generali sono indicati nella tabella seguente.  
  
|Operatore|Descrizione|  
|--------------|-----------------|  
|=|Uguale a|  
|!=|Diverso da|  
|\<|Minore di|  
|>|Maggiore di|  
|\<=|Minore o uguale a|  
|>=|Maggiore o uguale a|  
  
 Quando si confrontano due sequenze utilizzando gli operatori di confronto generali e la seconda sequenza include un valore che risulta True in base al confronto con un valore della prima sequenza, il risultato complessivo è True. Negli altri casi, il risultato è False. Ad esempio, il risultato di (1, 2, 3) = (3, 4) è True, in quanto il valore 3 è incluso in entrambe le sequenze.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3) = (3,4)')    
```  
  
 Il confronto prevede che i valori siano paragonabili. In particolare, vengono sottoposti al controllo statico. Per i confronti numerici, può verificarsi la promozione del tipo numeric. Se ad esempio il valore decimal 10 viene confrontato con il valore double 1e1, il valore decimal viene convertito in double. Si noti che tale operazione potrebbe generare risultati imprecisi, in quanto i confronti tra valori double non possono essere esatti.  
  
 Se uno dei valori non è tipizzato, viene eseguito il relativo cast al tipo dell'altro valore. Nell'esempio seguente, il valore 7 viene considerato come valore intero. Prima del confronto, il valore /a[1] non tipizzato viene convertito in valore intero. Il confronto tra valori interi restituisce True.  
  
```  
declare @x xml  
set @x='<a>6</a>'  
select @x.query('/a[1] < 7')  
```  
  
 Se invece il valore non tipizzato viene confrontato con una stringa o con un altro valore non tipizzato, verrà eseguito il relativo cast a xs:string. Nella query seguente, la stringa 6 viene confrontata con la stringa "17". Tale query restituisce False, a causa del confronto tra stringhe.  
  
```  
declare @x xml  
set @x='<a>6</a>'  
select @x.query('/a[1] < "17"')  
```  
  
 La query seguente restituisce foto di piccole dimensioni di un modello di prodotto derivato dal catalogo prodotti disponibile nel database di esempio AdventureWorks. La query confronta una sequenza di valori atomici restituiti da `PD:ProductDescription/PD:Picture/PD:Size` con la sequenza singleton "small". Se il confronto è true, restituisce l'elemento immagine <\> .  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
SELECT CatalogDescription.query('         
    for $P in /PD:ProductDescription/PD:Picture[PD:Size = "small"]         
    return $P') as Result         
FROM   Production.ProductModel         
WHERE  ProductModelID=19         
```  
  
 Nella query seguente viene confrontata una sequenza di numeri di telefono negli elementi <numero \> al valore letterale stringa "112-111-1111". La query confronta la sequenza di elementi dei numeri di telefono nella colonna AdditionalContactInfo per determinare se il documento include un numero di telefono specifico di un determinato cliente.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.value('         
   /aci:AdditionalContactInfo//act:telephoneNumber/act:number = "112-111-1111"', 'nvarchar(10)') as Result         
FROM Person.Contact         
WHERE ContactID=1         
```  
  
 La query restituisce True. Questo valore indica che il documento contiene tale numero. La query seguente è una versione leggermente modificata della query precedente. In questa query, i valori dei numeri di telefono recuperati dal documento vengono confrontati con una sequenza costituita da due numeri telefonici. Se il confronto è true, \> viene restituito l'elemento <Number.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('         
  if (/aci:AdditionalContactInfo//act:telephoneNumber/act:number = ("222-222-2222","112-111-1111"))         
  then          
     /aci:AdditionalContactInfo//act:telephoneNumber/act:number         
  else         
    ()') as Result         
FROM Person.Contact         
WHERE ContactID=1  
  
```  
  
 Risultato:  
  
```  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
    111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
    112-111-1111  
\</act:number>   
```  
  
## <a name="value-comparison-operators"></a>Operatori di confronto dei valori  
 Gli operatori di confronto dei valori vengono utilizzati per confrontare valori atomici. Si noti che nelle query è possibile utilizzare gli operatori di confronto generali anziché gli operatori di confronto dei valori.  
  
 Gli operatori di confronto dei valori sono indicati nella tabella seguente.  
  
|Operatore|Descrizione|  
|--------------|-----------------|  
|eq|Uguale a|  
|ne|Diverso da|  
|lt|Minore di|  
|gt|Maggiore di|  
|le|Minore o uguale a|  
|ge|Maggiore o uguale a|  
  
 Se i due valori confrontati soddisfano entrambi la condizione dell'operatore scelto, l'espressione restituirà True. In caso contrario, restituirà False. Se uno dei valori è una sequenza vuota, il risultato dell'espressione è False.  
  
 È possibile utilizzare questi operatori solo su valori atomici singleton. Pertanto, non è possibile specificare una sequenza come uno degli operandi.  
  
 Ad esempio, la query seguente recupera \<Picture> gli elementi per un modello di prodotto in cui le dimensioni dell'immagine sono "Small":  
  
```  
SELECT CatalogDescription.query('         
              declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";         
              for $P in /PD:ProductDescription/PD:Picture[PD:Size eq "small"]         
              return         
                    $P         
             ') as Result         
FROM Production.ProductModel         
WHERE ProductModelID=19         
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   `declare namespace` definisce il prefisso dello spazio dei nomi utilizzato successivamente nella query.  
  
-   Il \<Size> valore dell'elemento viene confrontato con il valore atomico specificato, ovvero "Small".  
  
-   Si noti che poiché gli operatori value funzionano solo su valori atomici, la funzione **Data ()** viene utilizzata in modo implicito per recuperare il valore del nodo. Pertanto, `data($P/PD:Size) eq "small"` restituisce lo stesso risultato.  
  
 Risultato:  
  
```  
\<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  \<PD:Angle>front\</PD:Angle>  
  \<PD:Size>small\</PD:Size>  
  \<PD:ProductPhotoID>31\</PD:ProductPhotoID>  
\</PD:Picture>  
```  
  
 Si noti che le regole per la promozione dei tipi sono identiche a quelle utilizzate per i confronti generali. Durante i confronti tra valori, inoltre, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizza le stesse regole di cast per i valori non tipizzati utilizzate durante i confronti generali. Le regole delle specifiche XQuery, invece, prevedono sempre l'esecuzione del cast del valore non tipizzato a xs:string durante i confronti tra valori.  
  
## <a name="node-comparison-operator"></a>Operatore di confronto dei nodi  
 L'operatore di confronto dei nodi, **is**, si applica solo ai tipi di nodo. Il risultato restituito indica se due nodi passati come operandi rappresentano lo stesso nodo nel documento di origine. Questo operatore restituisce True se i due operandi rappresentano lo stesso nodo. In caso contrario, restituisce False.  
  
 La query seguente controlla se il centro di lavorazione 10 è il primo nell'ambito del processo di produzione di un modello di prodotto specifico.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' AS AWMI)  
  
SELECT ProductModelID, Instructions.query('         
    if (  (//AWMI:root/AWMI:Location[@LocationID=10])[1]         
          is          
          (//AWMI:root/AWMI:Location[1])[1] )          
    then         
          <Result>equal</Result>         
    else         
          <Result>Not-equal</Result>         
         ') as Result         
FROM Production.ProductModel         
WHERE ProductModelID=7           
```  
  
 Risultato:  
  
```  
ProductModelID       Result          
-------------- --------------------------  
7              <Result>equal</Result>      
```  
  
## <a name="node-order-comparison-operators"></a>Operatori di confronto dell'ordine dei nodi  
 Gli operatori di confronto dell'ordine dei nodi confrontano coppie di nodi in base alle relative posizioni in un documento.  
  
 I confronti eseguiti, basati sull'ordine dei nodi all'interno del documento, sono i seguenti:  
  
-   `<<` : L' **operando 1** precede l' **operando 2** nell'ordine del documento.  
  
-   `>>` : L' **operando 1** segue l' **operando 2** nell'ordine del documento.  
  
 La query seguente restituisce true se la descrizione del catalogo dei prodotti ha l' \<Warranty> elemento visualizzato prima dell' \<Maintenance> elemento nell'ordine del documento per un determinato prodotto.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM)  
  
SELECT CatalogDescription.value('  
     (/PD:ProductDescription/PD:Features/WM:Warranty)[1] <<   
           (/PD:ProductDescription/PD:Features/WM:Maintenance)[1]', 'nvarchar(10)') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il metodo **value ()** del tipo di dati **XML** viene utilizzato nella query.  
  
-   Il risultato booleano della query viene convertito in **nvarchar (10)** e restituito.  
  
-   La query restituisce True.  
  
## <a name="see-also"></a>Vedere anche  
 [Sistema di tipi &#40;XQuery&#41;](../xquery/type-system-xquery.md)   
 [Espressioni XQuery](../xquery/xquery-expressions.md)  
  
  
