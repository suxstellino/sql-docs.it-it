---
description: Metodo Write
title: Metodo Write | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_Write
- _Stream::Write
helpviewer_keywords:
- Write method [ADO]
ms.assetid: 02982e6a-ac5f-4af2-b82e-ce12534b84b2
author: rothja
ms.author: jroth
ms.openlocfilehash: 312eba9678843932103875acd2a170a2e98f148c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056046"
---
# <a name="write-method"></a>Metodo Write
Scrive dati binari in un oggetto [flusso](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Stream.Write Buffer  
```  
  
#### <a name="parameters"></a>Parametri  
 *Buffer*  
 **Variant** che contiene una matrice di byte da scrivere.  
  
## <a name="remarks"></a>Commenti  
 I byte specificati vengono scritti nell'oggetto **flusso** senza spazi intermedi tra i byte.  
  
 La [posizione](./position-property-ado.md) corrente è impostata sul byte che segue i dati scritti. Il metodo **Write** non tronca il resto dei dati in un flusso. Se si desidera troncare questi byte, chiamare [SetEOS](./seteos-method.md).  
  
 Se si scrive oltre la posizione [EOS](./eos-property.md) corrente, le [dimensioni](./size-property-ado-stream.md) del **flusso** verranno aumentate in modo da contenere nuovi byte e **EOS** passerà al nuovo ultimo byte nel **flusso**.  
  
> [!NOTE]
>  Il metodo **Write** viene usato con i flussi binari (il [tipo](./type-property-ado-stream.md) è **adTypeBinary**). Per i flussi di testo (**tipo** è **adTypeText**), usare [WRITETEXT](./writetext-method.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo WriteText](./writetext-method.md)