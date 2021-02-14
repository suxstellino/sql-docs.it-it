---
title: IServerVirtualDeviceSet2::Open
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::Open.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: bb5341a03ee64b3b9d1d23508e1e59048f843b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347080"
---
# <a name="iservervirtualdeviceset2open-vdi"></a>IServerVirtualDeviceSet2::Open (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **Open** apre il set di dispositivi virtuali nel server e deve essere la prima chiamata eseguita usando l'handle di interfaccia fornito da COM.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::Open (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpName
);
```

## <a name="parameters"></a>Parametri

*lpInstanceName* Questa stringa identifica l'istanza di SQL Server a cui verrà inviato il comando SQL. È possibile passare NULL per identificare l'istanza predefinita nel computer corrente.

*lpName* Fornito dalla prima clausola VIRTUAL_DEVICE= del comando BACKUP o RESTORE. Questo nome viene usato come chiave per ottenere l'accesso al set di dispositivi virtuali creato dal client.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Funzione completata. |
| VD_E_INVALID | Il nome fornito non ha identificato un set di dispositivi virtuali accessibile per il server. |

## <a name="remarks"></a>Osservazioni

Dopo che questa funzione è stata richiamata correttamente, il server può procedere alla configurazione del set di dispositivi virtuali tramite GetConfiguration e SetConfiguration.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).