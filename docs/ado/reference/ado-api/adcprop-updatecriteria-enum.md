---
description: ADCPROP_UPDATECRITERIA_ENUM
title: ADCPROP_UPDATECRITERIA_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_UPDATECRITERIA_ENUM
helpviewer_keywords:
- ADCPROP_UPDATECRITERIA_ENUM [ADO]
ms.assetid: 33fd7b65-2ec8-4f62-91a7-630b5dab1aa2
author: rothja
ms.author: jroth
ms.openlocfilehash: 2cf53dbf2f1e49bf61420fa99d015932535def3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031463"
---
# <a name="adcprop_updatecriteria_enum"></a>ADCPROP_UPDATECRITERIA_ENUM
Specifica i campi che è possibile utilizzare per rilevare i conflitti durante un aggiornamento ottimistico di una riga dell'origine dati con un oggetto [Recordset](./recordset-object-ado.md) .  
  
 Utilizzare queste costanti con la proprietà dinamica "**criteri di aggiornamento**" del **Recordset** , a cui viene fatto riferimento nell' [indice della proprietà dinamica ADO](./ado-dynamic-property-index.md) e documentato nel [servizio Microsoft Cursor per OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) documentazione.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adCriteriaAllCols**|1|Rileva i conflitti se è stata modificata una colonna della riga dell'origine dati.|  
|**adCriteriaKey**|0|Rileva i conflitti se la colonna chiave della riga dell'origine dati è stata modificata, il che significa che la riga è stata eliminata.|  
|**adCriteriaTimeStamp**|3|Rileva i conflitti se il timestamp della riga dell'origine dati è stato modificato, il che significa che è stato eseguito l'accesso alla riga dopo che il **Recordset** è stato ottenuto.|  
|**adCriteriaUpdCols**|2|Rileva i conflitti se una delle colonne della riga dell'origine dati che corrispondono ai campi aggiornati del **Recordset** è stata modificata.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. AdcPropUpdateCriteria. ALLCOLS|  
|AdoEnums. AdcPropUpdateCriteria. KEY|  
|AdoEnums. AdcPropUpdateCriteria. TIMESTAMP|  
|AdoEnums. AdcPropUpdateCriteria. UPDCOLS|