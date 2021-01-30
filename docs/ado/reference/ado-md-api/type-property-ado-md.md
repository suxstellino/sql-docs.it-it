---
description: Proprietà Type (ADO MD)
title: Proprietà Type (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Type
- Type
helpviewer_keywords:
- Type property [ADO MD]
ms.assetid: 34698910-64b9-41d8-8531-9de12f2b1e32
author: rothja
ms.author: jroth
ms.openlocfilehash: 383105d5e33c3443cda93c5d2e8db77aaef57134
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169685"
---
# <a name="type-property-ado-md"></a>Proprietà Type (ADO MD)
Indica il tipo del [membro](./member-object-ado-md.md)corrente.  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore [MemberTypeEnum](./membertypeenum.md) ed è di sola lettura.  
  
## <a name="remarks"></a>Commenti  
 Questa proprietà è supportata solo per gli oggetti [membro](./member-object-ado-md.md) che appartengono a un oggetto [Level](./level-object-ado-md.md) . Si verifica un errore quando si fa riferimento a questa proprietà dagli oggetti **membro** che appartengono a un oggetto [position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Member (ADO MD)](./member-object-ado-md.md)