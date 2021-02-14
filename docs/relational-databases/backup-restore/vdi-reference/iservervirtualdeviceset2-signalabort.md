---
title: IServerVirtualDeviceSet2::SignalAbort
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::SignalAbort.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7dd21dfb56d093a6d35d977577971545c3fb1263
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347049"
---
# <a name="iservervirtualdeviceset2signalabort-vdi"></a>IServerVirtualDeviceSet2::SignalAbort (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **SignalAbort** segnala che dovrebbe verificarsi una terminazione anomala.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::SignalAbort ();
```

## <a name="return-value"></a>Valore restituito

Restituisce un *HRESULT* che indica l'esito positivo o negativo della chiamata al metodo. Il valore NOERROR indica l'esito positivo della chiamata del metodo. Un valore diverso da zero indica che si è verificato un errore.

## <a name="remarks"></a>Osservazioni

In qualsiasi momento il server può scegliere di interrompere l'operazione di backup o ripristino.

Questa routine segnala che tutte le operazioni devono essere interrotte. L'interfaccia nel suo complesso entra in uno stato di interruzione. Nessun altro comando viene accettato in alcun dispositivo. L'agente di completamento restituisce ERROR_OPERATION_ABORTED per ogni richiesta in sospeso e torna al chiamante. Tutti i completamenti registrati nel client vengono ignorati.

Il server garantisce che non vi siano altri usi richiesti per i buffer o i dispositivi restituiti dall'interfaccia dispositivo virtuale. Il server esegue quindi una pulizia delle terminazioni anomale, che deve includere la chiamata della funzione IServerVirtualDeviceSet2::Close.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).