---
description: RecordStatusEnum
title: RecordStatusEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordStatusEnum
helpviewer_keywords:
- RecordStatusEnum enumeration [ADO]
ms.assetid: 506fdd70-4452-4e83-95d5-c94311988dfa
author: rothja
ms.author: jroth
ms.openlocfilehash: 695c0496f81afcb3d03991360fb9480d956740f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166684"
---
# <a name="recordstatusenum"></a>RecordStatusEnum
Specifica lo [stato](./status-property-ado-recordset.md) di un record per quanto riguarda gli aggiornamenti in batch e altre operazioni bulk.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adRecCanceled**|0x100|Indica che il record non è stato salvato perché l'operazione è stata annullata.|  
|**adRecCantRelease**|0x400|Indica che il nuovo record non è stato salvato perché il record esistente è stato bloccato.|  
|**adRecConcurrencyViolation**|0x800|Indica che il record non è stato salvato perché è in uso la concorrenza ottimistica.|  
|**adRecDBDeleted**|0x40000|Indica che il record è già stato eliminato dall'origine dati.|  
|**adRecDeleted**|0x4|Indica che il record è stato eliminato.|  
|**adRecIntegrityViolation**|0x1000|Indica che il record non è stato salvato perché l'utente ha violato i vincoli di integrità.|  
|**adRecInvalid**|0x10|Indica che il record non è stato salvato perché il relativo segnalibro non è valido.|  
|**adRecMaxChangesExceeded**|0x2000|Indica che il record non è stato salvato a causa di un numero eccessivo di modifiche in sospeso.|  
|**adRecModified**|0x2|Indica che il record è stato modificato.|  
|**adRecMultipleChanges**|0x40|Indica che il record non è stato salvato perché avrebbe interessato più record.|  
|**adRecNew**|0x1|Indica che il record è nuovo.|  
|**adRecObjectOpen**|0x4000|Indica che il record non è stato salvato a causa di un conflitto con un oggetto di archiviazione aperto.|  
|**adRecOK**|0|Indica che il record è stato aggiornato correttamente.|  
|**adRecOutOfMemory**|0x8000|Indica che il record non è stato salvato perché la memoria del computer è esaurita.|  
|**adRecPendingChanges**|0x80|Indica che il record non è stato salvato perché fa riferimento a un inserimento in sospeso.|  
|**adRecPermissionDenied**|0x10000|Indica che il record non è stato salvato perché l'utente non dispone di autorizzazioni sufficienti.|  
|**adRecSchemaViolation**|0x20000|Indica che il record non è stato salvato poiché viola la struttura del database sottostante.|  
|**adRecUnmodified**|0x8|Indica che il record non è stato modificato.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 AdoEnums. RecordStatus.  
  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. RecordStatus. annullato|  
|AdoEnums. RecordStatus. CANTRELEASE|  
|AdoEnums. RecordStatus. CONCURRENCYVIOLATION|  
|AdoEnums. RecordStatus. DBDELETED|  
|AdoEnums. RecordStatus. DELETED|  
|AdoEnums. RecordStatus. INTEGRITYVIOLATION|  
|AdoEnums. RecordStatus. non valido|  
|AdoEnums. RecordStatus. MAXCHANGESEXCEEDED|  
|AdoEnums. RecordStatus. MODIFIED|  
|AdoEnums. RecordStatus. MULTIPLECHANGES|  
|AdoEnums. RecordStatus. NEW|  
|AdoEnums. RecordStatus. OBJECTOPEN|  
|AdoEnums. RecordStatus. OK|  
|AdoEnums. RecordStatus. OUTOFMEMORY|  
|AdoEnums. RecordStatus. PENDINGCHANGES|  
|AdoEnums. RecordStatus. PERMISSIONDENIED|  
|AdoEnums. RecordStatus. SCHEMAVIOLATION|  
|AdoEnums. RecordStatus. unmodified|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà Status (Recordset - ADO)](./status-property-ado-recordset.md)