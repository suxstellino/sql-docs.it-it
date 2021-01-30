---
description: Proprietà IsolationLevel
title: Proprietà IsolationLevel | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::IsolationLevel
helpviewer_keywords:
- IsolationLevel property
ms.assetid: ea84e4b2-fbf2-4eef-b9ce-796b22e21800
author: rothja
ms.author: jroth
ms.openlocfilehash: dc9f116c565321051184d16fa0cdf963cc4bfe0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170926"
---
# <a name="isolationlevel-property"></a>Proprietà IsolationLevel
Indica il livello di isolamento per un oggetto [Connection](./connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore [IsolationLevelEnum](./isolationlevelenum.md) . Il valore predefinito è **adXactReadCommitted**.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **IsolationLevel** per impostare il livello di isolamento di un oggetto **Connection** . L'impostazione non ha effetto fino alla successiva chiamata del metodo [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) . Se il livello di isolamento richiesto non è disponibile, il provider può restituire il livello di isolamento successivo senza aggiornare la proprietà **IsolationLevel** .  
  
 La proprietà **IsolationLevel** è di lettura/scrittura.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Se utilizzata su un oggetto **connessione** lato client, la proprietà **IsolationLevel** può essere impostata solo su **adXactUnspecified**. Poiché gli utenti utilizzano oggetti **Recordset** disconnessi in una cache sul lato client, è possibile che si verifichino problemi multiutente. Ad esempio, quando due utenti diversi tentano di aggiornare lo stesso record, Remote Data Service consente semplicemente all'utente che aggiorna il record prima di "vincere". La richiesta di aggiornamento del secondo utente avrà esito negativo con un errore.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Connection (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà IsolationLevel e Mode (VB)](./isolationlevel-and-mode-properties-example-vb.md)   
 [Esempio di proprietà IsolationLevel e Mode (VC + +)](./isolationlevel-and-mode-properties-example-vc.md)