---
description: StreamOpenOptionsEnum
title: StreamOpenOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamOpenOptionsEnum
helpviewer_keywords:
- StreamOpenOptionsEnum enumeration [ADO]
ms.assetid: 85b6c57f-47ed-46ba-bd92-07882ae9e9d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 5da222318b827eae05d07d7cad4a121a568d6540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166404"
---
# <a name="streamopenoptionsenum"></a>StreamOpenOptionsEnum
Specifica le opzioni per l'apertura di un oggetto [flusso](./stream-object-ado.md) . I valori possono essere combinati con un'operazione OR.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adOpenStreamAsync**|1|Apre l'oggetto **flusso** in modalità asincrona.|  
|**adOpenStreamFromRecord**|4|Identifica il contenuto del parametro di *origine* come oggetto [record](./record-object-ado.md) già aperto. Il comportamento predefinito consiste nel considerare *source* come un URL che punta direttamente a un nodo in una struttura ad albero. Viene aperto il flusso predefinito associato al nodo.|  
|**adOpenStreamUnspecified**|-1|Valore predefinito. Specifica l'apertura dell'oggetto **flusso** con le opzioni predefinite.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo Open (Stream - ADO)](./open-method-ado-stream.md)