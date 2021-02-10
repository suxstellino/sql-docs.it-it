---
description: Eventi WillChangeField e FieldChangeComplete (ADO)
title: Eventi WillChangeField e FieldChangeComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldChangeComplete
- Recordset::WillChangeField
- Recordset::FieldChangeComplete
- WillChangeField
helpviewer_keywords:
- WillChangeField event [ADO]
- fieldchangecomplete event [ADO]
ms.assetid: 3e49fb89-c45b-4d39-823e-3cc887c59b37
author: rothja
ms.author: jroth
ms.openlocfilehash: 4aef5291d7e4c539200eb0cfd34edf3174c44c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056136"
---
# <a name="willchangefield-and-fieldchangecomplete-events-ado"></a>Eventi WillChangeField e FieldChangeComplete (ADO)
L'evento **WillChangeField** viene chiamato prima che un'operazione in sospeso modifichi il valore di uno o più oggetti [Field](./field-object.md) nel [Recordset](./recordset-object-ado.md). L'evento **FieldChangeComplete** viene chiamato dopo che il valore di uno o più oggetti **Field** è stato modificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
WillChangeField cFields, Fields, adStatus, pRecordset  
FieldChangeComplete cFields, Fields, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parametri  
 *cFields*  
 Oggetto **Long** che indica il numero di oggetti **campo** nei *campi*.  
  
 *Fields*  
 Per **WillChangeField**, il parametro *Fields* è una matrice di **varianti** che contiene gli oggetti **Field** con i valori originali. Per **FieldChangeComplete**, il parametro *Fields* è una matrice di **varianti** che contiene gli oggetti **Field** con i valori modificati.  
  
 *pError*  
 Oggetto [Error](./error-object.md) . Viene descritto l'errore che si è verificato se il valore di *adStatus* è **adStatusErrorsOccurred**; in caso contrario, non è impostato.  
  
 *adStatus*  
 Valore di stato di [EventStatusEnum](./eventstatusenum.md) .  
  
 Quando viene chiamato **WillChangeField** , questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è riuscita. Viene impostato su **adStatusCantDeny** se questo evento non può richiedere l'annullamento dell'operazione in sospeso.  
  
 Quando viene chiamato **FieldChangeComplete** , questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è stata eseguita correttamente oppure su **adStatusErrorsOccurred** se l'operazione non è riuscita.  
  
 Prima della restituzione di **WillChangeField** , impostare questo parametro su **adStatusCancel** per richiedere l'annullamento dell'operazione in sospeso.  
  
 Prima della restituzione di **FieldChangeComplete** , impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive.  
  
 *pRecordset*  
 Oggetto **Recordset** . **Recordset** per il quale si è verificato l'evento.  
  
## <a name="remarks"></a>Commenti  
 È possibile che si verifichi un evento **WillChangeField** o **FieldChangeComplete** quando si imposta la proprietà [value](./value-property-ado.md) e si chiama il metodo [Update](./update-method.md) con i parametri di matrice Field e value.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Riepilogo dei gestori eventi ADO](../../guide/data/ado-event-handler-summary.md)