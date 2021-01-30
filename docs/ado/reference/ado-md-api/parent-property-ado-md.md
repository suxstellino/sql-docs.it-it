---
description: Proprietà Parent (ADO MD)
title: Proprietà Parent (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parent
- Member::Parent
helpviewer_keywords:
- Parent property [ADO MD]
ms.assetid: 32c278c1-d8e1-4bb7-9ecd-2fbfdffee34b
author: rothja
ms.author: jroth
ms.openlocfilehash: fd5b6c1adafca65b48f79eb5debc61ea45381705
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164451"
---
# <a name="parent-property-ado-md"></a>Proprietà Parent (ADO MD)
Indica il membro padre del [membro](./member-object-ado-md.md) corrente in una gerarchia.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un oggetto [membro](./member-object-ado-md.md) ed è di sola lettura.  
  
## <a name="remarks"></a>Commenti  
 Un membro che si trova al livello superiore di una gerarchia (la radice) non ha un elemento padre. Questa proprietà è supportata solo per gli oggetti **membro** che appartengono a un oggetto [Level](./level-object-ado-md.md) . Si verifica un errore quando si fa riferimento a questa proprietà dagli oggetti **membro** che appartengono a un oggetto [position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Member (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Children (ADO MD)](./children-property-ado-md.md)