---
description: Metodo Flush (ADO)
title: Metodo Flush (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Flush
- _Stream::raw_Flush
helpviewer_keywords:
- Flush method [ADO]
ms.assetid: 938522b4-f836-4c80-8d27-a598a000f0ee
author: rothja
ms.author: jroth
ms.openlocfilehash: ce390555df75e2fd1d1a9a68257ba5952ee482f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167298"
---
# <a name="flush-method-ado"></a>Metodo Flush (ADO)
Impone il contenuto del [flusso](./stream-object-ado.md) rimanente nel buffer ADO all'oggetto sottostante a cui è associato il **flusso** .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Stream.Flush  
```  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo può essere utilizzato per inviare il contenuto del buffer del flusso all'oggetto sottostante, ad esempio il nodo o il file rappresentato dall'URL che rappresenta l'origine dell'oggetto **flusso** . Questo metodo deve essere chiamato quando si desidera assicurarsi che tutte le modifiche apportate al contenuto di un **flusso** siano state scritte. Tuttavia, con ADO, in genere non è necessario chiamare **Flush**, in quanto ADO Scarica continuamente il buffer nel modo più possibile in background. Le modifiche al contenuto di un **flusso** vengono eseguite automaticamente e non vengono memorizzate nella cache finché non viene chiamato lo **svuotamento** .  
  
 La chiusura di un **flusso** con il metodo [Close](./close-method-ado.md) Scarica automaticamente il contenuto di un **flusso** ; non è necessario chiamare in modo esplicito lo **svuotamento** immediatamente prima della **chiusura**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)