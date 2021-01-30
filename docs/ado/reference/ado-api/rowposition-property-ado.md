---
description: Proprietà RowPosition (ADO)
title: Proprietà RowPosition (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::put_RowPosition
- ADORecordConstruction::PutRowPosition
- ADORecordConstruction::GetRowPosition
- ADORecordConstruction::RowPosition
- ADORecordConstruction::get_RowPosition
helpviewer_keywords:
- RowPosition property [ADO]
ms.assetid: 9d068fed-39bf-4842-afc3-686a2af2145d
author: rothja
ms.author: jroth
ms.openlocfilehash: a836ca9dd23e6bb43d6bcf3a2707c4b523d530f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166618"
---
# <a name="rowposition-property-ado"></a>Proprietà RowPosition (ADO)
Ottiene o imposta un OLE DB oggetto **RowPosition** da/in un oggetto **ADORecordsetConstruction** . Quando si utilizza **put_RowPosition** per impostare l'oggetto **RowPosition** , l'oggetto **Recordset** risultante utilizza l'oggetto **RowPosition** per determinare la riga corrente.  
  
 Proprietà di lettura/scrittura.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT get_RowPosition([out, retval] IUnknown** ppRowPos);  
HRESULT put_RowPosition([in] IUnknown* pRowPos);  
```  
  
## <a name="parameters"></a>Parametri  
 *ppRowPos*  
 Puntatore a un oggetto OLE DB **RowPosition** .  
  
 *PRowPos*  
 Oggetto OLE DB **RowPosition** .  
  
## <a name="return-values"></a>Valori restituiti  
 Questo metodo di proprietà restituisce i valori HRESULT standard, inclusi S_OK e E_FAIL.  
  
## <a name="remarks"></a>Commenti  
 Quando questa proprietà è impostata, se l'oggetto **set di righe** nell'oggetto **RowPosition** è diverso dall'oggetto **set di righe** nell'oggetto **Recordset** , il primo esegue l'override di quest'ultimo. Lo stesso comportamento si applica anche al **capitolo** corrente di **RowPosition** .  
  
## <a name="applies-to"></a>Si applica a  
 [Interfaccia ADORecordsetConstruction](./adorecordsetconstruction-interface.md)