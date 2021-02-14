---
title: IClientVirtualDeviceSet2::MapBufferHandle
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IClientVirtualDeviceSet2::MapBufferHandle.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28ce82433b718bc98c6a27e66385b97996fe65a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347219"
---
# <a name="iclientvirtualdeviceset2mapbufferhandle-vdi"></a>IClientVirtualDeviceSet2::MapBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **MapBufferHandle** viene usata per ottenere un indirizzo del buffer valido da un handle del buffer ottenuto da un altro processo.

## <a name="syntax"></a>Sintassi

```c
HRESULT IClientVirtualDeviceSet2::MapBufferHandle (
   DWORD      dwBuffer,
   BYTE**      ppBuffer
);
```

## <a name="parameters"></a>Parametri

*dwBuffer* Handle restituito da IClientVirtualDeviceSet2::GetBufferHandle.

*ppBuffer* Indirizzo del buffer valido nel processo corrente.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Funzione completata. |
| VD_E_PROTOCOL | Il set di dispositivi virtuali non è attualmente aperto. |
| VD_E_INVALID | ppBuffer è un handle non valido. |

## <a name="remarks"></a>Osservazioni

Prestare attenzione a comunicare correttamente gli handle. Gli handle sono locali in un singolo set di dispositivi virtuali. I processi partner che condividono un handle devono assicurarsi che gli handle del buffer vengano usati solo nell'ambito del set di dispositivi virtuali da cui è stato originariamente ottenuto il buffer.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).