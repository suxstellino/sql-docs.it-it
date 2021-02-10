---
description: AffectEnum
title: AffectEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AffectEnum
helpviewer_keywords:
- AffectEnum enumeration [ADO]
ms.assetid: 1ab921a0-6c57-43b4-9291-701b2599f3e8
author: rothja
ms.author: jroth
ms.openlocfilehash: 797baaaa554e75fa11287e2288f9a9ffc610b6be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035621"
---
# <a name="affectenum"></a>AffectEnum
Specifica quali record sono interessati da un'operazione.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adAffectAll**|3|Se al **Recordset** non è applicato alcun [filtro](./filter-property.md) , influiscono su tutti i record.<br /><br /> Se la proprietà **Filter** è impostata su un criterio di stringa, ad esempio "Author =' Smith '", l'operazione influiscono sui record visibili nel capitolo corrente.<br /><br /> Se la proprietà **Filter** è impostata su un membro di [FilterGroupEnum](./filtergroupenum.md) o di una matrice di segnalibri, l'operazione avrà effetto su tutte le righe del **Recordset**. **Nota: adAffectAll** è nascosto nell'Visualizzatore oggetti Visual Basic.|  
|**adAffectAllChapters**|4|Influiscono su tutti i record di tutti i capitoli di pari livello del **Recordset**, inclusi quelli non visibili tramite un **filtro** attualmente applicato.|  
|**adAffectCurrent**|1|Influiscono solo sul record corrente.|  
|**adAffectGroup**|2|Interessa solo i record che soddisfano l'impostazione della proprietà [filtro](./filter-property.md) corrente. Per usare questa opzione, è necessario impostare la proprietà **Filter** su un valore **FilterGroupEnum** o una matrice di **segnalibri** .|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. influisce su ALL|  
|AdoEnums. influisce. ALLCHAPTERS|  
|AdoEnums. influisce. CURRENT|  
|AdoEnums. influisce. GROUP|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Metodo CancelBatch (ADO)](./cancelbatch-method-ado.md)  
        [Metodo Delete (Recordset - ADO)](./delete-method-ado-recordset.md)  
    :::column-end:::
    :::column:::
        [Metodo Resync](./resync-method.md)  
        [Metodo UpdateBatch](./updatebatch-method.md)  
    :::column-end:::
:::row-end:::