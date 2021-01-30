---
description: Eventi ConnectComplete e Disconnect (ADO)
title: Eventi ConnectComplete e Disconnect (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Disconnect
- Connection::ConnectComplete
- ConnectComplete
- Connection::Disconnect
helpviewer_keywords:
- Disconnect event [ADO]
- ConnectComplete event [ADO]
ms.assetid: 568f5252-d069-4d99-a01b-2ada87ad1304
author: rothja
ms.author: jroth
ms.openlocfilehash: 4b451f72d33baf52642f00926e644892f5b955ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164614"
---
# <a name="connectcomplete-and-disconnect-events-ado"></a>Eventi ConnectComplete e Disconnect (ADO)
L'evento **ConnectComplete** viene chiamato dopo l'avvio di una connessione. L'evento di **disconnessione** viene chiamato al termine di una connessione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
ConnectComplete pError, adStatus, pConnection  
Disconnect adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Parametri  
 *pError*  
 Oggetto [Error](./error-object.md) . Viene descritto l'errore che si è verificato se il valore di *adStatus* è **adStatusErrorsOccurred**; in caso contrario, non è impostato.  
  
 *adStatus*  
 Valore [EventStatusEnum](./eventstatusenum.md) che restituisce sempre **adStatusOK**.  
  
 Quando viene chiamato **ConnectComplete** , questo parametro è impostato su **adStatusCancel** se un evento **WillConnect** ha richiesto l'annullamento della connessione in sospeso.  
  
 Prima della restituzione di uno degli eventi, impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive. Tuttavia, la chiusura e la riapertura della [connessione](./connection-object-ado.md) comporta la riesecuzione di questi eventi.  
  
 *pConnection*  
 Oggetto **connessione** per cui si applica questo evento.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Riepilogo dei gestori eventi ADO](../../guide/data/ado-event-handler-summary.md)