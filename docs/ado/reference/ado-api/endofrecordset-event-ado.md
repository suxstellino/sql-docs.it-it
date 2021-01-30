---
description: Evento EndOfRecordset (ADO)
title: Evento EndOfRecordset (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EndOfRecordset
- Recordset::EndOfRecordset
helpviewer_keywords:
- EndOfRecordset event [ADO]
ms.assetid: 475de5e2-f634-4954-9edf-0027a6ba38d6
author: rothja
ms.author: jroth
ms.openlocfilehash: 9369b22c012773627c17da55a336aac116dc4e06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171232"
---
# <a name="endofrecordset-event-ado"></a>Evento EndOfRecordset (ADO)
L'evento **EndOfRecordset** viene chiamato quando si tenta di spostarsi in una riga oltre la fine del [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
EndOfRecordset fMoreData, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parametri  
 *fMoreData*  
 Valore **VARIANT_BOOL** che, se impostato su VARIANT_TRUE, indica che nel **Recordset** sono state aggiunte più righe.  
  
 *adStatus*  
 Valore di stato di [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) .  
  
 Quando viene chiamato **EndOfRecordset** , questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è riuscita. Viene impostato su **adStatusCantDeny** se questo evento non può richiedere l'annullamento dell'operazione che ha causato l'evento.  
  
 Prima della restituzione di **EndOfRecordset** , impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive.  
  
 *pRecordset*  
 Oggetto **Recordset** . **Recordset** per il quale si è verificato l'evento.  
  
## <a name="remarks"></a>Commenti  
 Se l'operazione [MoveNext](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) ha esito negativo, è possibile che si verifichi un evento **EndOfRecordset** .  
  
 Questo gestore eventi viene chiamato quando viene effettuato un tentativo di spostarsi oltre la fine dell'oggetto **Recordset** , probabilmente in seguito alla chiamata a **MoveNext**. Tuttavia, in questo caso, è possibile recuperare più record da un database e aggiungerli alla fine del **Recordset**. In tal caso, impostare *fMoreData* su VARIANT_TRUE e restituire da **EndOfRecordset**. Chiamare quindi di nuovo **MoveNext** per accedere ai record appena recuperati.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Riepilogo dei gestori eventi ADO](../../../ado/guide/data/ado-event-handler-summary.md)
