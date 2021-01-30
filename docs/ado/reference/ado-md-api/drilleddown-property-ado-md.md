---
description: Proprietà DrilledDown (ADO MD)
title: Proprietà DrilledDown (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DrilledDown
- Member::DrilledDown
helpviewer_keywords:
- DrilledDown property [ADO MD]
ms.assetid: bf39dd36-fc7a-4f6e-86c0-fa71430c0d86
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e25d23cfdbf7f33f024203e25814b16d4ec6da4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169853"
---
# <a name="drilleddown-property-ado-md"></a>Proprietà DrilledDown (ADO MD)
Indica se gli elementi figlio seguono immediatamente il [membro](./member-object-ado-md.md) sull'asse.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **booleano** ed è di sola lettura. **DrilledDown** restituisce **true** se non sono presenti membri figlio del membro corrente sull'asse. **DrilledDown** restituisce **false** se il membro corrente ha uno o più membri figlio sull'asse.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **DrilledDown** per determinare se è presente almeno un elemento figlio di questo membro sull'asse immediatamente successivo a questo membro. Queste informazioni sono utili per la visualizzazione del membro.  
  
 Questa proprietà è supportata solo per gli oggetti [membro](./member-object-ado-md.md) che appartengono a un oggetto [position](./position-object-ado-md.md) . Si verifica un errore quando si fa riferimento a questa proprietà dagli oggetti **membro** che appartengono a un oggetto [Level](./level-object-ado-md.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Member (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà ParentSameAsPrev (ADO MD)](./parentsameasprev-property-ado-md.md)