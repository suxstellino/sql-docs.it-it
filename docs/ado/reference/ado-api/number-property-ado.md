---
description: Proprietà Number (ADO)
title: Proprietà Number (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Number
- Error::GetNumber
- Error::get_Number
helpviewer_keywords:
- number property [ADO]
ms.assetid: f92323c5-dd11-4a63-a505-d9014a0f067f
author: rothja
ms.author: jroth
ms.openlocfilehash: c39192c6158dfdd180c6f3235668157b9750e1e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041561"
---
# <a name="number-property-ado"></a>Proprietà Number (ADO)
Indica il numero che identifica in modo univoco un oggetto [Error](./error-object.md) .  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **Long** che può corrispondere a una delle costanti [ErrorValueEnum](./errorvalueenum.md) .  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **Number** per determinare l'errore che si è verificato. Il valore della proprietà è un numero univoco che corrisponde alla condizione di errore.  
  
 La raccolta [Errors](./errors-collection-ado.md) restituisce un valore HRESULT in formato esadecimale, ad esempio 0x80004005, o valore Long (ad esempio, 2147467259). Questi valori HRESULT possono essere generati dai componenti sottostanti, ad esempio OLE DB o persino OLE. Per ulteriori informazioni su questi numeri, vedere la pagina relativa agli [errori (OLE DB)](/previous-versions/windows/desktop/ms724533(v=vs.85)) nella Guida [di riferimento per programmatori OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85))*.*  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Error](./error-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description (proprietà)](./description-property.md)   
 [HelpContext, proprietà di filelima](./helpcontext-helpfile-properties.md)   
 [Proprietà Source (Error - ADO)](./source-property-ado-error.md)