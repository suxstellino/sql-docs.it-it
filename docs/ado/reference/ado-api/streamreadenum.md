---
description: StreamReadEnum
title: StreamReadEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamReadEnum
helpviewer_keywords:
- StreamReadEnum enumeration [ADO]
ms.assetid: cfa1b416-003a-436f-a21b-bd2397e54db3
author: rothja
ms.author: jroth
ms.openlocfilehash: 8fe599d5a737cb1f7d1eef0120c5e0b18d2de31a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056566"
---
# <a name="streamreadenum"></a>StreamReadEnum
Specifica se l'intero flusso o la riga successiva devono essere letti da un oggetto [flusso](./stream-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adReadAll**|-1|Valore predefinito. Legge tutti i byte dal flusso, dalla posizione corrente verso l'indicatore [EOS](./eos-property.md) . Si tratta dell'unico valore **StreamReadEnum** valido con flussi binari (il [tipo](./type-property-ado-stream.md) è **adTypeBinary**).|  
|**adReadLine**|-2|Legge la riga successiva dal flusso (designato dalla proprietà [LineSeparator](./lineseparator-property-ado.md) ).|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Metodo Read](./read-method.md)  
    :::column-end:::
    :::column:::
        [Metodo ReadText](./readtext-method.md)  
    :::column-end:::
:::row-end:::