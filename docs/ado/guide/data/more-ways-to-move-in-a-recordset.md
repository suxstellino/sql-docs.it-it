---
description: Altri metodi per lo spostamento in un recordset
title: Altri modi per spostarsi in un recordset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- Recordset object [ADO], moving
- MovePrevious method [ADO]
ms.assetid: 9f8cf1b2-3def-453f-a0ff-4646c5f15262
author: rothja
ms.author: jroth
ms.openlocfilehash: 78acb389a38896d05a684b0dbd940942a026655e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032713"
---
# <a name="more-ways-to-move-in-a-recordset"></a>Altri metodi per lo spostamento in un recordset
I quattro metodi seguenti vengono usati per spostarsi o scorrere nel **Recordset**: [MoveFirst, MoveLast, MoveNext e MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md). Alcuni di questi metodi non sono disponibili per i cursori di sola trasmissione.  
  
 **MoveFirst** imposta la posizione del record corrente sul primo record del **Recordset**. **MoveLast** consente di modificare la posizione del record corrente nell'ultimo record del **Recordset**. Per utilizzare **MoveFirst** o **MoveLast**, è necessario che l'oggetto **Recordset** supporti i segnalibri o lo spostamento all'indietro. in caso contrario, la chiamata al metodo genererà un errore.  
  
 **MoveNext** sposta la posizione del record corrente in un punto successivo. Se si usa l'ultimo record quando si chiama **MoveNext**, **EOF** verrà impostato su **true**. **MovePrevious** sposta la posizione del record corrente in una posizione precedente. Se si usa il primo record quando si chiama **MovePrevious**, **BOF** verrà impostato su **true**. È consigliabile controllare le proprietà **EOF** e **BOF** quando si usano questi metodi e spostare nuovamente il cursore in una posizione di record corrente valida se si esce da una estremità del **Recordset**, come illustrato di seguito:  
  
```  
. . .  
oRs.MoveNext  
If oRs.EOF Then oRs.MoveLast  
. . .   
```  
  
 O, nel caso del metodo **MovePrevious** :  
  
```  
. . .   
oRs.MovePrevious  
If oRs.BOF Then oRs.MoveFirst  
. . .  
```  
  
 Nei casi in cui il **Recordset** è stato filtrato o ordinato e i dati del record corrente vengono modificati, anche la posizione potrebbe cambiare. In questi casi, il metodo **MoveNext** funziona normalmente, ma tenere presente che la posizione viene spostata un record in un altro punto dalla nuova posizione, non dalla posizione precedente. Se, ad esempio, si modificano i dati nel record corrente, in modo che il record venga spostato alla fine del **Recordset** ordinato, significa che la chiamata a **MoveNext** comporta l'impostazione del record corrente sulla posizione dopo l'ultimo record nel **Recordset** (**EOF**  =  **true**).  
  
 Il comportamento dei vari metodi di spostamento dell'oggetto **Recordset** dipende, in una certa misura, dai dati all'interno del **Recordset**. I nuovi record aggiunti al **Recordset** vengono inizialmente aggiunti in un ordine specifico, definito dall'origine dati e possono essere dipendenti in modo implicito o esplicito sui dati nel nuovo record. Se, ad esempio, un ordinamento o un join viene eseguito all'interno della query che popola il **Recordset**, il nuovo record verrà inserito nella posizione appropriata all'interno del **Recordset**. Se l'ordinamento non viene specificato in modo esplicito durante la creazione del **Recordset**, le modifiche nell'implementazione dell'origine dati possono causare la modifica involontaria dell'ordine delle righe restituite. Inoltre, le funzioni di ordinamento, filtro e modifica del **Recordset** possono influenzare l'ordine e possibilmente quali righe del recordset saranno visibili.  
  
 Pertanto, **MoveNext**, **MovePrevious**, **MoveFirst**, **MoveLast** e **Move** sono tutti sensibili ad altre operazioni eseguite sullo stesso **Recordset**. ADO tenterà sempre di mantenere la posizione corrente fino a quando non viene spostata in modo esplicito, ma a volte le modifiche coinvolte rendono difficile comprendere gli effetti di uno spostamento successivo. Se, ad esempio, si chiama **MoveFirst** per posizionarsi sulla prima riga di un **Recordset** ordinato e si modifica l'ordinamento dall'ordine crescente a quello decrescente, si rimane nella stessa riga, ma ora si tratta dell'ultima riga del **Recordset**. **MoveFirst** consente di passare a una riga diversa (la nuova prima riga).  
  
 Per un altro esempio, se si è posizionati in una riga specifica al centro di un **Recordset** e si chiama **Delete** e quindi si chiama **MoveNext**, si sta ora eseguendo il record immediatamente dopo il record eliminato. Ma la chiamata a **MovePrevious** rende il record precedente a quello in cui è stato eliminato il record corrente, perché il record eliminato non viene più conteggiato nell'appartenenza attiva del **Recordset**.  
  
 È particolarmente difficile definire una semantica di spostamento coerente in tutti i provider per i metodi che si spostano in relazione al record corrente- **MovePrevious**, **MoveNext** e **Move** -in base alla modifica dei dati nel record corrente. Se, ad esempio, si utilizza un **Recordset** ordinato e filtrato e si modificano i dati del record corrente in modo che precedano tutti gli altri record, ma anche i dati modificati non corrispondono più al filtro, non è chiaro dove un'operazione **MoveNext** dovrebbe essere eseguita. La conclusione più sicura è che lo spostamento relativo all'interno di un **Recordset** è più rischioso dello spostamento assoluto (ad esempio, l'utilizzo di **MoveFirst** o **MoveLast**) quando i dati cambiano durante la modifica, l'aggiunta o l'eliminazione di record. L'ordinamento e il filtro devono essere basati su una chiave primaria o un ID, perché questo tipo di valore non deve essere modificato.