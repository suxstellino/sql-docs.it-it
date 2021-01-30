---
description: Proprietà LevelDepth (ADO MD)
title: Proprietà LevelDepth (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- LevelDepth
- Member::LevelDepth
helpviewer_keywords:
- LevelDepth property [ADO MD]
ms.assetid: 8a1cfe2c-f207-4445-b152-ade090f64608
author: rothja
ms.author: jroth
ms.openlocfilehash: e987d2cab7f20930bbdefbd4bcf959717a28ba7e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169796"
---
# <a name="leveldepth-property-ado-md"></a>Proprietà LevelDepth (ADO MD)
Indica il numero di livelli tra la radice della gerarchia e un [membro](./member-object-ado-md.md).  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **Long** integer ed è di sola lettura.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **LevelDepth** per determinare la distanza dell'oggetto [membro](./member-object-ado-md.md)dal livello radice della gerarchia. Il **LevelDepth** di un membro al livello radice è 0. Corrisponde alla proprietà [Depth](./depth-property-ado-md.md) di un oggetto [Level](./level-object-ado-md.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Member (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Depth (ADO MD)](./depth-property-ado-md.md)   
 [Oggetto Level (ADO MD)](./level-object-ado-md.md)