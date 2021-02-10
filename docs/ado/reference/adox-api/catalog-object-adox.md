---
description: Oggetto Catalog (ADOX)
title: Oggetto Catalog (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog
helpviewer_keywords:
- Catalog object [ADOX]
ms.assetid: bb651639-a488-4e38-b6de-0ed99fa4dd92
author: rothja
ms.author: jroth
ms.openlocfilehash: 00e698ed85e739b2ea6a8dccebef7ce36e3be8e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054476"
---
# <a name="catalog-object-adox"></a>Oggetto Catalog (ADOX)
Contiene le raccolte ([tabelle](./tables-collection-adox.md), [viste](./views-collection-adox.md), [utenti](./users-collection-adox.md), [gruppi](./groups-collection-adox.md)e [procedure](./procedures-collection-adox.md)) che descrivono il catalogo dello schema di un'origine dati.  
  
## <a name="remarks"></a>Commenti  
 È possibile modificare l'oggetto **Catalogo** aggiungendo o rimuovendo oggetti o modificando gli oggetti esistenti. Alcuni provider potrebbero non supportare tutti gli oggetti del **Catalogo** o supportare solo la visualizzazione delle informazioni sullo schema.  
  
 Con le proprietà e i metodi di un oggetto **Catalogo** , è possibile:  
  
-   Aprire il catalogo impostando la proprietà [ActiveConnection](./activeconnection-property-adox.md) su un oggetto [connessione](../ado-api/connection-object-ado.md) ADO o una stringa di connessione valida.  
  
-   Creare un nuovo catalogo con il metodo [create](./create-method-adox.md) .  
  
-   Determinare i proprietari degli oggetti in un **Catalogo** con i metodi [GetObjectOwner](./getobjectowner-method-adox.md) e [SetObjectOwner](./setobjectowner-method.md) .  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Catalog](./catalog-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActiveConnection di Catalog (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Esempio di proprietà Command e CommandText (VB)](./command-and-commandtext-properties-example-vb.md)   
 [Metodo di chiusura della connessione, esempio di proprietà del tipo di tabella (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Esempio di metodo Create (VB)](./create-method-example-vb.md)   
 [Esempio di proprietà Method Append, Key Type, RelatedColumn, RelatedTable e UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Raccolta Parameters, esempio di proprietà Command (VB)](./parameters-collection-command-property-example-vb.md)   
 [Esempio di proprietà ParentCatalog (VB)](./parentcatalog-property-example-vb.md)   
 [Esempio di metodo Append di procedure (VB)](./procedures-append-method-example-vb.md)   
 [Esempio di metodo Delete delle procedure (VB)](./procedures-delete-method-example-vb.md)   
 [Esempio di metodo Refresh di Procedures (VB)](./procedures-refresh-method-example-vb.md)   
 [Esempio di raccolte views e Fields (VB)](./views-and-fields-collections-example-vb.md)   
 [Esempio di metodo Append views (VB)](./views-append-method-example-vb.md)   
 [Raccolta views, esempio di proprietà CommandText (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Esempio di metodo Delete views (VB)](./views-delete-method-example-vb.md)   
 [Esempio di metodo Refresh views (VB)](./views-refresh-method-example-vb.md)   
 [Raccolta Groups (ADOX)](./groups-collection-adox.md)   
 [Raccolta Procedures (ADOX)](./procedures-collection-adox.md)   
 [Raccolta Tables (ADOX)](./tables-collection-adox.md)   
 [Raccolta utenti (ADOX)](./users-collection-adox.md)   
 [Raccolta Views (ADOX)](./views-collection-adox.md)