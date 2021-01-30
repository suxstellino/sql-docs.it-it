---
description: Proprietà Prepared (ADO)
title: Proprietà preparata (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Prepared
helpviewer_keywords:
- Prepared property [ADO]
ms.assetid: 11ca8825-765e-4bb4-a6ce-3f6564ad8755
author: rothja
ms.author: jroth
ms.openlocfilehash: f20fb9b9f4050ebb0f382b2e36e2f7d5b6231fa9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170561"
---
# <a name="prepared-property-ado"></a>Proprietà Prepared (ADO)
Indica se salvare una versione compilata di un [comando](./command-object-ado.md) prima dell'esecuzione.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **booleano** che, se impostato su **true**, indica che il comando deve essere preparato.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **preparata** per fare in modo che il provider salvi una versione preparata (o compilata) della query specificata nella proprietà [CommandText](./commandtext-property-ado.md) prima della prima esecuzione di un oggetto [comando](./command-object-ado.md) . Questo può rallentare la prima esecuzione di un comando, ma una volta che il provider compila un comando, il provider utilizzerà la versione compilata del comando per le esecuzioni successive, con conseguente miglioramento delle prestazioni.  
  
 Se la proprietà è **false**, il provider eseguirà direttamente l'oggetto **Command** senza creare una versione compilata.  
  
 Se il provider non supporta la preparazione dei comandi, può restituire un errore quando questa proprietà è impostata su **true**. Se il provider non restituisce un errore, ignora semplicemente la richiesta di preparazione del comando e imposta la proprietà **preparata** su **false**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà preparata (VB)](./prepared-property-example-vb.md)   
 [Esempio della proprietà Prepared (VC++)](./prepared-property-example-vc.md)