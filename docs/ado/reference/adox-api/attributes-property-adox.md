---
description: Proprietà Attributes (ADOX)
title: Proprietà Attributes (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::put_Attributes
- _Column::Attributes
- _Column::PutAttributes
- _Column::get_Attributes
- _Column::GetAttributes
helpviewer_keywords:
- Attributes property [ADOX]
ms.assetid: e3abb359-79a3-4c22-b3a8-2900817e0d23
author: rothja
ms.author: jroth
ms.openlocfilehash: 8db5140c3adbc0df74c4aae26fba9eebf98f21b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050432"
---
# <a name="attributes-property-adox"></a>Proprietà Attributes (ADOX)
Descrive le caratteristiche della colonna.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Long** . Il valore specifica le caratteristiche della tabella rappresentata dall'oggetto [colonna](./column-object-adox.md) . Il valore può essere una combinazione di costanti [ColumnAttributesEnum](./columnattributesenum.md) . Il valore predefinito è zero (**0**), che non è né **adColFixed** né **adColNullable**.  
  
## <a name="applies-to"></a>Si applica a  
  
- [Oggetto Column (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio della proprietà Attributes (VB)](./attributes-property-example-vb.md)