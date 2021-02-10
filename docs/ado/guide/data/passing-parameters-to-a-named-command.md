---
description: Passaggio di parametri a un comando con nome
title: Passaggio di parametri a un comando denominato | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- named commands [ADO]
- commands [ADO], passing parameters to a named command
ms.assetid: 36e0cdbe-7f50-40f5-af0d-700f5d8dc75a
author: rothja
ms.author: jroth
ms.openlocfilehash: 11fa07dd7af4512f8b7f41917d2778fecd6584ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032643"
---
# <a name="passing-parameters-to-a-named-command"></a>Passaggio di parametri a un comando con nome
Proprio come il risultato del comando viene passato come variabile *out* del comando denominato, i parametri per un comando con parametri possono essere passati come *nelle* variabili al comando denominato.  
  
 Nell'esempio di codice seguente si tenta di recuperare tutti gli ordini effettuati dal cliente il cui **CustomerID** è "ALKFI" dal database Northwind. Il valore di **CustomerID** viene specificato al momento della chiamata del comando denominato.  
  
```  
Const DS = "MySqlServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objConn As New ADODB.Connection  
Dim objRs As New ADODB.Recordset  
Dim objComm As New ADODB.Command  
  
CommandText = "SELECT OrderID, OrderDate, " & _  
                     "RequiredDate, ShippedDate " & _  
                     "FROM Orders " & _  
                     "WHERE CustomerID = ? " & _  
                     "ORDER BY OrderID"  
  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to the data source.  
objConn.Open ConnectionString  
  
' Set a named command.  
objComm.CommandText = CommandText  
objComm.CommandType = adCmdText  
objComm.Name = "GetOrdersOf"  
Set objComm.ActiveConnection = objConn  
  
' Call the named command, passing a CustomerID value  
' as the input parameter.   
'    "ALFKI" is the required input parameter,  
'    objRs is the resultant output variable.  
objConn.GetOrdersOf "ALKFI", objRs  
  
' Display the result.  
Debug.Print "All orders by ALFKI:"  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
Loop  
  
' Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
Set objComm = Nothing  
```  
  
 Si noti che tutti i parametri di input devono precedere qualsiasi variabile di output e i tipi di dati dei parametri devono corrispondere o possono essere convertiti in quelli dei campi corrispondenti. L'istruzione seguente:  
  
```  
objConn.GetOrdersOf 12345, objRs  
```  
  
 -genera un errore di tipi di dati non corrispondenti, perché il parametro di input obbligatorio è di tipo **stringa** , non di un tipo **Integer** .  
  
 La chiamata seguente:  
  
```  
objConn.GetOrdersOf "12345", objRs  
```  
  
 -è valido, ma produrrà un set di risultati vuoto perché nel database non sono presenti record di questo tipo.  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Connection (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)
