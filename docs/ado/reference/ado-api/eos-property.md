---
description: Proprietà EOS
title: Proprietà EOS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EOS
- Stream::EOS
helpviewer_keywords:
- EOS property
ms.assetid: 57e08c5f-f3ed-4ecd-8c66-50b83b1031d1
author: rothja
ms.author: jroth
ms.openlocfilehash: 45268bfba7669f3c202cec9bc652424981e1234e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025125"
---
# <a name="eos-property"></a>Proprietà EOS
Indica se la posizione corrente è alla fine del [flusso](../../../ado/reference/ado-api/stream-object-ado.md).  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **booleano** che indica se la posizione corrente è alla fine del flusso. **EOS** restituisce **true** se non sono presenti altri byte nel flusso. restituisce **false** se sono presenti più byte che seguono la posizione corrente.  
  
 Per impostare la fine della posizione del flusso, utilizzare il metodo [SetEOS](../../../ado/reference/ado-api/seteos-method.md) . Per determinare la posizione corrente, usare la proprietà [position](../../../ado/reference/ado-api/position-property-ado.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà EOS e LineSeparator e metodo SkipLine (VB)](../../../ado/reference/ado-api/eos-and-lineseparator-properties-and-skipline-method-example-vb.md)   
 [Oggetto Stream (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)
