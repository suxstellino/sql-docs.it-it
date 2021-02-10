---
description: MemberTypeEnum
title: MemberTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- MemberTypeEnum
helpviewer_keywords:
- MemberTypeEnum enumeration [ADO MD]
ms.assetid: 5d8132c0-7ca2-4f86-8336-1b34213869ad
author: rothja
ms.author: jroth
ms.openlocfilehash: b83747edabc12f2f84d728a456255513ee1f0f66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051082"
---
# <a name="membertypeenum"></a>MemberTypeEnum
Specifica l'impostazione per la proprietà [Type](./type-property-ado-md.md) di un oggetto [membro](./member-object-ado-md.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adMemberAll**|4|Indica che l'oggetto **membro** rappresenta tutti i membri del livello.|  
|**adMemberFormula**|3|Indica che l'oggetto **membro** viene calcolato utilizzando un'espressione di formula.|  
|**adMemberMeasure**|2|Indica che l'oggetto **membro** appartiene alla dimensione Measures e rappresenta un attributo quantitativo.|  
|**adMemberRegular**|1|Valore predefinito. Indica che l'oggetto **membro** rappresenta un'istanza di un'entità business.|  
|**adMemberUnknown**|0|Impossibile determinare il tipo del membro.|