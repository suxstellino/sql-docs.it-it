---
title: Esecuzione di un DiffGram tramite ADO (SQLXML)
description: Informazioni su come eseguire un file DiffGram in un'applicazione Visual Basic Microsoft utilizzando ADO (SQLXML 4,0) per stabilire una connessione a un'istanza di Microsoft SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- providers [SQLXML], SQLOLEDB Provider
- ADO [SQLXML]
- SQLXMLOLEDB Provider, DiffGrams
- data providers [SQLXML], SQLOLEDB Provider
- DiffGrams [SQLXML], ADO
ms.assetid: 741fce82-de83-4923-86eb-30acb5b9a5e6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a28fe03f8e52d58f0e31c2e7d3cabebfd8a6113c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431499"
---
# <a name="executing-a-diffgram-by-using-ado-sqlxml-40"></a>Esecuzione di un DiffGram mediante ADO (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Questa applicazione [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic utilizza ADO per stabilire una connessione a un'istanza di Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], quindi esegue un DiffGram. In questa applicazione il DiffGram e lo schema XSD vengono archiviati in un file. L'applicazione carica il DiffGram dal file specificato. È possibile utilizzare qualsiasi DiffGram (e lo schema XSD associato) descritto in esempi di [DiffGram](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
 Il processo per l'applicazione di esempio è il seguente:  
  
-   Oggetto **conn** (**ADODB. Connessione**) stabilisce una connessione a un'istanza in esecuzione di SQL Server in un server specifico.  
  
-   L'oggetto **cmd** (**ADODB. Command**) viene eseguito sulla connessione stabilita.  
  
-   Il sottolinguaggio del comando viene impostato su DBGUID_MSSQLXML.  
  
-   Il DiffGram viene copiato nel flusso di comandi (**strmIn**) da un file.  
  
-   Il flusso di output del comando è impostato sull'oggetto **StrmOut** (**ADODB. Stream**) per ricevere i dati restituiti.  
  
-   Quando si utilizza il provider SQLOLEDB, per impostazione predefinita si otterrà la funzionalità Microsoft SQLXML fornita da Sqlxmlx.dll. Per utilizzare Sqlxml4.dll con il provider SQLOLEDB, è necessario impostare la proprietà della **versione SQLXML** su **SQLXML. 4.0** nell'oggetto **connessione** del provider SQLOLEDB.  
  
-   Il comando (DiffGram) viene eseguito.  
  
 Di seguito viene riportato il codice dell'applicazione di esempio:  
  
> [!NOTE]  
>  Nel codice è necessario specificare il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nella stringa di connessione.  
  
```  
Private Sub Command1_Click()  
  Dim cmd As New ADODB.Command  
  Dim conn As New ADODB.Connection  
  Dim strmOut As New ADODB.Stream  
  Dim strmIn As New ADODB.Stream  
  
  'Open a connection to SQL Server.  
  conn.Provider = "SQLOLEDB"  
  conn.Open "server=SqlServerName; database=tempdb; Integrated Security=SSPI; "  
  conn.Properties("SQLXML Version") = "SQLXML.4.0"  
  Set cmd.ActiveConnection = conn  
  strmIn.Open  
  strmIn.Charset = "UTF-8"  
  strmIn.LoadFromFile "C:\SomeFilePath\SampleDiffGram.xml"  
  strmIn.Position = 0  
  Set cmd.CommandStream = strmIn  
  
  strmOut.Open  
  cmd.Properties("Output Stream").Value = strmOut  
  cmd.Properties("Output Encoding").Value = "UTF-8"  
  
  cmd.Dialect = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  cmd.Properties("Mapping Schema") = "C:\SomeFilePath\SampleDiffGram.xml"  
  cmd.Execute , , adExecuteStream  
  strmOut.Position = 0  
  Set cmd = Nothing  
  strmOut.Charset = "UTF-8"  
  strmOut.SaveToFile "C:\DropIt.txt", adSaveCreateOverWrite  
  strmOut.Close  
  Set strmOut = Nothing  
  
End Sub  
```  
  
### <a name="to-test-the-diffgram"></a>Per testare DiffGram  
  
1.  In una cartella del computer, copiare uno degli DiffGram e lo schema XSD corrispondente da uno degli esempi negli [esempi di DiffGram](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
2.  Aprire Visual Basic e creare un progetto Standard Exe.  
  
3.  Aggiungere i riferimenti seguenti al progetto:  
  
    ```  
    Microsoft ActiveX Data Objects 2.8 Library  
    ```  
  
4.  Nella casella degli strumenti fare clic su **CommandButton**, quindi creare un pulsante nel form.  
  
5.  Fare doppio clic sul pulsante per modificare il codice e aggiungere il codice dell'applicazione fornito nell'argomento.  
  
6.  Modificare il codice per specificare i nomi dei file DiffGram e XSD. Modificare anche la stringa di connessione in modo appropriato.  
  
7.  Eseguire l'applicazione. Il risultato dell'esecuzione dipende dal DiffGram che si esegue.  

