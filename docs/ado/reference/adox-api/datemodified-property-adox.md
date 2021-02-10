---
description: Proprietà DateModified (ADOX)
title: Proprietà DateModified (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::get_DateModified
- _Table::DateModified
- _Table::GetDateModified
helpviewer_keywords:
- DateModified property [ADOX]
ms.assetid: fed09266-1547-4bda-9088-c254d81cc738
author: rothja
ms.author: jroth
ms.openlocfilehash: ef5fe0610059d8dff38732d356cbd8b1562ed7d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054286"
---
# <a name="datemodified-property-adox"></a>Proprietà DateModified (ADOX)
Indica la data dell'Ultima modifica apportata all'oggetto.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **Variant** che specifica la data di modifica. Il valore è null se **DateModified** non è supportato dal provider.  
  
## <a name="remarks"></a>Commenti  
 La proprietà **DateModified** è null per gli oggetti appena accodati. Dopo aver accodato una nuova [vista](./view-object-adox.md) o [routine](./procedure-object-adox.md), è necessario chiamare il metodo [Refresh](../ado-api/refresh-method-ado.md) della raccolta [views](./views-collection-adox.md) o [Procedures](./procedures-collection-adox.md) per ottenere i valori per la proprietà **DateModified** .  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Procedure (ADOX)](./procedure-object-adox.md)  
    :::column-end:::
    :::column:::
        [Oggetto Table (ADOX)](./table-object-adox.md)  
    :::column-end:::
    :::column:::
        [Oggetto View (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà DateCreated e DateModified (VB)](./datecreated-and-datemodified-properties-example-vb.md)   
 [Proprietà DateCreated (ADOX)](./datecreated-property-adox.md)