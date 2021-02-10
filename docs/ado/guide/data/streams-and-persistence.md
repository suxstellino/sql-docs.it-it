---
description: Flussi e persistenza
title: Flussi e persistenza | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisted streams [ADO]
- streams [ADO], persistence
ms.assetid: ad5bf52c-fd10-4cfa-bf7d-fcedcaa41eea
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b554afa949acea4f816ce66e92e897477c2fe00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032423"
---
# <a name="streams-and-persistence"></a>Flussi e persistenza
L' [](../../../ado/reference/ado-api/recordset-object-ado.md) oggetto recordset [Salva](../../../ado/reference/ado-api/save-method.md) il metodo archivia o rende *permanente* un **Recordset** in un file e il metodo [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) ripristina il **Recordset** da tale file.  
  
 Con ADO 2,7 o versioni successive, i metodi Save e **Open** possono **salvare** in modo permanente un **Recordset** anche in un oggetto [Stream](../../../ado/reference/ado-api/stream-object-ado.md) . Questa funzionalità è particolarmente utile quando si utilizzano Remote Data Service (RDS) e Active Server Pages (ASP).  
  
 Per ulteriori informazioni sul modo in cui è possibile utilizzare la persistenza in pagine ASP, vedere la documentazione ASP corrente.  
  
 Di seguito sono riportati alcuni scenari in cui viene illustrato come è possibile utilizzare gli oggetti **flusso** e la persistenza.  
  
## <a name="scenario-1"></a>Scenario 1  
 Questo scenario salva semplicemente un **Recordset** in un file e quindi in un **flusso**. Apre quindi il flusso permanente in un altro **Recordset**.  
  
```  
Dim rs1 As ADODB.Recordset  
Dim rs2 As ADODB.Recordset  
Dim stm As ADODB.Stream  
  
Set rs1 = New ADODB.Recordset  
Set rs2 = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
rs1.Open   "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;""", adopenStatic, adLockReadOnly, adCmdText  
rs1.Save "c:\myfolder\mysubfolder\myrs.xml", adPersistXML  
rs1.Save stm, adPersistXML  
rs2.Open stm  
```  
  
## <a name="scenario-2"></a>Scenario 2  
 Questo scenario rende permanente un **Recordset** in un **flusso** in formato XML. Il **flusso** viene quindi letto in una stringa che può essere esaminata, modificata o visualizzata.  
  
```  
Dim rs As ADODB.Recordset  
Dim stm As ADODB.Stream  
Dim strRst As String  
  
Set rs = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
' Open, save, and close the recordset.   
rs.Open "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
rs.Save stm, adPersistXML  
rs.Close  
Set rs = nothing  
  
' Put saved Recordset into a string variable.  
strRst = stm.ReadText(adReadAll)  
  
' Examine, manipulate, or display the XML data.  
...  
```  
  
## <a name="scenario-3"></a>Scenario 3  
 Questo codice di esempio mostra il codice ASP che rende permanente un **Recordset** come XML direttamente nell'oggetto **Response** :  
  
```  
...  
<%  
response.ContentType = "text/xml"  
  
' Create and open a Recordset.  
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select * from Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
  
' Save Recordset directly into output stream.  
rs.Save Response, adPersistXML   
  
' Close Recordset.  
rs.Close  
Set rs = nothing  
%>  
...  
```  
  
## <a name="scenario-4"></a>Scenario 4  
 In questo scenario, il codice ASP scrive il contenuto del **Recordset** in formato ADTG nel client. Il [servizio Microsoft Cursor per OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) può utilizzare questi dati per creare un **Recordset** disconnesso.  
  
 Una nuova proprietà di RDS [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md), [URL](../../../ado/reference/rds-api/url-property-rds.md), punta alla pagina ASP che genera il **Recordset**. Ciò significa che è possibile ottenere un oggetto **Recordset** senza servizi desktop remoto usando l'oggetto [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) sul lato server o l'utente che scrive un oggetto business. In questo modo, il modello di programmazione RDS viene semplificato in modo significativo.  
  
 Codice lato server, denominato https://server/directory/recordset.asp:  
  
```  
<%  
Dim rs   
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select au_fname, au_lname, phone from Authors", ""& _  
        "Provider=sqloledb;Data Source=MyServer;" & _  
        "Initial Catalog=Pubs;Integrated Security=SSPI;"  
response.ContentType = "multipart/mixed"  
rs.Save response, adPersistADTG  
%>  
```  
  
 Codice lato client:  
  
```  
<HTML>  
<HEAD>  
<TITLE>RDS Query Page</TITLE>  
</HEAD>  
<body>  
<CENTER>  
<H1>Remote Data Service 2.5</H1>  
<TABLE DATASRC="#DC1">  
   <TR>   
      <TD><SPAN DATAFLD="au_fname"></SPAN></TD>  
      <TD><SPAN DATAFLD="au_lname"></SPAN></TD>  
      <TD><SPAN DATAFLD="phone"></SPAN></TD>  
   </TR>  
</TABLE>  
<BR>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
    ID=DC1 HEIGHT=1 WIDTH = 1>  
    <PARAM NAME="URL" VALUE="https://server/directory/recordset.asp">  
</OBJECT>  
  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 Gli sviluppatori hanno anche la possibilità di usare un oggetto **Recordset** nel client:  
  
```  
...  
function GetRs()   
    {  
    rs = CreateObject("ADODB.Recordset");  
    rs.Open "https://server/directory/recordset.asp"  
    DC1.SourceRecordset = rs;  
    }  
...  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Open (recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Oggetto record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Metodo Save](../../../ado/reference/ado-api/save-method.md)
