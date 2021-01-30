---
description: Proprietà Item (ADO)
title: Proprietà Item (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parameters::GetItem
- Indexes::GetItem
- Parameters::Item
- Tables::Item
- Procedures::Item
- Users::GetItem
- Tables::GetItem
- Procedures::GetItem
- Users::get_Item
- Users::Item
- Views::GetItem
- Groups::Item
- Groups::get_Item
- Columns::Item
- Indexes::Item
- Fields15::GetItem
- Columns::GetItem
- Fields::Item
- Indexes::get_Item
- Columns::get_Item
- Fields15::Item
- Views::get_Item
- Groups::GetItem
- Errors::get_Item
- Fields15::get_Item
- Tables::get_Item
- Views::Item
- Errors::GetItem
- Parameters::get_Item
- Errors::Item
- Procedures::get_Item
helpviewer_keywords:
- Item property [ADO]
ms.assetid: e11484bb-c5c7-42d8-9bb8-21572125d727
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ae6b59ffe896180f5977288bbee16925b7d33b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170917"
---
# <a name="item-property-ado"></a>Proprietà Item (ADO)
Indica un membro specifico di una raccolta, in base al nome o al numero ordinale.  
  
## <a name="syntax"></a>Sintassi  
  
```  
Set object = collection.Item ( Index )  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un riferimento a un oggetto.  
  
## <a name="parameters"></a>Parametri  
 *Index*  
 Espressione **Variant** che restituisce il nome o il numero ordinale di un oggetto in una raccolta.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **Item** per restituire un oggetto specifico in una raccolta. Se **Item** non riesce a trovare un oggetto nella raccolta corrispondente all'argomento *index* , si verificherà un errore. Inoltre, alcune raccolte non supportano gli oggetti denominati; per queste raccolte, è necessario usare i riferimenti numerici ordinali.  
  
 La proprietà **Item** è la proprietà predefinita per tutte le raccolte. Pertanto, i seguenti formati di sintassi sono intercambiabili:  
  
```  
collection.Item (Index)  
collection (Index)  
```  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Raccolta Axes (ADO MD)](../ado-md-api/axes-collection-ado-md.md)  
        [Raccolta Columns (ADOX)](../adox-api/columns-collection-adox.md)  
        [Raccolta CubeDefs (ADO MD)](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Raccolta Dimensions (ADO MD)](../ado-md-api/dimensions-collection-ado-md.md)  
        [Raccolta Errors (ADO)](./errors-collection-ado.md)  
        [Raccolta Fields (ADO)](./fields-collection-ado.md)  
        [Raccolta di Groups (ADOX)](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Raccolta Hierarchies (ADO MD)](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Raccolta Indexes (ADOX)](../adox-api/indexes-collection-adox.md)  
        [Raccolta Keys (ADOX)](../adox-api/keys-collection-adox.md)  
        [Raccolta Levels (ADO MD)](../ado-md-api/levels-collection-ado-md.md)  
        [Raccolta Members (ADO MD)](../ado-md-api/members-collection-ado-md.md)  
        [Raccolta Parameters (ADO)](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Raccolta Positions (ADO MD)](../ado-md-api/positions-collection-ado-md.md)  
        [Raccolta Procedures (ADOX)](../adox-api/procedures-collection-adox.md)  
        [Raccolta Properties (ADO)](./properties-collection-ado.md)  
        [Raccolta Tables (ADOX)](../adox-api/tables-collection-adox.md)  
        [Raccolta Users (ADOX)](../adox-api/users-collection-adox.md)  
        [Raccolta Views (ADOX)](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Item (VB)](./item-property-example-vb.md)   
 [Esempio della proprietà Item (VC++)](./item-property-example-vc.md)