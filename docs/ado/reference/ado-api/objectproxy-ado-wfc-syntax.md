---
description: ObjectProxy (sintassi ADO/WFC)
title: ObjectProxy (sintassi ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ObjectProxy collection [ADO]
ms.assetid: f68f58bc-ad28-46cc-9fb3-099e1a678397
author: rothja
ms.author: jroth
ms.openlocfilehash: 9e136118be65296cf2a4c97b68afecda1cc6de30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167015"
---
# <a name="objectproxy-ado---wfc-syntax"></a>ObjectProxy (sintassi ADO/WFC)
Un oggetto **ObjectProxy** rappresenta un server e viene restituito dal metodo **CreateObject** dell'oggetto [DataSpace](../rds-api/dataspace-object-rds.md) . La classe ObjectProxy dispone di un metodo, **Call**, che può richiamare un metodo sul server e restituire un oggetto risultante da tale chiamata.  
  
 **pacchetto com. ms. wfc. Data**  
  
## <a name="methods"></a>Metodi  
  
### <a name="call-method-adowfc-syntax"></a>Metodo Call (sintassi ADO/WFC)  
 Richiama un metodo sul server rappresentato da ObjectProxy. Facoltativamente, gli argomenti del metodo possono essere passati come una matrice di oggetti.  
  
#### <a name="syntax"></a>Sintassi  
  
```  
public Object ObjectProxy.( String method )  
public Object ObjectProxy.( String method, Object[] args)  
```  
  
#### <a name="returns"></a>Restituisce  
 Oggetto  
 Oggetto risultante dalla chiamata al metodo.  
  
#### <a name="parameters"></a>Parametri  
 *ObjectProxy*  
 Oggetto **ObjectProxy** che rappresenta il server.  
  
 *Metodo*  
 Stringa contenente il nome del metodo da richiamare sul server.  
  
 *args*  
 facoltativo. Matrice di oggetti che sono argomenti del metodo nel server. I tipi di dati Java vengono automaticamente convertiti in tipi di dati appropriati per l'utilizzo nel server.