---
title: Usare i risultati di query FOR XML nel codice di un'applicazione | Microsoft Docs
description: Informazioni su come usare i risultati di una query SQL con le clausole FOR XML in un'applicazione.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, application code usage
- XML [SQL Server], FOR XML clause
- ASP.NET [SQL Server]
- .NET Framework [SQL Server], FOR XML data
- ADO [SQL Server]
- XML data islands [SQL Server]
- data islands [SQL Server]
ms.assetid: 41ae67bd-ece9-49ea-8062-c8d658ab4154
author: rothja
ms.author: jroth
ms.openlocfilehash: fa42eca0883961376dbab3707b61c7d31135459e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487132"
---
# <a name="use-for-xml-results-in-application-code"></a>Utilizzare i risultati di query FOR XML nel codice di un'applicazione
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  L'utilizzo delle clausole FOR XML nelle query SQL consente di recuperare e inoltre di eseguire il cast dei risultati delle query come dati XML. Se i risultati di una query FOR XML possono essere utilizzati nel codice XML dell'applicazione, è possibile eseguire le operazioni seguenti:  
  
-   Eseguire query nelle tabelle SQL per le istanze di valori di [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
-   Applicare la [Direttiva TYPE in query FOR XML](../../relational-databases/xml/type-directive-in-for-xml-queries.md) per restituire il risultato di query che contengono dati di testo o di tipo image in formato XML.  
  
 In questo argomento vengono forniti esempi di questi approcci.  
  
## <a name="retrieving-for-xml-data-with-ado-and-xml-data-islands"></a>Recupero dei dati di query FOR XML con ADO e isole di dati XML  
 L'oggetto ADO **Stream** o altri oggetti che supportano l'interfaccia COM **IStream** , ad esempio gli oggetti ASP (Active Server Pages) **Request** e **Response** possono essere usati per includere i risultati quando si eseguono query FOR XML.  
  
 Ad esempio, il codice ASP seguente mostra i risultati di una query eseguita sulla colonna con tipo di dati **xml** Demographics della tabella Sales.Store del database di esempio AdventureWorks. La query cerca specificatamente il valore dell'istanza della colonna relativo alla riga in cui CustomerID è uguale a 3.  
  
```  
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<!-- %  Option Explicit      % -->  
<!-- 'Request.ServerVariables("SERVER_NAME") & ";" & _ -->  
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
   sConn = "Provider=SQLOLEDB;Data Source=(local);" & _  
            "Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
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
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO</sql:query></ROOT>"  
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
   Dim root  
   Set root = xmlDoc.documentElement.childNodes.Item(0).childNodes.Item(0).childNodes.Item(0)  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI><B>" & child.nodeName &  ":</B>  " & child.Text  & "</LI>"  
   Next  
   MsgBox xmlDoc.xml  
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
  
 Questa pagina ASP di esempio contiene codice VBScript sul lato server che utilizza ADO per eseguire la query FOR XML e restituire i risultati XML in un'isola di dati XML, MyDataIsle. L'isola di dati XML viene quindi restituita nel browser per un'ulteriore elaborazione sul lato client, durante la quale viene utilizzato codice VBScript aggiuntivo per elaborare il contenuto dell'isola di dati XML. Il processo viene eseguito prima di visualizzare il contenuto come parte del DHTML risultante e di aprire una finestra di messaggio in cui è visualizzato il contenuto pre-elaborato dell'isola di dati XML.  
  
#### <a name="to-test-this-example"></a>Per testare l'esempio  
  
1.  Verificare che IIS e il database di esempio AdventureWorks per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siano installati.  
  
     Per eseguire l'esempio, è necessario che sia installato Internet Information Services (IIS) 5.0 o versioni successive e che sia attivato il supporto ASP. È inoltre necessario installare il database di esempio AdventureWorks.  
  
2.  Copiare il codice di esempio fornito in precedenza e incollarlo nell'editor XML o di testo in uso. Salvare il file come RetrieveResults.asp nella directory radice utilizzata per IIS, che in genere è C:Inetpub\wwwroot.  
  
3.  Aprire la pagina ASP in una finestra del browser specificando l'URL seguente. Sostituire innanzitutto 'MyServer' con "localhost" o con il nome effettivo del server in cui sono installati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e IIS.  
  
    ```  
    https://MyServer/RetrieveResults.asp  
    ```  
  
 I risultati visualizzati per la pagina HTML creata saranno simili all'output di esempio seguente:  
  
##### <a name="server-side-processing"></a>Elaborazione sul lato server  
 Page Generated \@ 3/11/2006 3:36:02 PM  
  
 Connect String = Provider=SQLOLEDB;Data Source=MyServer;Initial Catalog=AdventureWorks;Integrated Security=SSPI;  
  
 ADO Version = 2.8  
  
 adoConn.State = 1  
  
 Query String = SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO  
  
 Pushing XML to client for processing  
  
##### <a name="client-side-processing-of-xml-document-mydataisle"></a>Elaborazione sul lato client del documento XML MyDataIsle  
  
-   **AnnualSales:** 1500000  
  
-   **AnnualRevenue:** 150000  
  
-   **BankName:** Primary International  
  
-   **BusinessType:** OS  
  
-   **YearOpened:** 1974  
  
-   **Specialty:** Strada  
  
-   **SquareFeet:** 38000  
  
-   **Brands:** 3  
  
-   **Internet:** DSL  
  
-   **NumberEmployees:** 40  
  
 Nella finestra di messaggio VBScript verrà visualizzato il contenuto originale non filtrato dell'isola di dati XML seguente, restituito dai risultati della query FOR XML.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Sales.Store>  
    <Demographics>  
      <StoreSurvey xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey">  
        <AnnualSales>1500000</AnnualSales>  
        <AnnualRevenue>150000</AnnualRevenue>  
        <BankName>Primary International</BankName>  
        <BusinessType>OS</BusinessType>  
        <YearOpened>1974</YearOpened>  
        <Specialty>Road</Specialty>  
        <SquareFeet>38000</SquareFeet>  
        <Brands>3</Brands>  
        <Internet>DSL</Internet>  
        <NumberEmployees>40</NumberEmployees>  
      </StoreSurvey>  
    </Demographics>  
  </Sales.Store>  
</ROOT>  
```  
  
## <a name="retrieving-for-xml-data-with-aspnet-and-the-net-framework"></a>Recupero dei dati di query FOR XML con ASP.NET e .NET Framework  
 Come nell'esempio precedente, il codice ASP.NET seguente mostra i risultati di una query eseguita sulla colonna con tipo di dati **xml** Demographics della tabella Sales.Store del database di esempio AdventureWorks. Come nell'esempio precedente, la query cerca il valore dell'istanza della colonna relativo alla riga in cui CustomerID è uguale a 3.  
  
 Nell'esempio, per la restituzione e il rendering dei risultati della query FOR XML vengono utilizzate le API gestite di Microsoft .NET Framework seguenti:  
  
1.  **SqlConnection** consente di stabilire una connessione a SQL Server, basata sul contenuto di una variabile stringa di connessione specificata, strConn.  
  
2.  **SqlDataAdapter** funge quindi da adattatore dati e consente di eseguire la query FOR XML usando la connessione SQL e una stringa di query SQL specificata.  
  
3.  Dopo avere eseguito la query, viene chiamato il metodo **SqlDataAdapter.Fill** , che viene quindi passato a un'istanza di un **DataSet** MyDataSet per riempire il set di dati con l'output della query FOR XML.  
  
4.  Viene quindi chiamato il metodo **DataSet.GetXml** per restituire i risultati della query come una stringa visualizzabile nella pagina HTML generata dal server.  
  
    ```  
    <%@ Page Language="VB" %>  
    <HTML>  
    <HEAD>  
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
    </HEAD>  
    <BODY>  
    <%  
    Dim s as String  
    s = "<H3>Server-side processing</H3>" & _  
        "Page Generated @ " & Now() & "<BR/>"  
  
    Dim SQL As String   
    SQL = "SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO"  
  
    Dim strConn As String   
    strConn = "Server=(local);Database=AdventureWorks;Integrated Security=SSPI;"  
  
    Dim MySqlConn As New System.Data.SqlClient.SqlConnection(strConn)  
    Dim MySqlAdapter As New System.Data.SqlClient.SqlDataAdapter(SQL,MySqlConn)  
    Dim MyDataSet As New System.Data.DataSet  
  
    MySqlConn.Open()  
    s = s & "<P>SqlConnection opened.</P>"   
  
    MySqlAdapter.Fill(MyDataSet)  
    s = s & "<P>" & MyDataSet.GetXml  & "</P>"  
  
    MySqlConn.Close()  
    s = s & "<P>SqlConnection closed.</P>"   
  
    Message.InnerHtml=s  
    %>  
    <SPAN id="Message" runat=server />  
    </BODY>  
    </HTML>  
    ```  
  
#### <a name="to-test-this-example"></a>Per testare l'esempio  
  
1.  Verificare che IIS e il database di esempio AdventureWorks per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siano installati.  
  
     Per eseguire l'esempio, è necessario che sia installato Internet Information Services (IIS) 5.0 o versioni successive e che sia attivato il supporto ASP.NET. È inoltre necessario installare il database di esempio AdventureWorks.  
  
2.  Copiare il codice fornito in precedenza e incollarlo nell'editor XML o di testo in uso. Salvare il file come RetrieveResults.aspx nella directory radice utilizzata per IIS, che in genere è C:Inetpub\wwwroot.  
  
3.  Aprire la pagina ASP.NET in una finestra del browser specificando l'URL seguente. Sostituire innanzitutto 'MyServer' con "localhost" o con il nome effettivo del server in cui sono installati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e IIS.  
  
    ```  
    https://MyServer/RetrieveResults.aspx  
    ```  
  
 I risultati visualizzati per la pagina HTML creata saranno simili all'output di esempio seguente:  
  
##### <a name="server-side-processing"></a>Elaborazione sul lato server  
  
```  
Page Generated @ 3/11/2006 3:36:02 PM  
  
SqlConnection opened.  
  
<Sales.Store><Demographics><StoreSurvey xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey"><AnnualSales>1500000</AnnualSales><AnnualRevenue>150000</AnnualRevenue><BankName>Primary International</BankName><BusinessType>OS</BusinessType><YearOpened>1974</YearOpened><Specialty>Road</Specialty><SquareFeet>38000</SquareFeet><Brands>3</Brands><Internet>DSL</Internet><NumberEmployees>40</NumberEmployees></StoreSurvey></Demographics></Sales.Store>  
  
SqlConnection closed.  
```  
  
> [!NOTE]  
>  Il supporto del tipo di dati **xml** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di richiedere che il risultato di una query FOR XML venga restituito come tipo di dati **xml** invece che come dati tipizzati di stringa o immagine, specificando la [direttiva TYPE](../../relational-databases/xml/type-directive-in-for-xml-queries.md). L'uso della direttiva TYPE nelle query FOR XML consente di ottenere l'accesso a livello di programmazione ai risultati FOR XML, in modo analogo a quanto illustrato in [Utilizzo di dati XML nelle applicazioni](../../relational-databases/xml/use-xml-data-in-applications.md).  
  
## <a name="see-also"></a>Vedere anche  
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
