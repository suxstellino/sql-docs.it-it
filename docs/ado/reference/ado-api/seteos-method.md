---
description: Metodo SetEOS
title: Metodo SetEos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SetEOS
- _Stream::SetEOS
helpviewer_keywords:
- SetEOS method [ADO]
ms.assetid: 707c18ca-6a56-4970-bbd6-ae1fb86a0b8a
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b9685f0ca0b5e9bdd1b484c7d424308619307d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170274"
---
# <a name="seteos-method"></a>Metodo SetEOS
Imposta la posizione che rappresenta la fine del flusso.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Stream.SetEOS  
```  
  
## <a name="remarks"></a>Osservazioni  
 **SetEOS** aggiorna il valore della proprietà [EOS](./eos-property.md) , rendendo la [posizione](./position-property-ado.md) corrente la fine del flusso. Tutti i byte o i caratteri successivi alla posizione corrente vengono troncati.  
  
 Poiché [Write](./write-method.md), [WRITETEXT](./writetext-method.md)e [CopyTo](./copyto-method-ado.md) non troncano valori aggiuntivi negli oggetti **flusso** esistenti, è possibile troncare questi byte o caratteri impostando la nuova posizione di fine del flusso con **SetEOS**.  
  
> [!CAUTION]
>  Se si imposta **EOS** su una posizione prima della fine effettiva del flusso, si perderanno tutti i dati dopo la nuova posizione **EOS** .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)