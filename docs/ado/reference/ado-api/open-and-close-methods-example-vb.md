---
description: Esempio dei metodi Open e Close (VB)
title: Esempio di metodi Open e Close (VB) | Microsoft Docs
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
- Close method [ADO], Visual Basic example
- Open method [ADO], Visual Basic example
ms.assetid: 1311d561-0e86-40f5-8cbc-ad8f13e626d1
author: rothja
ms.author: jroth
ms.openlocfilehash: b2c1798c1574aa671677ffbf944db595a48c16cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041451"
---
# <a name="open-and-close-methods-example-vb"></a>Esempio dei metodi Open e Close (VB)
In questo esempio vengono utilizzati i metodi **Open** e [Close](./close-method-ado.md) per gli oggetti [Recordset](./recordset-object-ado.md) e [Connection](./connection-object-ado.md) aperti.  
  
```  
'BeginOpenVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub OpenX()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim rstEmployees As ADODB.Recordset  
    Dim strCnxn As String  
    Dim strSQLEmployees As String  
    Dim varDate As Variant  
  
    ' Open connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Set Cnxn = New ADODB.Connection  
    Cnxn.Open strCnxn  
  
    ' Open employee table  
    Set rstEmployees = New ADODB.Recordset  
    strSQLEmployees = "employee"  
    rstEmployees.Open strSQLEmployees, Cnxn, adOpenKeyset, adLockOptimistic, adCmdTable  
  
    ' Assign the first employee record's hire date  
    ' to a variable, then change the hire date  
    varDate = rstEmployees!hire_date  
    Debug.Print "Original data"  
    Debug.Print "  Name - Hire Date"  
    Debug.Print "  " & rstEmployees!fname & " " & _  
        rstEmployees!lname & " - " & rstEmployees!hire_date  
    rstEmployees!hire_date = #1/1/1900#  
    rstEmployees.Update  
    Debug.Print "Changed data"  
    Debug.Print "  Name - Hire Date"  
    Debug.Print "  " & rstEmployees!fname & " " & _  
        rstEmployees!lname & " - " & rstEmployees!hire_date  
  
    ' Requery Recordset and reset the hire date  
    rstEmployees.Requery  
    rstEmployees!hire_date = varDate  
    rstEmployees.Update  
    Debug.Print "Data after reset"  
    Debug.Print "  Name - Hire Date"  
    Debug.Print "  " & rstEmployees!fname & " " & _  
       rstEmployees!lname & " - " & rstEmployees!hire_date  
  
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
'EndOpenVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Close (ADO)](./close-method-ado.md)   
 [Oggetto Connection (ADO)](./connection-object-ado.md)   
 [Metodo Open (connessione ADO)](./open-method-ado-connection.md)   
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)