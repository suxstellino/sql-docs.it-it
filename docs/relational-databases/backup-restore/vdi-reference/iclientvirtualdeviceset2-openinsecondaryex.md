---
title: IClientVirtualDeviceSet2::OpenInSecondaryEx
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IClientVirtualDeviceSet2::OpenInSecondaryEx.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 18e53141365aa4581cef05c14d35cf53c8f24387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347209"
---
# <a name="iclientvirtualdeviceset2openinsecondaryex-vdi"></a>IClientVirtualDeviceSet2::OpenInSecondaryEx (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **OpenInSecondaryEx** apre il set di dispositivi virtuali in un client secondario. Il client primario deve avere già usato CreateEx e GetConfiguration per configurare il set di dispositivi virtuali.

## <a name="syntax"></a>Sintassi

```c
HRESULT IClientVirtualDeviceSet2::OpenInSecondaryEx (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpSetName
);
```

## <a name="parameters"></a>Parametri

*lpInstanceName* Questa stringa identifica l'istanza di SQL Server a cui verrà inviato il comando SQL.

*lpSetName* Identifica il set. Questo nome fa distinzione tra maiuscole e minuscole e deve corrispondere al nome usato dal client primario per richiamare IClientVirtualDeviceSet2::Create.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Funzione completata. |
| VD_E_PROTOCOL | Il set di dispositivi virtuali è stato aperto o il set di dispositivi virtuali non è pronto per accettare richieste di apertura dai client secondari. |
| VD_E_ABORT | Operazione interrotta. |

## <a name="remarks"></a>Osservazioni

Quando si usa un modello a più processi, il client primario è responsabile del rilevamento della terminazione normale e anomala dei client secondari.

Il nome dell'istanza deve identificare l'istanza di a cui viene inviato il comando T-SQL. NULL identifica l'istanza predefinita. Non è accettato alcun prefisso "machineName\".

OpenInSecondaryEx sostituisce l'originale IClientVirtualDeviceSet::OpenInSecondary, definito nell'interfaccia di SQL Server versione 7.0 originale. Per i nuovi progetti di sviluppo usare OpenInSecondaryEx.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).