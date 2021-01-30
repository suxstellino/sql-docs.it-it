---
description: Metodo CancelBatch (ADO)
title: Metodo CancelBatch (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_CancelBatch
- Recordset15::CancelBatch
helpviewer_keywords:
- CancelBatch method [ADO]
ms.assetid: dbdc2574-e44e-4d95-b03d-4a5d9e9adf3c
author: rothja
ms.author: jroth
ms.openlocfilehash: edbafa81c787bb4dafed6688f43907a87504c3c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164742"
---
# <a name="cancelbatch-method-ado"></a>Metodo CancelBatch (ADO)
Annulla un aggiornamento batch in sospeso.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.CancelBatchAffectRecords  
```  
  
#### <a name="parameters"></a>Parametri  
 *AffectRecords*  
 facoltativo. Valore [AffectEnum](./affectenum.md) che indica il numero di record su cui avrà effetto il metodo **CancelBatch** .  
  
## <a name="remarks"></a>Commenti  
 Usare il metodo **CancelBatch** per annullare gli aggiornamenti in sospeso in un [Recordset](./recordset-object-ado.md) in modalità di aggiornamento batch. Se il **Recordset** è in modalità di aggiornamento immediato, la chiamata a **CancelBatch** senza **adAffectCurrent** genera un errore.  
  
 Se si modifica il record corrente o si aggiunge un nuovo record quando si chiama **CancelBatch**, ADO chiama prima il metodo [CancelUpdate](./cancelupdate-method-ado.md) per annullare le modifiche memorizzate nella cache. Successivamente, tutte le modifiche in sospeso nel **Recordset** vengono annullate.  
  
 Il record corrente può essere determinabile dopo una chiamata **CancelBatch** , soprattutto se si è in corso l'aggiunta di un nuovo record. Per questo motivo, è consigliabile impostare la posizione del record corrente su una posizione nota nel **Recordset** dopo la chiamata a **CancelBatch** . Ad esempio, chiamare il metodo [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
 Se il tentativo di annullare gli aggiornamenti in sospeso ha esito negativo a causa di un conflitto con i dati sottostanti (ad esempio, se un record è stato eliminato da un altro utente), il provider restituisce gli avvisi alla raccolta degli [errori](./errors-collection-ado.md) , ma non interrompe l'esecuzione del programma. Si verifica un errore di run-time solo se sono presenti conflitti in tutti i record richiesti. Utilizzare la proprietà [Filter](./filter-property.md) (**adFilterAffectedRecords**) e la proprietà [status](./status-property-ado-recordset.md) per individuare i record con conflitti.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi UpdateBatch e CancelBatch (VB)](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [Esempio di metodi UpdateBatch e CancelBatch (VC + +)](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [Metodo Cancel (ADO)](./cancel-method-ado.md)   
 [Metodo Cancel (RDS)](../rds-api/cancel-method-rds.md)   
 [Metodo CancelUpdate (ADO)](./cancelupdate-method-ado.md)   
 [Metodo CancelUpdate (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Metodo Clear (ADO)](./clear-method-ado.md)   
 [Proprietà LockType (ADO)](./locktype-property-ado.md)   
 [Metodo UpdateBatch](./updatebatch-method.md)