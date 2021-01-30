---
description: SearchDirectionEnum
title: SearchDirectionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SearchDirectionEnum
helpviewer_keywords:
- SearchDirectionEnum enumeration [ADO]
ms.assetid: 81272ae3-2165-4f4e-adfe-9ede0368cb17
author: rothja
ms.author: jroth
ms.openlocfilehash: 957e84b3a98f0248e41a83301ef2c9a9c1943b11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166581"
---
# <a name="searchdirectionenum"></a>SearchDirectionEnum
Specifica la direzione di una ricerca di record in un [Recordset](./recordset-object-ado.md).  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adSearchBackward**|-1|Esegue la ricerca all'indietro, arrestandosi all'inizio del **Recordset**. Se non viene trovata alcuna corrispondenza, il puntatore del record viene posizionato in [BOF](./bof-eof-properties-ado.md).|  
|**adSearchForward**|1|Cerca in futuro, arrestandosi alla fine del **Recordset**. Se non viene trovata alcuna corrispondenza, il puntatore del record viene posizionato in corrispondenza del valore [EOF](./bof-eof-properties-ado.md).|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. SearchDirection. Backward|  
|AdoEnums. SearchDirection. INOLTR|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo Find (ADO)](./find-method-ado.md)