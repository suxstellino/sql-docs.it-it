---
description: Eventi BeginTransComplete, CommitTransComplete e RollbackTransComplete (ADO)
title: Eventi BeginTrans, CommitTrans, RollbackTrans (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CommitTransComplete
- Connection::BeginTransComplete
- Connection::RollbackTransComplete
- Connection::CommitTransComplete
- RollbackTransComplete
- BeginTransComplete
helpviewer_keywords:
- CommitTransComplete event [ADO]
- RollbackTransComplete event [ADO]
- BeginTransComplete event [ADO]
ms.assetid: ec4e4b38-e9c6-4757-b2ef-4e468ae5f1d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 2c00bcdbbfb477f4f612fedd0a71fe8131858588
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027571"
---
# <a name="begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado"></a>Eventi BeginTransComplete, CommitTransComplete e RollbackTransComplete (ADO)
Questi eventi verranno chiamati al termine dell'esecuzione dell'operazione associata sull'oggetto [connessione](./connection-object-ado.md) .  
  
-   **BeginTransComplete** viene chiamato dopo l'operazione [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
-   **CommitTransComplete** viene chiamato dopo l'operazione [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
-   **RollbackTransComplete** viene chiamato dopo l'operazione [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
BeginTransComplete TransactionLevel, pError, adStatus, pConnection  
CommitTransComplete pError, adStatus, pConnection  
RollbackTransComplete pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Parametri  
 *TransactionLevel*  
 Valore **Long** che contiene il nuovo livello di transazione di **BeginTrans** che ha causato l'evento.  
  
 *pError*  
 Oggetto [Error](./error-object.md) . Viene descritto l'errore che si è verificato se il valore di EventStatusEnum è **adStatusErrorsOccurred**; in caso contrario, non è impostato.  
  
 *adStatus*  
 Valore di stato di [EventStatusEnum](./eventstatusenum.md) . Quando viene chiamato uno di questi eventi, questo parametro è impostato su **adStatusOK** se l'operazione che ha causato l'evento è riuscita o **adStatusErrorsOccurred** se l'operazione non è riuscita.  
  
 Questi eventi possono impedire le notifiche successive impostando questo parametro su **adStatusUnwantedEvent** prima che l'evento venga restituito.  
  
 *pConnection*  
 Oggetto **connessione** per il quale si è verificato l'evento.  
  
## <a name="remarks"></a>Commenti  
 In Visual C++, più **connessioni** possono condividere lo stesso metodo di gestione degli eventi. Il metodo utilizza l'oggetto **Connection** restituito per determinare l'oggetto che ha causato l'evento.  
  
 Se la proprietà [Attributes](./attributes-property-ado.md) è impostata su **adXactCommitRetaining** o **adXactAbortRetaining**, viene avviata una nuova transazione dopo il commit o il rollback di una transazione. Utilizzare l'evento **BeginTransComplete** per ignorare tutto il primo evento di avvio della transazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Esempio di metodi BeginTrans, CommitTrans e RollbackTrans (VB)](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [Riepilogo del gestore eventi ADO](../../guide/data/ado-event-handler-summary.md)   
 [Metodi BeginTrans, CommitTrans e RollbackTrans (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)