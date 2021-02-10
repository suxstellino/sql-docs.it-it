---
description: Raccolta Tables (ADOX)
title: Raccolta Tables (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Tables
- Tables
helpviewer_keywords:
- Tables collection [ADOX]
ms.assetid: 38d750e7-f3fb-426e-b4b4-55eea4f1a654
author: rothja
ms.author: jroth
ms.openlocfilehash: 5d95bec64e9751d8663c34d1bf9d9d301c163eb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053472"
---
# <a name="tables-collection-adox"></a>Raccolta Tables (ADOX)
Contiene tutti gli oggetti [tabella](./table-object-adox.md) di un catalogo.  
  
## <a name="remarks"></a>Commenti  
 Il metodo [Append](./append-method-adox-tables.md) per una raccolta **Tables** è univoco per ADOX. È possibile:  
  
-   Aggiungere una nuova tabella alla raccolta con il metodo **Append** .  
  
 Le proprietà e i metodi rimanenti sono standard per le raccolte ADO. È possibile:  
  
-   Accedere a una tabella nella raccolta con la proprietà [Item](../ado-api/item-property-ado.md) .  
  
-   Restituisce il numero di tabelle contenute nella raccolta con la proprietà [count](../ado-api/count-property-ado.md) .  
  
-   Rimuovere una tabella dalla raccolta con il metodo [Delete](./delete-method-adox-collections.md) .  
  
-   Aggiornare gli oggetti della raccolta in modo che corrispondano allo schema del database corrente con il metodo [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Alcuni provider possono restituire altri oggetti dello schema, ad esempio una vista, nella raccolta **Tables** . Pertanto, alcune raccolte ADOX possono contenere più riferimenti allo stesso oggetto. Se si elimina l'oggetto da una raccolta, la modifica non sarà visibile in un'altra raccolta che fa riferimento all'oggetto eliminato fino a quando non viene chiamato il metodo **Refresh** sulla raccolta. Con il provider OLE DB per Microsoft Jet, ad esempio, le visualizzazioni vengono restituite con la raccolta **Tables** . Se si elimina una vista, è necessario aggiornare la raccolta **Tables** prima che la raccolta rifletta la modifica.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi della raccolta Tables](./tables-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActiveConnection di Catalog (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Metodi di Accodamento di colonne e tabelle, esempio di proprietà Name (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Metodo di chiusura della connessione, esempio di proprietà del tipo di tabella (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Esempio di proprietà Method Append, Key Type, RelatedColumn, RelatedTable e UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Oggetto Table (ADOX)](./table-object-adox.md)