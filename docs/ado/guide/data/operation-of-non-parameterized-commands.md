---
description: Funzionamento dei comandi senza parametri
title: Operazione di comandi senza parametri | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- non-parameterized commands [ADO]
- data shaping [ADO], non-parameterized commands
ms.assetid: 9700e50a-9f17-4ba3-8afb-f750741dc6ca
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee255f895e934c4405d888d72f15b8cee9159d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037201"
---
# <a name="operation-of-non-parameterized-commands"></a>Funzionamento dei comandi senza parametri
Per i comandi senza parametri, vengono eseguiti tutti i comandi del provider e i **Recordset** vengono creati durante l'esecuzione del comando. Se il comando viene eseguito in modo sincrono, tutti i **Recordset** verranno popolati in modo completo. Se è stata selezionata una modalità di popolamento asincrono, lo stato popolato dei **Recordset** dipenderà dalla modalità di popolamento e dalle dimensioni dei **Recordset**.  
  
 Ad esempio, il *comando padre* può restituire un **Recordset** di clienti per una società di una tabella Customers e il *comando figlio* può restituire un **Recordset** di ordini per tutti i clienti da una tabella Orders.  
  
```  
SHAPE {SELECT * FROM Customers}   
   APPEND ({SELECT * FROM Orders} AS chapOrders   
   RELATE customerID TO customerID)  
```  
  
 Per le relazioni padre-figlio senza parametri, ogni oggetto **Recordset** padre e figlio deve disporre di una colonna in comune per associarli. Le colonne vengono denominate nella clausola di CORRELAzione, prima della *colonna padre* e quindi della *colonna figlio*. Le colonne possono avere nomi diversi nei rispettivi oggetti **Recordset** , ma devono fare riferimento alle stesse informazioni per poter specificare una relazione significativa. Ad esempio, gli oggetti recordset **Customers** e **Orders** possono avere entrambi un campo CustomerID. Poiché l'appartenenza al **Recordset** figlio è determinata dal comando del provider, il **Recordset** figlio può contenere righe orfane. Queste righe orfane sono inaccessibili senza ulteriori rishaping.  
  
 Data Shaping aggiunge una colonna del capitolo al **Recordset** padre. I valori nella colonna capitolo sono riferimenti a righe nel **Recordset** figlio, che soddisfano la clausola di correlazione. Ovvero, lo stesso valore si trova nella *colonna padre* di una riga padre specificata, così come si trova nella *colonna figlio* di tutte le righe del capitolo figlio. Quando più clausole TO vengono utilizzate nella stessa clausola CORRELAta, vengono combinate in modo implicito utilizzando un operatore AND. Se le colonne padre nella clausola di CORRELAzione non costituiscono una chiave per il **Recordset** padre, una singola riga figlio può avere più righe padre.  
  
 Quando si accede al riferimento nella colonna capitolo, ADO recupera automaticamente il **Recordset** rappresentato dal riferimento. Si noti che in un comando senza parametri, sebbene sia stato recuperato l'intero **Recordset** figlio, il capitolo presenta solo un subset di righe.  
  
 Se la colonna accodata non dispone di un *alias di capitolo*, verrà generato automaticamente un nome. Un oggetto [campo](../../reference/ado-api/field-object.md) per la colonna verrà accodato alla raccolta di [campi](../../reference/ado-api/fields-collection-ado.md) dell'oggetto **Recordset** e il tipo di dati sarà **adChapter**.  
  
 Per informazioni sull'esplorazione di un **Recordset** gerarchico, vedere [accesso alle righe in un recordset gerarchico](./accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di data shaping](./data-shaping-example.md)   
 [Grammatica forma formale](./formal-shape-grammar.md)   
 [Comandi Shape in generale](./shape-commands-in-general.md)