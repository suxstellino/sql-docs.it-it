---
description: Metodo CancelUpdate (ADO)
title: Metodo CancelUpdate (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CancelUpdate
helpviewer_keywords:
- CancelUpdate method [ADO]
ms.assetid: eaa856cc-c786-462e-890c-c896261b1741
author: rothja
ms.author: jroth
ms.openlocfilehash: 12ec3d30de9fc938d51ea342c468b522674b0086
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167798"
---
# <a name="cancelupdate-method-ado"></a>Metodo CancelUpdate (ADO)
Annulla tutte le modifiche apportate alla riga corrente o nuova di un oggetto [Recordset](./recordset-object-ado.md) oppure alla raccolta di [campi](./fields-collection-ado.md) di un oggetto [record](./record-object-ado.md) , prima di chiamare il metodo [Update](./update-method.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.CancelUpdaterecord.Fields.CancelUpdate  
```  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="recordset"></a>recordset  
 Utilizzare il metodo **CancelUpdate** per annullare tutte le modifiche apportate alla riga corrente o per eliminare una nuova riga aggiunta. Non è possibile annullare le modifiche apportate alla riga corrente o a una nuova riga dopo aver chiamato il metodo **Update** , a meno che le modifiche non facciano parte di una transazione di cui è possibile eseguire il rollback con il metodo [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) o che facciano parte di un aggiornamento batch. Nel caso di un aggiornamento batch, è possibile annullare l' **aggiornamento** con il metodo **CancelUpdate** o [CancelBatch](./cancelbatch-method-ado.md) .  
  
 Se si aggiunge una nuova riga quando si chiama il metodo **CancelUpdate** , la riga corrente diventa la riga corrente prima della chiamata a [AddNew](./addnew-method-ado.md) .  
  
 Se si è in modalità di modifica e si vuole spostare il record corrente (ad esempio, usando i metodi [Move](./move-method-ado.md), [NextRecordset](./nextrecordset-method-ado.md)o [Close](./close-method-ado.md) ), è possibile usare **CancelUpdate** per annullare le modifiche in sospeso. Potrebbe essere necessario eseguire questa operazione se l'aggiornamento non può essere pubblicato correttamente nell'origine dati. Ad esempio, un tentativo di eliminazione che ha esito negativo a causa di violazioni di integrità referenziale lascerà il **Recordset** in modalità di modifica dopo una chiamata a [Delete](./delete-method-ado-recordset.md).  
  
## <a name="record"></a>Registra  
 Il metodo **CancelUpdate** Annulla tutti gli inserimenti o le eliminazioni in sospeso degli oggetti [campo](./field-object.md) e Annulla gli aggiornamenti in sospeso dei campi esistenti e li ripristina nei valori originali. La proprietà [status](./status-property-ado-recordset.md) di tutti i campi nella raccolta **Fields** è impostata su **adFieldOK**.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Raccolta Fields (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Update e CancelUpdate (VB)](./update-and-cancelupdate-methods-example-vb.md)   
 [Esempio di metodi Update e CancelUpdate (VC + +)](./update-and-cancelupdate-methods-example-vc.md)   
 [Metodo AddNew (ADO)](./addnew-method-ado.md)   
 [Metodo Cancel (ADO)](./cancel-method-ado.md)   
 [Metodo Cancel (RDS)](../rds-api/cancel-method-rds.md)   
 [Metodo CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [Metodo CancelUpdate (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Proprietà EditMode](./editmode-property.md)   
 [Metodo Update](./update-method.md)