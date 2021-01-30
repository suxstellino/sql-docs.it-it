---
description: Metodo Update
title: Metodo Update | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Update
helpviewer_keywords:
- Update method [ADO]
ms.assetid: 6b2a9c31-1a7e-40db-8a53-30720d0f6cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 279d0e82bff4d71a2c3b18bbdc7ff88f0b9581a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172429"
---
# <a name="update-method"></a>Metodo Update
Salva le modifiche apportate alla riga corrente di un oggetto [Recordset](./recordset-object-ado.md) o alla raccolta di [campi](./fields-collection-ado.md) di un oggetto [record](./record-object-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.Update Fields, Values  
record.Fields.Update  
```  
  
#### <a name="parameters"></a>Parametri  
 *Fields*  
 facoltativo. **Variant** che rappresenta un singolo nome o una matrice **Variant** che rappresenta nomi o posizioni ordinali del campo o dei campi che si desidera modificare.  
  
 *Valori*  
 facoltativo. **Variant** che rappresenta un singolo valore o una matrice **Variant** che rappresenta i valori per il campo o i campi nel nuovo record.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="recordset"></a>recordset  
 Usare il metodo **Update** per salvare le modifiche apportate al record corrente di un oggetto **Recordset** , a partire dalla chiamata al metodo [AddNew](./addnew-method-ado.md) o dalla modifica dei valori di campo in un record esistente. L'oggetto **Recordset** deve supportare gli aggiornamenti.  
  
 Per impostare i valori dei campi, effettuare una delle operazioni seguenti:  
  
-   Assegnare i valori alla proprietà [value](./value-property-ado.md) di un oggetto [campo](./field-object.md) e chiamare il metodo **Update** .  
  
-   Passare un nome di campo e un valore come argomenti con la chiamata di **aggiornamento** .  
  
-   Passare una matrice di nomi di campo e una matrice di valori con la chiamata di **aggiornamento** .  
  
 Quando si utilizzano matrici di campi e valori, deve essere presente un numero uguale di elementi in entrambe le matrici. Inoltre, l'ordine dei nomi di campo deve corrispondere all'ordine dei valori di campo. Se il numero e l'ordine dei campi e i valori non corrispondono, si verificherà un errore.  
  
 Se l'oggetto **Recordset** supporta l'aggiornamento in batch, è possibile memorizzare nella cache più modifiche a uno o più record localmente fino a quando non si chiama il metodo [UpdateBatch](./updatebatch-method.md) . Se si modifica il record corrente o si aggiunge un nuovo record quando si chiama il metodo **UpdateBatch** , ADO chiamerà automaticamente il metodo **Update** per salvare le modifiche in sospeso apportate al record corrente prima di trasmettere le modifiche in batch al provider.  
  
 Se si passa dal record che si aggiunge o si modifica prima di chiamare il metodo **Update** , ADO chiamerà automaticamente **Update** per salvare le modifiche. È necessario chiamare il metodo [CancelUpdate](./cancelupdate-method-ado.md) se si desidera annullare le modifiche apportate al record corrente o eliminare un record appena aggiunto.  
  
 Il record corrente rimane aggiornato dopo la chiamata al metodo **Update** .  
  
## <a name="record"></a>Registra  
 Il metodo **Update** finalizza le aggiunte, le eliminazioni e gli aggiornamenti ai campi nella raccolta [Fields](./fields-collection-ado.md) di un oggetto **record** .  
  
 Ad esempio, i campi eliminati con il metodo **Delete** vengono contrassegnati per l'eliminazione immediatamente ma rimangono nella raccolta. È necessario chiamare il metodo **Update** per eliminare effettivamente questi campi dalla raccolta del provider.  
  
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
 [Metodo CancelUpdate (ADO)](./cancelupdate-method-ado.md)   
 [Proprietà EditMode](./editmode-property.md)   
 [Metodo UpdateBatch](./updatebatch-method.md)