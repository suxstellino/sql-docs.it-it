---
description: Metodo GetPermissions (ADOX)
title: Metodo GetPermissions (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _User25::GetPermissions
- _Group25::raw_GetPermissions
- _Group25::GetPermissions
- _User25::raw_GetPermissions
helpviewer_keywords:
- GetPermissions method [ADOX]
ms.assetid: df201c1f-c76a-465d-98f0-83b7fc36e6e3
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c4d96176e4bdad70cbd5c09a59adcade6f99f81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169385"
---
# <a name="getpermissions-method-adox"></a>Metodo GetPermissions (ADOX)
Restituisce le autorizzazioni per un [gruppo](./group-object-adox.md) o un [utente](./user-object-adox.md) in un contenitore oggetto o oggetto.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
ReturnValue=GroupOrUser.GetPermissions(Name, ObjectType    [,ObjectTypeId])  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **Long** che specifica una maschera di maschera contenente le autorizzazioni del gruppo o dell'utente nell'oggetto. Questo valore può essere costituito da una o più costanti [RightsEnum](./rightsenum.md) .  
  
#### <a name="parameters"></a>Parametri  
 *Nome*  
 Valore **Variant** che specifica il nome dell'oggetto per il quale impostare le autorizzazioni. Impostare *nome* su un valore null se si desidera ottenere le autorizzazioni per il contenitore di oggetti.  
  
 *ObjectType*  
 Valore **Long** che può essere una delle costanti [ObjectTypeEnum](./objecttypeenum.md) , che specifica il tipo dell'oggetto per il quale ottenere le autorizzazioni.  
  
 *ObjectTypeId*  
 facoltativo. Valore **Variant** che specifica il GUID per un tipo di oggetto provider non definito dalla specifica OLE DB. Questo parametro è obbligatorio se *ObjectType* è impostato su **adPermObjProviderSpecific**; in caso contrario, non viene utilizzato.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Group (ADOX)](./group-object-adox.md)  
    :::column-end:::
    :::column:::
        [Oggetto User (ADOX)](./user-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi GetPermissions e sepermissions (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Proprietà Name (ADOX)](./name-property-adox.md)   
 [Metodo SetPermissions (ADOX)](./setpermissions-method-adox.md)