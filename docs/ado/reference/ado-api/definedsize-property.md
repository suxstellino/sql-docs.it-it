---
description: Proprietà DefinedSize
title: Proprietà DefinedSize | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::DefinedSize
helpviewer_keywords:
- DefinedSize property [ADO]
ms.assetid: 3ee27314-a305-4fbc-8433-9ee9a909afd6
author: rothja
ms.author: jroth
ms.openlocfilehash: 756fe40bf7916dfa3e56a3e559be2874b5e8cd85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171311"
---
# <a name="definedsize-property"></a>Proprietà DefinedSize
Indica la capacità dei dati di un oggetto [campo](../../../ado/reference/ado-api/field-object.md) .  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **Long** che riflette la dimensione definita di un campo, che dipende dal tipo di dati dell'oggetto campo. Per ulteriori informazioni, vedere il [tipo](../../../ado/reference/ado-api/type-property-ado.md) . Per un campo che utilizza un tipo di dati a lunghezza fissa, il valore restituito corrisponde alla dimensione del tipo di dati in byte. Per un campo che utilizza un tipo di dati a lunghezza variabile, questo è uno dei seguenti:  
  
1.  Lunghezza massima del campo in caratteri (per **adVarChar** e **adVarWChar**) o in byte (per **adVarBinary** e **adVarNumeric**) se il campo ha una lunghezza definita. Ad esempio, il campo **adVarChar (5)** ha una lunghezza massima di 5.  
  
2.  Lunghezza massima del tipo di dati in caratteri (per **adChar** e **adWChar**) o in byte (per **adBinary** e **adNumeric**) se il campo non dispone di una lunghezza definita.  
  
3.  ~ 0 (bit per bit, il valore è diverso da 0; tutti i bit sono impostati su 1) se né il campo né il tipo di dati hanno una lunghezza massima definita.  
  
4.  Per i tipi di dati che non hanno una lunghezza, questo valore è impostato su ~ 0 (bit per bit, il valore è diverso da 0; tutti i bit sono impostati su 1).  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **DefinedSize** per determinare la capacità dei dati di un oggetto **campo** .  
  
 Le proprietà **DefinedSize** e [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) sono diverse. Si consideri, ad esempio, un oggetto **campo** con un tipo dichiarato di **adVarChar** e un valore della proprietà **DefinedSize** di 50, contenente un singolo carattere. Il valore della proprietà **ActualSize** restituito è la lunghezza in byte del singolo carattere.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Field](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActualSize e DefinedSize (VB)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [Esempio di proprietà ActualSize e DefinedSize (VC + +)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [Proprietà ActualSize (ADO)](../../../ado/reference/ado-api/actualsize-property-ado.md)
