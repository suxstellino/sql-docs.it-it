---
description: Evento onReadyStateChange (Servizi Desktop remoto)
title: Evento onReadyStateChange (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- onReadyStateChange event [ADO]
ms.assetid: bf2ae3ac-bfe4-4709-b50a-ea7c282c3164
author: rothja
ms.author: jroth
ms.openlocfilehash: ce2a7fa6980f6b76c44b6934812c9736b5bf3bc8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168882"
---
# <a name="onreadystatechange-event-rds"></a>Evento onReadyStateChange (Servizi Desktop remoto)
L'evento **onReadyStateChange** viene chiamato ogni volta che viene modificato il valore della proprietà [ReadyState](./readystate-property-rds.md) .  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
onReadyStateChange  
```  
  
#### <a name="parameters"></a>Parametri  
 No.  
  
## <a name="remarks"></a>Osservazioni  
 La proprietà **ReadyState** riflette lo stato di un [RDS. Oggetto DataControl](./datacontrol-object-rds.md) in quanto recupera i dati in modo asincrono nell'oggetto [Recordset](../ado-api/recordset-object-ado.md) . Usare l'evento **onReadyStateChange** per monitorare le modifiche apportate alla proprietà **ReadyState** quando si verificano. Questa operazione è più efficiente rispetto al controllo periodico del valore della proprietà.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di modello di eventi ADO (VC + +)](../ado-api/ado-events-model-example-vc.md)   
 [Riepilogo dei gestori eventi ADO](../../guide/data/ado-event-handler-summary.md)