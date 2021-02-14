---
title: IClientVirtualDeviceSet2::GetBufferHandle
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IClientVirtualDeviceSet2::GetBufferHandle.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3751af7f6d784d58fce46dfe24605260f530a99e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347278"
---
# <a name="iclientvirtualdeviceset2getbufferhandle-vdi"></a>IClientVirtualDeviceSet2::GetBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Alcune applicazioni potrebbero richiedere più di un processo per operare sui buffer restituiti da **IClientVirtualDevice2::GetCommand**. In questi casi, il processo che riceve il comando può usare **GetBufferHandle** per ottenere un handle indipendente dal processo che identifica il buffer. Questo handle può quindi essere comunicato a qualsiasi altro processo che ha lo stesso set di dispositivi virtuali aperto. Tale processo può quindi usare IClientVirtualDeviceSet2::MapBufferHandle per ottenere l'indirizzo del buffer. L'indirizzo sarà probabilmente diverso da quello del partner, perché ogni processo può eseguire il mapping dei buffer a indirizzi diversi.

## <a name="syntax"></a>Sintassi

```c
HRESULT IClientVirtualDeviceSet2::GetBufferHandle (
   BYTE*         pBuffer,
   DWORD*      pBufferHandle
);
```

## <a name="parameters"></a>Parametri

*pBuffer* Indirizzo di un buffer ottenuto da un comando Read o Write.

*pBufferHandle* Viene restituito un identificatore univoco per il buffer.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Funzione completata. |
| VD_E_PROTOCOL | Il set di dispositivi virtuali non è attualmente aperto. |
| VD_E_INVALID | pBuffer non è un indirizzo valido. |

## <a name="remarks"></a>Osservazioni

Il processo che richiama la funzione GetBufferHandle è responsabile della chiamata a IClientVirtualDevice2::CompleteCommand al termine del trasferimento dei dati.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).