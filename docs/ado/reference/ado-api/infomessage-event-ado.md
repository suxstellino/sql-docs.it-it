---
description: Evento InfoMessage (ADO)
title: Evento InfoMessage (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection::InfoMessage
- InfoMessage
helpviewer_keywords:
- InfoMessage event [ADO]
ms.assetid: 468c87dd-e3bc-4084-9941-94d10743d4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: aabc5d54aed934a9bfa9c1695f2ceb8844e8962b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167214"
---
# <a name="infomessage-event-ado"></a>Evento InfoMessage (ADO)
L'evento **InfoMessage** viene chiamato ogni volta che viene generato un avviso durante un'operazione **ConnectionEvent** .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
InfoMessage pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Parametri  
 *pError*  
 Oggetto [Error](./error-object.md) . Questo parametro contiene gli eventuali errori restituiti. Se vengono restituiti più errori, enumerare la raccolta di **errori** per trovarli.  
  
 *adStatus*  
 Valore di stato di [EventStatusEnum](./eventstatusenum.md) . Se viene generato un avviso, *adStatus* è impostato su **adStatusOK** e *perror* contiene l'avviso.  
  
 Prima della restituzione di questo evento, impostare questo parametro su **adStatusUnwantedEvent** per impedire le notifiche successive.  
  
 *pConnection*  
 Oggetto [Connection](./connection-object-ado.md) . Connessione per la quale si è verificato l'avviso. Ad esempio, è possibile che si verifichino avvisi durante l'apertura di un oggetto **connessione** o l'esecuzione di un [comando](./command-object-ado.md) in una **connessione**.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Riepilogo del gestore eventi ADO](../../guide/data/ado-event-handler-summary.md)   
 [Oggetto Connection (ADO)](./connection-object-ado.md)