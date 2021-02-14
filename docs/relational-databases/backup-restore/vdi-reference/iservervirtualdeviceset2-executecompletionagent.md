---
title: IServerVirtualDeviceSet2::ExecuteCompletionAgent
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::ExecuteCompletionAgent.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75b09df68be51f74cbb62dac5ddf039259ad79f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347098"
---
# <a name="iservervirtualdeviceset2executecompletionagent-vdi"></a>IServerVirtualDeviceSet2::ExecuteCompletionAgent (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **ExecuteCompletionAgent** viene usata per implementare il ciclo principale dell'agente di completamento.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::ExecuteCompletionAgent ();
```

## <a name="return-value"></a>Valore restituito

Restituisce un *HRESULT* che indica l'esito positivo o negativo della chiamata al metodo. Il valore NOERROR indica l'esito positivo della chiamata del metodo. Un valore diverso da zero indica che si è verificato un errore.

## <a name="remarks"></a>Osservazioni

L'agente di completamento fornisce un meccanismo tramite il quale SQL Server può sincronizzarsi con i completamenti dei comandi del dispositivo virtuale. Deve essere attivo prima che sia possibile inviare comandi e deve rimanere attivo fino alla chiusura di tutti i dispositivi.

Poiché SQL Server potrebbe dover eseguire un'inizializzazione speciale dei thread, questa interfaccia non avvia un nuovo thread di controllo. Al contrario, SQL Server configura un thread e quindi passa il controllo a questa routine. Il thread deve essere bloccabile nei meccanismi di comunicazione interprocesso (IPC) di Windows NT e in grado di chiamare qualsiasi funzione di callback fornita con i comandi inviati tramite IServerVirtualDevice::SendCommand.

Questa funzione non restituirà il controllo fino a quando non viene richiamato IServerVirtualDeviceSet2::Close o SignalAbort.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).