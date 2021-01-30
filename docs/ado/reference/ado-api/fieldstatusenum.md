---
description: FieldStatusEnum
title: FieldStatusEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldStatusEnum
helpviewer_keywords:
- FieldStatusEnum enumeration [ADO]
ms.assetid: e06da1e2-303f-41b2-a3b0-61e233da152c
author: rothja
ms.author: jroth
ms.openlocfilehash: 968a4205a12e9f6ff36c3bff512cf41ae60ca469
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167349"
---
# <a name="fieldstatusenum"></a>FieldStatusEnum
Specifica lo [stato](./status-property-ado-field.md) di un [oggetto campo](./field-object.md).  
  
 I valori ** \* adFieldPending* indicano l'operazione che ha causato l'impostazione dello stato e possono essere combinati con altri valori di stato.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|_ *adFieldAlreadyExists**|26|Indica che il campo specificato esiste già.|  
|**adFieldBadStatus**|12|Indica che un valore di stato non valido è stato inviato da ADO al provider OLE DB. Le possibili cause includono un provider OLE DB 1,0 o 1,1 o una combinazione non corretta di [valore](./value-property-ado.md) e [stato](./status-property-ado-field.md).|  
|**adFieldCannotComplete**|20|Indica che il server dell'URL specificato da [source](./source-property-ado-record.md) non è riuscito a completare l'operazione.|  
|**adFieldCannotDeleteSource**|23|Indica che durante un'operazione di spostamento un albero o un sottoalbero è stato spostato in una nuova posizione, ma non è stato possibile eliminare l'origine.|  
|**adFieldCantConvertValue**|2|Indica che il campo non può essere recuperato o archiviato senza perdita di dati.|  
|**adFieldCantCreate**|7|Indica che non è stato possibile aggiungere il campo perché il provider ha superato una limitazione, ad esempio il numero di campi consentiti.|  
|**adFieldDataOverflow**|6|Indica che i dati restituiti dal provider hanno causato un overflow del tipo di dati del campo.|  
|**adFieldDefault**|13|Indica che durante l'impostazione dei dati è stato utilizzato il valore predefinito per il campo.|  
|**adFieldDoesNotExist**|16|Indica che il campo specificato non esiste.|  
|**adFieldIgnore**|15|Indica che questo campo è stato ignorato durante l'impostazione dei valori dei dati nell'origine. Il provider non ha impostato alcun valore.|  
|**adFieldIntegrityViolation**|10|Indica che il campo non può essere modificato perché è un'entità calcolata o derivata.|  
|**adFieldInvalidURL**|17|Indica che l'URL dell'origine dati contiene caratteri non validi.|  
|**adFieldIsNull**|3|Indica che il provider ha restituito un valore VARIANT di tipo VT_NULL e che il campo non è vuoto.|  
|**adFieldOK**|0|Valore predefinito. Indica che il campo è stato aggiunto o eliminato correttamente.|  
|**adFieldOutOfSpace**|22|Indica che il provider non è in grado di ottenere spazio di archiviazione sufficiente per completare un'operazione di spostamento o copia.|  
|**adFieldPendingChange**|0x40000|Indica che il campo è stato eliminato e quindi aggiunto nuovamente, ad esempio con un tipo di dati diverso o che è stato modificato il valore del campo che in precedenza aveva lo stato **adFieldOK** . Il formato finale del campo modificherà la raccolta [Fields](./fields-collection-ado.md) dopo la chiamata del metodo [Update](./update-method.md) .|  
|**adFieldPendingDelete**|0x20000|Indica che l'operazione di **eliminazione** ha causato l'impostazione dello stato. Il campo è stato contrassegnato per l'eliminazione dalla raccolta **Fields** dopo la chiamata del metodo **Update** .|  
|**adFieldPendingInsert**|0x10000|Indica che l'operazione di **Accodamento** ha causato l'impostazione dello stato. Il **campo** è stato contrassegnato per essere aggiunto alla raccolta **Fields** dopo la chiamata del metodo **Update** .|  
|**adFieldPendingUnknown**|0x80000|Indica che il provider non è in grado di determinare quale operazione ha causato l'impostazione dello stato dei campi.|  
|**adFieldPendingUnknownDelete**|0x100000|Indica che il provider non è in grado di determinare quale operazione ha causato l'impostazione dello stato del campo e che il campo verrà eliminato dalla raccolta **Fields** dopo la chiamata del metodo **Update** .|  
|**adFieldPermissionDenied**|9|Indica che il campo non può essere modificato perché è definito come di sola lettura.|  
|**adFieldReadOnly**|24|Indica che il campo nell'origine dati è definito come di sola lettura.|  
|**adFieldResourceExists**|19|Indica che il provider non è stato in grado di eseguire l'operazione perché un oggetto esiste già nell'URL di destinazione e non è in grado di sovrascrivere l'oggetto.|  
|**adFieldResourceLocked**|18|Indica che il provider non è stato in grado di eseguire l'operazione perché l'origine dati è bloccata da una o più applicazioni o processi.|  
|**adFieldResourceOutOfScope**|25|Indica che un URL di origine o di destinazione esula dall'ambito del record corrente.|  
|**adFieldSchemaViolation**|11|Indica che il valore viola il vincolo dello schema dell'origine dati per il campo.|  
|**adFieldSignMismatch**|5|Indica che il valore di dati restituito dal provider è stato firmato, ma il tipo di dati del valore del campo ADO è senza segno.|  
|**adFieldTruncated**|4|Indica che i dati a lunghezza variabile sono stati troncati durante la lettura dall'origine dati.|  
|**adFieldUnavailable**|8|Indica che il provider non è stato in grado di determinare il valore durante la lettura dall'origine dati. Ad esempio, la riga è stata appena creata, il valore predefinito per la colonna non è disponibile e non è stato ancora specificato un nuovo valore.|  
|**adFieldVolumeNotFound**|21|Indica che il provider non è in grado di individuare il volume di archiviazione indicato dall'URL.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Queste costanti non dispongono di equivalenti ADO/WFC.  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà Status (Field - ADO)](./status-property-ado-field.md)