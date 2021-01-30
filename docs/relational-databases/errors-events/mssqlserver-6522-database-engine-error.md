---
description: MSSQLSERVER_6522
title: MSSQLSERVER_6522
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 6522 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 99c0a18330de144cc2a20ead150b1c0c5b7a3fc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199019"
---
# <a name="mssqlserver_6522"></a>MSSQLSERVER_6522
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|6522|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|SQLCLR_UDF_EXEC_FAILED|
|Testo del messaggio|Errore di .NET Framework durante l'esecuzione dell'aggregazione o routine definita dall'utente "%.*ls": %ls.|
||

## <a name="explanation"></a>Spiegazione

Esaminare gli scenari seguenti:

### <a name="scenario-1"></a>Scenario 1

Si crea una routine Common Language Runtime (CLR) che fa riferimento a un assembly Microsoft .NET Framework. L'assembly .NET Framework non è documentato in [922672](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). Successivamente si installa .NET Framework 3.5 o un hotfix basato su .NET Framework 2.0.

### <a name="scenario-2"></a>Scenario 2

Si crea un assembly e quindi si registra l'assembly in un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Successivamente si installa una versione diversa dell'assembly nella GAC (Global Assembly Cache).

Quando si esegue la routine CLR o si usa l'assembly da uno di questi scenari in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], viene visualizzato un messaggio di errore simile al seguente:

> Server:  Messaggio 6522, livello 16, stato 2, riga 1  
Errore di .NET Framework durante l'esecuzione dell'aggregazione o routine definita dall'utente ‘getsid’:
>
> System.IO.FileLoadException: Impossibile caricare il file o l'assembly 'System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' o una delle relative dipendenze. L'assembly nell'archivio host ha una firma diversa dall'assembly nella GAC. (Eccezione da HRESULT: 0x80131050)

## <a name="possible-cause"></a>Possibile causa

Quando CLR carica un assembly, verifica che lo stesso assembly si trovi nella GAC. Se nella GAC è presente lo stesso assembly, CLR verifica che gli ID della versione del modulo (MVID) di questi assembly corrispondano. Se i MVID degli assembly non corrispondono, viene visualizzato il messaggio di errore menzionato nella sezione [Spiegazione](#explanation).

Quando un assembly viene ricompilato, il MVID dell'assembly cambia. Di conseguenza, se si aggiorna .NET Framework, gli assembly .NET Framework hanno MVID diversi perché tali assembly vengono ricompilati. Inoltre, se si aggiorna un assembly personalizzato, l'assembly viene ricompilato. Per questo motivo l'assembly ha anche un MVID diverso.

## <a name="user-action"></a>Azione utente

### <a name="action-1"></a>Azione 1

Per trovare un soluzione per lo scenario 1 nella sezione [Spiegazione](#explanation), è necessario aggiornare manualmente gli assembly .NET Framework in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per eseguire questa operazione, usare l'istruzione `ALTER ASSEMBLY` per puntare alla nuova versione dell'assembly .NET Framework nella cartella seguente:

`%Windir%\Microsoft.NET\Framework\Version`

> [!NOTE]
> **Version** rappresenta la versione di .NET Framework installata o aggiornata.

### <a name="action-2"></a>Azione 2

Per trovare una soluzione per lo scenario 2 nella sezione [Spiegazione](#explanation), usare l'istruzione `ALTER ASSEMBLY` per aggiornare l'assembly nel database.

Se il problema persiste dopo aver eseguito questa operazione, eliminare l'assembly dal database, quindi registrare la nuova versione dell'assembly nel database.

## <a name="more-information"></a>Ulteriori informazioni

Non è consigliabile usare assembly .NET Framework che non sono documentati nei [criteri di supporto per gli assembly .NET Framework non testati nell'ambiente ospitato nell’oggetto CLR di SQL Server](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). Sono elencati gli assembly testati nell'ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ospitato in CLR.

### <a name="description-of-clr-routines"></a>Descrizione delle routine CLR

Le routine CLR includono gli oggetti seguenti, implementati usando l'integrazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il supporto CLR di .NET Framework:

- Funzioni definite dall'utente con valori scalari
- Funzioni definite dall'utente con valori di tabella
- Procedure definite dall'utente
- Trigger definiti dall'utente
- Tipi di dati definiti dall'utente
- Funzioni di aggregazione definite dall'utente

### <a name="assemblies-to-update-after-you-install-the-net-framework-35"></a>Assembly da aggiornare dopo l'installazione di .NET Framework 3.5

Dopo aver installato .NET Framework 3.5 è necessario usare l'istruzione ALTER ASSEMBLY per aggiornare gli assembly seguenti:

- Accessibility.dll
- AspNetMMCExt.dll
- Cscompmgd.dll
- IEExecRemote.dll
- IEHost.dll
- IIEHost.dll
- Microsoft.Build.Conversion.dll
- Microsoft.Build.Engine.dll
- Microsoft.Build.Framework.dll
- Microsoft.Build.Tasks.dll 
- Microsoft.Build.Utilities.dll 
- Microsoft.CompactFramework.Build.Tasks.dll 
- Microsoft.JScript.dll 
- Microsoft.VisualBasic.Vsa.dll 
- Microsoft.Vsa.dll 
- Microsoft.Vsa.Vb.CodeDOMProcessor.dll 
- Microsoft_VsaVb.dll 
- Sysglobl.dll 
- System.Configuration.Install.dll 
- System.Design.dll 
- System.DirectoryServices.dll 
- System.DirectoryServices.Protocols.dll 
- System.Drawing.dll 
- System.Drawing.Design.dll 
- System.EnterpriseServices.dll 
- System.Management.dll 
- System.Messaging.dll 
- System.Runtime.Serialization.Formatters.Soap.dll 
- System.ServiceProcess.dll 
- System.Web.dll 
- System.Web.Mobile.dll 
- System.Web.RegularExpressions.dll 

Questi assembly si trovano nella cartella seguente:

`%Windir%\Microsoft.NET\Framework\v2.0.50727`

### <a name="how-to-preserve-the-data-from-user-defined-data-types-after-you-drop-an-assembly"></a>Come mantenere i dati da tipi di dati definiti dall'utente dopo l'eliminazione di un assembly

Se si elimina un assembly usato da un tipo di dati definito dall'utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile usare uno dei metodi seguenti per conservare i dati.

Si supponga che lo scenario sia il seguente:

- Si crea un assembly il cui nome è *MyAssembly.dll*.
- L'assembly MyAssembly fa riferimento all'assembly `System.DirectoryServices.dll`.
- Si ha un tipo di dati definito dall'utente il cui nome è *MyDateTime*.
- Il tipo di dati *MyDateTime* usa l'assembly *MyAssembly.dll*.
- Si crea una tabella denominata *MyTable*.
- La tabella *MyTable* contiene i dati del tipo di dati *MyDateTime*.

#### <a name="method-1-use-the-bcpexe-utility"></a>Metodo 1: Usare l'utilità Bcp.exe

1. Usare l'utilità Bcp.exe insieme all'opzione -n per copiare i dati dalla tabella MyTable in un file. Ad esempio, eseguire questo comando a un prompt dei comandi:

    ```console
    bcp MyDatabase.dbo.MyTable out C:\MyFile.bcp -n -SSQLServerName -T
    ```

2. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio seguire questa procedura:
   1. Eliminare la tabella MyTable.
   2. Eliminare il tipo di dati MyDateTime.
   3. Eliminare l’assembly `System.DirectoryServices.dll`.
   4. Eliminare l'assembly MyAssembly.
3. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio seguire questa procedura:

   1. Registrare l'assembly `System.DirectoryServices.dll`.
   2. Registrare l'assembly MyAssembly.
   3. Creare il tipo di dati MyDateTime.
   4. Creare una nuova tabella con la stessa struttura della tabella MyTable.
4. Usare l'utilità Bcp.exe insieme all'opzione -n per importare i dati dal file nella tabella MyTable. Ad esempio, eseguire questo comando a un prompt dei comandi:
  
    ```console
    bcp MyDatabase.dbo.MyTable in C:\MyFile.bcp -n -SSQLServerName -T
    ```

#### <a name="method-2-use-the-insert--select-statement"></a>Metodo 2: Usare INSERT ... Istruzione SELECT

Si supponga che il tipo di dati MyDateTime occupi 9 byte nella risorsa di archiviazione.

1. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio creare una nuova tabella contenente una colonna del tipo di dati `VARBINARY(9)` eseguendo l'istruzione seguente:

    ```sql
    CREATE TABLE TempTable (c1 VARBINARY(9));
    ```

2. Eseguire la seguente istruzione INSERT ... SELECT per popolare la tabella TempTable:

    ```sql
    INSERT INTO TempTable SELECT CAST(c1 as VARBINARY(9)) FROM MyTable;
    ```

3. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio seguire questa procedura:
   1. Eliminare la tabella MyTable.
   2. Eliminare il tipo di dati MyDateTime.
   3. Eliminare l’assembly System.DirectoryServices.dll.
   4. Eliminare l'assembly MyAssembly.
4. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio seguire questa procedura:
   1. Registrare l’assembly System.DirectoryServices.dll.
   2. Registrare l'assembly MyAssembly.
   3. Creare il tipo di dati MyDateTime.
   4. Creare una nuova tabella con la stessa struttura della tabella MyTable.
5. Eseguire la seguente istruzione INSERT ... SELECT per popolare la tabella MyTable:

    ```sql
    INSERT INTO MyTable SELECT c1 FROM TempTable;
    ```

## <a name="references"></a>Riferimenti

- Per altre informazioni sulla versione dell'assembly, vedere la [documentazione ritirata di Visual Studio 2005](https://www.microsoft.com/download/details.aspx?id=55984).
- Per altre informazioni su come aggiornare un assembly, vedere [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md).
- Per altre informazioni su come eliminare un assembly, vedere [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md).
- Per altre informazioni su come registrare un assembly in un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md).
- Per altre informazioni sull'utilità Bcp.exe, vedere [https://msdn2.microsoft.com/library/ms162802.aspx](../../tools/bcp-utility.md).