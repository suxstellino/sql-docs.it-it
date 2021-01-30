---
description: Proprietà Precision (ADOX)
title: Proprietà Precision (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::put_Precision
- _Column::PutPrecision
- _Column::GetPrecision
- _Column::get_Precision
- _Column::Precision
helpviewer_keywords:
- Precision property [ADOX]
ms.assetid: 0e0ecbbf-d7de-49d4-a128-5a519ecd54ba
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ecb674380de526f729246a249e2369f0cf90e4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164122"
---
# <a name="precision-property-adox"></a>Proprietà Precision (ADOX)
Indica la precisione massima dei valori dei dati nella [colonna](./column-object-adox.md).  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta e restituisce un valore **Long** che rappresenta la precisione massima dei valori dei dati nella colonna quando la proprietà [Type](./type-property-column-adox.md) è di tipo numerico. La **precisione** viene ignorata per tutti gli altri tipi di dati.  
  
## <a name="remarks"></a>Commenti  
 Il valore predefinito è zero (**0**).  
  
 Questa proprietà è di sola lettura per gli oggetti [Column](./column-object-adox.md) già accodati a una raccolta.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Column (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di codice ADOX: esempio di proprietà NumericScale e Precision (VB)](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [Proprietà Type (Column) (ADOX)](./type-property-column-adox.md)   
 [Oggetto Column (ADOX)](./column-object-adox.md)