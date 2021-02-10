---
description: Metodo Cancel (ADO)
title: Metodo Cancel (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Cancel
- _Record::Cancel
- _Connection::Cancel
- Command25::Cancel
- _Stream::Cancel
helpviewer_keywords:
- Cancel method [ADO]
ms.assetid: e0db4e15-6787-41e2-8f13-9e9b524d620a
author: rothja
ms.author: jroth
ms.openlocfilehash: c33cedffcdd4eb9b738b952afaa060af9d99ff6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027487"
---
# <a name="cancel-method-ado"></a>Metodo Cancel (ADO)
Annulla l'esecuzione di una chiamata al metodo asincrono in sospeso.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.Cancel  
```  
  
## <a name="remarks"></a>Osservazioni  
 Usare il metodo **Cancel** per terminare l'esecuzione di una chiamata al metodo asincrono, ovvero un metodo richiamato con l'opzione **adAsyncConnect**, **adAsyncExecute** o **adAsyncFetch** .  
  
 Nella tabella seguente viene illustrata l'attività terminata quando si utilizza il metodo **Cancel** su un particolare tipo di oggetto.  
  
|Se l'oggetto è un *oggetto*|L'ultima chiamata asincrona a questo metodo viene terminata|  
|----------------------|-------------------------------------------------------------|  
|[Comando](./command-object-ado.md)|[Eseguire](./execute-method-ado-command.md)|  
|[Connection](./connection-object-ado.md)|[Esegui](./execute-method-ado-connection.md) o [Apri](./open-method-ado-connection.md)|  
|[Record](./record-object-ado.md)|[CopyRecord](./copyrecord-method-ado.md), [DeleteRecord](./deleterecord-method-ado.md), [MoveRecord](./moverecord-method-ado.md)o [Open](./open-method-ado-record.md)|  
|[Recordset](./recordset-object-ado.md)|[Apri](./open-method-ado-recordset.md)|  
|[Flusso](./stream-object-ado.md)|[Apri](./open-method-ado-stream.md)|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Command (ADO)](./command-object-ado.md)  
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Record (ADO)](./record-object-ado.md)  
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Stream (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Cancel (VB)](./cancel-method-example-vb.md)   
 [Esempio di metodo Cancel (VBScript)](../rds-api/cancel-method-example-vbscript.md)   
 [Esempio di metodo Cancel (VC + +)](./cancel-method-example-vc.md)   
 [Metodo Cancel (RDS)](../rds-api/cancel-method-rds.md)   
 [Metodo CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [Metodo CancelUpdate (ADO)](./cancelupdate-method-ado.md)   
 [Metodo CancelUpdate (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Metodo Execute (comando ADO)](./execute-method-ado-command.md)   
 [Metodo Execute (connessione ADO)](./execute-method-ado-connection.md)   
 [Metodo Open (connessione ADO)](./open-method-ado-connection.md)   
 [Metodo Open (Recordset - ADO)](./open-method-ado-recordset.md)