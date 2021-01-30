---
description: Oggetto Table (ADOX)
title: Oggetto Table (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Table
helpviewer_keywords:
- Table object [ADOX]
ms.assetid: a6d74000-0828-49ba-850a-63da865f8802
author: rothja
ms.author: jroth
ms.openlocfilehash: ba2064096c36bf6490daa8d472801de7594cfdcc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164020"
---
# <a name="table-object-adox"></a>Oggetto Table (ADOX)
Rappresenta una tabella di database che include colonne, indici e chiavi.  
  
## <a name="remarks"></a>Commenti  
 Il codice seguente crea una nuova **tabella**:  
  
```  
Dim obj As New Table  
```  
  
 Con le proprietà e le raccolte di un oggetto **Table** è possibile:  
  
-   Identificare la tabella con la proprietà [Name Property (ADOX)](./name-property-adox.md) .  
  
-   Determinare il tipo di tabella con la proprietà del [tipo (tabella) (ADOX)](./type-property-table-adox.md) .  
  
-   Accedere alle colonne di database della tabella con la raccolta [Columns (ADOX)](./columns-collection-adox.md) .  
  
-   Accedere agli indici della tabella con la [raccolta Indexes (ADOX)](./indexes-collection-adox.md).  
  
-   Accedere alle chiavi della tabella con la [raccolta Keys (ADOX)](./keys-collection-adox.md).  
  
-   Specificare il catalogo proprietario della tabella con la proprietà [ParentCatalog (ADOX)](./parentcatalog-property-adox.md) .  
  
-   Restituire le informazioni sulla data con le proprietà [DateCreated (ADOX)](./datecreated-property-adox.md) e [DATEMODIFIED Property (ADOX)](./datemodified-property-adox.md) .  
  
-   Accedere alle proprietà di tabella specifiche del provider con la raccolta [Properties (ADO)](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  Il provider di dati potrebbe non supportare tutte le proprietà degli oggetti **tabella** . Se è stato impostato un valore per una proprietà non supportata dal provider, si verificherà un errore. Per i nuovi oggetti **tabella** , l'errore si verificherà quando l'oggetto viene aggiunto alla raccolta. Per gli oggetti esistenti, l'errore si verificherà quando si imposta la proprietà.  
>   
>  Quando si creano oggetti **tabella** , l'esistenza di un valore predefinito appropriato per una proprietà facoltativa non garantisce che il provider supporti la proprietà. Per ulteriori informazioni sulle proprietà supportate dal provider, vedere la documentazione del provider.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Table](./table-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActiveConnection di Catalog (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Metodi di Accodamento di colonne e tabelle, esempio di proprietà Name (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Metodo di chiusura della connessione, esempio di proprietà del tipo di tabella (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Esempio di proprietà Method Append, Key Type, RelatedColumn, RelatedTable e UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Esempio di proprietà ParentCatalog (VB)](./parentcatalog-property-example-vb.md)   
 [Raccolta Columns (ADOX)](./columns-collection-adox.md)   
 [Raccolta Indexes (ADOX)](./indexes-collection-adox.md)   
 [Raccolta Keys (ADOX)](./keys-collection-adox.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)   
 [Raccolta Tables (ADOX)](./tables-collection-adox.md)