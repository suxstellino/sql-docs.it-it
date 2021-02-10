---
description: Uso di un oggetto Connection
title: Utilizzo di un oggetto Connection | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 4b34f971-5699-43e7-9b15-137d334fa66e
author: rothja
ms.author: jroth
ms.openlocfilehash: 36344c8b8c03677ac3b0ae25db097917d8d10d2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036801"
---
# <a name="using-a-connection-object"></a>Uso di un oggetto Connection
Prima di aprire un oggetto **connessione** , è necessario definire determinate informazioni sull'origine dati e il tipo di connessione. La maggior parte di queste informazioni viene mantenuta dal parametro *ConnectionString* del [metodo Open](../../../ado/reference/ado-api/open-method-ado-connection.md) nell'oggetto **Connection** o dalla [proprietà ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) nell'oggetto **Connection** . Una stringa di connessione è costituita da un elenco di coppie argomento/valore separate da punti e virgola, con i valori racchiusi tra virgolette singole. Ad esempio:  
  
```  
Dim sConn As String  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
```  
  
> [!NOTE]
>  È inoltre possibile specificare un nome di origine dati (DSN) ODBC o un file di collegamento dati (UDL) in una stringa di connessione. Per ulteriori informazioni sui DSN, vedere [gestione delle origini dati](../../../odbc/admin/managing-data-sources.md) in ODBC Programmer ' s Reference. Per altre informazioni su UDL, vedere [Cenni preliminari sulle API di data link](/previous-versions/windows/desktop/ms718102(v=vs.85)) in OLE DB Programmer ' s Reference.  
  
 In genere, è possibile stabilire una connessione chiamando il metodo **Connection. Open** con una *stringa di connessione* appropriata come parametro. Un esempio è illustrato nel frammento di codice Visual Basic seguente:  
  
```  
Dim oConn As ADODB.Connection  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
' Open a connection.  
Set oConn = New ADODB.Connection  
.Open   
  
' Make a query over the connection.  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
Set oRs = New ADODB.Recordset  
oRs.Open sSQL, , adOpenStatic, adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
' Close the connection.  
oConn.Close  
Set oConn = Nothing  
  
```  
  
 Qui **ORS. Open** accetta una variabile dell'oggetto **Connection** (*oConn*) come valore del parametro *ActiveConnection* . Inoltre, la proprietà **Connection. CursorLocation** presuppone il valore predefinito di **adUseServer come**. Confrontare questo esempio con l'esempio [HelloData](../../../ado/guide/data/hellodata-a-simple-ado-application.md) nella sezione precedente. L'istruzione seguente genera errori in fase di esecuzione.  
  
```  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
```