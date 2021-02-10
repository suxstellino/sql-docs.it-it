---
description: Metodo WriteText
title: Metodo WriteText | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d310e2beaf69968721e2edc5c65c57cb88cf492
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056036"
---
# <a name="writetext-method"></a>Metodo WriteText
Scrive una stringa di testo specificata in un oggetto [flusso](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>Parametri  
 *Dati*  
 Valore **stringa** che contiene il testo in caratteri da scrivere.  
  
 *Opzioni*  
 facoltativo. Valore [StreamWriteEnum](./streamwriteenum.md) che specifica se è necessario scrivere un carattere separatore di riga alla fine della stringa specificata.  
  
## <a name="remarks"></a>Commenti  
 Le stringhe specificate vengono scritte nell'oggetto **flusso** senza spazi o caratteri intermedi tra ogni stringa.  
  
 La [posizione](./position-property-ado.md) corrente è impostata sul carattere che segue i dati scritti. Il metodo **WRITETEXT** non tronca il resto dei dati in un flusso. Se si desidera troncare questi caratteri, chiamare [SetEOS](./seteos-method.md).  
  
 Se si scrive oltre la posizione [EOS](./eos-property.md) corrente, le [dimensioni](./size-property-ado-stream.md) del **flusso** verranno aumentate in modo da contenere nuovi caratteri e **EOS** passerà al nuovo ultimo byte nel **flusso**.  
  
> [!NOTE]
>  Il metodo **WRITETEXT** viene usato con i flussi di testo (il [tipo](./type-property-ado-stream.md) è **adTypeText**). Per i flussi binari (**tipo** è **adTypeBinary**), usare [Write](./write-method.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Write](./write-method.md)