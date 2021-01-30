---
description: Esempio della proprietà CacheSize (JScript)
title: Esempio di proprietà CacheSize (JScript) | Microsoft Docs
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
- CacheSize property [ADO], JScript example
ms.assetid: 3675f641-b4b1-48ff-ba33-8d9ea064cd04
author: rothja
ms.author: jroth
ms.openlocfilehash: 3087f5f35d27ca09afa860dbff52a5cec0b8b345
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171543"
---
# <a name="cachesize-property-example-jscript"></a>Esempio della proprietà CacheSize (JScript)
In questo esempio viene usata la proprietà [CacheSize](./cachesize-property-ado.md) per mostrare la differenza nelle prestazioni per un'operazione eseguita con e senza una cache di 30 record. Tagliare e incollare il codice seguente nel blocco note o in un altro editor di testo e salvarlo come **CacheSizeJS. asp**.  
  
```  
<!-- BeginCacheSizeJS -->  
<%@ Language="JScript" %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<HTML>  
<HEAD>  
<title>CacheSize Property Example (JScript)</title>  
<style>  
<!--  
body {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
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
</HEAD>  
<BODY>  
<h1>CacheSize Property Example (JScript)</h1>  
<%  
    // connection and recordset variables  
    var Cnxn = Server.CreateObject("ADODB.Connection")  
    var strCnxn = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='Northwind';Integrated Security='SSPI';";  
    var rsCustomer = Server.CreateObject("ADODB.Recordset");  
    // caching variables  
    var Now = new Date();  
    var Start = Now.getTime();  
    var End, Cache, NoCache  
  
    try  
    {  
        // open connection  
        Cnxn.Open(strCnxn)  
  
        // open a recordset on the Employee table using client-side cursor  
        rsCustomer.CursorLocation = adUseClient;  
        rsCustomer.Open("Customers", strCnxn);  
  
        // loop through the recordset 20 times  
        for (var i=1; i<=20; i++)  
        {  
            rsCustomer.MoveFirst();  
            while (!rsCustomer.EOF)  
            {  
                // do something with the record  
                var strTemp = new String(rsCustomer("CompanyName"));  
                rsCustomer.MoveNext();  
            }  
        }  
  
        Now = new Date();  
        End = Now.getTime();  
        NoCache = End - Start;  
  
        // cache records in groups of 30  
        rsCustomer.MoveFirst();  
        rsCustomer.CacheSize = 30;  
  
        Now = new Date();  
        Start = Now.getTime();  
  
        // loop through the recordset 20 times  
        for (var i=1; i<=20; i++)  
        {  
            rsCustomer.MoveFirst();  
            while (!rsCustomer.EOF)  
            {  
                // do something with the record  
                var strTemp = new String(rsCustomer("CompanyName"));  
                rsCustomer.MoveNext();  
            }  
        }  
  
        Now = new Date();  
        End = Now.getTime();  
        var Cache = End - Start;  
    }  
    catch (e)  
    {  
        Response.Write(e.message);  
    }  
    finally  
    {  
        // clean up  
        if (rsCustomer.State == adStateOpen)  
            rsCustomer.Close;  
        if (Cnxn.State == adStateOpen)  
            Cnxn.Close;  
        rsCustomer = null;  
        Cnxn = null;  
    }  
%>  
  
    <table border="2">  
        <tr class="thead2">  
            <th>No Cache</th>  
            <th>30 Record Cache</th>  
        </tr>  
        <tr class="tbody">  
            <td><%=NoCache%></td>  
            <td><%=Cache%></td>  
        </tr>  
    </table>  
  
</BODY>  
</HTML>  
<!-- EndCacheSizeJS -->  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà CacheSize (ADO)](./cachesize-property-ado.md)   
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)