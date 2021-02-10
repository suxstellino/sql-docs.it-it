---
description: Scenario di persistenza di recordset XML
title: Scenario di persistenza recordset XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence scenario
ms.assetid: 353d569a-043a-4397-9ee6-564c4af8d5f6
author: rothja
ms.author: jroth
ms.openlocfilehash: bbad3ddcfd8c34be6ea911f4cef580832f1c55b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032223"
---
# <a name="xml-recordset-persistence-scenario"></a>Scenario di persistenza di recordset XML
In questo scenario verrà creata un'applicazione di Active Server Pages (ASP) che salva il contenuto di un oggetto recordset direttamente nell'oggetto risposta ASP.  
  
> [!NOTE]
>  Per questo scenario è necessario che nel server sia installato Internet Information Server 5,0 (IIS) o versione successiva.  
  
 Il recordset restituito viene visualizzato in Internet Explorer utilizzando un [oggetto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md).  
  
 Per creare questo scenario, sono necessari i passaggi seguenti:  
  
-   Configurare l'applicazione  
  
-   Ottenere i dati  
  
-   Inviare i dati  
  
-   Ricevere e visualizzare i dati  
  
## <a name="step-1-set-up-the-application"></a>Passaggio 1: configurare l'applicazione  
 Creare una directory virtuale IIS denominata "xmlpermanente" con autorizzazioni di script. Creare due nuovi file di testo nella cartella a cui punta la directory virtuale, uno denominato "XMLResponse. asp", l'altro denominato "Default.htm".  
  
## <a name="step-2-get-the-data"></a>Passaggio 2: ottenere i dati  
 In questo passaggio verrà scritto il codice per aprire un recordset ADO e prepararsi per inviarlo al client. Aprire il file XMLResponse. asp con un editor di testo, ad esempio Blocco note, e inserire il codice seguente.  
  
```  
<%@ language="VBScript" %>  
  
<!-- #include file='adovbs.inc' -->  
  
<%  
  Dim strSQL, strCon  
  Dim adoRec   
  Dim adoCon   
  Dim xmlDoc   
  
  ' You will need to change "MySQLServer" below to the name of the SQL   
  ' server machine to which you want to connect.  
  strCon = "Provider=sqloledb;Data Source=MySQLServer;Initial Catalog=Pubs;Integrated Security=SSPI;"  
  Set adoCon = server.createObject("ADODB.Connection")  
  adoCon.Open strCon  
  
  strSQL = "SELECT Title, Price FROM Titles ORDER BY Price"  
  Set adoRec = Server.CreateObject("ADODB.Recordset")  
  adoRec.Open strSQL, adoCon, adOpenStatic, adLockOptimistic, adCmdText  
```  
  
 Assicurarsi di modificare il valore del `Data Source` parametro in sul `strCon` nome del computer Microsoft SQL Server.  
  
 Lasciare aperto il file e continuare con il passaggio successivo.  
  
## <a name="step-3-send-the-data"></a>Passaggio 3: inviare i dati  
 Ora che si dispone di un recordset, è necessario inviarlo al client salvando il file come XML nell'oggetto risposta ASP. Aggiungere il codice seguente alla fine di XMLResponse. ASP.  
  
```  
  Response.ContentType = "text/xml"  
  Response.Expires = 0  
  Response.Buffer = False  
  
  Response.Write "<?xml version='1.0'?>" & vbNewLine  
  adoRec.save Response, adPersistXML  
  adoRec.Close  
  Set adoRec=Nothing  
%>  
```  
  
 Si noti che l'oggetto di risposta ASP viene specificato come destinazione per il [metodo Save](../../reference/ado-api/save-method.md)del recordset. La destinazione del metodo Save può essere un oggetto che supporta l'interfaccia IStream, ad esempio un oggetto ADO [Stream](../../reference/ado-api/stream-object-ado.md)o un nome di file che include il percorso completo in cui deve essere salvato il recordset.  
  
 Salvare e chiudere XMLResponse. asp prima di andare al passaggio successivo. Copiare inoltre il file Adovbs. Inc dalla cartella di installazione predefinita della libreria ADO alla stessa cartella in cui è stato salvato il file XMLResponse. ASP.  
  
## <a name="step-4-receive-and-display-the-data"></a>Passaggio 4: ricevere e visualizzare i dati  
 In questo passaggio verrà creato un file HTML con un oggetto [datacontrollo incorporato (RDS)](../../reference/rds-api/datacontrol-object-rds.md) che punta al file XMLResponse. asp per ottenere il recordset. Aprire default.htm con un editor di testo, ad esempio Blocco note, e aggiungere il codice seguente. Sostituire "SqlServer" nell'URL con il nome del server.  
  
```  
<HTML>  
<HEAD><TITLE>ADO Recordset Persistence Sample</TITLE></HEAD>  
<BODY>  
  
<TABLE DATASRC="#RDC1" border="1">  
  <TR>  
<TD><SPAN DATAFLD="title"></SPAN></TD>  
<TD><SPAN DATAFLD="price"></SPAN></TD>  
  </TR>  
</TABLE>  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="RDC1">  
   <PARAM NAME="URL" VALUE="XMLResponse.asp">  
</OBJECT>  
  
</BODY>  
</HTML>  
```  
  
 Chiudere il file di default.htm e salvarlo nella stessa cartella in cui è stato salvato XMLResponse. ASP. Utilizzando Internet Explorer 4,0 o versione successiva, aprire l'URL https://*SqlServer*/XMLPersist/default.htm e osservare i risultati. I dati vengono visualizzati in una tabella DHTML associata. A questo punto aprire l'URL https:// *SqlServer* /XMLPersist/XMLResponse.asp e osservare i risultati. Viene visualizzato il codice XML.  
  
## <a name="see-also"></a>Vedere anche  
 [Save (metodo)](../../reference/ado-api/save-method.md)   
 [Persistenza di record in formato XML](./persisting-records-in-xml-format.md)