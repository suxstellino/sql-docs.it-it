---
description: Chiamata di una stored procedure con Command
title: Chiamata di una stored procedure con un comando | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 685f7652-2271-4ede-b552-2eeb8c756b4c
author: rothja
ms.author: jroth
ms.openlocfilehash: c34dbecd5e078e19029161b29b66187b02514166
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033331"
---
# <a name="calling-a-stored-procedure-with-a-command"></a>Chiamata di una stored procedure con Command
È possibile usare un comando per chiamare un stored procedure. L'esempio di codice alla fine di questo argomento si riferisce a un stored procedure nel database di esempio Northwind, denominato CustOrdersOrders, che è definito nel modo seguente.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 Per ulteriori informazioni su come definire e chiamare stored procedure, vedere la documentazione SQL Server.  
  
 Questa stored procedure è simile al comando utilizzato nei [parametri dell'oggetto comando](./command-object-parameters.md). Accetta un parametro ID cliente e restituisce informazioni sugli ordini del cliente. Nell'esempio di codice seguente viene utilizzato questo stored procedure come origine per un **Recordset** ADO.  
  
 L'utilizzo della stored procedure consente di accedere a un'altra funzionalità di ADO: il metodo di **aggiornamento** della raccolta **Parameters** . Utilizzando questo metodo, ADO è in grado di inserire automaticamente tutte le informazioni sui parametri richiesti dal comando in fase di esecuzione. L'utilizzo di questa tecnica comporta una riduzione delle prestazioni, in quanto ADO deve eseguire una query sull'origine dati per ottenere informazioni sui parametri.  
  
 Esistono altre importanti differenze tra l'esempio di codice seguente e il codice nei [parametri dell'oggetto comando](./command-object-parameters.md), in cui i parametri sono stati immessi manualmente. In primo luogo, questo codice non imposta la proprietà **preparata** su **true** perché è un SQL Server stored procedure ed è precompilato per definizione. In secondo luogo, la proprietà **CommandType** dell'oggetto **Command** è cambiata in **adCmdStoredProc** nel secondo esempio per informare ADO che il comando era un stored procedure.  
  
 Nel secondo esempio, infine, il parametro deve essere indicato dall'indice quando si imposta il valore, perché il nome del parametro potrebbe non essere noto in fase di progettazione. Se si conosce il nome del parametro, è possibile impostare la nuova proprietà [namedParameters](../../reference/ado-api/namedparameters-property-ado.md) dell'oggetto **Command** su true e fare riferimento al nome della proprietà. Ci si potrebbe chiedere perché la posizione del primo parametro indicato nel stored procedure ( @CustomerID ) è 1 anziché 0 ( `objCmd(1) = "ALFKI"` ). Questo perché il parametro 0 contiene un valore restituito dal SQL Server stored procedure.  
  
```  
'BeginAutoParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set CommandText equal to the stored procedure name.  
    objCmd.CommandText = "CustOrdersOrders"  
    objCmd.CommandType = adCmdStoredProc  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Automatically fill in parameter info from stored procedure.  
    objCmd.Parameters.Refresh  
  
    ' Set the param value.  
    objCmd(1) = "ALFKI"  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' ...then set new param value, re-execute command, and display.  
    objCmd(1) = "CACTU"  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
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
    Set objParm1 = Nothing  
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
    Set objParm1 = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndAutoParamCmd  
  
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
  
## <a name="see-also"></a>Vedere anche  
 [Articolo della Knowledge base 117500](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/185125)
