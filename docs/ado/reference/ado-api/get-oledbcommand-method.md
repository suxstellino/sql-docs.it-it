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
ms.openlocfilehash: f287d19a4a34e9c71f06aab63910465ade3982d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024195"
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