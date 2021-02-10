---
description: Oggetto Key (ADOX)
title: Oggetto Key (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Key
helpviewer_keywords:
- Key object [ADOX]
ms.assetid: 55f116fe-4d56-4892-bffe-0cdd6fc727c9
author: rothja
ms.author: jroth
ms.openlocfilehash: b0e2cdd3dcb1d6d0772e1b7508bee8063089a4e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054076"
---
# <a name="key-object-adox"></a>Oggetto Key (ADOX)
Rappresenta un campo chiave primaria, esterna o univoca di una tabella di database.  
  
## <a name="remarks"></a>Commenti  
 Il codice seguente crea una nuova **chiave**:  
  
```  
Dim obj As New Key  
```  
  
 Con le proprietà e le raccolte di un oggetto **chiave** , è possibile:  
  
-   Identificare la chiave con la proprietà [Name](./name-property-adox.md) .  
  
-   Determinare se la chiave è primaria, esterna o univoca con la proprietà [Type](./type-property-key-adox.md) .  
  
-   Accedere alle colonne di database della chiave con la raccolta [Columns](./columns-collection-adox.md) .  
  
-   Specificare il nome della tabella correlata con la proprietà [RelatedTable](./relatedtable-property-adox.md) .  
  
-   Determinare l'azione eseguita per l'eliminazione o l'aggiornamento di una chiave primaria con le proprietà [DeleteRule](./deleterule-property-adox.md) e [UpdateRule](./updaterule-property-adox.md) .  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Key](./key-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Method Append, Key Type, RelatedColumn, RelatedTable e UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Raccolta Columns (ADOX)](./columns-collection-adox.md)   
 [Raccolta Keys (ADOX)](./keys-collection-adox.md)