---
description: Esempio di proprietà CursorType, LockType e EditMode (VB)
title: Esempio di proprietà CursorType, LockType e EditMode (VB) | Microsoft Docs
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
- EditMode property [ADO], Visual Basic example
- CursorType property [ADO], Visual Basic example
- LockType property [ADO], Visual Basic example
ms.assetid: 2cb4a304-f40a-4897-8b93-82c2d8e93500
author: rothja
ms.author: jroth
ms.openlocfilehash: 2448e853f6c5e98a415f4a85228027465ea7192f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034501"
---
# <a name="cursortype-locktype-and-editmode-properties-example-vb"></a>Esempio di proprietà CursorType, LockType e EditMode (VB)
In questo esempio viene illustrata l'impostazione delle proprietà [CursorType](./cursortype-property-ado.md) e [LockType](./locktype-property-ado.md) prima di aprire un [Recordset](./recordset-object-ado.md). Mostra anche il valore della proprietà [EditMode](./editmode-property.md) in varie condizioni. Per eseguire questa procedura, è necessaria la funzione EditModeOutput.  
  
```  
'BeginEditModeVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    ' recordset variables  
    Dim rstEmployees As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim SQLEmployees As String  
  
     ' open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
      ' set recordset properties through object refs  
      ' instead of through arguments to Open method  
    Set rstEmployees = New ADODB.Recordset  
    Set rstEmployees.ActiveConnection = Cnxn  
    rstEmployees.CursorLocation = adUseClient  
    rstEmployees.CursorType = adOpenStatic  
    rstEmployees.LockType = adLockBatchOptimistic  
  
     ' open recordset with data from Employee table  
    SQLEmployees = "employee"  
    rstEmployees.Open SQLEmployees, , , , adCmdTable  
  
    ' Show the EditMode property under different editing states  
    rstEmployees.AddNew  
        rstEmployees!emp_id = "T-T55555M"  
        rstEmployees!fname = "temp_fname"  
        rstEmployees!lname = "temp_lname"  
            'call function below  
            'to output results to debug window  
        EditModeOutput "After AddNew:", rstEmployees.EditMode  
        rstEmployees.UpdateBatch  
        EditModeOutput "After UpdateBatch:", rstEmployees.EditMode  
        rstEmployees!fname = "test"  
        EditModeOutput "After Edit:", rstEmployees.EditMode  
    rstEmployees.Close  
  
    ' Delete new record because this is a demonstration  
    Cnxn.Execute "DELETE FROM employee WHERE emp_id = 'T-T55555M'"  
  
    ' clean up  
    Cnxn.Close  
    Set rstEmployees = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
  
Public Function EditModeOutput(strTemp As String, _  
   intEditMode As Integer)  
  
   ' Print report based on the value of the EditMode  
   ' property  
   Debug.Print strTemp  
   Debug.Print "  EditMode = ";  
  
   Select Case intEditMode  
      Case adEditNone  
         Debug.Print "adEditNone"  
      Case adEditInProgress  
         Debug.Print "adEditInProgress"  
      Case adEditAdd  
         Debug.Print "adEditAdd"  
   End Select  
  
End Function  
'EndEditModeVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà CursorType (ADO)](./cursortype-property-ado.md)   
 [CursorTypeEnum](./cursortypeenum.md)   
 [Proprietà EditMode](./editmode-property.md)   
 [EditModeEnum](./editmodeenum.md)   
 [Proprietà LockType (ADO)](./locktype-property-ado.md)   
 [LockTypeEnum](./locktypeenum.md)   
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)