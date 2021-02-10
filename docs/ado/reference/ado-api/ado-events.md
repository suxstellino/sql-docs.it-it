---
description: Eventi ADO
title: Eventi ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- events [ADO]
- ADO, events
ms.assetid: 0ded5ad9-8f83-4224-95af-38512783b972
author: rothja
ms.author: jroth
ms.openlocfilehash: 41431d92cd8c9dfea44ea88feadcce9df7edc3bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031273"
---
# <a name="ado-events"></a>Eventi ADO

|Event|Descrizione|  
|-|-|  
|[BeginTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Chiamato dopo l'operazione **BeginTrans** .|  
|[CommitTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Chiamato dopo l'operazione **CommitTrans** .|  
|[ConnectComplete](./connectcomplete-and-disconnect-events-ado.md)|Chiamato dopo l'avvio di una connessione.|  
|[Disconnetti](./connectcomplete-and-disconnect-events-ado.md)|Chiamato al termine di una connessione.|  
|[EndOfRecordset](./endofrecordset-event-ado.md)|Chiamato quando viene eseguito un tentativo di spostarsi in una riga oltre la fine del **Recordset**.|  
|[ExecuteComplete](./executecomplete-event-ado.md)|Chiamato dopo il completamento dell'esecuzione di un comando.|  
|[FetchComplete](./fetchcomplete-event-ado.md)|Chiamato dopo che tutti i record di un'operazione asincrona lunga sono stati recuperati nel **Recordset**.|  
|[FetchProgress](./fetchprogress-event-ado.md)|Chiamato periodicamente durante un'operazione asincrona lunga per segnalare il numero di righe attualmente recuperate nel **Recordset**.|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|Chiamato dopo che il valore di uno o più oggetti **Field** è stato modificato.|  
|[InfoMessage](./infomessage-event-ado.md)|Chiamato ogni volta che viene generato un avviso durante un'operazione **ConnectionEvent** .|  
|[MoveComplete](./willmove-and-movecomplete-events-ado.md)|Chiamato dopo la modifica della posizione corrente nel **Recordset** .|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|Chiamato dopo la modifica di uno o più record.|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Chiamato dopo che il **Recordset** è stato modificato.|  
|[RollbackTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Chiamato dopo l'operazione **RollbackTrans** .|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|Chiamato prima che un'operazione in sospeso modifichi il valore di uno o più oggetti **Field** nel **Recordset**.|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|Chiamato prima della modifica di uno o più record (righe) nel **Recordset** .|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Chiamato prima che un'operazione in sospeso modifichi il **Recordset**.|  
|[WillConnect](./willconnect-event-ado.md)|Chiamato prima dell'avvio di una connessione.|  
|[WillExecute](./willexecute-event-ado.md)|Viene chiamato subito prima dell'esecuzione di un comando in sospeso su questa connessione e offre all'utente la possibilità di esaminare e modificare i parametri di esecuzione in sospeso.|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|L'evento **WillMove** viene chiamato *prima* che un'operazione in sospeso modifichi la posizione corrente nel **Recordset**.|  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento all'API ADO](./ado-api-reference.md)   
 [Raccolte ADO](./ado-collections.md)   
 [Proprietà dinamiche ADO](./ado-dynamic-properties.md)   
 [Costanti enumerate ADO](./ado-enumerated-constants.md)   
 [Appendice B: errori ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Gestione degli eventi ADO](../../guide/data/handling-ado-events.md)   
 [Metodi ADO](./ado-methods.md)   
 [Modello a oggetti ADO](./ado-object-model.md)   
 [Oggetti e interfacce ADO](./ado-objects-and-interfaces.md)   
 [Proprietà ADO](./ado-properties.md)