---
title: IServerVirtualDeviceSet2::AllocateBuffer
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::AllocateBuffer.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42e51b53e7d19ba0ced7116acbb7505e98afd597
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347158"
---
# <a name="iservervirtualdeviceset2allocatebuffer-vdi"></a>IServerVirtualDeviceSet2::AllocateBuffer (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **AllocateBuffer** ottiene un buffer di memoria condiviso dal set di dispositivi virtuali.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::AllocateBuffer (
   LPVOID*      ppBuffer,
   UINT32      dwSize,
   UINT32      dwAlignment
);
```

## <a name="parameters"></a>Parametri

*ppBuffer* Restituisce un puntatore all'inizio del buffer.

*dwSize* Dimensioni del buffer in byte. Non include alcuna zona di prefisso richiesta dal client. L'eventuale zona è nascosta dal server e ci sarà spazio disponibile prima della restituzione dell'indirizzo del buffer.

*dwAlignment* Specifica il limite di allineamento per il buffer. Ad esempio, il valore 4096 assicura che il buffer sia allineato a un limite di 4096 byte. Questo significa che i 12 bit di ordine inferiore dell'indirizzo restituito saranno impostati su zero. Questo parametro deve essere una potenza di 2.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Viene restituito il buffer. |
| VD_E_MEMORY | Si è verificata una condizione di memoria insufficiente. |
| VD_E_INVALID | Parametro non valido. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).