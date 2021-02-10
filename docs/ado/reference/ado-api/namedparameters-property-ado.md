---
description: Proprietà NamedParameters (ADO)
title: Proprietà NamedParameters (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::NamedParameters
helpviewer_keywords:
- NamedParameters property [ADO]
ms.assetid: 42409387-026c-435f-a9b1-bf4167095875
author: rothja
ms.author: jroth
ms.openlocfilehash: 7541454e1a7545002d48201a50452bfd11931a27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041601"
---
# <a name="namedparameters-property-ado"></a>Proprietà NamedParameters (ADO)
Indica se i nomi dei parametri devono essere passati al provider.  
  
## <a name="remarks"></a>Commenti  
 Quando questa proprietà è true, ADO passa il valore della proprietà **Name** di ogni parametro nella raccolta di **parametri** per l' [oggetto Command](./command-object-ado.md). Il provider usa un nome di parametro per trovare la corrispondenza con i parametri nelle proprietà **CommandText** o **CommandStream** . Se questa proprietà è false (impostazione predefinita), i nomi dei parametri vengono ignorati e il provider usa l'ordine dei parametri per confrontare i valori con i parametri nelle proprietà **CommandText** o **CommandStream** .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà CommandText (ADO)](./commandtext-property-ado.md)   
 [Proprietà CommandStream (ADO)](./commandstream-property-ado.md)   
 [Raccolta Parameters (ADO)](./parameters-collection-ado.md)