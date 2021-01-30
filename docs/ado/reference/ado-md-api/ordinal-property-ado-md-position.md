---
description: Proprietà Ordinal (Position - ADO MD)
title: Proprietà ordinale (posizione ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Position::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: 6efe8b5d-a2d5-43a9-a5ea-f9244f8d4ec9
author: rothja
ms.author: jroth
ms.openlocfilehash: a560b34e6ec55892a6f779c4808e5a7c69d99bca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169736"
---
# <a name="ordinal-property-ado-md-position"></a>Proprietà Ordinal (Position - ADO MD)
Identifica in modo univoco una [posizione](./position-object-ado-md.md) lungo un asse.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **Long** integer ed è di sola lettura.  
  
## <a name="remarks"></a>Commenti  
 La proprietà **ordinale** di un oggetto [position](./position-object-ado-md.md) corrisponde all'indice della **posizione** all'interno dell' [insieme Positions](./positions-collection-ado-md.md) .  
  
 È possibile recuperare rapidamente una cella utilizzando il **numero ordinale** della **posizione** lungo ogni asse con la proprietà [Item](./item-property-ado-md-cellset.md) dell'oggetto [cellt](./cellset-object-ado-md.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Position (ADO MD)](./position-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto cellt (ADO MD)](./cellset-object-ado-md.md)   
 [Proprietà Item (ADO MD Cellt)](./item-property-ado-md-cellset.md)   
 [Proprietà Ordinal (Cell - ADO MD)](./ordinal-property-ado-md-cell.md)