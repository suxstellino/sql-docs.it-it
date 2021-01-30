---
description: Metodo get_OLEDBCommand
title: Metodo get_OLEDBCommand | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- get_OLEDBCommand method [ADO]
ms.assetid: 23d551f5-3d5b-434b-ade6-fef15f1710e7
author: rothja
ms.author: jroth
ms.openlocfilehash: edd8816389088c077f43ed7b36f01ebd61010e98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171003"
---
# <a name="get_oledbcommand-method"></a>Metodo get_OLEDBCommand
Restituisce il comando OLE DB sottostante, propagando prima di tutto le informazioni sui parametri impostate nel comando ADO al comando OLE DB.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT get_OLEDBCommand(  
      IUnknown **ppOLEDBCommand  
);  
```  
  
#### <a name="parameters"></a>Parametri  
 *ppOLEDBCommand*  
 out Puntatore a una posizione del puntatore in cui verrà scritto il puntatore IUnknown per il OLE DB comando sottostante.  
  
## <a name="applies-to"></a>Si applica a  
 [IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))