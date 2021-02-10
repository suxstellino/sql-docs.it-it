---
description: Funzionamento dei comandi con parametri
title: Operazione di comandi con parametri | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
ms.assetid: 4fae0d54-83b6-4ead-99cc-bcf532daa121
author: rothja
ms.author: jroth
ms.openlocfilehash: 55eaf3798ee8d14a776da14010b4b6617d0e2723
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032653"
---
# <a name="operation-of-parameterized-commands"></a>Funzionamento dei comandi con parametri
Se si utilizza un **Recordset** figlio di grandi dimensioni, in particolare rispetto alle dimensioni del **Recordset** padre, ma è necessario accedere solo a pochi capitoli figlio, potrebbe risultare più efficiente utilizzare un comando con parametri.  
  
 Un *comando senza parametri* recupera l'intero **Recordset** padre e figlio, aggiunge una colonna del capitolo all'elemento padre e quindi assegna un riferimento al capitolo figlio correlato per ogni riga padre.  
  
 Un *comando con parametri* recupera l'intero **Recordset** padre, ma recupera solo il **Recordset** del capitolo quando si accede alla colonna del capitolo. Questa differenza nella strategia di recupero può produrre vantaggi significativi a livello di prestazioni.  
  
 Ad esempio, è possibile specificare quanto segue:  
  
```  
SHAPE {SELECT * FROM customer}   
   APPEND ({SELECT * FROM orders WHERE cust_id = ?}   
   RELATE cust_id TO PARAMETER 0)  
```  
  
 Le tabelle padre e figlio hanno un nome di colonna in comune *cust_id*. Il *comando figlio* dispone di un segnaposto "?", a cui fa riferimento la clausola di correlazione (ovvero "... PARAMETRO 0 ").  
  
> [!NOTE]
>  La clausola PARAMETER riguarda esclusivamente la sintassi del comando Shape. Non è associato né all'oggetto [parametro](../../reference/ado-api/parameter-object.md) ADO né alla raccolta [Parameters](../../reference/ado-api/parameters-collection-ado.md) .  
  
 Quando viene eseguito il comando Shape con parametri, si verifica quanto segue:  
  
1.  Il *comando padre* viene eseguito e restituisce un **Recordset** padre dalla tabella Customers.  
  
2.  Una colonna del capitolo viene aggiunta al **Recordset** padre.  
  
3.  Quando si accede alla colonna del capitolo di una riga padre, il *comando figlio* viene eseguito usando il valore della Customer.cust_id come valore del parametro.  
  
4.  Tutte le righe nel set di righe del provider di dati creato nel passaggio 3 vengono utilizzate per popolare il **Recordset** figlio. In questo esempio, si tratta di tutte le righe della tabella Orders in cui la cust_id è uguale al valore di customer.cust_id. Per impostazione predefinita, il **Recordset** figlio verrà memorizzato nella cache del client fino a quando non vengono rilasciati tutti i riferimenti al **Recordset** padre. Per modificare questo comportamento, impostare le **righe figlio della cache** della [proprietà dinamica](../../reference/ado-api/ado-dynamic-property-index.md) **Recordset** su **false**.  
  
5.  Un riferimento alle righe figlio recuperate, ovvero il capitolo del **Recordset** figlio, viene inserito nella colonna capitolo della riga corrente del **Recordset** padre.  
  
6.  I passaggi 3-5 vengono ripetuti quando si accede alla colonna del capitolo di un'altra riga.  
  
 Per impostazione predefinita, la proprietà dinamica **righe figlio della cache** è impostata su **true** . Il comportamento di memorizzazione nella cache varia a seconda dei valori dei parametri della query. In una query con un solo parametro, il **Recordset** figlio per un valore di parametro specificato verrà memorizzato nella cache tra le richieste per un elemento figlio con tale valore. Il codice seguente illustra questo processo:  
  
```  
SCmd = "SHAPE {select * from customer} " & _  
         "APPEND({select * from orders where cust_id = ?} " & _  
         "RELATE cust_id TO PARAMETER 0) AS chpCustOrder"  
Rst1.Open sCmd, Cnn1  
Set RstChild = Rst1("chpCustOrder").Value  
Rst1.MoveNext      ' Next cust_id passed to Param 0, & new rs fetched   
                   ' into RstChild.  
Rst1.MovePrevious  ' RstChild now holds cached rs, saving round trip.  
```  
  
 In una query con due o più parametri, un elemento figlio memorizzato nella cache viene usato solo se tutti i valori dei parametri corrispondono ai valori memorizzati nella cache.  
  
## <a name="parameterized-commands-and-complex-parent-child-relations"></a>Comandi con parametri e relazioni padre-figlio complesse  
 Oltre a utilizzare i comandi con parametri per migliorare le prestazioni di una gerarchia dei tipi equi-join, è possibile utilizzare i comandi con parametri per supportare relazioni padre-figlio più complesse. Si consideri ad esempio un piccolo database di League con due tabelle: uno costituito dai team (team_id, team_name) e l'altro dei giochi (date, home_team, visiting_team).  
  
 Utilizzando una gerarchia senza parametri, non è possibile correlare le tabelle teams e Games in modo tale che il **Recordset** figlio per ogni team contenga la pianificazione completa. È possibile creare capitoli che contengono la pianificazione Home o la pianificazione stradale, ma non entrambi. Ciò è dovuto al fatto che la clausola RELATE consente di limitare le relazioni padre-figlio del form (PC1 = CC1) e (PC2 = PC2). Quindi, se il comando includesse "CORRELAre team_id a home_team, team_id a visiting_team", si otterrebbero solo giochi in cui un team stava giocando a se stesso. Si vuole usare "(team_id = home_team) o (team_id = visiting_team)", ma il provider di forme non supporta la clausola o.  
  
 Per ottenere il risultato desiderato, è possibile usare un comando con parametri. Ad esempio:  
  
```  
SHAPE {SELECT * FROM teams}   
APPEND ({SELECT * FROM games WHERE home_team = ? OR visiting_team = ?}   
        RELATE team_id TO PARAMETER 0,   
               team_id TO PARAMETER 1)   
```  
  
 Questo esempio sfrutta la maggiore flessibilità della clausola SQL WHERE per ottenere i risultati necessari.  
  
> [!NOTE]
>  Quando si usano le clausole WHERE, i parametri non possono usare i tipi di dati SQL per text, ntext e image. in caso contrario, verrà generato un errore che contiene la descrizione seguente: `Invalid operator for data type` .  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di data shaping](./data-shaping-example.md)   
 [Grammatica forma formale](./formal-shape-grammar.md)   
 [Comandi Shape in generale](./shape-commands-in-general.md)