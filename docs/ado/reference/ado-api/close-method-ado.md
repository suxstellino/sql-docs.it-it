---
description: Metodo Close (ADO)
title: Metodo Close (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Close
- _Stream::Close
- _Record::Close
helpviewer_keywords:
- Close method [ADO]
ms.assetid: 3cdf27d1-a180-4cff-8e42-95dec5fb1b55
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a7c0ea176f203bd9b7b2febc17475e6af385d7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171519"
---
# <a name="close-method-ado"></a>Metodo Close (ADO)
Chiude un oggetto aperto e tutti gli oggetti dipendenti.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.Close  
```  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare il metodo **Close** per chiudere una [connessione](./connection-object-ado.md), un [record](./record-object-ado.md), un [Recordset](./recordset-object-ado.md)o un oggetto [Stream](./stream-object-ado.md) per liberare tutte le risorse di sistema associate. La chiusura di un oggetto non comporta la rimozione dalla memoria; è possibile modificare le impostazioni delle proprietà e riaprirle in un secondo momento. Per eliminare completamente un oggetto dalla memoria, chiudere l'oggetto e impostare la variabile oggetto su *Nothing* (in Visual Basic).  
  
## <a name="connection"></a>Connessioni  
 L'utilizzo del metodo **Close** per chiudere un oggetto **Connection** chiude anche tutti gli oggetti **Recordset** attivi associati alla connessione. Un oggetto [Command](./command-object-ado.md) associato all'oggetto **Connection** che si sta chiudendo verrà mantenuto, ma non sarà più associato a un oggetto **Connection** . ovvero, la relativa proprietà [ActiveConnection](./activeconnection-property-ado.md) verrà impostata su **Nothing**. Inoltre, la raccolta di [parametri](./parameters-collection-ado.md) dell'oggetto **comando** verrà cancellata da tutti i parametri definiti dal provider.  
  
 In un secondo momento è possibile chiamare il metodo [Open](./open-method-ado-connection.md) per ristabilire la connessione alla stessa origine dati o a un'altra. Mentre l'oggetto **Connection** è chiuso, la chiamata a metodi che richiedono una connessione aperta all'origine dati genera un errore.  
  
 La chiusura di un oggetto **Connection** mentre sono presenti oggetti **Recordset** aperti sulla connessione esegue il rollback di tutte le modifiche in sospeso in tutti gli oggetti **Recordset** . Se si chiude in modo esplicito un oggetto **Connection** (chiamando il metodo **Close** ) mentre è in corso una transazione, viene generato un errore. Se un oggetto **connessione** esula dall'ambito mentre è in corso una transazione, ADO esegue automaticamente il rollback della transazione.  
  
## <a name="recordset-record-stream"></a>Recordset, record, flusso  
 L'utilizzo del metodo **Close** per chiudere un **Recordset**, un **record** o un oggetto **Stream** rilascia i dati associati ed eventuali accessi esclusivi che potrebbero essere stati usati per i dati tramite questo particolare oggetto. In un secondo momento è possibile chiamare il metodo [Open](./open-method-ado-recordset.md) per riaprire l'oggetto con gli stessi attributi o modificati.  
  
 Mentre un oggetto **Recordset** viene chiuso, la chiamata a metodi che richiedono un cursore attivo genera un errore.  
  
 Se è in corso una modifica in modalità di aggiornamento immediato, la chiamata al metodo **Close** genera un errore. in alternativa, chiamare prima il metodo [Update](./update-method.md) o [CancelUpdate](./cancelupdate-method-ado.md) . Se si chiude l'oggetto **Recordset** in modalità di aggiornamento batch, tutte le modifiche apportate dall'ultima chiamata a [UpdateBatch](./updatebatch-method.md) andranno perse.  
  
 Se si usa il metodo [Clone](./clone-method-ado.md) per creare copie di un oggetto **Recordset** aperto, la chiusura dell'originale o di un clone non influisce sulle altre copie.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
        [Oggetto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
        [Oggetto Stream (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Open e Close (VB)](./open-and-close-methods-example-vb.md)   
 [Esempio di metodi Open e Close (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Esempio di metodi Open e Close (VC + +)](./open-and-close-methods-example-vc.md)   
 [Metodo Open (connessione ADO)](./open-method-ado-connection.md)   
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Metodo Save](./save-method.md)