---
description: Esempio del metodo GetRows (VB)
title: Esempio di metodo GetRows (VB) | Microsoft Docs
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
- Getrows method [ADO], Visual Basic example
ms.assetid: 9f7c78bb-7bb8-4c4f-8e5a-4d3bfc8a208f
author: rothja
ms.author: jroth
ms.openlocfilehash: 00000e6e053cc7782f234f1e59005c831402dc46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020855"
---
# <a name="getrows-method-example-vb"></a>Esempio del metodo GetRows (VB)
Questo esempio usa il metodo [GetRows](./getrows-method-ado.md) per recuperare un numero specificato di righe da un [Recordset](./recordset-object-ado.md) e per riempire una matrice con i dati risultanti. Il metodo **GetRows** restituirà un numero di righe inferiore al numero desiderato in due casi: se è stato raggiunto [EOF](./bof-eof-properties-ado.md) o se **GetRows** ha tentato di recuperare un record eliminato da un altro utente. La funzione restituisce **false** solo se si verifica il secondo caso. Per eseguire questa procedura, è necessaria la funzione GetRowsOK.  
  
```  
'BeginGetRowsVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
     ' connection and recordset variables  
    Dim rstEmployees As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strSQLEmployees As String  
    Dim strCnxn As String  
    ' array variable  
    Dim arrEmployees As Variant  
    ' detail variables  
    Dim strMessage As String  
    Dim intRows As Integer  
    Dim intRecord As Integer  
  
    ' open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' open recordset client-side to enable RecordCount  
    Set rstEmployees = New ADODB.Recordset  
    strSQLEmployees = "SELECT fName, lName, hire_date FROM Employee ORDER BY lName"  
    rstEmployees.Open strSQLEmployees, Cnxn, adOpenStatic, adLockReadOnly, adCmdText  
  
    ' get user input for number of rows  
    Do  
        strMessage = "Enter number of rows to retrieve:"  
        intRows = Val(InputBox(strMessage))  
  
          ' if bad user input exit the loop  
        If intRows <= 0 Then  
            MsgBox "Please enter a positive number", vbOKOnly, "Not less than zero!"  
        ' if number of requested records is over the total  
        ElseIf intRows > rstEmployees.RecordCount Then  
            MsgBox "Not enough records in Recordset to retrieve " & intRows & " rows.", _  
            vbOKOnly, "Over the available total"  
        Else  
            Exit Do  
        End If  
    Loop  
  
      ' else put the data in an array and print  
    arrEmployees = rstEmployees.GetRows(intRows)  
  
    Dim x As Integer, y As Integer  
  
    For x = 0 To intRows - 1  
        For y = 0 To 2  
            Debug.Print arrEmployees(y, x) & " ";  
        Next y  
        Debug.Print vbCrLf  
    Next x  
  
    ' clean up  
    rstEmployees.Close  
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
'EndGetRowsVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo GetRows (ADO)](./getrows-method-ado.md)   
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)