---
title: Eseguire query XPath con spazi dei nomi (SQLXMLOLEDB)
description: Informazioni su come specificare gli spazi dei nomi in SQLXML 4.0 quando si eseguono query XPath con il provider SQLXMLOLEDB.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, executing XPath queries
- namespaces property
- queries [SQLXML], SQLXMLOLEDB Provider
- XPath queries [SQLXML], namespaces
- XPath queries [SQLXML], SQLXMLOLEDB Provider
- namespaces [SQLXML], XPath queries
ms.assetid: 024a4b7d-435d-47ba-9e80-2c2f640108f5
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1cfd963299f9e9364759bf588e4df774c1db72ef
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491960"
---
# <a name="executing-xpath-queries-with-namespaces-sqlxmloledb-provider"></a>Esecuzione di query XPath con spazi dei nomi (provider SQLXMLOLEDB)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Le query XPath possono includere spazi dei nomi. Se gli elementi dello schema sono spazi dei nomi qualificati, ovvero includono uno spazio dei nomi di destinazione, è necessario che nelle query XPath eseguite sullo schema sia specificato lo spazio dei nomi in questione.  
  
 Poiché l'utilizzo del carattere jolly (*) non è supportato in SQLXML 4.0, è necessario specificare la query XPath utilizzando un prefisso dello spazio dei nomi. Per risolvere questo prefisso, usare la proprietà namespaces per specificare l'associazione dello spazio dei nomi.  
  
 Nell'esempio seguente la query XPath specifica gli spazi dei nomi usando il carattere jolly ( ) e le funzioni \* XPath local-name() e namespace-uri(). Questa query XPath restituisce tutti gli elementi in cui il nome locale è **Contact** e l'URI dello spazio dei nomi è **urn:myschema:Contacts**.  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 In SQLXML 4.0 questa query XPath deve essere specificata con un prefisso dello spazio dei nomi. Un esempio è **x:Contact**, dove **x** è il prefisso dello spazio dei nomi. Si consideri lo schema XSD seguente:  
  
```  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:con="urn:myschema:Contacts"  
            targetNamespace="urn:myschema:Contacts">  
<complexType name="ContactType">  
  <attribute name="CID" sql:field="ContactID" type="ID"/>  
  <attribute name="FName" sql:field="FirstName" type="string"/>  
  <attribute name="LName" sql:field="LastName"/>   
</complexType>  
<element name="Contact" type="con:ContactType" sql:relation="Person.Contact"/>  
</schema>  
```  
  
 Poiché questo schema definisce lo spazio dei nomi di destinazione, una query XPath (ad esempio "Employee") eseguita sullo schema deve includere lo spazio dei nomi.  
  
 Si tratta di un'applicazione Visual Basic [!INCLUDE[msCoName](../../../includes/msconame-md.md)] di esempio che esegue una query XPath (x:Employee) sullo schema XSD precedente. Per risolvere il prefisso, l'associazione dello spazio dei nomi viene specificata tramite la proprietà namespaces.  
  
> [!NOTE]  
>  Nel codice è necessario specificare il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nella stringa di connessione. In questo esempio viene inoltre specificato l'utilizzo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) per il provider di dati che richiede l'installazione di un software client di rete aggiuntivo. Per altre informazioni, vedere [Requisiti di sistema per SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md).  
  
```  
Option Explicit  
Private Sub Form_Load()  
    Dim con As New ADODB.Connection  
    Dim cmd As New ADODB.Command  
    Dim stm As New ADODB.Stream  
    con.Open "provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Data Source=SqlServerName;Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
    Set cmd.ActiveConnection = con  
    stm.Open  
    cmd.Properties("Output Stream").Value = stm  
    cmd.Properties("Output Encoding") = "utf-8"  
    cmd.Properties("Mapping schema") = "C:\DirectoryPath\con-ex.xml"  
    cmd.Properties("namespaces") = "xmlns:x='urn:myschema:Contacts'"  
    '  Debug.Print "Set Command Dialect to DBGUID_XPATH"  
    cmd.Dialect = "{ec2a4293-e898-11d2-b1b7-00c04f680c56}"  
    cmd.CommandText = "x:Contact"  
    cmd.Execute , , adExecuteStream   
    stm.Position = 0  
    Debug.Print stm.ReadText(adReadAll)  
End Sub  
```  
  
### <a name="to-test-this-application"></a>Per testare l'applicazione  
  
1.  Salvare lo schema XSD di esempio in una cartella.  
  
2.  Creare un progetto eseguibile di Visual Basic in cui copiare il codice. Modificare il percorso di directory specificato in base alle esigenze.  
  
3.  Aggiungere il riferimento al progetto seguente:  
  
    ```  
    "Microsoft ActiveX Data Objects 2.8 Library"  
    ```  
  
4.  Eseguire l'applicazione.  

 Risultato parziale:  
  
```  
<y0:Employee xmlns:y0="urn:myschema:Contacts"   
             LName="Achong" CID="1" FName="Gustavo"/>  
<y0:Employee xmlns:y0="urn:myschema:Employees"   
             LName="Abel" CID="2" FName="Catherine"/>  
```  
  
 I prefissi generati nel documento XML sono arbitrari, ma consentono di eseguire il mapping allo stesso spazio dei nomi.  
  
  
