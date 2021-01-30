---
description: PositionEnum
title: PositionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PositionEnum
helpviewer_keywords:
- PositionEnum enumeration
ms.assetid: e69af0a5-3405-4b72-9c6e-6b188ff746fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 22760113f966379d8e1705420853705114d9c362
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170564"
---
# <a name="positionenum"></a>PositionEnum
Specifica la posizione corrente del puntatore del record all'interno di un [Recordset](./recordset-object-ado.md).  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adPosBOF**|-2|Indica che il puntatore al record corrente si trova in BOF (ovvero la proprietà [BOF](./bof-eof-properties-ado.md) è **true**).|  
|**adPosEOF**|-3|Indica che il puntatore al record corrente si trova in EOF (ovvero la proprietà [EOF](./bof-eof-properties-ado.md) è **true**).|  
|**adPosUnknown**|-1|Indica che il **Recordset** è vuoto, che la posizione corrente è sconosciuta oppure il provider non supporta la proprietà [AbsolutePage](./absolutepage-property-ado.md) o [AbsolutePosition](./absoluteposition-property-ado.md) .|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. Position. BOF|  
|AdoEnums. Position. EOF|  
|AdoEnums. Position. UNKNOWN|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Proprietà AbsolutePage (ADO)](./absolutepage-property-ado.md)  
    :::column-end:::
    :::column:::
        [Proprietà AbsolutePosition (ADO)](./absoluteposition-property-ado.md)  
    :::column-end:::
:::row-end:::