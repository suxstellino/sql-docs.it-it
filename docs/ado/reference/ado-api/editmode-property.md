---
description: Proprietà EditMode
title: Proprietà EditMode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::EditMode
helpviewer_keywords:
- EditMode property
ms.assetid: a1b04bb2-8c8b-47f9-8477-bfd0368b6f68
author: rothja
ms.author: jroth
ms.openlocfilehash: 5cec956ced454f0220b8d545e8e05c8fbcf8a760
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025412"
---
# <a name="editmode-property"></a>Proprietà EditMode
Indica lo stato di modifica del record corrente.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore [EditModeEnum](../../../ado/reference/ado-api/editmodeenum.md) .  
  
## <a name="remarks"></a>Commenti  
 ADO gestisce un buffer di modifica associato al record corrente. Questa proprietà indica se sono state apportate modifiche a questo buffer o se è stato creato un nuovo record. Utilizzare la proprietà **EditMode** per determinare lo stato di modifica del record corrente. È possibile verificare la presenza di modifiche in sospeso se un processo di modifica è stato interrotto e determinare se è necessario utilizzare il metodo [Update](../../../ado/reference/ado-api/update-method.md) o [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) .  
  
 In *modalità di aggiornamento immediato* la proprietà **EditMode** viene reimpostata su **adEditNone** dopo che è stata chiamata una chiamata al metodo **Update** . Quando una chiamata a [Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md) non elimina correttamente il record o i record nell'origine dati (ad esempio, a causa di violazioni di integrità referenziale), il [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) rimane in modalità di modifica (**EditMode**  =  **adEditInProgress**). Pertanto, **CancelUpdate** deve essere chiamato prima di spostare il record corrente (ad esempio, [Move](../../../ado/reference/ado-api/move-method-ado.md), [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)o [Close](../../../ado/reference/ado-api/close-method-ado.md) ).  
  
 In *modalità di aggiornamento batch* (in cui il provider memorizza nella cache più modifiche e le scrive nell'origine dati sottostante solo quando si chiama il metodo [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) ), il valore della proprietà **EditMode** viene modificato quando viene eseguita la prima operazione e non viene reimpostato da una chiamata al metodo **Update** . Le operazioni successive non modificano il valore della proprietà **EditMode** , anche se vengono eseguite operazioni diverse. Ad esempio, se la prima operazione consiste nell'aggiungere un nuovo record e il secondo apporta modifiche a un record esistente, la proprietà di **EditMode** sarà ancora **adEditAdd**. La proprietà **EditMode** non viene reimpostata su **adEditNone** fino al termine della chiamata a **UpdateBatch**. Per determinare quali operazioni sono state eseguite, impostare la proprietà [Filter](../../../ado/reference/ado-api/filter-property.md) su [adFilterPending](../../../ado/reference/ado-api/filtergroupenum.md) in modo da rendere visibili solo i record con modifiche in sospeso ed esaminare la proprietà [status](../../../ado/reference/ado-api/status-property-ado-recordset.md) di ogni record per determinare quali modifiche sono state apportate ai dati.  
  
> [!NOTE]
>  **EditMode** può restituire un valore valido solo se è presente un record corrente. **EditMode** restituirà un errore se [BOF o EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) è true o se il record corrente è stato eliminato.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà CursorType, LockType e EditMode (VB)](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vb.md)   
 [Esempio di proprietà CursorType, LockType e EditMode (VC + +)](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vc.md)   
 [Metodo AddNew (ADO)](../../../ado/reference/ado-api/addnew-method-ado.md)   
 [Metodo Delete (recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Metodo CancelUpdate (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [Metodo Update](../../../ado/reference/ado-api/update-method.md)
