---
description: InheritTypeEnum
title: InheritTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- InheritTypeEnum
helpviewer_keywords:
- InheritTypeEnum enumeration [ADOX]
ms.assetid: c2f6ce79-c4b3-4d40-ac95-21025208f991
author: rothja
ms.author: jroth
ms.openlocfilehash: ef61a93080846e8fc65a02e592ca3b171c1f92bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049981"
---
# <a name="inherittypeenum"></a>InheritTypeEnum
Specifica il modo in cui gli oggetti erediteranno le autorizzazioni impostate con [le autorizzazioni.](./setpermissions-method-adox.md)  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adInheritBoth**|3|Entrambi gli oggetti e altri contenitori contenuti nell'oggetto primario ereditano la voce.|  
|**adInheritContainers**|2|Gli altri contenitori contenuti nell'oggetto primario ereditano la voce.|  
|**adInheritNone**|0|Valore predefinito. Non si verifica alcuna ereditarietà.|  
|**adInheritNoPropagate**|4|I flag **adInheritObjects** e **adInheritContainers** non vengono propagati a una voce ereditata.|  
|**adInheritObjects**|1|Gli oggetti non contenitore nel contenitore ereditano le autorizzazioni.|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo SetPermissions (ADOX)](./setpermissions-method-adox.md)