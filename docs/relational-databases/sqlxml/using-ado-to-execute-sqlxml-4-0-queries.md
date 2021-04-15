---
title: Utilizzo di ADO per eseguire query SQLXML 4.0
description: Informazioni su come eseguire query SQLXML 4.0 in un'applicazione basata su COM usando estensioni SQLXML per ActiveX Data Objects (ADO).
ms.custom: ''
ms.date: 12/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- query testers [SQLXML]
- test scripts
- ADO [SQLXML]
- queries [SQLXML], ADO
- SQLXML, ADO
ms.assetid: 3d54e3bb-7c5f-427e-82f8-1403a54c4f53
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4af0a1c4aa0c942092149dbb398c46da895ef7b7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490392"
---
# <a name="using-ado-to-execute-sqlxml-40-queries"></a>Utilizzo di ADO per eseguire query SQLXML 4.0
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Nelle versioni precedenti di SQLXML viene fornito il supporto per l'esecuzione di query basate su HTTP mediante le directory virtuali IIS e il filtro ISAPI SQLXML. In SQLXML 4.0 questi componenti sono stati rimossi in quanto simili e, a partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], è stata fornita una funzionalità sostitutiva con i servizi Web XML nativi.  
  
 In alternativa, è possibile eseguire query e utilizzare SQLXML 4.0 con le applicazioni basate su COM sfruttando le estensioni SQLXML di ActiveX Data Objects (ADO), introdotte per la prima volta in Microsoft Data Access Components (MDAC) 2.6 e versioni successive.  
  
 Questo argomento illustra l'uso di SQLXML e ADO come parte di un'applicazione Visual Basic Scripting Edition (VBScript) (uno script con estensione vbs). Vengono illustrate le procedure di configurazione iniziale per consentire di ricreare e testare gli esempi di query forniti nella documentazione di SQLXML 4.0.  
  
## <a name="creating-the-sqlxml-40-test-script"></a>Creazione dello script di test di SQLXML 4.0  
 In questa procedura viene creato un file VBScript con estensione vbs, ovvero Sqlxml4test.vbs, che può essere utilizzato per eseguire query SQLXML sfruttando le estensioni ADO di SQLXML in ADO 2.6 e versioni successive.  
  
#### <a name="to-create-the-sqlxml-40-query-tester-using-ado-vbscript"></a>Per creare il file di test della query SQLXML 4.0 utilizzando ADO (VBScript).  
  
1.  Copiare il codice seguente e incollarlo in un file di testo: Salvare il file come Sqlxml4test.vbs.  
  
    ```  
    WScript.Echo "Query process may take a few seconds to complete. Please be patient."  
  
    ' Note that for SQL Server Native Client to be used as the data provider,  
    ' it needs to be installed on the client computer first. Also, SQLXML extensions   
    ' for ADO are used and available in MDAC 2.6 or later.  
  
    'Set script variables.  
    inputFile = "@@FILE_NAME@@"  
    strServer = "@@SERVER_NAME@@"  
    strDatabase = "@@DATABASE_NAME@@"  
    dbGuid = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  
    ' Establish ADO connection to SQL Server and   
    ' create an instance of the ADO Command object.  
    Set conn = CreateObject("ADODB.Connection")  
    Set cmd = CreateObject("ADODB.Command")  
    conn.Open "Provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Server=" & strServer & _  
              ";Database=" & strDatabase & ";Integrated Security=SSPI"  
    Set cmd.ActiveConnection = conn  
  
    ' Create the input stream as an instance of the ADO Stream object.  
    Set inStream = CreateObject("ADODB.Stream")  
    inStream.Open  
    inStream.Charset = "utf-8"  
    inStream.LoadFromFile inputFile  
  
    ' Set ADO Command instance to use input stream.  
    Set cmd.CommandStream = inStream  
  
    ' Set the command dialect.  
    cmd.Dialect = dbGuid  
  
    ' Set a second ADO Stream instance for use as a results stream.   
    Set outStream = CreateObject("ADODB.Stream")  
    outStream.Open  
  
    ' Set dynamic properties used by the SQLXML ADO command instance.   
    cmd.Properties("XML Root").Value = "ROOT"  
    cmd.Properties("Output Encoding").Value = "UTF-8"  
  
    ' Connect the results stream to the command instance and execute the command.  
    cmd.Properties("Output Stream").Value = outStream  
    cmd.Execute , , 1024  
  
    ' Echo cropped/partial results to console.  
    WScript.Echo Left(outStream.ReadText, 1023)  
  
    inStream.Close  
    outStream.Close  
    ```  
  
2.  Aggiornare i valori dello script seguenti per l'esempio che si sta tentando di testare e l'ambiente di prova.  
  
    -   Trovare "`@@FILE_NAME@@`" e sostituirlo con il nome del file modello.  
  
    -   Trovare "`@@SERVER_NAME@@`" e sostituirlo con il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ad esempio, "`(local)`" se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione in locale).  
  
    -   Trovare "`@@DATABASE_NAME@@`" e sostituirlo con il nome del database (ad esempio "`AdventureWorks2012`" o "`tempdb`").  
  
     Aggiornare gli altri valori se menzionato nelle istruzioni specifiche per l'esempio che si sta tentando di ricreare in locale nel computer.  
  
3.  Salvare il file e chiuderlo.  
  
4.  Verificare la creazione di eventuali file aggiuntivi, ad esempio schemi o modelli XML che fanno parte dell'esempio che si sta tentando di ricreare in locale nel computer. Questi file devono trovarsi nella stessa directory in cui è stato salvato il file dello script di test (Sqlxml4test.vbs).  
  
5.  Seguire le istruzioni riportate nella sezione successiva per informazioni sull'utilizzo dello script di test SQLXML 4.0.  

## <a name="using-the-sqlxml-40-test-script"></a>Utilizzo dello script di test di SQLXML 4.0  
 Nella procedura seguente viene illustrato come utilizzare i file Sqlxml4test.vbs per testare le query di esempio fornite in questa documentazione.  
  
#### <a name="to-use-the-sqlxml-40-query-tester"></a>Per utilizzare il file di test della query SQLXML 4.0  
  
1.  Verificare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sia installato, come segue:  
  
    1.  Dal menu **Start** scegliere Impostazioni **e** quindi fare clic su **Pannello di controllo**.  
  
    2.  In Pannello di controllo aprire **Installazione applicazioni**  
  
    3.  Nell'elenco dei programmi attualmente installati verificare che **Microsoft SQL Server Native Client sia** visualizzato nell'elenco.  
  
        > [!NOTE]  
        >  Se è necessario installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, vedere [Installazione di SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md).  
  
2.  Verificare che la versione di MDAC installata per il computer client sia 2.6 o successiva. Se è necessario verificare le informazioni sulla versione di MDAC, è possibile usare lo strumento mdac Component Checker, disponibile come download gratuito dal sito Web Microsoft, [http://www.microsoft.com](https://www.microsoft.com) . Per ulteriori informazioni, cercare "MDAC Component Checker" sul sito Web Microsoft.  
  
3.  Eseguire lo script.  
  
     È possibile eseguire il file VBScript alla riga di comando utilizzando Cscript.exe o facendo doppio clic sul file Sqlxml4test.vbs per richiamare Windows Script Host (WScript.exe).  
  
     Quando viene eseguito, lo script visualizza un messaggio per avvisare che l'esecuzione potrebbe richiedere tempo prima che vengano restituiti e visualizzati i risultati della query come output dello script. Una volta visualizzato l'output, confrontarne il contenuto con i risultati previsti per l'esempio.  
  
  
