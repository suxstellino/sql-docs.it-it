---
title: IClientVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IClientVirtualDeviceSet2::Close.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: fee9c16399ba091df1878774c38751a2d92880fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347276"
---
# <a name="iclientvirtualdeviceset2close-vdi"></a>IClientVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **Close** chiude il set di dispositivi virtuali creato da IClientVirtualDeviceSet2::Create. Il risultato è il rilascio di tutte le risorse associate al set di dispositivi virtuali.

## <a name="syntax"></a>Sintassi

```c
HRESULT IClientVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Viene restituito dopo la corretta chiusura del set di dispositivi virtuali. |
| VD_E_PROTOCOL | Non è stata eseguita alcuna azione perché il set di dispositivi virtuali non era aperto. |
| VD_E_OPEN | I dispositivi erano ancora aperti. |

## <a name="remarks"></a>Osservazioni

La chiamata di Close è una dichiarazione del client che tutte le risorse usate dal set di dispositivi virtuali devono essere rilasciate. Il client deve verificare che tutte le attività che coinvolgono i buffer di dati e i dispositivi virtuali siano terminate prima di richiamare Close. Tutte le interfacce di dispositivo virtuale restituite da OpenDevice vengono invalidate da Close.

Il client è autorizzato a eseguire una chiamata Create sull'interfaccia del set di dispositivi virtuali al termine della chiamata di Close. Questa chiamata creerebbe un nuovo set di dispositivi virtuali per un'operazione di backup o ripristino successiva.

Se Close viene chiamato quando uno o più dispositivi virtuali sono ancora aperti, viene restituito VD_E_OPEN. In questo caso, SignalAbort viene attivato internamente per garantire un arresto corretto, se possibile. Le risorse VDI vengono rilasciate. Il client deve attendere un'indicazione VD_E_CLOSE su ogni dispositivo prima di richiamare IClientVirtualDeviceSet2::Close. Se il client sa che il set di dispositivi virtuali si trova già in uno stato di chiusura anomala, non deve attendere un'indicazione VD_E_CLOSE da GetCommand e può richiamare IClientVirtualDeviceSet2::Close appena terminano le attività sui buffer condivisi.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).