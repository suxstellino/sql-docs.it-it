---
description: Proprietà Name (ADOX)
title: Proprietà Name (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::PutName
- _Table::GetName
- _Key::Name
- _Key::get_Name
- _Column::GetName
- _Index::Name
- _Index::put_Name
- _Column::PutName
- _Key::put_Name
- _Table::put_Name
- _User25::PutName
- _Index::get_Name
- _Column::get_Name
- _Group25::Name
- _Group25::get_Name
- _Column::Name
- _User25::get_Name
- _Table::Name
- _Group25::GetName
- _Index::PutName
- _Column::put_Name
- _Key::GetName
- _Table::get_Name
- _User25::Name
- _User25::put_Name
- _Index::GetName
- _User25::GetName
helpviewer_keywords:
- Name property [ADOX]
ms.assetid: 81b92baf-b6b9-4f4e-9f33-4503795518cd
author: rothja
ms.author: jroth
ms.openlocfilehash: 83ef51697cd3ad1ad1a01355baead5d90788813f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171997"
---
# <a name="name-property-adox"></a>Proprietà Name (ADOX)
Indica il nome dell'oggetto.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **stringa** .  
  
## <a name="remarks"></a>Commenti  
 Non è necessario che i nomi siano univoci all'interno di una raccolta.  
  
 La proprietà **Name** è di lettura/scrittura negli oggetti [Column](./column-object-adox.md), [Group](./group-object-adox.md), [Key](./key-object-adox.md), [index](./index-object-adox.md), [Table](./table-object-adox.md)e [User](./user-object-adox.md) . La proprietà **Name** è di sola lettura negli oggetti [Catalog](./catalog-object-adox.md), [procedure](./procedure-object-adox.md)e [visualizzazione](./view-object-adox.md) .  
  
 Per gli oggetti di lettura/scrittura (**colonna**, **gruppo**, **chiave**, **Indice**, **tabella** e oggetti **utente** ), il valore predefinito è una stringa vuota ("").  
  
> [!NOTE]
>  Per le chiavi, questa proprietà è di sola lettura sugli oggetti **chiave** già accodati a una raccolta. Per le tabelle, questa proprietà è di sola lettura per gli oggetti **tabella** già accodati a una raccolta.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Column (ADOX)](./column-object-adox.md)  
        [Oggetto Group (ADOX)](./group-object-adox.md)  
        [Oggetto Index (ADOX)](./index-object-adox.md)  
    :::column-end:::
    :::column:::
        [Oggetto Key (ADOX)](./key-object-adox.md)  
        [Oggetto Procedure (ADOX)](./procedure-object-adox.md)  
        [Oggetto Property (ADO)](../ado-api/property-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Table (ADOX)](./table-object-adox.md)  
        [Oggetto User (ADOX)](./user-object-adox.md)  
        [Oggetto View (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Metodi di Accodamento di colonne e tabelle, esempio di proprietà Name (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Esempio di proprietà Method Append, Key Type, RelatedColumn, RelatedTable e UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Esempio della proprietà ParentCatalog (VB)](./parentcatalog-property-example-vb.md)