---
description: RecordTypeEnum
title: RecordTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordTypeEnum
helpviewer_keywords:
- RecordTypeEnum enumeration [ADO]
ms.assetid: f557e537-015d-4ba7-8a41-a6f00b366a91
author: rothja
ms.author: jroth
ms.openlocfilehash: 8ef671384a552f240930e24a073f979f6e84da8d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166674"
---
# <a name="recordtypeenum"></a>RecordTypeEnum
Specifica il tipo di oggetto [record](./record-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adSimpleRecord**|0|Indica un record *semplice* (non contiene nodi figlio).|  
|**adCollectionRecord**|1|Indica un record di *raccolta* (contiene i nodi figlio).|  
|**adRecordUnknown**|-1|Indica che il tipo di questo **record** è sconosciuto.|  
|**adStructDoc**|2|Indica un tipo speciale di record di *raccolta* che rappresenta i documenti strutturati com.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà RecordType (ADO)](./recordtype-property-ado.md)