---
description: Metodo GetDataProviderDSO
title: Metodo GetDataProviderDSO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- GetDataProviderDSO Method [ADO]
ms.assetid: 5a4c6bd5-0c79-4f81-a977-0561392d8d50
author: rothja
ms.author: jroth
ms.openlocfilehash: c784e5c3951f78ef28bfd0d0008570d1b1e68ca8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167282"
---
# <a name="getdataproviderdso-method"></a>Metodo GetDataProviderDSO
Recupera l'oggetto origine dati OLE DB sottostante dal provider di forme.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT GetDataProviderDSO(  
      IUnknown **ppDataProviderDSOIUnknown  
);  
```  
  
#### <a name="parameters"></a>Parametri  
 *ppDataProviderDSOIUnknown*  
 out  Puntatore a un puntatore che restituisce l'IUnknown dell'oggetto origine dati OLE DB sottostante.  
  
## <a name="remarks"></a>Commenti  
 Questo metodo non AddRef il puntatore di interfaccia. Se il chiamante prevede di mantenere il puntatore, il chiamante deve eseguire il AddRef e il rilascio richiesti.  
  
## <a name="applies-to"></a>Si applica a  
 [Interfaccia IDSOShapeExtensions](./idsoshapeextensions-interface.md)