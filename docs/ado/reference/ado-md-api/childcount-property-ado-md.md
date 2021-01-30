---
description: Proprietà ChildCount (ADO MD)
title: Proprietà ChildCount (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ChildCount
- Member::ChildCount
helpviewer_keywords:
- ChildCount property [ADO MD]
ms.assetid: 5463be22-ca50-43ea-9c92-468fc8eda280
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f8147a6fdba9101c846b58f83e7ac8d25aa79ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174330"
---
# <a name="childcount-property-ado-md"></a>Proprietà ChildCount (ADO MD)
Indica il numero di membri per i quali l'oggetto [membro](./member-object-ado-md.md) corrente è l'elemento padre in una gerarchia.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **Long** integer ed è di sola lettura.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **childCount** per restituire una stima del numero di elementi figlio di un **membro** . Gli elementi figlio effettivi di un **membro** possono essere restituiti dalla proprietà [Children](./children-property-ado-md.md) .  
  
 Per gli oggetti **membro** di un oggetto [position](./position-object-ado-md.md) , il numero massimo restituito è 65536. Se il numero effettivo di elementi figlio supera 65536, il valore restituito sarà comunque 65536. Pertanto, l'applicazione deve interpretare un **childCount** di 65536 come uguale o maggiore di 65536 figli.  
  
 Per gli oggetti **membro** di un oggetto [Level](./level-object-ado-md.md) , utilizzare la proprietà [conteggio](../ado-api/count-property-ado.md) raccolta ADO nella raccolta **Children** per determinare il numero esatto di elementi figlio. La determinazione del numero esatto di elementi figlio può essere lenta se il numero di elementi figlio nella raccolta è elevato.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Member (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Children (ADO MD)](./children-property-ado-md.md)   
 [Proprietà Count (ADO)](../ado-api/count-property-ado.md)   
 [Raccolta Members (ADO MD)](./members-collection-ado-md.md)