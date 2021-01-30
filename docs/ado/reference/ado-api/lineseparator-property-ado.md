---
description: Proprietà LineSeparator (ADO)
title: Proprietà LineSeparator (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::LineSeparator
helpviewer_keywords:
- LineSeparator property [ADO]
ms.assetid: 0b20fbb8-6b83-48ec-b442-f96c8a4bafbb
author: rothja
ms.author: jroth
ms.openlocfilehash: 629d2948bbbf62987f1352712df02521cf002189
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167174"
---
# <a name="lineseparator-property-ado"></a>Proprietà LineSeparator (ADO)
Indica il carattere binario da utilizzare come separatore di riga negli oggetti del [flusso](./stream-object-ado.md) di testo.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore [LineSeparatorsEnum](./lineseparatorsenum.md) che indica il carattere separatore di riga usato nel **flusso**. Il valore predefinito è **adCRLF**.  
  
## <a name="remarks"></a>Commenti  
 **LineSeparator** viene utilizzato per interpretare le linee durante la lettura del contenuto di un **flusso** di testo. Le righe possono essere ignorate con il metodo [SkipLine](./skipline-method.md) .  
  
 **LineSeparator** viene utilizzato solo con gli oggetti del **flusso** di testo (il [tipo](./type-property-ado-stream.md) è **adTypeText**). Questa proprietà viene ignorata se il **tipo** è **adTypeBinary**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Stream (ADO)](./stream-object-ado.md)