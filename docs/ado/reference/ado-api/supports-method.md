---
description: Metodo Supports
title: Metodo Supports | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Supports
- Recordset15::Supports
helpviewer_keywords:
- Supports method [ADO]
ms.assetid: 298fc41c-0b55-42fc-b373-c5133b4da6a5
author: rothja
ms.author: jroth
ms.openlocfilehash: 26de39ffdd918a8281998b32d89c68fd1babf75e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170088"
---
# <a name="supports-method"></a>Metodo Supports
Determina se un oggetto [Recordset](./recordset-object-ado.md) specificato supporta un particolare tipo di funzionalità.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **booleano** che indica se tutte le funzionalità identificate dall'argomento *CursorOptions* sono supportate dal provider.  
  
#### <a name="parameters"></a>Parametri  
 *CursorOptions*  
 Espressione **Long** costituita da uno o più valori [CursorOptionEnum](./cursoroptionenum.md) .  
  
## <a name="remarks"></a>Commenti  
 Utilizzare il metodo **Supports** per determinare i tipi di funzionalità supportati da un oggetto **Recordset** . Se l'oggetto **Recordset** supporta le funzionalità le cui costanti corrispondenti si trovano in *CursorOptions*, il metodo **Supports** restituisce **true**. In caso contrario, restituisce **false**.  
  
> [!NOTE]
>  Sebbene il metodo **Supports** possa restituire **true** per una determinata funzionalità, non garantisce che il provider possa rendere la funzionalità disponibile in tutte le circostanze. Il metodo **Supports** restituisce semplicemente un valore che indica se il provider può supportare la funzionalità specificata, supponendo che vengano soddisfatte determinate condizioni. Il metodo **Supports** , ad esempio, può indicare che un oggetto **Recordset** supporta gli aggiornamenti anche se il cursore è basato su un join a più tabelle, alcune colonne di che non sono aggiornabili.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Supports (VB)](./supports-method-example-vb.md)   
 [Esempio di metodo Supports (VC + +)](./supports-method-example-vc.md)   
 [Proprietà CursorType (ADO)](./cursortype-property-ado.md)