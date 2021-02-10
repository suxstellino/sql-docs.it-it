---
description: CursorOptionEnum
title: CursorOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CursorOptionEnum
helpviewer_keywords:
- CursorOptionEnum enumeration [ADO]
ms.assetid: 4e10cda7-ce81-4466-94c2-844d38191cf1
author: rothja
ms.author: jroth
ms.openlocfilehash: 53916dc6392bc2889b694233c80dae809e254240
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025975"
---
# <a name="cursoroptionenum"></a>CursorOptionEnum
Specifica la funzionalità che il metodo [supporta](./supports-method.md) deve testare.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adAddNew**|0x1000400|Supporta il metodo [AddNew](./addnew-method-ado.md) per aggiungere nuovi record.|  
|**adApproxPosition**|0x4000|Supporta le proprietà [AbsolutePosition](./absoluteposition-property-ado.md) e [AbsolutePage](./absolutepage-property-ado.md) .|  
|**adBookmark**|0x2000|Supporta la proprietà [Bookmark](./bookmark-property-ado.md) per accedere a record specifici.|  
|**adDelete**|0x1000800|Supporta il metodo [Delete](./delete-method-ado-recordset.md) per eliminare i record.|  
|**adFind**|0x80000|Supporta il metodo [Find](./find-method-ado.md) per individuare una riga in un [Recordset](./recordset-object-ado.md).|  
|**adHoldRecords**|0x100|Recupera più record o modifica la posizione successiva senza eseguire il commit di tutte le modifiche in sospeso.|  
|**adIndex**|0x100000|Supporta la proprietà [index](./index-property.md) per assegnare un nome a un indice.|  
|**adMovePrevious**|0x200|Supporta i metodi [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) e [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) e i metodi [Move](./move-method-ado.md) o [GetRows](./getrows-method-ado.md) per spostare la posizione del record corrente indietro senza richiedere segnalibri.|  
|**adNotify**|0x40000|Indica che il provider di dati sottostante supporta le notifiche, che determinano se gli eventi del **Recordset** sono supportati.|  
|**adResync**|0x20000|Supporta il metodo di [Risincronizzazione](./resync-method.md) per aggiornare il cursore con i dati visibili nel database sottostante.|  
|**adSeek**|0x200000|Supporta il metodo [Seek](./seek-method.md) per individuare una riga in un **Recordset**.|  
|**adUpdate**|0x1008000|Supporta il metodo [Update](./update-method.md) per modificare i dati esistenti.|  
|**adUpdateBatch**|0x10000|Supporta l'aggiornamento in batch (metodi[UpdateBatch](./updatebatch-method.md) e [CancelBatch](./cancelbatch-method-ado.md) ) per trasmettere i gruppi di modifiche al provider.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. CursorOption. ADDNEW|  
|AdoEnums. CursorOption. APPROXPOSITION|  
|AdoEnums. CursorOption. BOOKMARK|  
|AdoEnums. CursorOption. DELETE|  
|AdoEnums. CursorOption. FIND|  
|AdoEnums. CursorOption. HOLDRECORDS|  
|AdoEnums. CursorOption. INDEX|  
|AdoEnums. CursorOption. MOVEPREVIOUS|  
|AdoEnums. CursorOption. NOTIFY|  
|AdoEnums. CursorOption. Resync|  
|AdoEnums. CursorOption. SEEK|  
|AdoEnums. CursorOption. UPDATE|  
|AdoEnums. CursorOption. UPDATEBATCH|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo Supports](./supports-method.md)