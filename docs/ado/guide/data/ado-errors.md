---
description: Errori ADO Run-Time
title: Errori ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO]
ms.assetid: 9bb84114-a1df-4122-a1b8-ad98dcd85cc3
author: rothja
ms.author: jroth
ms.openlocfilehash: bed9958455af424c4f18cb0f6051eb30e2efaa3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033391"
---
# <a name="ado-run-time-errors"></a>Errori ADO Run-Time
Gli errori ADO vengono segnalati al programma come errori di run-time. È possibile utilizzare il meccanismo di intercettazione degli errori del linguaggio di programmazione per intercettarli e gestirli. In Visual Basic, ad esempio, utilizzare l'istruzione **On Error** . In Visual C++, dipende dal metodo usato per accedere alle librerie ADO. Con #import, usare un blocco **try-catch** . In caso contrario, i programmatori C++ devono recuperare in modo esplicito l'oggetto Error chiamando **GetErrorInfo**. Nella seguente Visual Basic subroutine viene illustrato l'intercettazione di un errore ADO:

```
' BeginErrorHandlingVB01
Private Sub Form_Load()
' Turn on error handling
On Error GoTo FormLoadError

'Open the database and the recordset for processing.
'
Dim strCnn As String
strCnn = "Provider=sqloledb;" & _
    "Data Source=a-iresmi2000;" & _
    "Initial Catalog=Northwind;Integrated Security=SSPI"

' cnn is a Public Connection Object because
' it was defined WithEvents
Set cnn = New ADODB.Connection
cnn.Open strCnn

' The next line of code intentionally causes
' an error by trying to open a connection
' that has already been opened.
cnn.Open strCnn

' rst is a Public Recordset because it
' was defined WithEvents
Set rst = New ADODB.Recordset
rst.Open "Customers", cnn

Exit Sub

' Error handler
FormLoadError:
    Dim strErr As String
    Select Case Err
        Case adErrObjectOpen
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            strErr = strErr & "Error reported by: " & Err.Source & vbCrLf
            strErr = strErr & "Help File: " & Err.HelpFile & vbCrLf
            strErr = strErr & "Topic ID: " & Err.HelpContext
            MsgBox strErr
            Debug.Print strErr
            Err.Clear
            Resume Next
        ' If some other error occurs that
        ' has nothing to do with ADO, show
        ' the number and description and exit.
        Case Else
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            MsgBox strErr
            Debug.Print strErr
            Unload Me
    End Select
End Sub
' EndErrorHandlingVB01
```

 Questa **Form_Load** routine evento crea intenzionalmente un errore tentando di aprire lo stesso oggetto **connessione** due volte. La seconda volta che viene chiamato il metodo **Open** , il gestore degli errori viene attivato. In questo caso l'errore è di tipo **adErrObjectOpen**, quindi il gestore degli errori Visualizza il messaggio seguente prima di riprendere l'esecuzione del programma:

```
Error #3705: Operation is not allowed when the object is open.
Error reported by: ADODB.Connection
Help File: E:\WINNT\HELP\ADO260.CHM Topic ID: 1003705
```

 Il messaggio di errore include ogni informazione fornita dall'oggetto Visual Basic **Err** , ad eccezione del valore **LastDLLError** , che non si applica qui. Il numero di errore indica l'errore che si è verificato. La descrizione è utile nei casi in cui non si desidera gestire l'errore manualmente. È possibile semplicemente passarla all'utente. Anche se in genere si vogliono usare messaggi personalizzati per l'applicazione, non è possibile prevedere ogni errore. la descrizione fornisce alcuni indizi relativi a ciò che si è verificato un errore. Nel codice di esempio, l'errore è stato segnalato dall'oggetto **Connection** . Il tipo o l'ID programmatico dell'oggetto sarà visualizzato qui, non un nome di variabile.

> [!NOTE]
>  L'oggetto Visual Basic **Err** contiene solo informazioni sull'errore più recente. La raccolta di **errori** ADO dell'oggetto **Connection** contiene un oggetto **Error** per ogni errore generato dall'operazione ADO più recente. Utilizzare la raccolta **Errors** anziché l'oggetto **Err** per gestire più errori. Per ulteriori informazioni sulla raccolta di **errori** , vedere la pagina relativa agli [errori del provider](./provider-errors.md). Tuttavia, se non è presente alcun oggetto **connessione** valido, l'oggetto **Err** è l'unica fonte per informazioni sugli errori ADO.

 Quali tipi di operazioni potrebbero causare errori ADO? Gli errori ADO comuni possono implicare l'apertura di un oggetto, ad esempio una **connessione** o un **Recordset**, il tentativo di aggiornare i dati o la chiamata di un metodo o di una proprietà non supportata dal provider.

 Gli errori di OLE DB possono anche essere passati all'applicazione come errori di run-time nella raccolta di **errori** .

 Nell'argomento seguente vengono fornite ulteriori informazioni sugli errori ADO.

-   [Informazioni di riferimento sugli errori ADO](./ado-error-reference.md)