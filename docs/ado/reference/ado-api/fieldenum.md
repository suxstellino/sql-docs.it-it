---
description: FieldEnum
title: FieldEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldEnum
helpviewer_keywords:
- FieldEnum enumeration [ADO]
ms.assetid: be4eda13-d4e4-4d6b-bb0d-3310b0a96fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: d111bac142126b8d5c4d9ec14616c91a18e70d4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024627"
---
# <a name="fieldenum"></a>FieldEnum
Specifica i campi speciali a cui viene fatto riferimento nella raccolta di [campi](./fields-collection-ado.md) di un oggetto [record](./record-object-ado.md) .  
  
## <a name="remarks"></a>Commenti  
 Queste costanti forniscono un "collegamento" per accedere ai campi speciali associati a un **record**. Recuperare l'oggetto [campo](./field-object.md) dalla raccolta **Fields** e quindi ottenere il relativo contenuto con la proprietà [value](./value-property-ado.md) dell'oggetto **campo** .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adDefaultStream**|-1|Fa riferimento al campo contenente l'oggetto [flusso](./stream-object-ado.md) predefinito associato a un **record**.|  
|**adRecordURL**|-2|Fa riferimento al campo contenente la stringa URL assoluta per il **record** corrente.|