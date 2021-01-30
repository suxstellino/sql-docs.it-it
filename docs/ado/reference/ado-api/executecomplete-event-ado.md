---
description: Evento ExecuteComplete (ADO)
title: Evento ExecuteComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection::ExecuteComplete
- ExecuteComplete
helpviewer_keywords:
- ExecuteComplete event [ADO]
ms.assetid: 62470d42-e511-494c-bec4-ad4591734b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: c64856ccbd3cdada037ffd77a9ac3b0e74add570
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171125"
---
# <a name="executecomplete-event-ado"></a>Evento ExecuteComplete (ADO)
L'evento **ExecuteComplete** viene chiamato dopo il completamento dell'esecuzione di un comando.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
ExecuteComplete RecordsAffected, pError, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Parametri  
 *RecordsAffected*  
 Valore **Long** che indica il numero di record interessati dal comando.  
  
 *pError*  
 Oggetto [Error](../../../ado/reference/ado-api/error-object.md) . Viene descritto l'errore che si è verificato se il valore di **adStatus** è **adStatusErrorsOccurred**; in caso contrario, non è impostato.  
  
 *adStatus*  
 Valore di stato di [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) . Quando viene chiamato questo evento, questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è riuscita o **adStatusErrorsOccurred** se l'operazione non è riuscita.  
  
 Prima della restituzione di questo evento, impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive.  
  
 *pCommand*  
 Oggetto [Command](../../../ado/reference/ado-api/command-object-ado.md) eseguito. Contiene un oggetto **Command** anche quando si **chiamaConnection.Execute** o **Recordset. Open** senza creare esplicitamente un **comando**. in questo caso l'oggetto **Command** viene creato internamente da ADO.  
  
 *pRecordset*  
 Oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) che è il risultato del comando eseguito. Questo **Recordset** può essere vuoto. Non eliminare mai questo oggetto recordset dall'interno di questo gestore eventi. Questa operazione comporterà una violazione di accesso quando ADO tenterà di accedere a un oggetto che non esiste più.  
  
 *pConnection*  
 Oggetto [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Connessione su cui è stata eseguita l'operazione.  
  
## <a name="remarks"></a>Commenti  
 Un evento **ExecuteComplete** può verificarsi a causa della **connessione.** [Eseguire](../../../ado/reference/ado-api/execute-method-ado-connection.md)il **comando.** [Eseguire](../../../ado/reference/ado-api/execute-method-ado-command.md), **Recordset.** [Aprire](../../../ado/reference/ado-api/open-method-ado-recordset.md), **Recordset.** [Eseguire una query](../../../ado/reference/ado-api/requery-method.md)o un **Recordset.** Metodi [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md) .  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Riepilogo dei gestori eventi ADO](../../../ado/guide/data/ado-event-handler-summary.md)
