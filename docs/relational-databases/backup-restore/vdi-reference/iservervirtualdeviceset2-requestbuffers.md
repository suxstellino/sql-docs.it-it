---
title: IServerVirtualDeviceSet2::RequestBuffers
titlesuffix: SQL Server VDI reference
description: Questo articolo include informazioni di riferimento sul comando IServerVirtualDeviceSet2::RequestBuffers.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: ddbafcb7c9389fbe26252e341a9d73056a9c1ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347061"
---
# <a name="iservervirtualdeviceset2requestbuffers-vdi"></a>IServerVirtualDeviceSet2::RequestBuffers (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La funzione **RequestBuffers** informa l'interfaccia VDI che il server necessiterà di un certo numero di buffer con requisiti specifici di dimensioni e allineamento.

## <a name="syntax"></a>Sintassi

```c
HRESULT IServerVirtualDeviceSet2::RequestBuffers (
   DWORD   dwSize,
   DWORD   dwAlignment,
   DWORD   dwCount
);
```

## <a name="parameters"></a>Parametri

*dwSize* identifica le dimensioni di ogni buffer. Queste dimensioni devono includere solo la regione necessaria per i dati. L'interfaccia VDI gestisce tutti i requisiti di allineamento e prefisso.

*dwAlignment* Allineamento richiesto per questi buffer. È possibile usare un valore più restrittivo del valore di allineamento di base specificato con 'BeginConfiguration'. Se il valore è 0, per impostazione predefinita verrà impostato il valore di allineamento di base.

*dwCount* Numero di buffer che verranno richiesti da 'AllocateBuffer' con le dimensioni e l'allineamento specificati.

## <a name="return-value"></a>Valore restituito

|Valore restituito | Spiegazione |
|---|---|
| NOERROR | Funzione completata. |
| VD_E_ABORT | È stata richiesta l'interruzione. |
| VD_E_PROTOCOL | Il set non si trova in uno stato in cui possono essere dichiarate le allocazioni del buffer (vedere la matrice delle transizioni di stato). |
| VD_E_MEMORY | Non è stato possibile ottenere la memoria richiesta. |

## <a name="remarks"></a>Osservazioni

Questo metodo deve essere usato prima dell'allocazione dei buffer con AllocateBuffer. I set di buffer con dimensioni e allineamento specifici vengono richiesti con RequestBuffers e quindi i singoli buffer vengono allocati con AllocateBuffer.

Durante la fase di configurazione, le chiamate di RequestBuffers vengono "sommate" in modo che alla chiamata di EndConfiguration sia possibile usare una singola area del buffer, che viene allocata in quel momento. Al termine della configurazione, qualsiasi chiamata di RequestBuffers comporta l'allocazione immediata di più spazio nel buffer.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Informazioni di riferimento sull'interfaccia dispositivo virtuale di SQL Server](reference-virtual-device-interface.md).