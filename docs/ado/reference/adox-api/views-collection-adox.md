---
description: Raccolta Views (ADOX)
title: Raccolta views (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Views
- Views
helpviewer_keywords:
- Views collection [ADOX]
ms.assetid: a55d380c-2b7b-4b57-af74-8ba0b3de0db9
author: rothja
ms.author: jroth
ms.openlocfilehash: 51a32bb1952e5c8100ed1d13cb7ba72b99e8994d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169093"
---
# <a name="views-collection-adox"></a>Raccolta Views (ADOX)
Contiene tutti gli oggetti di [visualizzazione](./view-object-adox.md) di un catalogo.  
  
## <a name="remarks"></a>Commenti  
 Il metodo [Append](./append-method-adox-views.md) per una raccolta **views** è univoco per ADOX. È possibile:  
  
-   Aggiungere una nuova vista alla raccolta con il metodo **Append** .  
  
 Le proprietà e i metodi rimanenti sono standard per le raccolte ADO. È possibile:  
  
-   Accedere a una vista della raccolta con la proprietà [Item](../ado-api/item-property-ado.md) .  
  
-   Restituisce il numero di visualizzazioni contenute nella raccolta con la proprietà [count](../ado-api/count-property-ado.md) .  
  
-   Rimuovere una vista dalla raccolta con il metodo [Delete](./delete-method-adox-collections.md) .  
  
-   Aggiornare gli oggetti della raccolta in modo che corrispondano allo schema del database corrente con il metodo [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi della raccolta Views](./views-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di raccolte views e Fields (VB)](./views-and-fields-collections-example-vb.md)   
 [Esempio di metodo Append views (VB)](./views-append-method-example-vb.md)   
 [Raccolta views, esempio di proprietà CommandText (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Esempio di metodo Delete views (VB)](./views-delete-method-example-vb.md)   
 [Esempio di metodo Refresh views (VB)](./views-refresh-method-example-vb.md)   
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Oggetto View (ADOX)](./view-object-adox.md)