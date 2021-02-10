---
description: Errori del provider
title: Errori del provider | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors collection [ADO]
- provider errors [ADO]
- event-related errors [ADO]
- errors [ADO], provider
- Error object [ADO], provider errors
ms.assetid: cc7d6ff9-2034-45c6-9d61-90b177010054
author: rothja
ms.author: jroth
ms.openlocfilehash: a13bcc83c360533b1137af69b688221157dc0dc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032608"
---
# <a name="provider-errors"></a>Errori del provider
Quando si verifica un errore del provider, viene restituito un errore di run-time di-2147467259. Quando viene visualizzato questo errore, controllare la raccolta di **errori** dell'oggetto **connessione** attivo, che conterrà uno o più errori che descrivono cosa si è verificato.  
  
## <a name="the-ado-errors-collection"></a>Raccolta di errori ADO  
 Poiché una particolare operazione ADO può produrre più errori del provider, ADO espone una raccolta di oggetti Error tramite l'oggetto **Connection** . Questa raccolta non contiene oggetti se un'operazione termina correttamente e contiene uno o più oggetti **Error** se si è verificato un errore e il provider ha generato uno o più errori. Esaminare ogni oggetto errore per determinare la corretta origine dell'errore.  
  
 Al termine della gestione degli errori che si sono verificati, è possibile cancellare la raccolta chiamando il metodo **Clear** . È particolarmente importante deselezionare esplicitamente la raccolta **Errors** prima di chiamare il metodo **Resync**, **UpdateBatch** o **CancelBatch** su un oggetto **Recordset** , il metodo **Open** su un oggetto **Connection** o impostare la proprietà **Filter** per un oggetto **Recordset** . Deselezionando la raccolta in modo esplicito, è possibile essere certi che gli oggetti **Error** della raccolta non vengano lasciati da un'operazione precedente.  
  
 Alcune operazioni possono generare avvisi oltre agli errori. Gli avvisi sono rappresentati anche da oggetti **Error** nella raccolta **Errors** . Quando un provider aggiunge un avviso alla raccolta, non genera un errore di run-time. Controllare la proprietà **count** della raccolta **Errors** per determinare se un avviso è stato generato da una determinata operazione. Se il conteggio è uno o più, un oggetto **Error** è stato aggiunto alla raccolta. Non appena si è determinato che la raccolta **Errors** contiene errori o avvisi, è possibile scorrere la raccolta e recuperare le informazioni su ogni oggetto **Error** che contiene. Nell'esempio di Visual Basic breve seguente viene illustrato quanto segue:  
  
```  
' BeginErrorHandlingVB02  
Private Function DeleteCustomer(ByVal CompanyName As String) As Long  
    On Error GoTo DeleteCustomerError  
  
    rst.Find "CompanyName='" & CompanyName & "'"  
DeleteCustomerError:  
Dim objError As ADODB.Error  
Dim strError As String  
  
    If cnn.Errors.Count > 0 Then  
        For Each objError In cnn.Errors  
            strError = strError & "Error #" & objError.Number & _  
                " " & objError.Description & vbCrLf & _  
                "NativeError: " & objError.NativeError & vbCrLf & _  
                "SQLState: " & objError.SQLState & vbCrLf & _  
                "Reported by: " & objError.Source & vbCrLf & _  
                "Help file: " & objError.HelpFile & vbCrLf & _  
                "Help Context ID: " & objError.HelpContext  
        Next  
        MsgBox strError  
    End If  
End Function  
' EndErrorHandlingVB02  
```  
  
 La routine di gestione degli errori include un ciclo **for each** che esamina ogni oggetto nella raccolta **Errors** . In questo esempio viene accumulato un messaggio per la visualizzazione. In un programma funzionante è necessario scrivere il codice per eseguire un'attività appropriata per ogni errore, ad esempio la chiusura di tutti i file aperti e l'arresto del programma in modo ordinato.  
  
## <a name="the-error-object"></a>Oggetto Error  
 Esaminando un oggetto **Error** è possibile determinare l'errore che si è verificato e, più importante, l'applicazione o l'oggetto che ha causato l'errore. L'oggetto **Error** presenta le proprietà seguenti:  
  
|Nome proprietà|Descrizione|  
|-------------------|-----------------|  
|**Descrizione**|Descrizione testuale dell'errore che si è verificato.|  
|**HelpContext, fileguida**|Si riferisce all'argomento della guida e al file della guida che contengono una descrizione dell'errore che si è verificato.|  
|**NativeError**|Numero di errore specifico del provider.|  
|**Number**|Valore long integer che rappresenta il numero (elencato in **ErrorValueEnum**) dell'errore che si è verificato.|  
|**Origine**|Indica il nome dell'oggetto o dell'applicazione che ha generato un errore.|  
|**SQLState**|Codice di errore di cinque caratteri restituito dal provider durante il processo di un'istruzione SQL.|  
  
 L'oggetto **errore** ADO è molto simile all'oggetto Visual Basic **Err** standard. Le relative proprietà descrivono l'errore che si è verificato. Oltre al numero dell'errore, si ricevono anche due informazioni correlate. La proprietà **NativeError** contiene un numero di errore specifico per il provider in uso. Nell'esempio precedente, il provider è il provider di OLE DB Microsoft per SQL Server, quindi **NativeError** conterrà errori specifici per SQL Server. La proprietà **SQLSTATE** include un codice di cinque lettere che descrive un errore in un'istruzione SQL.  
  
## <a name="event-related-errors"></a>Errori Event-Related  
 L'oggetto **Error** viene usato anche quando si verificano errori correlati agli eventi. È possibile determinare se si è verificato un errore nel processo che ha generato un evento ADO controllando l'oggetto **errore** passato come parametro di evento.  
  
 Se l'operazione che provoca un evento viene conclusa correttamente, il parametro *adStatus* del gestore eventi verrà impostato su *adStatusOK*. D'altra parte, se l'operazione che ha generato l'evento ha avuto esito negativo, il parametro *adStatus* è impostato su *adStatusErrorsOccurred*. In tal caso, il parametro *perror* conterrà un oggetto **Error** che descrive l'errore.
