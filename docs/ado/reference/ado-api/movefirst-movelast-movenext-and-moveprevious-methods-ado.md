---
description: Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (ADO)
title: Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::MoveLast
- Recordset15::MoveNext
- Recordset15::raw_MoveNext
- Recordset15::raw_MovePrevious
- Recordset15::MoveFirst
- Recordset15::raw_MoveLast
- Recordset15::MovePrevious
- Recordset15::raw_MoveFirst
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- MovePrevious method [ADO]
ms.assetid: a61a01a7-5b33-4150-9126-21dfa63654cb
author: rothja
ms.author: jroth
ms.openlocfilehash: a267298dd652a307d6e3365cb48e30f9fc667ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041731"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-ado"></a>Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (ADO)
Passa al record primo, ultimo, successivo o precedente in un oggetto [Recordset](./recordset-object-ado.md) specificato e fa in modo che registri il record corrente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
## <a name="remarks"></a>Osservazioni  
 Usare il metodo **MoveFirst** per spostare la posizione del record corrente nel primo record del **Recordset**.  
  
 Usare il metodo **MoveLast** per spostare la posizione del record corrente nell'ultimo record del **Recordset**. L'oggetto **Recordset** deve supportare i segnalibri o lo spostamento all'indietro del cursore. in caso contrario, la chiamata al metodo genererà un errore.  
  
 Una chiamata a **MoveFirst** o **MoveLast** quando il **Recordset** è vuoto (sia **BOF** che **EOF** sono true) genera un errore.  
  
 Utilizzare il metodo **MoveNext** per spostare in avanti la posizione del record corrente di un record (verso la parte inferiore del **Recordset**). Se l'ultimo record è il record corrente e si chiama il metodo **MoveNext** , ADO imposta il record corrente sulla posizione dopo l'ultimo record nel **Recordset** ([EOF](./bof-eof-properties-ado.md) è **true**). Un tentativo di andare avanti quando la proprietà **EOF** è già **true** genera un errore.  
  
 In ADO 2,5 e versioni successive, quando il **Recordset** è stato filtrato o ordinato e i dati del record corrente vengono modificati, la chiamata al metodo **MoveNext** sposta il cursore a due record in avanti rispetto al record corrente. Questo è dovuto al fatto che quando il record corrente viene modificato, il record successivo diventa il nuovo record corrente. La chiamata a **MoveNext** dopo la modifica sposta il cursore di un record in base al nuovo record corrente. Si tratta di un comportamento diverso rispetto a ADO 2,1 e versioni precedenti. In queste versioni precedenti, la modifica dei dati di un record corrente nel **Recordset** ordinato o filtrato non comporta la modifica della posizione del record corrente e **MoveNext** sposta il cursore sul record successivo subito dopo il record corrente.  
  
 Usare il metodo **MovePrevious** per spostare la posizione del record corrente di un record indietro (verso la parte superiore del **Recordset**). L'oggetto **Recordset** deve supportare i segnalibri o lo spostamento all'indietro del cursore. in caso contrario, la chiamata al metodo genererà un errore. Se il primo record è il record corrente e si chiama il metodo **MovePrevious** , ADO imposta il record corrente sulla posizione prima del primo record nel **Recordset** ([BOF](./bof-eof-properties-ado.md) è **true**). Un tentativo di spostarsi all'indietro quando la proprietà **BOF** è già **true** genera un errore. Se l'oggetto **Recordset** non supporta segnalibri o movimenti di cursore a ritroso, il metodo **MovePrevious** genererà un errore.  
  
 Se il **Recordset** è di sola trasmissione e si desidera supportare lo scorrimento avanti e indietro, è possibile utilizzare la proprietà [CacheSize](./cachesize-property-ado.md) per creare una cache di record che supporti lo spostamento all'indietro del cursore tramite il metodo [Move](./move-method-ado.md) . Poiché i record memorizzati nella cache vengono caricati in memoria, è consigliabile evitare di memorizzare nella cache più record del necessario. È possibile chiamare il metodo **MoveFirst** in un oggetto **Recordset** di sola trasmissione; in questo modo, il provider può eseguire di nuovo il comando che ha generato l'oggetto **Recordset** .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi MoveFirst, MoveLast, MoveNext e MovePrevious (VB)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vb.md)   
 [Esempio di metodi MoveFirst, MoveLast, MoveNext e MovePrevious (VBScript)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vbscript.md)   
 [Esempio di metodi MoveFirst, MoveLast, MoveNext e MovePrevious (VC + +)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vc.md)   
 [Metodo Move (ADO)](./move-method-ado.md)   
 [Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [Metodo MoveRecord (ADO)](./moverecord-method-ado.md)