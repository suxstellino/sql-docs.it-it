---
description: ObjectTypeEnum
title: ObjectTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ObjectTypeEnum
helpviewer_keywords:
- ObjectTypeEnum enumeration [ADOX]
ms.assetid: 3fdecfca-aa91-4596-ad98-610f1b7f840b
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b8aa3bdaa75994330325f32cf03676d48a00463
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164143"
---
# <a name="objecttypeenum"></a>ObjectTypeEnum
Specifica il tipo di oggetto di database per il quale impostare le autorizzazioni o la proprietà.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adPermObjColumn**|2|L'oggetto è una colonna.|  
|**adPermObjDatabase**|3|L'oggetto è un database.|  
|**adPermObjProcedure**|4|L'oggetto è una routine.|  
|**adPermObjProviderSpecific**|-1|L'oggetto è un tipo definito dal provider. Si verificherà un errore se il parametro *ObjectType* è **adPermObjProviderSpecific** e non viene fornito un *ObjectTypeId* .|  
|**adPermObjTable**|1|L'oggetto è una tabella.|  
|**adPermObjView**|5|L'oggetto è una vista.|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Metodo GetObjectOwner (ADOX)](./getobjectowner-method-adox.md)  
        [Metodo GetPermissions (ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [Metodo SetObjectOwner](./setobjectowner-method.md)  
        [Metodo SetPermissions (ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::