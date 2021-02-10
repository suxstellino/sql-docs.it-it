---
description: Recupero di set di risultati nei flussi
title: Recupero di set di risultati in flussi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO], retrieving query results
- query results into stream [ADO]
- retrieving results into stream [ADO]
ms.assetid: 996c1321-c926-4f57-8297-85c8c20de974
author: rothja
ms.author: jroth
ms.openlocfilehash: f625a4f96aa5ec083057af4118315d9df494530e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032543"
---
# <a name="retrieving-resultsets-into-streams"></a>Recupero di set di risultati nei flussi
Anziché ricevere i risultati nell'oggetto **Recordset** tradizionale, ADO può invece recuperare i risultati della query in un flusso. Per contenere questi risultati, è possibile usare l'oggetto **flusso** ADO o altri oggetti che supportano l'interfaccia com **IStream** , ad esempio gli oggetti **richiesta** e **risposta** ASP. Un uso di questa funzionalità è quello di recuperare i risultati in formato XML. Con SQL Server, ad esempio, i risultati XML possono essere restituiti in diversi modi, ad esempio utilizzando la clausola FOR XML con una query SQL SELECT o utilizzando una query XPath.  
  
 Per ricevere i risultati della query in formato flusso anziché in un **Recordset**, è necessario specificare la costante **adExecuteStream** da **ExecuteOptionEnum** come parametro del metodo **Execute** di un oggetto **Command** . Se il provider supporta questa funzionalità, i risultati verranno restituiti in un flusso al momento dell'esecuzione. Potrebbe essere necessario specificare proprietà aggiuntive specifiche del provider prima dell'esecuzione del codice. Ad esempio, con il provider di Microsoft OLE DB per SQL Server, è necessario specificare proprietà come il **flusso di output** nella raccolta **Properties** dell'oggetto **Command** . Per ulteriori informazioni sulle proprietà dinamiche specifiche di SQL Server correlate a questa funzionalità, vedere Proprietà XML-Related nel documentazione online di SQL Server.  
  
## <a name="for-xml-query-example"></a>PER XML Query esempio  
 Nell'esempio seguente viene scritto in VBScript nel database Northwind:  
  
```html
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<%  Option Explicit      %>  
  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Developer Studio"/>  
<META HTTP-EQUIV="Content-Type" content="text/html"; charset="iso-8859-1">  
<TITLE>FOR XML Query Example</TITLE>  
  
<STYLE>  
   BODY  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
  
   H3  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
</STYLE>  
  
<!-- #include file="adovbs.inc" -->  
<%  
   Response.Write "<H3>Server-side processing</H3>"  
  
   Response.Write "Page Generated @ " & Now() & "<BR/>"  
  
   Dim adoConn  
   Set adoConn = Server.CreateObject("ADODB.Connection")  
  
   Dim sConn  
   sConn = "Provider=SQLOLEDB;Data Source=" & _  
      Request.ServerVariables("SERVER_NAME") & ";" & _  
      Initial Catalog=Northwind;Integrated Security=SSPI;"  
  
   Response.write "Connect String = " & sConn & "<BR/>"  
  
   adoConn.ConnectionString = sConn  
   adoConn.CursorLocation = adUseClient  
  
   adoConn.Open  
  
   Response.write "ADO Version = " & adoConn.Version & "<BR/>"  
   Response.write "adoConn.State = " & adoConn.State & "<BR/>"  
  
   Dim adoCmd  
   Set adoCmd = Server.CreateObject("ADODB.Command")  
   Set adoCmd.ActiveConnection = adoConn  
  
   Dim sQuery  
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT * FROM PRODUCTS WHERE ProductName='Gumbr Gummibrchen' FOR XML AUTO</sql:query></ROOT>"  
  
   Response.write "Query String = " & sQuery & "<BR/>"  
  
   Dim adoStreamQuery  
   Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
   adoStreamQuery.Open  
   adoStreamQuery.WriteText sQuery, adWriteChar  
   adoStreamQuery.Position = 0  
  
   adoCmd.CommandStream = adoStreamQuery  
   adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
  
   Response.write "Pushing XML to client for processing "  & "<BR/>"  
  
   adoCmd.Properties("Output Stream") = Response  
   Response.write "<XML ID='MyDataIsle'>"  
   adoCmd.Execute , , 1024  
   Response.write "</XML>"  
  
%>  
  
<SCRIPT language="VBScript" For="window" Event="onload">  
   Dim xmlDoc  
   Set xmlDoc = MyDataIsle.XMLDocument  
   xmlDoc.resolveExternals=false  
   xmlDoc.async=false  
  
   If xmlDoc.parseError.Reason <> "" then  
      Msgbox "parseError.Reason = " & xmlDoc.parseError.Reason  
   End If  
  
   Dim root, child  
   Set root = xmlDoc.documentElement  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI>" & child.getAttribute("ProductName") & "</LI>"  
   Next  
</SCRIPT>  
  
</HEAD>  
  
<BODY>  
  
   <H3>Client-side processing of XML Document MyDataIsle</H3>  
   <UL id=log>  
   </UL>  
  
</BODY>  
</HTML>  
<!-- EndRecordAndStreamVBS -->  
  
```  
  
 La clausola FOR XML indica SQL Server restituire dati sotto forma di documento XML.  
  
### <a name="for-xml-syntax"></a>Sintassi FOR XML  
  
```syntax
FOR XML [RAW|AUTO|EXPLICIT]  
```  
  
 FOR XML RAW genera elementi riga generici con valori di colonna come attributi. FOR XML AUTO usa l'euristica per generare una struttura ad albero gerarchica con nomi di elementi basati sui nomi di tabella. FOR XML EXPLICIT genera una tabella universale con relazioni completamente descritte dai metadati.  
  
 Di seguito è riportato un esempio di istruzione SQL SELECT FOR XML:  
  
```sql
SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO  
```  
  
 Il comando può essere specificato in una stringa, come illustrato in precedenza, assegnato a **CommandText** o sotto forma di query modello XML assegnata a **CommandStream**. Per altre informazioni sulle query di modelli XML, vedere [flussi di comandi](../../../ado/guide/data/command-streams.md) in ADO o uso di flussi per l'input del comando nella documentazione online di SQL Server.  
  
 Come query modello XML, la query FOR XML viene visualizzata come segue:  
  
```xml
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO </sql:query>  
```  
  
 In questo esempio viene specificato l'oggetto di **risposta** ASP per la proprietà del **flusso di output** :  
  
```vb
adoCmd.Properties("Output Stream") = Response  
```  
  
 Specificare quindi il parametro **adExecuteStream** di **Execute**. Questo esempio esegue il wrapping del flusso nei tag XML per creare un'isola di dati XML:  
  
```vb
Response.write "<XML ID=MyDataIsle>"  
adoCmd.Execute , , adExecuteStream  
Response.write "</XML>"  
```  
  
### <a name="remarks"></a>Commenti  
 A questo punto, il codice XML è stato trasmesso al browser client ed è pronto per essere visualizzato. Questa operazione viene eseguita utilizzando VBScript sul lato client per associare il documento XML a un'istanza del DOM e scorrere in ciclo ogni nodo figlio per compilare un elenco di prodotti in HTML.
