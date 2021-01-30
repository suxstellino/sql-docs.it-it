---
description: Esempio del metodo GetRows (JScript)
title: Esempio di metodo GetRows (JScript) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- JScript
helpviewer_keywords:
- Getrows method [ADO], JScript example
ms.assetid: d33467a5-5a56-450d-98c1-c3ce6f9f103c
author: rothja
ms.author: jroth
ms.openlocfilehash: 992255020a1188507fc64cccb12f9d19061c56fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167271"
---
# <a name="getrows-method-example-jscript"></a>Esempio del metodo GetRows (JScript)
Questo esempio usa il metodo [GetRows](./getrows-method-ado.md) per recuperare tutte le righe della tabella *Custiomers* da un [Recordset](./recordset-object-ado.md) e per riempire una matrice con i dati risultanti. Il metodo **GetRows** restituirà un numero di righe inferiore al numero desiderato in due casi: se è stato raggiunto [EOF](./bof-eof-properties-ado.md) o se **GetRows** ha tentato di recuperare un record eliminato da un altro utente. La funzione restituisce **false** solo se si verifica il secondo caso. Tagliare e incollare il codice seguente nel blocco note o in un altro editor di testo e salvarlo come **GetRowsJS. asp**.  
  
```  
<!-- BeginGetRowsJS -->  
<%@ LANGUAGE="JScript" %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<html>  
  
<head>  
<title>ADO Recordset.GetRows Example (JScript)</title>  
<style>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead {  
   background-color: #008080;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.thead2 {  
   background-color: #800000;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</style>  
</head>  
  
<body bgcolor="white">  
  
<h1>ADO Recordset.GetRows Example (JScript)</h1>  
    <!-- Page text goes here -->  
<%  
        var Connect = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='Northwind';Integrated Security='SSPI';";  
    var mySQL = "select * from customers;";  
    var showblank = " ";  
    var shownull = "-null-";  
  
    var connTemp = Server.CreateObject("ADODB.Connection");  
  
    try  
    {  
        connTemp.Open(Connect);  
        var rsTemp = Server.CreateObject("ADODB.Recordset");  
        rsTemp.ActiveConnection = connTemp;  
        rsTemp.CursorLocation = adUseClient;  
        rsTemp.CursorType = adOpenKeyset;  
        rsTemp.LockType = adLockOptimistic;  
        rsTemp.Open(mySQL);  
  
        rsTemp.MoveFirst();  
  
        if (rsTemp.RecordCount == 0)  
        {  
            Response.Write("No records matched ");  
            Response.Write (mySQL & "So cannot make table...");  
            connTemp.Close();  
            Response.End();  
        } else  
        {  
            Response.Write('<table width="100%" border="2">');  
            Response.Write('<tr class="thead2">');  
  
            //  Headings On The Table for each Field Name  
            for (var i=0; i<rsTemp.Fields.Count; i++)  
            {  
                fieldObject = rsTemp.fields(i);  
                Response.Write('<td width="' + Math.floor(100 / rsTemp.Fields.Count) + '%">' + fieldObject.name + "</td>");  
            }  
  
            Response.Write("</tr>");  
  
            // JScript doesn't support multi-dimensional arrays  
            // so we'll convert the returned array to a single  
            // dimensional JScript array and then display the data.  
            tempArray = rsTemp.GetRows();  
            recArray = tempArray.toArray();  
  
            var col = 1;  
            var maxCols = rsTemp.Fields.Count;  
  
            for (var thisField=0; thisField<recArray.length; thisField++)  
            {  
                if (col == 1)  
                    Response.Write('<tr class="tbody">');  
                if (recArray[thisField] == null)  
                        recArray[thisField] = shownull;  
                if (recArray[thisField] == "")  
                        recArray[thisField] = showblank;  
                Response.Write("<td>" + recArray[thisField] + "</td>");  
                col++  
                if (col > maxCols)  
                {  
                    Response.Write("</tr>");  
                    col = 1;  
                }  
            }  
            Response.Write("</table>");  
        }  
    }  
    catch (e)  
    {  
        Response.Write(e.message);  
    }  
    finally  
    {  
        // clean up  
        if (rsTemp.State == adStateOpen)  
            rsTemp.Close;  
        if (connTemp.State == adStateOpen)  
            connTemp.Close;  
        rsTemp = null;  
        connTemp = null;  
    }  
%>  
  
</body>  
  
</html>  
<!-- EndGetRowsJS -->  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo GetRows (ADO)](./getrows-method-ado.md)   
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)