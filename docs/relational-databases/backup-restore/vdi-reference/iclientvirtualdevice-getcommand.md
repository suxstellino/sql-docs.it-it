---
title: IClientVirtualDevice::GetCommand
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IClientVirtualDevice::GetCommand.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: f57e2b71e99f0d4fcc1370aa6da0bcd5310c6a13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347257"
---
# <a name="iclientvirtualdevicegetcommand-vdi"></a>IClientVirtualDevice::GetCommand (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **GetCommand** viene usata per ottenere il comando successivo accodato per un dispositivo. Quando richiesto, questa funzione attende il comando successivo.

## <a name="syntax"></a>Sintassi

```c
HRESULT IClientVirtualDevice::GetCommand (
   DWORD               dwTimeOut,
   VDC_Command**      const ppCmd
);
```

## <a name="parameters"></a>Parametri

*ppCmd* Quando un comando viene restituito correttamente, il parametro restituisce l'indirizzo di un comando da eseguire. La memoria restituita è di sola lettura. Quando il comando viene completato, questo puntatore viene passato alla routine CompleteCommand. Per altre informazioni su ogni comando, vedere Comandi.

*dwTimeOut* Tempo di attesa, in millisecondi. Usare INFINTE per un'attesa illimitata. Usare 0 per eseguire il polling di un comando. Se non è attualmente disponibile alcun comando, viene restituito VD_E_TIMEOUT. Se si verifica il timeout, il client decide l'azione successiva.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | È stato recuperato un comando. |
| VD_E_CLOSE | Il dispositivo è stato chiuso dal server. |
| VD_E_TIMEOUT | Nessun comando disponibile e timeout scaduto. |
| VD_E_ABORT | Il client o il server ha usato SignalAbort per forzare l'arresto. |

## <a name="remarks"></a>Osservazioni

La restituzione di VD_E_CLOSE indica che SQL Server ha chiuso il dispositivo. Fa parte della normale procedura di arresto. Dopo la chiusura di tutti i dispositivi, il client richiama IClientVirtualDeviceSet2::Close per chiudere il set di dispositivi virtuali.

Quando questa routine deve bloccarsi per attendere un comando, il thread viene lasciato in una condizione di avviso.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).