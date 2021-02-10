---
description: Proprietà Mode (ADO)
title: Proprietà Mode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Mode
- _Stream::Mode
- _Record::Mode
helpviewer_keywords:
- Mode property [ADO]
ms.assetid: 808661eb-0d7c-4e6d-8e40-9dc3bef3d77a
author: rothja
ms.author: jroth
ms.openlocfilehash: ae638bd51ea34a81efa5fb3192592fa8f3a005c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041781"
---
# <a name="mode-property-ado"></a>Proprietà Mode (ADO)
Indica le autorizzazioni disponibili per la modifica dei dati in una [connessione](./connection-object-ado.md), un [record](./record-object-ado.md)o un oggetto [flusso](./stream-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore [ConnectModeEnum](./connectmodeenum.md) . Il valore predefinito per una **connessione** è **adModeUnknown**. Il valore predefinito per un oggetto **record** è **adModeRead**. Il valore predefinito per un **flusso** associato a un'origine sottostante (aperto con un URL come origine o come **flusso** predefinito di un **record**) è **adModeRead**. Il valore predefinito per un **flusso** non associato a un'origine sottostante (di cui è stata creata un'istanza in memoria) è **adModeUnknown**.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **mode** per impostare o restituire le autorizzazioni di accesso in uso da parte del provider nella connessione corrente. È possibile impostare la proprietà **mode** solo quando l'oggetto **connessione** è chiuso.  
  
 Per un oggetto **Stream** , se la modalità di accesso non è specificata, viene ereditata dall'origine utilizzata per aprire l'oggetto **flusso** . Se, ad esempio, un **flusso** viene aperto da un oggetto **record** , per impostazione predefinita viene aperto nella stessa modalità del **record**.  
  
 Questa proprietà è di lettura/scrittura mentre l'oggetto è chiuso e in sola lettura mentre l'oggetto è aperto.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Se utilizzata su un oggetto **connessione** lato client, la proprietà **mode** può essere impostata solo su **adModeUnknown**.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Stream (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà IsolationLevel e Mode (VB)](./isolationlevel-and-mode-properties-example-vb.md)   
 [Esempio di proprietà IsolationLevel e Mode (VC + +)](./isolationlevel-and-mode-properties-example-vc.md)