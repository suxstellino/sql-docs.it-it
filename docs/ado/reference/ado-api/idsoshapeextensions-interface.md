---
description: Interfaccia IDSOShapeExtensions
title: Interfaccia IDSOShapeExtensions | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- IDSOShapeExtensions interface [ADO]
ms.assetid: ad4ba313-1161-4bc7-b8f6-4083305bc81e
author: rothja
ms.author: jroth
ms.openlocfilehash: 329c7b3193bd86ad05c757229477b5934d6e650d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020798"
---
# <a name="idsoshapeextensions-interface"></a>Interfaccia IDSOShapeExtensions
Ottiene l'oggetto origine dati OLE DB sottostante per il provider di forme.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
interface IDSOShapeExtensions: public IUnknown {  
public:  
      HRESULT  GetDataProviderDSO(  
            IUnknown **ppDataProviderDSOIUnknown  
      );  
};  
```  
  
## <a name="methods"></a>Metodi  
  
|Metodo|Descrizione|  
|-|-|  
|[Metodo GetDataProviderDSO](./getdataproviderdso-method.md)|Recupera l'oggetto origine dati OLE DB sottostante dal provider di forme.|  
  
## <a name="requirements"></a>Requisiti  
 **Versione:** ADO 2,0 e versioni successive  
  
 **Libreria:** msado15.dll  
  
 **UUID:** 00000283-0000-0010-8000-00AA006D2EA4