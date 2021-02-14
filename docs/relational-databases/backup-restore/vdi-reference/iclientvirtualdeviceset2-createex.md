---
title: IClientVirtualDeviceSet2::CreateEx
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IClientVirtualDeviceSet2::CreateEx.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: b65dfc635b69519aae4198844a166373234559b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347245"
---
# <a name="iclientvirtualdeviceset2createex-vdi"></a>IClientVirtualDeviceSet2::CreateEx (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **CreateEx** crea il set di dispositivi virtuali.

## <a name="syntax"></a>Sintassi

```c
HRESULT IClientVirtualDeviceSet2::CreateEx (
   LPCWSTR         lpInstanceName,
   LPCWSTR         lpName,
   VDConfig*      pCfg
);
```

## <a name="parameters"></a>Parametri

*lpInstanceName* Questa stringa identifica l'istanza di SQL Server a cui verrà inviato il comando SQL.

*lpName* Identifica il set di dispositivi virtuali. È necessario seguire le regole per i nomi usati da CreateFileMapping(). È possibile usare qualsiasi carattere eccetto la barra rovesciata (\)). Si tratta di una stringa Unicode a caratteri "wide". È consigliabile anteporre alla stringa il nome del prodotto o della società dell'utente e il nome del database.

*pCfg* Configurazione per il set di dispositivi virtuali. Per altre informazioni, vedere Configurazione.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Funzione completata. |
| VD_E_NOTSUPPORTED | Uno o più campi della configurazione non sono validi o altrimenti non supportati. |
| VD_E_PROTOCOL | Il set di dispositivi virtuali è stato creato. |

## <a name="remarks"></a>Osservazioni

Il metodo CreateEx deve essere chiamato una sola volta per ogni operazione di backup o ripristino. Dopo aver richiamato il metodo Close, il client può riutilizzare l'interfaccia per creare un altro set di dispositivi virtuali.

Il nome dell'istanza deve identificare l'istanza a cui viene inviato il comando Transact-SQL. NULL identifica l'istanza predefinita. Non è accettato alcun prefisso "machineName\".

Le chiamate a CreateEx (e Create) modificheranno il DACL di sicurezza nell'handle di processo nel processo client. Per questo motivo, qualsiasi altra modifica dell'handle di processo deve essere serializzata con la chiamata di CreateEx. CreateEx eseguirà la serializzazione con altre chiamate a CreateEx, ma non sarà in grado di eseguire la serializzazione con l'elaborazione esterna. L'accesso viene concesso all'account che esegue il servizio SQL Server.

Il metodo CreateEx sostituisce il metodo Create definito nell'oggetto IClientVirtualDeviceSet originale. Il metodo Create originale è deprecato e non deve essere usato in futuro per lo sviluppo. Il metodo Create originale implementa un tipo di supporto per il nome dell'istanza con la variabile di ambiente _VIRTUAL_SERVER_NAME_. Se tale variabile è impostata nell'ambiente, il metodo Create chiama internamente CreateEx, passando il valore di _VIRTUAL_SERVER_NAME_ come nome dell'istanza.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).