---
description: ActionEnum
title: ActionEnum indicante | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ActionEnum
helpviewer_keywords:
- ActionEnum enumeration [ADOX]
ms.assetid: f948febd-c885-4621-823b-421e116fec4e
author: rothja
ms.author: jroth
ms.openlocfilehash: 35f4ddd60d4056cebbf0383aa0afb240c3ecb722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169664"
---
# <a name="actionenum"></a>ActionEnum
Specifica il tipo di azione da eseguire quando viene chiamata l' [autorizzazione](./setpermissions-method-adox.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adAccessDeny**|3|Al gruppo o all'utente verranno negate le autorizzazioni specificate.|  
|**adAccessGrant**|1|Il gruppo o l'utente avrà almeno le autorizzazioni richieste.|  
|**adAccessRevoke**|4|Eventuali diritti di accesso espliciti al gruppo o all'utente verranno revocati.|  
|**adAccessSet**|2|Il gruppo o l'utente avrà esattamente le autorizzazioni richieste.|