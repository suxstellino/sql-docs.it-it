---
description: IsolationLevelEnum
title: IsolationLevelEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- IsolationLevelEnum
helpviewer_keywords:
- IsolationLevelEnum enumeration [ADO]
ms.assetid: 8e17a7bc-b8a3-4ae2-b6c9-ce088ad31fdf
author: rothja
ms.author: jroth
ms.openlocfilehash: 780e61f5aabea993f3af4d36b9547cca75377d99
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167198"
---
# <a name="isolationlevelenum"></a>IsolationLevelEnum
Specifica il livello di isolamento della transazione per un oggetto [Connection](./connection-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adXactUnspecified**|-1|Indica che il provider utilizza un livello di isolamento diverso da quello specificato, ma che non è possibile determinare il livello.|  
|**adXactChaos**|16|Indica che non è possibile sovrascrivere le modifiche in sospeso da transazioni più isolate.|  
|**adXactBrowse**|256|Indica che da una transazione è possibile visualizzare le modifiche di cui non è stato eseguito il commit in altre transazioni.|  
|**adXactReadUncommitted**|256|Uguale a **adXactBrowse**.|  
|**adXactCursorStability**|4096|Indica che da una transazione è possibile visualizzare le modifiche in altre transazioni solo dopo che ne è stato eseguito il commit.|  
|**adXactReadCommitted**|4096|Uguale a **adXactCursorStability**.|  
|**adXactRepeatableRead**|65536|Indica che da una transazione non è possibile visualizzare le modifiche apportate in altre transazioni, ma che la riesecuzione delle query può recuperare nuovi oggetti **Recordset** .|  
|**adXactIsolated**|1048576|Indica che le transazioni vengono eseguite nell'isolamento di altre transazioni.|  
|**adXactSerializable**|1048576|Uguale a **adXactIsolated**.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. IsolationLevel. Unspecified|  
|AdoEnums. IsolationLevel. CHAOS|  
|AdoEnums. IsolationLevel. BROWSe|  
|AdoEnums. IsolationLevel. READUNCOMMITTED|  
|AdoEnums. IsolationLevel. CURSORSTABILITY|  
|AdoEnums. IsolationLevel. READCOMMITTED|  
|AdoEnums. IsolationLevel. REPEATABLEREAD|  
|AdoEnums. IsolationLevel. ISOLAted|  
|AdoEnums. IsolationLevel. SERIALIZABLE|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà IsolationLevel](./isolationlevel-property.md)