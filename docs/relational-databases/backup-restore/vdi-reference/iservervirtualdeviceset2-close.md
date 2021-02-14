---
title: IServerVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::Close.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bc520d4cb4ded130c6d44a2fef70ddd4b5ff5d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347147"
---
# <a name="iservervirtualdeviceset2close-vdi"></a>IServerVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **Close** chiude un set di dispositivi virtuali aperto da IServerVirtualDeviceSet2::Open. Il risultato è il rilascio di tutte le risorse associate al set di dispositivi virtuali. L'handle IServerVirtualDeviceSet2 non è utile dopo il completamento di questa funzione e deve essere restituito a COM.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| VD_E_PROTOCOL | I dispositivi erano ancora aperti. |

## <a name="remarks"></a>Osservazioni

Non eseguire la chiusura del set di dispositivi virtuali prima di chiudere i dispositivi. Se si verifica questa situazione, viene restituito VD_E_PROTOCOL. Con questa azione, Close rilascia immediatamente il mapping della memoria condivisa. Il server è soggetto a violazioni di accesso se continua a dare per scontata la proprietà delle risorse restituite dall'interfaccia dispositivo virtuale. L'interfaccia esegue l'elaborazione SignalAbort.

L'agente di completamento, se in esecuzione, completa tutti i comandi in attesa prima di restituire il controllo al chiamante. Tutti i comandi in attesa vengono completati con ERROR_OPERATION_ABORTED. Ovvero viene richiamata la funzione di callback per ognuno di questi comandi.

In tutti i casi, inclusi quelli in cui vengono restituiti errori, Close rilascia tutte le risorse per l'interfaccia dispositivo virtuale. Tutti i buffer e gli altri puntatori di interfaccia restituiti dall'interfaccia VDI diventano non validi.

È importante assicurarsi che l'agente di completamento sia stato terminato prima che la libreria COM venga scaricata. Se la libreria viene scaricata prima che l'agente di completamento restituisca il controllo al chiamante, il processo potrebbe causare una violazione dell'istruzione.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).