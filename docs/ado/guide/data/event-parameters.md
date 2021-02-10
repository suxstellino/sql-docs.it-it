---
description: Parametri evento
title: Parametri evento | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Error parameter [ADO]
- Object parameter [ADO]
- Status parameter [ADO]
- events [ADO], parameters
- Reason parameter [ADO]
- event parameters [ADO]
ms.assetid: bd5c5afa-d301-4899-acda-40f98a6afa4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f60d32ccd968c39da4fcea3978a46389e49ae49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033211"
---
# <a name="event-parameters"></a>Parametri evento
Ogni gestore eventi dispone di un parametro status che controlla il gestore eventi. Per gli eventi completi, questo parametro viene usato anche per indicare l'esito positivo o negativo dell'operazione che ha generato l'evento. La maggior parte degli eventi completi dispone inoltre di un parametro error per fornire informazioni sugli errori che potrebbero essersi verificati e uno o più parametri oggetto che fanno riferimento agli oggetti ADO utilizzati per eseguire l'operazione. Ad esempio, l'evento [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md) include i parametri oggetto per il **comando**, il **Recordset** e gli oggetti **connessione** associati all'evento. Nell'esempio seguente Microsoft® Visual Basic®, è possibile visualizzare gli oggetti pCommand, pRecordset e pConnection che rappresentano gli oggetti **Command**, **Recordset** e **Connection** usati dal metodo **Execute** .  
  
```  
Private Sub connEvent_ExecuteComplete(ByVal RecordsAffected As Long, _  
     ByVal pError As ADODB.Error, _  
     adStatus As ADODB.EventStatusEnum, _  
     ByVal pCommand As ADODB.Command, _  
     ByVal pRecordset As ADODB.Recordset, _  
     ByVal pConnection As ADODB.Connection)  
```  
  
 Ad eccezione dell'oggetto **Error** , gli stessi parametri vengono passati agli eventi di. In questo modo è possibile esaminare tutti gli oggetti che verranno utilizzati nell'operazione in sospeso e determinare se l'operazione deve essere consentita per il completamento.  
  
 Alcuni gestori di eventi hanno un parametro *reason* , che fornisce informazioni aggiuntive sui motivi per cui si è verificato l'evento. Ad esempio, gli eventi **WillMove** e **MoveComplete** possono verificarsi a causa di uno dei metodi di navigazione (**MoveNext**, **MovePrevious** e così via) chiamati o come risultato di una riesecuzione della query.  
  
## <a name="status-parameter"></a>Parametro status  
 Quando viene chiamata la routine del gestore eventi, il parametro *status* viene impostato su uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**adStatusOK**|Viene passato a entrambi gli eventi e completi. Questo valore indica che l'operazione che ha causato l'evento è stata completata correttamente.|  
|**adStatusErrorsOccurred**|Passato solo agli eventi completi. Questo valore indica che l'operazione che ha causato l'evento non è riuscita o che l'operazione è stata annullata da un evento. Per ulteriori informazioni, controllare il parametro *Error* .|  
|**adStatusCantDeny**|Passato a solo eventi. Questo valore indica che l'operazione non può essere annullata dall'evento. Deve essere eseguita.|  
  
 Se si determina nel caso in cui l'operazione deve continuare, lasciare invariato il parametro *status* . Tuttavia, se il parametro di stato in ingresso non è stato impostato su **adStatusCantDeny**, è possibile annullare l'operazione in sospeso cambiando *lo stato* in **adStatusCancel**. Quando si esegue questa operazione, l'evento completo associato all'operazione ha il relativo parametro *status* impostato su **adStatusErrorsOccurred**. L'oggetto **Error** passato all'evento complete conterrà il valore **adErrOperationCancelled**.  
  
 Se non si desidera più elaborare un evento, è possibile impostare *lo stato* su **adStatusUnwantedEvent** e l'applicazione non riceverà più la notifica dell'evento. Tuttavia, tenere presente che alcuni eventi possono essere generati per più di un motivo. In tal caso, è necessario specificare **adStatusUnwantedEvent** per ogni motivo possibile. Ad esempio, per interrompere la ricezione di notifiche di eventi **RecordChange** in sospeso, è necessario impostare il parametro *status* su **adStatusUnwantedEvent** per **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete** e **adRsnFirstChange** non appena si verificano.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**adStatusUnwantedEvent**|Richiedere che questo gestore eventi non riceva altre notifiche.|  
|**adStatusCancel**|Richiede l'annullamento dell'operazione che sta per verificarsi.|  
  
## <a name="error-parameter"></a>Parametro error  
 Il parametro *Error* è un riferimento a un oggetto [Error](../../reference/ado-api/error-object.md) ADO. Quando il parametro *status* è impostato su **adStatusErrorsOccurred**, l'oggetto **Error** contiene informazioni dettagliate sul motivo per cui l'operazione non è riuscita. Se l'evento di evento associato a un evento completo ha annullato l'operazione impostando il parametro *status* su **adStatusCancel**, l'oggetto Error è sempre impostato su **adErrOperationCancelled**.  
  
## <a name="object-parameter"></a>Parametro dell'oggetto  
 Ogni evento riceve uno o più oggetti che rappresentano gli oggetti interessati dall'operazione. Ad esempio, l'evento **ExecuteComplete** riceve un oggetto **Command** , un oggetto **Recordset** e un oggetto **Connection** .  
  
## <a name="reason-parameter"></a>Parametro reason  
 Il parametro *reason* , *adReason*, fornisce informazioni aggiuntive sui motivi per cui si è verificato l'evento. Gli eventi con un parametro *adReason* possono essere chiamati più volte, anche per la stessa operazione, per un motivo diverso ogni volta. Ad esempio, il gestore dell'evento **WillChangeRecord** viene chiamato per le operazioni che stanno per eseguire o annullare l'inserimento, l'eliminazione o la modifica di un record. Se si desidera elaborare un evento solo quando si verifica per un motivo particolare, è possibile utilizzare il parametro *adReason* per filtrare le occorrenze a cui non si è interessati. Se, ad esempio, si desidera elaborare gli eventi di modifica dei record solo quando si verificano perché è stato aggiunto un record, è possibile utilizzare un valore simile al seguente.  
  
```  
' BeginEventExampleVB01  
Private Sub rsTest_WillChangeRecord(ByVal adReason As ADODB.EventReasonEnum, ByVal cRecords As Long, adStatus As ADODB.EventStatusEnum, ByVal pRecordset As ADODB.Recordset)  
   If adReason = adRsnAddNew Then  
       ' Process event  
       '...  
   Else  
       ' Cancel event notification for all  
       ' other possible adReason values.  
       adStatus = adStatusUnwantedEvent  
   End If  
End Sub  
' EndEventExampleVB01  
```  
  
 In questo caso, è possibile che si verifichi una notifica per ogni altro motivo. Tuttavia, si verificherà solo una volta per ogni motivo. Dopo che la notifica si è verificata una volta per ogni motivo, si riceverà una notifica solo per l'aggiunta di un nuovo record.  
  
 Al contrario, è necessario impostare *adStatus* su **adStatusUnwantedEvent** solo una volta per richiedere che un gestore eventi senza un parametro **adReason** interrompa la ricezione delle notifiche degli eventi.  
  
## <a name="see-also"></a>Vedere anche  
 [Riepilogo del gestore eventi ADO](./ado-event-handler-summary.md)   
 [Creazione di un'istanza di evento ADO per lingua](./ado-event-instantiation-by-language.md)   
 [Interazione tra i gestori eventi](./how-event-handlers-work-together.md)   
 [Tipi di eventi](./types-of-events.md)