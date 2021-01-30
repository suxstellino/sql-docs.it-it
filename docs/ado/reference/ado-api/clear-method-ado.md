---
description: Metodo Clear (ADO)
title: Metodo Clear (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Errors::raw_Clear
- Errors::Clear
helpviewer_keywords:
- Clear method [ADO]
ms.assetid: 0a61ba7a-20b8-426a-91a0-9040e7c5a98a
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f79e81947a549057db5dbff7f6715492d08923a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164714"
---
# <a name="clear-method-ado"></a>Metodo Clear (ADO)
Rimuove tutti gli oggetti [Error](./error-object.md) dalla raccolta [Errors](./errors-collection-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Errors.Clear  
```  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare il metodo **Clear** sulla raccolta [Errors](./errors-collection-ado.md) per rimuovere tutti gli oggetti [Error](./error-object.md) esistenti dalla raccolta. Quando si verifica un errore, ADO cancella automaticamente la raccolta degli **errori** e la riempie con gli oggetti **Error** in base al nuovo errore.  
  
 Alcune proprietà e metodi restituiscono avvisi visualizzati come oggetti **Error** nella raccolta **Errors** ma non interrompono l'esecuzione di un programma. Prima di chiamare i metodi [Resync](./resync-method.md), [UpdateBatch](./updatebatch-method.md)o [CancelBatch](./cancelbatch-method-ado.md) su un oggetto [Recordset](./recordset-object-ado.md) ; metodo [Open](./open-method-ado-connection.md) su un oggetto [Connection](./connection-object-ado.md) . in alternativa, impostare la proprietà [Filter](./filter-property.md) per un oggetto **Recordset** , chiamare il metodo **Clear** sulla raccolta **Errors** . In questo modo, è possibile leggere la proprietà [count](./count-property-ado.md) della raccolta **Errors** per verificare la presenza di avvisi restituiti.  
  
## <a name="applies-to"></a>Si applica a  
 [Raccolta Errors (ADO)](./errors-collection-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Execute, Requery e Clear (VB)](./execute-requery-and-clear-methods-example-vb.md)   
 [Esempio di metodi Execute, Requery e Clear (VBScript)](./execute-requery-and-clear-methods-example-vbscript.md)   
 [Esempio di metodi Execute, Requery e Clear (VC + +)](./execute-requery-and-clear-methods-example-vc.md)   
 [Metodo CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [Metodo Delete (raccolta di campi ADO)](./delete-method-ado-fields-collection.md)   
 [Metodo Delete (raccolta Parameters ADO)](./delete-method-ado-parameters-collection.md)   
 [Metodo Delete (recordset ADO)](./delete-method-ado-recordset.md)   
 [Filter (proprietà)](./filter-property.md)   
 [Metodo di risincronizzazione](./resync-method.md)   
 [Metodo UpdateBatch](./updatebatch-method.md)