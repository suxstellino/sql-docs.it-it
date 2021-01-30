---
description: Oggetto Index (ADOX)
title: Oggetto index (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index
helpviewer_keywords:
- Index object [ADOX]
ms.assetid: 6b9578c0-bc94-46b9-b801-c18e14b04b31
author: rothja
ms.author: jroth
ms.openlocfilehash: a11389324337d4e5a9deea347dda8ecd5cea3016
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172040"
---
# <a name="index-object-adox"></a>Oggetto Index (ADOX)
Rappresenta un indice di una tabella di database.  
  
## <a name="remarks"></a>Commenti  
 Il codice seguente crea un nuovo **Indice**:  
  
```  
Dim obj As New Index  
```  
  
 Con le proprietà e le raccolte di un oggetto **index** è possibile:  
  
-   Identificare l'indice con la proprietà [Name](./name-property-adox.md) .  
  
-   Accedere alle colonne del database dell'indice con la raccolta [Columns](./columns-collection-adox.md) .  
  
-   Consente di specificare se le chiavi dell'indice devono essere univoche con la proprietà [Unique](./unique-property-adox.md) .  
  
-   Consente di specificare se l'indice è la chiave primaria di una tabella con la proprietà [PrimaryKey](./primarykey-property-adox.md) .  
  
-   Consente di specificare se i record con valori null nei rispettivi campi di indice hanno voci di indice con la proprietà [IndexNulls](./indexnulls-property-adox.md) .  
  
-   Consente di specificare se l'indice è in cluster con la proprietà [Clustered](./clustered-property-adox.md) .  
  
-   Accedere alle proprietà di indice specifiche del provider con la raccolta [Properties](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  Si verificherà un errore durante l'aggiunta di una [colonna](./column-object-adox.md) alla raccolta **Columns** di un **Indice** se la **colonna** non esiste in un oggetto [Table](./table-object-adox.md) già accodato alla raccolta [Tables](./tables-collection-adox.md) .  
  
> [!NOTE]
>  Il provider di dati potrebbe non supportare tutte le proprietà degli oggetti **Indice** . Se è stato impostato un valore per una proprietà non supportata dal provider, si verificherà un errore. Per i nuovi oggetti **index** , l'errore si verificherà quando l'oggetto viene aggiunto alla raccolta. Per gli oggetti esistenti, l'errore si verificherà quando si imposta la proprietà.  
  
> [!NOTE]
>  Quando si creano oggetti **index** , l'esistenza di un valore predefinito appropriato per una proprietà facoltativa non garantisce che il provider supporti la proprietà. Per ulteriori informazioni sulle proprietà supportate dal provider, vedere la documentazione del provider.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Index](./index-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Append degli indici (VB)](./indexes-append-method-example-vb.md)   
 [Esempio di proprietà IndexNulls (VB)](./indexnulls-property-example-vb.md)   
 [Esempio di proprietà PrimaryKey e Unique (VB)](./primarykey-and-unique-properties-example-vb.md)   
 [Esempio di proprietà SortOrder (VB)](./sortorder-property-example-vb.md)   
 [Raccolta Columns (ADOX)](./columns-collection-adox.md)   
 [Raccolta Indexes (ADOX)](./indexes-collection-adox.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)