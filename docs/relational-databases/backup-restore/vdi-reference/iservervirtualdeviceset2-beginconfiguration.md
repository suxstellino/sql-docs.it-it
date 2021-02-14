---
title: IServerVirtualDeviceSet2::BeginConfiguration
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::BeginConfiguration.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0d543ee25e92dbaccf44719f98bc838030fa3005
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347151"
---
# <a name="iservervirtualdeviceset2beginconfiguration-vdi"></a>IServerVirtualDeviceSet2::BeginConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Il server richiama la funzione **BeginConfiguration** per iniziare la configurazione del set di dispositivi virtuali.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::BeginConfiguration (
   DWORD   dwFeatures,
   DWORD   dwAlignment,
   DWORD   dwBlockSize,
   DWORD   dwMaxTransferSize,
   DWORD   dwTimeout
);
```

## <a name="parameters"></a>Parametri

*dwFeatures* Maschera delle funzionalità modificate. VDF_WriteMedia e/o VDF_ReadMedia.

*dwAlignment* Allineamento finale. Se 0, l'impostazione predefinita è dwBlockSize. Deve essere una potenza di 2, >= dwBlockSize e <= 64 KB.

*dwBlockSize* Unità minima di trasferimento, in byte. Deve essere una potenza di 2, >=512 e <= 64 KB.

*dwMaxTransferSize* Il trasferimento più grande che verrà tentato. Deve essere un multiplo di 64 KB.

*dwTimeout* Millisecondi di attesa che il client primario completi la dichiarazione delle aree del buffer che fornirà.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Il set di dispositivi virtuali è in stato configurabile. |
| VD_E_ABORT | È stato richiamato SignalAbort. |
| VD_E_PROTOCOL | Il set di dispositivi virtuali non si trova nello stato connesso. |

## <a name="remarks"></a>Osservazioni

Dopo che questa funzione è stata richiamata, il set di dispositivi virtuali passa allo stato configurabile, in cui viene deciso il layout del buffer.
Una volta impostata la configurazione di base, in base ai parametri, questi valori rimangono fissi per il ciclo di vita del set di dispositivi virtuali. La proprietà di allineamento per il set di dispositivi virtuali viene usata per controllare l'allineamento dei buffer di dati. Questo valore imposta un valore di allineamento minimo che può essere sottoposto a override in base ai singoli buffer.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).