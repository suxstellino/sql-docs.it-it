---
description: ADCPROP_ASYNCTHREADPRIORITY_ENUM
title: ADCPROP_ASYNCTHREADPRIORITY_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_ASYNCTHREADPRIORITY_ENUM
helpviewer_keywords:
- ADCPROP_ASYNCTHREADPRIORITY_ENUM [ADO]
ms.assetid: f0965617-17d8-41e0-98d0-f824274735a6
author: rothja
ms.author: jroth
ms.openlocfilehash: ff25342a8ba7976b883ecdca8e55f8695bf45099
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031473"
---
# <a name="adcprop_asyncthreadpriority_enum"></a>ADCPROP_ASYNCTHREADPRIORITY_ENUM
Per un oggetto [Recordset](./recordset-object-ado.md) RDS, specifica la priorità di esecuzione del thread asincrono che recupera i dati.  
  
 Utilizzare queste costanti con la proprietà dinamica "**priorità thread in background**" del **Recordset** , a cui viene fatto riferimento nell'indice della proprietà dinamica ADO-to-OLE DB e documentato nel [servizio Microsoft Cursor per OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) documentazione.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adPriorityAboveNormal**|4|Imposta la priorità tra il normale e il massimo.|  
|**adPriorityBelowNormal**|2|Imposta la priorità tra il minimo e il normale.|  
|**adPriorityHighest**|5|Imposta la priorità sul massimo possibile.|  
|**AdPriorityLowest**|1|Imposta la priorità sul più basso possibile.|  
|**adPriorityNormal**|3|Imposta la priorità su Normal.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. AdcPropAsyncThreadPriority. ABOVENORMAL|  
|AdoEnums. AdcPropAsyncThreadPriority. BELOWNORMAL|  
|AdoEnums. AdcPropAsyncThreadPriority. massimo|  
|AdoEnums. AdcPropAsyncThreadPriority. minor|  
|AdoEnums. AdcPropAsyncThreadPriority. NORMAL|