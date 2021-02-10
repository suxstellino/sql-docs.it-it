---
description: Clausola COMPUTE di Shape
title: Clausola COMPUTE di Shape | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- compute clause [ADO]
- data shaping [ADO], COMPUTE clause
ms.assetid: 3fdfead2-b5ab-4163-9b1d-3d2143a5db8c
author: rothja
ms.author: jroth
ms.openlocfilehash: b246b1e70f18836455008f8d9e3c1b972e71f183
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037001"
---
# <a name="shape-compute-clause"></a>Clausola COMPUTE di Shape
Una clausola COMPUTE di forma genera un **Recordset** padre, le cui colonne sono costituite da un riferimento al **Recordset** figlio. colonne facoltative il cui contenuto è il capitolo, il nuovo o le colonne calcolate oppure il risultato dell'esecuzione di funzioni di aggregazione sul **Recordset** figlio o su un **Recordset** precedentemente definito. e tutte le colonne del **Recordset** figlio elencate nella clausola facoltativa by.  
  
## <a name="syntax"></a>Sintassi  
  
```  
SHAPE child-command [AS] child-alias  
   COMPUTE child-alias [[AS] name], [appended-column-list]  
   [BY grp-field-list]  
```  
  
## <a name="description"></a>Descrizione  
 Le parti di questa clausola sono le seguenti:  
  
 *comando figlio*  
 È costituito da uno dei seguenti elementi:  
  
-   Comando di query racchiuso tra parentesi graffe (" {} ") che restituisce un oggetto **Recordset** figlio. Il comando viene emesso al provider di dati sottostante e la relativa sintassi dipende dai requisiti del provider. Si tratta in genere del linguaggio SQL, sebbene ADO non richieda un linguaggio di query specifico.  
  
-   Nome di un **Recordset** con forma esistente.  
  
-   Un altro comando Shape.  
  
-   Parola chiave TABLE, seguita dal nome di una tabella nel provider di dati.  
  
 *alias figlio*  
 Alias usato per fare riferimento al **Recordset** restituito dal *comando figlio.* L' *alias figlio* è obbligatorio nell'elenco di colonne nella clausola COMPUTE e definisce la relazione tra gli oggetti **Recordset** padre e figlio.  
  
 *accodato-elenco colonne*  
 Elenco in cui ogni elemento definisce una colonna nell'elemento padre generato. Ogni elemento contiene una colonna del capitolo, una nuova colonna, una colonna calcolata o un valore risultante da una funzione di aggregazione nel **Recordset** figlio.  
  
 *GRP-Field-List*  
 Elenco di colonne negli oggetti **Recordset** padre e figlio che specifica la modalità di raggruppamento delle righe nell'elemento figlio.  
  
 Per ogni colonna nell' *elenco GRP-Field-List* è presente una colonna corrispondente negli oggetti **Recordset** figlio e padre. Per ogni riga del **Recordset** padre, le colonne *GRP-Field-List* hanno valori univoci e il **Recordset** figlio a cui fa riferimento la riga padre è costituito esclusivamente da righe figlio le cui colonne *GRP-Field-List* hanno gli stessi valori della riga padre.  
  
 Se la clausola BY è inclusa, le righe del **Recordset** figlio verranno raggruppate in base alle colonne nella clausola COMPUTE. Il **Recordset** padre conterrà una riga per ogni gruppo di righe nel **Recordset** figlio.  
  
 Se la clausola BY viene omessa, l'intero **Recordset** figlio viene considerato come un singolo gruppo e il **Recordset** padre conterrà esattamente una riga. Tale riga fa riferimento all'intero **Recordset** figlio. Se si omette la clausola BY, è possibile calcolare le aggregazioni "totale complessivo" sull'intero **Recordset** figlio.  
  
 Ad esempio:  
  
```  
SHAPE {select * from Orders} AS orders             COMPUTE orders, SUM(orders.OrderAmount) as TotalSales         
```  
  
 Indipendentemente dal modo in cui viene formato il **Recordset** padre (utilizzando il calcolo o l'utilizzo di Append), contiene una colonna del capitolo utilizzata per correlarla a un **Recordset** figlio. Se si desidera, è possibile che il **Recordset** padre includa anche colonne che contengono aggregazioni (Sum, min, Max e così via) sulle righe figlio. Sia l'elemento padre che il **Recordset** figlio possono contenere colonne che contengono un'espressione nella riga del **Recordset**, oltre a colonne nuove e inizialmente vuote.  
  
## <a name="operation"></a>Operazione  
 Il *comando figlio* viene emesso al provider, che restituisce un **Recordset** figlio.  
  
 La clausola COMPUTE specifica le colonne del **Recordset** padre, che può essere un riferimento al **Recordset** figlio, una o più aggregazioni, un'espressione calcolata o nuove colonne. Se è presente una clausola BY, le colonne definite vengono aggiunte anche al **Recordset** padre. La clausola BY specifica il modo in cui vengono raggruppate le righe del **Recordset** figlio.  
  
 Si supponga, ad esempio, di disporre di una tabella, denominata demografia, costituita da campi stato, città e popolazione. (Le figure della popolazione nella tabella sono fornite esclusivamente come esempio).  
  
|State|City|Popolazione|  
|-----------|----------|----------------|  
|WA|Seattle|700.000|  
|OR|Medford|200.000|  
|OR|Portland|400.000|  
|CA|Los Angeles|800.000|  
|CA|San Diego|600.000|  
|WA|Tacoma|500.000|  
|OR|Corvallis|300.000|  
  
 A questo punto, eseguire il comando Shape:  
  
```  
rst.Open  "SHAPE {select * from demographics} AS rs "  & _  
          "COMPUTE rs, SUM(rs.population) BY state", _  
           objConnection  
```  
  
 Questo comando apre un **Recordset** con forma a due livelli. Il livello padre è un **Recordset** generato con una colonna di aggregazione ( `SUM(rs.population)` ), una colonna che fa riferimento al **Recordset** figlio ( `rs` ) e una colonna per il raggruppamento del **Recordset** figlio ( `state` ). Il livello figlio è il **Recordset** restituito dal comando di query ( `select * from demographics` ).  
  
 Le righe di dettaglio del **Recordset** figlio verranno raggruppate in base allo stato, ma in caso contrario in nessun ordine particolare. Ovvero, i gruppi non saranno in ordine alfabetico o numerico. Se si desidera che il **Recordset** padre venga ordinato, è possibile utilizzare il metodo di **ordinamento del recordset** per ordinare il **Recordset** padre.  
  
 È ora possibile esplorare il **Recordset** padre aperto e accedere agli oggetti **Recordset** di dettaglio figlio. Per ulteriori informazioni, vedere [accesso alle righe in un recordset gerarchico](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="resultant-parent-and-child-detail-recordsets"></a>Recordset di dettagli padre e figlio risultanti  
  
### <a name="parent"></a>Padre  
  
|SUM (RS. Popolazione|rs|State|  
|---------------------------|--------|-----------|  
|1,3 milioni|Riferimento a child1|CA|  
|1,2 milioni|Riferimento a child2|WA|  
|1,1 milioni|Riferimento a child3|OR|  
  
## <a name="child1"></a>Child1  
  
|State|City|Popolazione|  
|-----------|----------|----------------|  
|CA|Los Angeles|800.000|  
|CA|San Diego|600.000|  
  
## <a name="child2"></a>Child2  
  
|State|City|Popolazione|  
|-----------|----------|----------------|  
|WA|Seattle|700.000|  
|WA|Tacoma|500.000|  
  
## <a name="child3"></a>Child3  
  
|State|City|Popolazione|  
|-----------|----------|----------------|  
|OR|Medford|200.000|  
|OR|Portland|400.000|  
|OR|Corvallis|300.000|  
  
## <a name="see-also"></a>Vedere anche  
 [Accesso alle righe in un recordset gerarchico](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [Cenni preliminari sulla data shaping](../../../ado/guide/data/data-shaping-overview.md)   
 [Field (oggetto)](../../../ado/reference/ado-api/field-object.md)   
 [Grammatica forma formale](../../../ado/guide/data/formal-shape-grammar.md)   
 [Oggetto recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Provider richiesti per la definizione dei dati](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Clausola APPEND della forma](../../../ado/guide/data/shape-append-clause.md)   
 [Comandi per la forma in generale](../../../ado/guide/data/shape-commands-in-general.md)   
 [Proprietà Value (ADO)](../../../ado/reference/ado-api/value-property-ado.md)   
 [Funzioni di Visual Basic, Applications Edition](../../../ado/guide/data/visual-basic-for-applications-functions.md)
