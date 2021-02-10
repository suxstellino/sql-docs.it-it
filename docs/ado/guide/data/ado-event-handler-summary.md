---
description: Eventi di connessione ADO e recordset
title: Riepilogo del gestore eventi ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- event handlers [ADO]
ms.assetid: b34f4472-5e04-4a2c-ab64-38d6eca31a69
author: rothja
ms.author: jroth
ms.openlocfilehash: a447c149769d892149096c980b35e77e49de05da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028363"
---
# <a name="ado-connection-and-recordset-events"></a>Eventi di connessione ADO e recordset
Due oggetti ADO possono generare eventi, ovvero l'oggetto [connessione](../../reference/ado-api/connection-object-ado.md) e l'oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) . La famiglia **ConnectionEvent** si riferisce alle operazioni sull'oggetto **Connection** e la famiglia **RecordsetEvent** riguarda le operazioni sull'oggetto **Recordset** .

-   **Eventi di connessione**: gli eventi vengono generati quando viene avviata una transazione in una connessione, ne viene eseguito il commit o ne viene eseguito il rollback; Quando viene eseguito un [comando](../../reference/ado-api/command-object-ado.md) ; Quando viene generato un avviso durante un'operazione relativa a un **evento di connessione** ; oppure quando viene avviata o terminata una **connessione** .

-   **Eventi recordset**: gli eventi vengono emessi per operazioni di recupero asincrone e quando si navigano tra le righe di un oggetto **Recordset** , si modifica un campo in una riga di un **Recordset**, si modifica una riga in un **Recordset**, si apre un **Recordset** con un cursore sul lato server, si chiude un **Recordset** o si apportano modifiche nel **Recordset**.

 Nelle tabelle seguenti vengono riepilogati gli eventi e le relative descrizioni.

|ConnectionEvent|Descrizione|
|---------------------|-----------------|
|[BeginTransComplete, CommitTransComplete, RollbackTransComplete](../../reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|**Gestione delle transazioni** : notifica che la transazione corrente sulla connessione è stata avviata, ne è stato eseguito il commit o ne è stato eseguito il rollback.|
|[WillConnect](../../reference/ado-api/willconnect-event-ado.md), [ConnectComplete, Disconnetti](../../reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|**Gestione della connessione** : notifica che la connessione corrente viene avviata, è stata avviata o è terminata.|
|[WillExecute](../../reference/ado-api/willexecute-event-ado.md), [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md)|**Gestione esecuzione comandi** : notifica che l'esecuzione del comando corrente sulla connessione viene avviata o terminata.|
|[InfoMessage](../../reference/ado-api/infomessage-event-ado.md)|**Informational** : notifica che sono presenti informazioni aggiuntive sull'operazione corrente.|

|RecordsetEvent|Descrizione|
|--------------------|-----------------|
|[FetchProgress](../../reference/ado-api/fetchprogress-event-ado.md), [FetchComplete](../../reference/ado-api/fetchcomplete-event-ado.md)|**Stato di recupero** : notifica dello stato di avanzamento di un'operazione di recupero dei dati o del completamento dell'operazione di recupero. Questi eventi sono disponibili solo se il **Recordset** è stato aperto utilizzando un cursore sul lato client.|
|[WillChangeField, FieldChangeComplete](../../reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|**Gestione delle modifiche dei campi** : notifica che il valore del campo corrente cambierà o è stato modificato.|
|[WillMove, MoveComplete](../../reference/ado-api/willmove-and-movecomplete-events-ado.md), [EndOfRecordset](../../reference/ado-api/endofrecordset-event-ado.md)|**Gestione della navigazione** : notifica che la posizione della riga corrente in un **Recordset** cambierà, è cambiata o ha raggiunto la fine del **Recordset**.|
|[WillChangeRecord, RecordChangeComplete](../../reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|**Gestione delle modifiche delle righe** : notifica che un elemento nella riga corrente del **Recordset** cambierà o è stato modificato.|
|[WillChangeRecordset, RecordsetChangeComplete](../../reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|**Gestione modifiche recordset** : notifica che un elemento del **Recordset** corrente cambierà o è stato modificato.|

## <a name="see-also"></a>Vedere anche
 [Creazione di un'istanza dell'evento ADO per linguaggio](./ado-event-instantiation-by-language.md) [eventi ADO](../../reference/ado-api/ado-events.md) [parametri evento](./event-parameters.md) [come i gestori eventi operano insieme](./how-event-handlers-work-together.md) [tipi di eventi](./types-of-events.md)