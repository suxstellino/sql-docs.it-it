---
description: Proprietà OriginalValue (ADO)
title: Proprietà OriginalValue (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::OriginalValue
helpviewer_keywords:
- OriginalValue property [ADO]
ms.assetid: 6e33c6ec-14d9-4b1d-ba9b-cb99862e7bac
author: rothja
ms.author: jroth
ms.openlocfilehash: dd53935335a4f5df694a5a5175f2bd7d3505a108
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170646"
---
# <a name="originalvalue-property-ado"></a>Proprietà OriginalValue (ADO)
Indica il valore di un [campo](./field-object.md) esistente nel record prima che siano state apportate modifiche.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **Variant** che rappresenta il valore di un campo prima di qualsiasi modifica.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **OriginalValue** per restituire il valore del campo originale per un campo dal record corrente.  
  
 In *modalità di aggiornamento immediato* (in cui il provider scrive le modifiche nell'origine dati sottostante dopo aver chiamato il metodo [Update](./update-method.md) ), la proprietà **OriginalValue** restituisce il valore del campo esistente prima delle modifiche, ovvero dall'ultima chiamata al metodo **Update** . Si tratta dello stesso valore utilizzato dal metodo [CancelUpdate](./cancelupdate-method-ado.md) per sostituire la proprietà [value](./value-property-ado.md) .  
  
 In *modalità di aggiornamento batch* (in cui il provider memorizza nella cache più modifiche e le scrive nell'origine dati sottostante solo quando si chiama il metodo [UpdateBatch](./updatebatch-method.md) ), la proprietà **OriginalValue** restituisce il valore del campo esistente prima delle modifiche (ovvero, dall'ultima chiamata al metodo **UpdateBatch** ). Si tratta dello stesso valore utilizzato dal metodo [CancelBatch](./cancelbatch-method-ado.md) per sostituire la proprietà **value** . Quando si usa questa proprietà con la proprietà [UnderlyingValue](./underlyingvalue-property.md) , è possibile risolvere i conflitti che si verificano da aggiornamenti batch.  
  
## <a name="record"></a>Registra  
 Per gli oggetti [record](./record-object-ado.md) , la proprietà **OriginalValue** sarà vuota per i campi aggiunti prima della chiamata a [Update](./update-method.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Field](./field-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà OriginalValue e UnderlyingValue (VB)](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [Esempio di proprietà OriginalValue e UnderlyingValue (VC + +)](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [Proprietà UnderlyingValue](./underlyingvalue-property.md)