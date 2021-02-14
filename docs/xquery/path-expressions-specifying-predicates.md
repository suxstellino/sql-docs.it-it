---
title: Specifica di predicati in un passaggio dell'espressione di percorso | Microsoft Docs
description: Informazioni su come specificare i predicati nel passaggio dell'asse di un'espressione di percorso XQuery per filtrare una sequenza di nodi XML.
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
- predicates [XQuery]
- qualifiers [XQuery]
- path expressions [XQuery]
ms.assetid: 2660ceca-b8b4-4a1f-98a0-719ad5f89f81
author: rothja
ms.author: jroth
ms.openlocfilehash: ef2625be70c2af44f4a45ba00302b3d26afc101d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339946"
---
# <a name="path-expressions---specifying-predicates"></a>Espressioni di percorso - Specifica predicati
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Come descritto nell'argomento relativo alle [espressioni di percorso in XQuery](../xquery/path-expressions-xquery.md), un passaggio dell'asse in un'espressione di percorso include i componenti seguenti:  
  
-   [Un asse](../xquery/path-expressions-specifying-axis.md).  
  
-   Un test del nodo. Per altre informazioni, vedere [specifica del test di nodo in un passaggio dell'espressione di percorso](../xquery/path-expressions-specifying-node-test.md).  
  
-   Zero o più predicati Questo indirizzo è facoltativo.  
  
 Il predicato facoltativo rappresenta la terza parte del passo dell'asse in un'espressione di percorso.  
  
## <a name="predicates"></a>Predicati  
 Per filtrare una sequenza di nodi applicando un test specifico, è possibile utilizzare un predicato. L'espressione del predicato è racchiusa tra parentesi quadre ed è associata all'ultimo nodo di un'espressione di percorso.  
  
 Si supponga, ad esempio, che venga dichiarato un valore di parametro SQL (x) del tipo di dati **XML** , come illustrato nell'esempio seguente:  
  
```  
declare @x xml  
set @x = '  
<People>  
  <Person>  
    <Name>John</Name>  
    <Age>24</Age>  
  </Person>  
  <Person>  
    <Name>Goofy</Name>  
    <Age>54</Age>  
  </Person>  
  <Person>  
    <Name>Daffy</Name>  
    <Age>30</Age>  
  </Person>  
</People>  
'  
```  
  
 In questo caso, le espressioni seguenti che utilizzano un valore del predicato [1] in tre livelli del nodo diversi sono valide:  
  
```  
select @x.query('/People/Person/Name[1]')  
select @x.query('/People/Person[1]/Name')  
select @x.query('/People[1]/Person/Name')  
```  
  
 Si noti che in ognuno dei casi, il predicato viene associato al nodo dell'espressione di percorso nella quale è applicato. Ad esempio, la prima espressione di percorso seleziona il primo <`Name` elemento> all'interno di ogni nodo/People/Person e, con l'istanza XML specificata, restituisce quanto segue:  
  
```  
<Name>John</Name><Name>Goofy</Name><Name>Daffy</Name>  
```  
  
 Tuttavia, la seconda espressione di percorso seleziona tutti `Name` gli elementi <> sotto il primo nodo/People/Person. Viene pertanto restituito quando segue:  
  
```  
<Name>John</Name>  
```  
  
 È inoltre possibile modificare l'ordine di valutazione del predicato utilizzando le parentesi. Ad esempio, nell'espressione seguente viene utilizzato un set di parentesi per separare il percorso (/People/Person/Name) dal predicato [1]:  
  
```  
select @x.query('(/People/Person/Name)[1]')  
```  
  
 Nell'esempio, viene modificato l'ordine in base al quale è applicato il predicato perché prima viene valutato il percorso tra parentesi (/People/Person/Name) e quindi l'operatore [1] del predicato viene applicato al set contenente tutti i nodi corrispondenti a tale percorso. L'ordine delle operazioni sarebbe diverso senza le parentesi, perché l'operatore [1] verrebbe applicato come un nodo del test `child::Name`, in modo similare al primo esempio di espressione di percorso.  
  
### <a name="quantifiers-and-predicates"></a>Quantificatori e predicati  
 All'interno delle parentesi graffe del predicato è possibile utilizzare e aggiungere più quantificatori. Di seguito è riportato un esempio di utilizzo valido di più quantificatori in una sottoespressione con un predicato complesso, basato sull'esempio precedente.  
  
```  
select @x.query('/People/Person[contains(Name[1], "J") and xs:integer(Age[1]) < 40]/Name/text()')  
```  
  
 Il risultato di un'espressione del predicato viene convertito in un valore booleano ed è definito valore di verità del predicato. Nel risultato vengono restituiti unicamente i nodi della sequenza per i quali il valore di verità del predicato è True. Tutti gli altri nodi vengono invece scartati.  
  
 Ad esempio, l'espressione di percorso seguente include un predicato nel secondo passo:  
  
```  
/child::root/child::Location[attribute::LocationID=10]  
```  
  
 La condizione specificata dal predicato viene applicata a tutti gli elementi `Location` figlio del nodo dell'elemento <>. Vengono pertanto restituiti solo i centri di lavorazione il cui valore dell'attributo LocationID è 10.  
  
 L'espressione di percorso precedente viene eseguita nell'istruzione SELECT seguente:  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
 /child::AWMI:root/child::AWMI:Location[attribute::LocationID=10]  
')  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
### <a name="computing-predicate-truth-values"></a>Calcolo dei valori di verità del predicato  
 Per stabilire il valore di verità del predicato vengono applicate le regole seguenti, in base alle specifiche XQuery:  
  
1.  Se il valore dell'espressione del predicato è una sequenza vuota, il valore di verità del predicato è False.  
  
     Ad esempio:  
  
    ```  
    SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     /child::AWMI:root/child::AWMI:Location[attribute::LotSize]  
    ')  
    FROM Production.ProductModel  
    WHERE ProductModelID=7  
    ```  
  
     L'espressione di percorso in questa query restituisce solo i `Location` nodi <> elemento per i quali è stato specificato un attributo LotSize. Se il predicato restituisce una sequenza vuota per uno specifico <`Location`>, il centro di lavorazione non viene restituito nel risultato.  
  
2.  I valori dei predicati possono essere solo XS: Integer, XS: Boolean o Node \* . Per Node \* , il predicato restituisce true se sono presenti nodi e false per una sequenza vuota. Qualsiasi altro tipo numerico, ad esempio double e float, genera un errore di tipizzazione statica. Il valore di verità del predicato di un'espressione è True se e solo se il valore di tipo integer risultante è uguale al valore della posizione del contesto. Inoltre, solo i valori letterali integer e la funzione **Last ()** riducono la cardinalità dell'espressione Step filtrata su 1.  
  
     Ad esempio, la query seguente recupera il terzo nodo elemento figlio dell'elemento <`Features`>.  
  
    ```  
    SELECT CatalogDescription.query('  
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /child::PD:ProductDescription/child::PD:Features/child::*[3]  
    ')  
    FROM Production.ProductModel  
    WHERE ProductModelID=19  
    ```  
  
     Dalla query precedente si noti quanto segue:  
  
    -   Il terzo passo nell'espressione specifica un'espressione del predicato il cui valore è 3. Il valore di verità del predicato di questa espressione è pertanto True solo per i nodi la cui posizione del contesto è 3.  
  
    -   Nel terzo passo viene inoltre specificato un carattere jolly (*) che indica tutti i nodi nel test dei nodi. Il predicato filtra tuttavia i nodi e restituisce solo il nodo nella terza posizione.  
  
    -   La query restituisce il terzo nodo elemento figlio della <`Features`> elementi figlio del <elemento figlio `ProductDescription`> della radice del documento.  
  
3.  Se il valore dell'espressione predicato è un valore semplice booleano, il valore di verità del predicato è uguale al valore dell'espressione predicato.  
  
     Ad esempio, la query seguente viene specificata in base a una variabile di tipo **XML** che include un'istanza XML, l'istanza XML del sondaggio del cliente. La query recupera i clienti con figli. In questa query, corrisponde a \<HasChildren> 1 \</HasChildren> .  
  
    ```  
    declare @x xml  
    set @x='  
    <Survey>  
      <Customer CustomerID="1" >  
      <Age>27</Age>  
      <Income>20000</Income>  
      <HasChildren>1</HasChildren>  
      </Customer>  
      <Customer CustomerID="2" >  
      <Age>27</Age>  
      <Income>20000</Income>  
      <HasChildren>0</HasChildren>  
      </Customer>  
    </Survey>  
    '  
    declare @y xml  
    set @y = @x.query('  
      for $c in /child::Survey/child::Customer[( child::HasChildren[1] cast as xs:boolean ? )]  
      return   
          <CustomerWithChildren>  
              { $c/attribute::CustomerID }  
          </CustomerWithChildren>  
    ')  
    select @y  
    ```  
  
     Dalla query precedente si noti quanto segue:  
  
    -   L'espressione nel ciclo **for** prevede due passaggi e il secondo passaggio specifica un predicato. il cui valore è di tipo booleano. Se il valore è True, anche il valore di verità del predicato è True.  
  
    -   La query restituisce gli elementi `Customer` figlio di <>, il cui valore del predicato è true, degli \<Survey> elementi figlio della radice del documento. Risultato:  
  
        ```  
        <CustomerWithChildren CustomerID="1"/>   
        ```  
  
4.  Se il valore dell'espressione predicato è una sequenza che contiene almeno un nodo, il valore di verità del predicato è True.  
  
 Ad esempio, la query seguente recupera ProductModelID per i modelli di prodotto la cui descrizione del catalogo XML include almeno una funzionalità, un elemento figlio dell' `Features` elemento <>, dallo spazio dei nomi associato al prefisso **WM** .  
  
```  
SELECT ProductModelID  
FROM   Production.ProductModel  
WHERE CatalogDescription.exist('  
             declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
             /child::PD:ProductDescription/child::PD:Features[wm:*]  
             ') = 1  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La clausola WHERE specifica il [metodo exist () (tipo di dati XML)](../t-sql/xml/exist-method-xml-data-type.md).  
  
-   L'espressione di percorso all'interno del metodo **exist ()** specifica un predicato nel secondo passaggio. Se l'espressione del predicato restituisce una sequenza di almeno una caratteristica, il valore di verità dell'espressione del predicato è True. In questo caso, poiché il metodo **exist ()** restituisce true, viene restituito ProductModelID.  
  
## <a name="static-typing-and-predicate-filters"></a>Tipizzazione statica e filtri del predicato  
 I predicati possono inoltre influire sul tipo di un'espressione derivato staticamente. I valori letterali integer e la funzione **Last ()** riducono la cardinalità dell'espressione Step filtrata al massimo uno.  
  
  
