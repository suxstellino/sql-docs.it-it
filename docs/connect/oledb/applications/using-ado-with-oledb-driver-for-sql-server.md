---
title: Uso di ADO con il driver OLE DB
description: Informazioni sull'uso di ADO con il driver OLE DB, incluse le nuove funzionalità come Multiple Active Result Set, notifiche delle query, tipi definiti dall'utente o il tipo di dati xml.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, ADO
- data access [OLE DB Driver for SQL Server], ADO
- ADO [OLE DB Driver for SQL Server]
- MSOLEDBSQL, ADO
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c40b9c2541c724f448dd636d5f7217da223a6f92
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742811"
---
# <a name="using-ado-with-ole-db-driver-for-sql-server"></a>Uso di ADO con il driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Per sfruttare le nuove caratteristiche introdotte in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], come MARS (Multiple Active Result Set), notifiche delle query, tipi definiti dall'utente (UDT) o il nuovo tipo di dati **xml**, è necessario che nelle applicazioni esistenti che usano ADO (ActiveX Data Objects) come provider di accesso ai dati venga usato il driver OLE DB per SQL Server.  
  
 Per consentire l'uso in ADO delle nuove funzionalità delle ultime versioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], al driver OLE DB per SQL Server sono stati apportati alcuni miglioramenti che estendono le funzionalità principali di OLE DB. Questi miglioramenti consentono l'uso nelle applicazioni ADO delle più recenti funzionalità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e di due tipi di dati introdotti in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]: **xml** e **udt**. Consentono inoltre di sfruttare i miglioramenti ai tipi di dati **varchar**, **nvarchar** e **varbinary**. Nel driver OLE DB per SQL Server viene aggiunta la proprietà di inizializzazione SSPROP_INIT_DATATYPECOMPATIBILITY al set di proprietà DBPROPSET_SQLSERVERDBINIT per consentirne l'uso nelle applicazioni ADO, in modo che i nuovi tipi di dati siano esposti in modo compatibile con ADO. OLE DB Driver per SQL Server definisce anche una nuova parola chiave della stringa di connessione, denominata **DataTypeCompatibility**, impostata nella stringa di connessione.  

> [!NOTE]  
>  Le applicazioni ADO esistenti possono accedere a valori XML, UDT, nonché di campi binari e di testo valore di grandi dimensioni ed eseguirne l'aggiornamento utilizzando il provider SQLOLEDB. I nuovi tipi di dati **varchar(max)** , **nvarchar(max)** e **varbinary(max)** di grandi dimensioni vengono restituiti rispettivamente come i tipi ADO **adLongVarChar**, **adLongVarWChar** e **adLongVarBinary**. Le colonne XML vengono restituite come **adLongVarChar**, mentre le colonne con tipo definito dall'utente vengono restituite come **adVarBinary**. Se tuttavia, si usa OLE DB Driver per SQL Server (MSOLEDBSQL) invece di SQLOLEDB, è necessario assicurarsi di impostare la parola chiave **DataTypeCompatibility** su "80" in modo da consentire la corretta esecuzione del mapping dei nuovi tipi di dati ai tipi di dati ADO.  

## <a name="enabling-ole-db-driver-for-sql-server-from-ado"></a>Abilitazione di OLE DB Driver per SQL Server da ADO  
 Per abilitare l'uso del driver OLE DB per SQL Server, è necessario che nelle stringhe di connessione delle applicazioni ADO vengano implementate le parole chiave seguenti:  

-   `Provider=MSOLEDBSQL`  

-   `DataTypeCompatibility=80`  

 Per informazioni sulle parole chiave della stringa di connessione ADO supportate nel driver OLE DB per SQL Server, vedere [Uso delle parole chiave delle stringhe di connessione con driver OLE DB per SQL Server](using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

 Di seguito è riportato un esempio di stringa di connessione ADO completamente abilitata per l'uso nel driver OLE DB per SQL Server con l'abilitazione del servizio MARS:  

```  
Dim con As New ADODB.Connection  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  
```  

## <a name="examples"></a>Esempi  
 Le sezioni seguenti forniscono esempi di come è possibile usare ADO con OLE DB Driver per SQL Server.  

### <a name="retrieving-xml-column-data"></a>Recupero dei dati delle colonne XML  
 In questo esempio viene usato un recordset per recuperare e visualizzare i dati da una colonna XML nel database di esempio **AdventureWorks** di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

```  
Dim con As New ADODB.Connection  
Dim rst As New ADODB.Recordset  
Dim sXMLResult As String  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _   
         & "DataTypeCompatibility=80;"  

con.Open  

' Get the xml data as a recordset.  
Set rst.ActiveConnection = con  
rst.Source = "SELECT AdditionalContactInfo FROM Person.Contact " _  
   & "WHERE AdditionalContactInfo IS NOT NULL"  
rst.Open  

' Display the data in the recordset.  
While (Not rst.EOF)  
   sXMLResult = rst.Fields("AdditionalContactInfo").Value  
   Debug.Print (sXMLResult)  
   rst.MoveNext  
End While  

con.Close  
Set con = Nothing  
```  

> [!NOTE]  
>  L'applicazione di filtri al recordset non è supportata con le colonne XML. Se si prova ad applicare i filtri, viene generato un errore.  

### <a name="retrieving-udt-column-data"></a>Recupero dei dati delle colonne con tipo definito dall'utente  
 In questo esempio viene utilizzato un oggetto **Command** per eseguire una query SQL che restituisce un tipo definito dall'utente, vengono aggiornati i dati UDT, quindi i nuovi dati vengono nuovamente inseriti nel database. In questo esempio si presuppone che il tipo definito dall'utente (UDT) **Point** sia già stato registrato nel database.  

```  
Dim con As New ADODB.Connection  
Dim cmd As New ADODB.Command  
Dim rst As New ADODB.Recordset  
Dim strOldUDT As String  
Dim strNewUDT As String  
Dim aryTempUDT() As String  
Dim strTempID As String  
Dim i As Integer  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;"  

con.Open  

' Get the UDT value.  
Set cmd.ActiveConnection = con  
cmd.CommandText = "SELECT ID, Pnt FROM dbo.Points.ToString()"  
Set rst = cmd.Execute  
strTempID = rst.Fields(0).Value  
strOldUDT = rst.Fields(1).Value  

' Do something with the UDT by adding i to each point.  
arytempUDT = Split(strOldUDT, ",")  
i = 3  
strNewUDT = LTrim(Str(Int(aryTempUDT(0)) + i)) + "," + _  
   LTrim(Str(Int(aryTempUDT(1)) + i))  

' Insert the new value back into the database.  
cmd.CommandText = "UPDATE dbo.Points SET Pnt = '" + strNewUDT + _  
   "' WHERE ID = '" + strTempID + "'"  
cmd.Execute  

con.Close  
Set con = Nothing  
```  

### <a name="enabling-and-using-mars"></a>Abilitazione e utilizzo di MARS  
 In questo esempio viene costruita la stringa di connessione per abilitare MARS mediante il driver OLE DB per SQL Server, quindi vengono creati due oggetti di recordset da eseguire usando la stessa connessione.  

```  
Dim con As New ADODB.Connection  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  

Dim recordset1 As New ADODB.Recordset  
Dim recordset2 As New ADODB.Recordset  

Dim recordsaffected As Integer  
Set recordset1 =  con.Execute("SELECT * FROM Table1", recordsaffected, adCmdText)  
Set recordset2 =  con.Execute("SELECT * FROM Table2", recordsaffected, adCmdText)  

con.Close  
Set con = Nothing  
```  

 Nelle versioni precedenti del provider OLE DB questo codice determinerebbe la creazione di una connessione implicita alla seconda esecuzione, in quanto per una sola connessione sarebbe possibile aprire un solo set attivo di risultati. Poiché la connessione implicita non sarebbe inserita nel pool di connessioni OLE DB, questa situazione provocherebbe overhead aggiuntivo. Con la funzionalità MARS esposta dal driver OLE DB per SQL Server, si ottengono più risultati attivi su un'unica connessione.  

## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni con il driver OLE DB per SQL Server](building-applications-with-oledb-driver-for-sql-server.md)  
