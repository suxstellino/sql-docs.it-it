---
description: Proprietà Children (ADO MD)
title: Proprietà Children (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Children
- Children
helpviewer_keywords:
- Children property [ADO MD]
ms.assetid: 61d36468-1ccd-467a-9cb5-17d0bfacc766
author: rothja
ms.author: jroth
ms.openlocfilehash: 7fae1b50b219abc4e5841bb537d0befaf35a4b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055675"
---
# <a name="children-property-ado-md"></a>Proprietà Children (ADO MD)
Restituisce una raccolta di [membri](./members-collection-ado-md.md) per cui il [membro](./member-object-ado-md.md) corrente è l'elemento padre nella gerarchia.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce una raccolta di **membri** ed è di sola lettura.  
  
## <a name="remarks"></a>Commenti  
 La proprietà **Children** contiene una raccolta di **membri** per cui il **membro** corrente è l'elemento padre gerarchico. Gli oggetti **membro** a livello foglia non hanno membri figlio nella raccolta **Members** . Questa proprietà è supportata solo per gli oggetti **membro** che appartengono a un oggetto [Level](./level-object-ado-md.md) . Si verifica un errore quando si fa riferimento a questa proprietà dagli oggetti **membro** che appartengono a un oggetto [position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Member (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà ChildCount (ADO MD)](./childcount-property-ado-md.md)