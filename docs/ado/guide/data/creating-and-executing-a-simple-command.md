---
description: Creazione ed esecuzione di un comando semplice
title: Creazione ed esecuzione di un semplice comando | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Command object [ADO], creating and executing
- commands [ADO], creating and executing
ms.assetid: 0b81af6f-b9ae-4f7c-b59b-b5bdd775036f
author: rothja
ms.author: jroth
ms.openlocfilehash: d0fac62712f21e2f25416419252a1e13f8b47274
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037671"
---
# <a name="creating-and-executing-a-simple-command"></a>Creazione ed esecuzione di un comando semplice
Un semplice comando è uno che non è parametrizzato e non richiede alcuna persistenza. Esistono tre modi per creare ed eseguire un semplice comando.  
  
-   Uso di un oggetto **Command**  
  
-   Utilizzo di un oggetto **Connection**  
  
-   Uso di un oggetto **Recordset**  
  
## <a name="using-a-command-object"></a>Uso di un oggetto Command  
 Per creare un comando semplice usando un oggetto **Command** , è necessario assegnare l'istruzione alla proprietà **CommandText** di un oggetto **Command** e impostare il valore appropriato per la proprietà **CommandType** . Per eseguire il comando è necessario assegnare una connessione aperta alla proprietà **ActiveConnection** dell'oggetto **Command** , seguita da una chiamata al metodo **Execute** sull'oggetto **Command** .  
  
 Il frammento di codice seguente illustra il metodo di base per usare l'oggetto **Command** per eseguire un comando su un'origine dati. In questo esempio viene utilizzato un comando di restituzione di righe e vengono restituiti i risultati dell'esecuzione del comando come oggetto **Recordset** .  
  
```  
    'BeginBasicCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objRs As New ADODB.Recordset  
  
    objCmd.CommandText = "SELECT OrderID, OrderDate, " & _  
                         "RequiredDate, ShippedDate " & _  
                         "FROM Orders " & _  
                         "WHERE CustomerID = 'ALFKI' " & _  
                         "ORDER BY OrderID"  
    objCmd.CommandType = adCmdText  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print "ALFKI"  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndBasicCmd  
  
'BeginNewConnection  
Private Function GetNewConnection() As ADODB.Connection  
    Dim oCn As New ADODB.Connection  
    Dim sCnStr As String  
  
    sCnStr = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    oCn.Open sCnStr  
  
    If oCn.State = adStateOpen Then  
        Set GetNewConnection = oCn  
    End If  
  
End Function  
'EndNewConnection  
```  
  
## <a name="using-a-recordset-object"></a>Uso di un oggetto recordset  
 È anche possibile creare un comando come stringa di testo e visualizzarlo nel metodo **Open** su un oggetto **Recordset** , insieme al tipo di comando (adCmdText), per l'esecuzione. Il seguente frammento di codice illustra questa operazione.  
  
```  
  
Const DS = "MySqlServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objRs As New ADODB.Recordset  
Dim CommandText As String  
Dim ConnctionString As String  
  
CommandText = "SELECT OrderID, OrderDate, " & _  
                     "RequiredDate, ShippedDate " & _  
                     "FROM Orders " & _  
                     "WHERE CustomerID = 'ALFKI' " & _  
                     "ORDER BY OrderID"  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to data source and execute the SQL command.  
objRs.Open CommandText, ConnectionString, _  
            adOpenStatic, adLockReadOnly, adCmdText  
  
Debug.Print "ALFKI"  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
Loop  
  
'Clean up.  
objRs.Close  
Set objRs = Nothing  
```  
  
## <a name="using-a-connection-object"></a>Utilizzo di un oggetto Connection  
 È anche possibile eseguire un comando su un oggetto Connection aperto. L'esempio di codice precedente diventa ora il seguente:  
  
```  
Const DS = "MySqlServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objConn As New ADODB.Connection  
Dim objRs As New ADODB.Recordset  
  
CommandText = "SELECT OrderID, OrderDate, " & _  
                     "RequiredDate, ShippedDate " & _  
                     "FROM Orders " & _  
                     "WHERE CustomerID = 'ALFKI' " & _  
                     "ORDER BY OrderID"  
  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to the data source.  
objConn.Open ConnectionString  
  
' Execute command through the connection and display...  
Set objRs = objConn.Execute(CommandText)  
  
Debug.Print "ALFKI"  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
Loop  
  
'Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
```
