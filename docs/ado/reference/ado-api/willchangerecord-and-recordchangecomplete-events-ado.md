---
description: Eventi WillChangeRecord e RecordChangeComplete (ADO)
title: Eventi WillChangeRecord e RecordChangeComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- RecordChangeComplete
- Recordset::WillChangeRecord
- WillChangeRecord
- Recordset::RecordChangeComplete
helpviewer_keywords:
- WillChangeRecord event [ADO]
- recordchangecomplete event [ADO]
ms.assetid: cbc369fd-63af-4a7d-96ae-efa91b78ca69
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a713c39746db1d6a5317a4884ac0a2c5cbb23fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166273"
---
# <a name="willchangerecord-and-recordchangecomplete-events-ado"></a>Eventi WillChangeRecord e RecordChangeComplete (ADO)
L'evento **WillChangeRecord** viene chiamato prima della modifica di uno o più record (righe) nel [Recordset](./recordset-object-ado.md) . L'evento **RecordChangeComplete** viene chiamato dopo la modifica di uno o più record.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
WillChangeRecord adReason, cRecords, adStatus, pRecordset  
RecordChangeCompleteadReason, cRecords, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parametri  
 *adReason*  
 Valore [EventReasonEnum](./eventreasonenum.md) che specifica il motivo di questo evento. Il valore può essere **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete** o **adRsnFirstChange**.  
  
 *cRecords*  
 Valore **Long** che indica il numero di record modificati (interessati).  
  
 *pError*  
 Oggetto [Error](./error-object.md) . Viene descritto l'errore che si è verificato se il valore di *adStatus* è **adStatusErrorsOccurred**; in caso contrario, non è impostato.  
  
 *adStatus*  
 Valore di stato di [EventStatusEnum](./eventstatusenum.md) .  
  
 Quando viene chiamato **WillChangeRecord** , questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è riuscita. Viene impostato su **adStatusCantDeny** se questo evento non può richiedere l'annullamento dell'operazione in sospeso.  
  
 Quando viene chiamato **RecordChangeComplete** , questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è stata eseguita correttamente oppure su **adStatusErrorsOccurred** se l'operazione non è riuscita.  
  
 Prima della restituzione di **WillChangeRecord** , impostare questo parametro su **adStatusCancel** per richiedere l'annullamento dell'operazione che ha causato questo evento o impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive.  
  
 Prima della restituzione di **RecordChangeComplete** , impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive.  
  
 *pRecordset*  
 Oggetto **Recordset** . **Recordset** per il quale si è verificato l'evento.  
  
## <a name="remarks"></a>Commenti  
 È possibile che si verifichi un evento **WillChangeRecord** o **RecordChangeComplete** per il primo campo modificato in una riga a causa delle operazioni **Recordset** seguenti: [Update](./update-method.md), [Delete](./delete-method-ado-recordset.md), [CancelUpdate](./cancelupdate-method-ado.md), [AddNew](./addnew-method-ado.md), [UpdateBatch](./updatebatch-method.md)e [CancelBatch](./cancelbatch-method-ado.md). Il valore del **Recordset** [CursorType](./cursortype-property-ado.md) determina quali operazioni provocano l'esecuzione degli eventi.  
  
 Durante l'evento **WillChangeRecord** , la proprietà del [filtro](./filter-property.md) **Recordset** è impostata su **adFilterAffectedRecords**. Non è possibile modificare questa proprietà durante l'elaborazione dell'evento.  
  
 È necessario impostare il parametro **adStatus** su **adStatusUnwantedEvent** per ogni possibile valore **adReason** per arrestare completamente la notifica degli eventi per qualsiasi evento che includa un parametro **adReason** .  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Riepilogo dei gestori eventi ADO](../../guide/data/ado-event-handler-summary.md)