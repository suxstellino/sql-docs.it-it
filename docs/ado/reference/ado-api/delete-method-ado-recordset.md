---
description: Metodo Delete (Recordset - ADO)
title: Metodo Delete (recordset ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Delete
- Recordset15::Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 1eb9209c-602c-4507-b0c2-6527a599b67d
author: rothja
ms.author: jroth
ms.openlocfilehash: 81caf2da7f342cc7c31d1e4cd2bbad5b83852e9a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034401"
---
# <a name="delete-method-ado-recordset"></a>Metodo Delete (Recordset - ADO)
Elimina il record corrente o un gruppo di record.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.Delete AffectRecords  
```  
  
#### <a name="parameters"></a>Parametri  
 *AffectRecords*  
 Valore di [AffectEnum](../../../ado/reference/ado-api/affectenum.md) che determina il numero di record che influiranno sul metodo **Delete** . Il valore predefinito è **adAffectCurrent**.  
  
> [!NOTE]
>  **adAffectAll** e **adAffectAllChapters** non sono argomenti validi da **eliminare**.  
  
## <a name="remarks"></a>Commenti  
 L'utilizzo del metodo **Delete** contrassegna il record corrente o un gruppo di record in un oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) per l'eliminazione. Se l'oggetto **Recordset** non consente l'eliminazione dei record, si verificherà un errore. Se si è in modalità di aggiornamento immediato, le eliminazioni vengono eseguite immediatamente nel database. Se un record non può essere eliminato correttamente (ad esempio, a causa di violazioni di integrità del database), il record rimarrà in modalità di modifica dopo la chiamata a [Update](../../../ado/reference/ado-api/update-method.md). Ciò significa che è necessario annullare l'aggiornamento con [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) prima di spostare il record corrente (ad esempio, con [Close](../../../ado/reference/ado-api/close-method-ado.md), [Move](../../../ado/reference/ado-api/move-method-ado.md)o [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)).  
  
 Se si è in modalità di aggiornamento batch, i record vengono contrassegnati per l'eliminazione dalla cache e l'effettiva eliminazione viene eseguita quando si chiama il metodo [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) . Utilizzare la proprietà [Filter](../../../ado/reference/ado-api/filter-property.md) per visualizzare i record eliminati.  
  
 Il recupero dei valori di campo dal record eliminato genera un errore. Dopo l'eliminazione del record corrente, il record eliminato rimane aggiornato fino a quando non si passa a un record diverso. Quando ci si allontana dal record eliminato, non è più accessibile.  
  
 Se le eliminazioni vengono annidate in una transazione, è possibile recuperare i record eliminati con il metodo [RollbackTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) . Se si è in modalità di aggiornamento batch, è possibile annullare un'eliminazione in sospeso o un gruppo di eliminazioni in sospeso con il metodo [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) .  
  
 Se il tentativo di eliminare i record ha esito negativo a causa di un conflitto con i dati sottostanti (ad esempio, un record è già stato eliminato da un altro utente), il provider restituisce gli avvisi alla raccolta degli [errori](../../../ado/reference/ado-api/errors-collection-ado.md) , ma non interrompe l'esecuzione del programma. Si verifica un errore di run-time solo se sono presenti conflitti in tutti i record richiesti.  
  
 Se viene impostata la proprietà dinamica della [tabella univoca](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) e il **Recordset** è il risultato dell'esecuzione di un'operazione di join su più tabelle, il metodo **Delete** eliminerà solo le righe della tabella denominata nella proprietà [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Delete (VB)](../../../ado/reference/ado-api/delete-method-example-vb.md)   
 [Esempio di metodo Delete (VBScript)](../../../ado/reference/ado-api/delete-method-example-vbscript.md)   
 [Esempio di metodo Delete (VC + +)](../../../ado/reference/ado-api/delete-method-example-vc.md)   
 [Metodo Delete (raccolta di campi ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [Metodo Delete (raccolta Parameters ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [Metodo DeleteRecord (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
