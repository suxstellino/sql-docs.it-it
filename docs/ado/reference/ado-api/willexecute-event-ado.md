---
description: Evento WillExecute (ADO)
title: Evento WillExecute (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- WillExecute
- Connection::WillExecute
helpviewer_keywords:
- WillExecute event [ADO]
ms.assetid: dd755e46-f589-48a3-93a9-51ff998d44b5
author: rothja
ms.author: jroth
ms.openlocfilehash: 53be0dbdb8ecebb6b81c1b4300353a07c0f442e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056082"
---
# <a name="willexecute-event-ado"></a>Evento WillExecute (ADO)
L'evento **WillExecute** viene chiamato immediatamente prima dell'esecuzione di un comando in sospeso in una connessione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
WillExecute Source, CursorType, LockType, Options, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Parametri  
 *Origine*  
 **Stringa** che contiene un comando SQL o un nome di stored procedure.  
  
 *CursorType*  
 Oggetto [CursorTypeEnum](./cursortypeenum.md) che contiene il tipo di cursore per il **Recordset** che verrà aperto. Con questo parametro è possibile modificare il cursore in qualsiasi tipo durante un'operazione **Recordset**[Open Method (recordset ADO)](./open-method-ado-recordset.md) . *CursorType* verrà ignorato per qualsiasi altra operazione.  
  
 *LockType*  
 Oggetto [LockTypeEnum](./locktypeenum.md) che contiene il tipo di blocco per il **Recordset** che verrà aperto. Con questo parametro è possibile modificare il blocco in qualsiasi tipo durante un'operazione **RecordsetOpen** . *LockType* verrà ignorato per qualsiasi altra operazione.  
  
 *Opzioni*  
 Valore **Long** che indica le opzioni che possono essere utilizzate per eseguire il comando o aprire il **Recordset**.  
  
 *adStatus*  
 Valore di stato [EventStatusEnum](./eventstatusenum.md) che può essere **adStatusCantDeny** o **adStatusOK** quando viene chiamato questo evento. Se è **adStatusCantDeny**, questo evento potrebbe non richiedere l'annullamento dell'operazione in sospeso.  
  
 *pCommand*  
 Oggetto [comando (ADO)](./command-object-ado.md) per il quale viene applicata la notifica degli eventi.  
  
 *pRecordset*  
 Oggetto [Recordset (ADO)](./recordset-object-ado.md) per il quale viene applicata la notifica degli eventi.  
  
 *pConnection*  
 Oggetto di [connessione (ADO)](./connection-object-ado.md) per il quale viene applicata la notifica degli eventi.  
  
## <a name="remarks"></a>Commenti  
 Un evento **WillExecute** può verificarsi a causa di una connessione.  Metodo [Execute (connessione ADO)](./execute-method-ado-connection.md), metodo [Execute (comando ADO)](./execute-method-ado-command.md)o metodo [Open (recordset ADO)](./open-method-ado-recordset.md) il parametro *pConnection* deve sempre contenere un riferimento valido a un oggetto **Connection** . Se l'evento è dovuto a **Connection.Execute**, i parametri *pRecordset* e *pCommand* sono impostati su **Nothing**. Se l'evento è dovuto a **Recordset. Open**, il parametro *pRecordset* fa riferimento all'oggetto **Recordset** e il parametro *pCommand* è impostato su **Nothing**. Se l'evento è dovuto a **Command.Execute**, il parametro *pCommand* fa riferimento all'oggetto **Command** e il parametro *pRecordset* è impostato su **Nothing**.  
  
 **WillExecute** consente di esaminare e modificare i parametri di esecuzione in sospeso. Questo evento può restituire una richiesta di annullamento del comando in sospeso.  
  
> [!NOTE]
>  Se l'origine originale per un **comando** è un flusso specificato dalla proprietà [COMMANDSTREAM Property (ADO)](./commandstream-property-ado.md) , l'assegnazione di una nuova stringa al parametro _source_ WillExecute modifica l'origine del **comando**. La proprietà **CommandStream** verrà deselezionata e la proprietà [COMMANDTEXT Property (ADO)](./commandtext-property-ado.md) verrà aggiornata con la nuova origine. Il flusso originale specificato da **CommandStream** verrà rilasciato e non sarà possibile accedervi.  
  
 Se il dialetto della nuova stringa di origine è diverso dall'impostazione originale della proprietà della [Proprietà dialetto](./dialect-property.md) (che corrisponde a **CommandStream**), è necessario specificare il dialetto corretto impostando la proprietà **dialetto** dell'oggetto Command a cui fa riferimento *pCommand*.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Riepilogo del gestore eventi ADO](../../guide/data/ado-event-handler-summary.md)   
 [Oggetto Connection (ADO)](./connection-object-ado.md)