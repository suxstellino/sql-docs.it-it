---
description: Esempio del metodo Cancel (VB)
title: Esempio di metodo Cancel (VB) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Cancel method [ADO], Visual Basic example
ms.assetid: 5c0530ad-68d0-4cba-b1af-9386d566c7c5
author: rothja
ms.author: jroth
ms.openlocfilehash: 821b4bd876319306f184fe095f7a45c1a0c3629a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171526"
---
# <a name="cancel-method-example-vb"></a>Esempio del metodo Cancel (VB)
In questo esempio viene utilizzato il metodo [Cancel](./cancel-method-ado.md) per annullare un comando in esecuzione su un oggetto [Connection](./connection-object-ado.md) se la connessione è occupata.  
  
```  
'BeginCancelVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset and connection variables  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strCmdChange As String  
    Dim strCmdRestore As String  
     'record variables  
    Dim blnChanged As Boolean  
  
    ' Open a connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Define command strings  
    strCmdChange = "UPDATE titles SET type = 'self_help' WHERE type = 'psychology'"  
    strCmdRestore = "UPDATE titles SET type = 'psychology' " & _  
                     "WHERE type = 'self_help'"  
  
    ' Begin a transaction, then execute a command asynchronously  
    Cnxn.BeginTrans  
    Cnxn.Execute strCmdChange, , adAsyncExecute  
    ' do something else for a little while -  
    ' use i = 1 to 32000 to allow completion  
    Dim i As Integer  
    For i = 1 To 1000  
        i = i + i  
        Debug.Print i  
    Next i  
  
    ' If the command has NOT completed, cancel the execute and  
    ' roll back the transaction; otherwise, commit the transaction  
    If CBool(Cnxn.State And adStateExecuting) Then  
        Cnxn.Cancel  
        Cnxn.RollbackTrans  
        blnChanged = False  
        MsgBox "Update canceled."  
    Else  
        Cnxn.CommitTrans  
        blnChanged = True  
        MsgBox "Update complete."  
    End If  
  
    ' If the change was made, restore the data  
    ' because this is only a demo  
    If blnChanged Then  
        Cnxn.Execute strCmdRestore  
        MsgBox "Data restored."  
    End If  
  
    ' clean up  
    Cnxn.Close  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndCancelVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Cancel (ADO)](./cancel-method-ado.md)   
 [Oggetto Connection (ADO)](./connection-object-ado.md)