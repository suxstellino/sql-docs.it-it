---
description: Interazione tra i gestori eventi
title: Interazione tra i gestori eventi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- unpaired event handlers [ADO]
- paired event handlers [ADO]
- single event handlers [ADO]
- event handlers [ADO]
- multiple object event handlers [ADO]
ms.assetid: a86c8a02-dd69-420d-8a47-0188b339858d
author: rothja
ms.author: jroth
ms.openlocfilehash: dd6950ec37ddf974470e29ae554603723b299ef5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037351"
---
# <a name="how-event-handlers-work-together"></a>Interazione tra i gestori eventi
A meno che non si stia programmando in Visual Basic, è necessario implementare tutti i gestori eventi per gli eventi di **connessione** e **Recordset** , indipendentemente dal fatto che si elaborino effettivamente tutti gli eventi. La quantità di operazioni di implementazione che è necessario eseguire dipende dal linguaggio di programmazione. Per ulteriori informazioni, vedere [creazione di un'istanza dell'evento ADO in base al linguaggio](./ado-event-instantiation-by-language.md).  
  
## <a name="paired-event-handlers"></a>Gestori eventi associati  
 A ogni gestore eventi viene associato un gestore eventi **completo** . Ad esempio, quando l'applicazione modifica il valore di un campo, viene chiamato il gestore dell'evento **WillChangeField** . Se la modifica è accettabile, l'applicazione lascia invariato il parametro **adStatus** e l'operazione viene eseguita. Al termine dell'operazione, un evento **FieldChangeComplete** notifica all'applicazione che l'operazione è stata completata. Se il completamento è stato completato correttamente, **adStatus** contiene **adStatusOK**; in caso contrario, **adStatus** contiene **adStatusErrorsOccurred** ed è necessario controllare l'oggetto **Error** per determinare la provocazione dell'errore.  
  
 Quando viene chiamato **WillChangeField** , è possibile determinare che la modifica non deve essere apportata. In tal caso, impostare **adStatus** su **adStatusCancel.** L'operazione viene annullata e l'evento **FieldChangeComplete** riceve un valore **adStatus** di **adStatusErrorsOccurred**. L'oggetto **Error** contiene **adErrOperationCancelled** , in modo che il gestore **FieldChangeComplete** sappia che l'operazione è stata annullata. Tuttavia, è necessario controllare il valore del parametro **adStatus** prima di modificarlo, perché l'impostazione di **adStatus** su **adStatusCancel** non produce alcun effetto se il parametro è stato impostato su **adStatusCantDeny** alla voce della procedura.  
  
 A volte un'operazione può generare più di un evento. Ad esempio, l'oggetto **Recordset** presenta eventi associati per le modifiche dei **campi** e **registra** le modifiche. Quando l'applicazione modifica il valore di un **campo**, viene chiamato il gestore dell'evento **WillChangeField** . Se determina che l'operazione può continuare, viene generato anche il gestore dell'evento **WillChangeRecord** . Se questo gestore consente anche di continuare, viene apportata la modifica e vengono chiamati i gestori eventi **FieldChangeComplete** e **RecordChangeComplete** . L'ordine in cui vengono chiamati i gestori eventi per una determinata operazione non è definito, pertanto è consigliabile evitare di scrivere codice che dipende dalla chiamata di gestori in una determinata sequenza.  
  
 Nei casi in cui vengono generati più eventi, uno degli eventi potrebbe annullare l'operazione in sospeso. Ad esempio, quando l'applicazione modifica il valore di un **campo**, i gestori eventi **WillChangeField** e **WillChangeRecord** vengono in genere chiamati. Tuttavia, se l'operazione viene annullata nel primo gestore eventi, il gestore **completo** associato viene chiamato immediatamente con **adStatusOperationCancelled**. Il secondo gestore non viene mai chiamato. Se, tuttavia, il primo gestore eventi consente la prosecuzione dell'evento, verrà chiamato l'altro gestore eventi. Se l'operazione viene annullata, verranno chiamati entrambi gli eventi **completi** , come negli esempi precedenti.  
  
## <a name="unpaired-event-handlers"></a>Gestori di eventi non abbinati  
 Finché lo stato passato all'evento non è **adStatusCantDeny**, è possibile disattivare le notifiche degli eventi per qualsiasi evento restituendo **AdStatusUnwantedEvent** nel parametro *status* . Ad esempio, quando il gestore eventi **completo** viene chiamato per la prima volta, è possibile restituire **adStatusUnwantedEvent**. **Si riceveranno successivamente solo gli** eventi. Tuttavia, alcuni eventi possono essere attivati per più di un motivo. In tal caso, l'evento avrà un parametro *reason* . Quando si restituisce **adStatusUnwantedEvent**, si interrompe la ricezione delle notifiche per l'evento solo quando si verificano per quel particolare motivo. In altre parole, si riceverà una notifica per ogni possibile motivo per cui l'evento potrebbe essere attivato.  
  
 I gestori **eventi singoli possono** essere utili quando si desidera esaminare i parametri che verranno utilizzati in un'operazione. È possibile modificare i parametri dell'operazione o annullare l'operazione.  
  
 In alternativa, lasciare abilitata la notifica **completa** degli eventi. Quando viene chiamato il primo gestore eventi, restituisce **adStatusUnwantedEvent**. Si riceveranno successivamente solo gli eventi **completi** .  
  
 Singoli gestori eventi **completi** possono essere utili per la gestione delle operazioni asincrone. Ogni operazione asincrona ha un evento **completo** appropriato.  
  
 Ad esempio, il popolamento di un oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) di grandi dimensioni può richiedere molto tempo. Se l'applicazione è scritta in modo appropriato, è possibile avviare un' `Recordset.Open(...,adAsyncExecute)` operazione e continuare con altre elaborazioni. Alla fine, si riceverà una notifica quando il **Recordset** viene popolato da un evento **ExecuteComplete** .  
  
## <a name="single-event-handlers-and-multiple-objects"></a>Gestori di eventi singoli e più oggetti  
 La flessibilità di un linguaggio di programmazione come Microsoft Visual C++® consente di disporre di un gestore eventi per elaborare eventi da più oggetti. Ad esempio, è possibile avere un gestore eventi di **disconnessione** per elaborare eventi da diversi oggetti **connessione** . Se una delle connessioni è terminata, viene chiamato il gestore eventi di **disconnessione** . È possibile stabilire quale connessione ha causato l'evento perché il parametro dell'oggetto gestore eventi verrebbe impostato sull'oggetto **connessione** corrispondente.  
  
> [!NOTE]
>  Questa tecnica non può essere usata in Visual Basic perché tale linguaggio può correlare un solo oggetto a un gestore eventi.  
  
## <a name="see-also"></a>Vedere anche  
 [Riepilogo del gestore eventi ADO](./ado-event-handler-summary.md)   
 [Creazione di un'istanza di evento ADO per lingua](./ado-event-instantiation-by-language.md)   
 [Parametri evento](./event-parameters.md)   
 [Tipi di eventi](./types-of-events.md)