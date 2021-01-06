---
description: MSSQLSERVER_17207
title: MSSQLSERVER_17207
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17204 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: d95f6b04265550d758064fe277c21de6be862ec5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642442"
---
# <a name="mssqlserver_17207"></a>MSSQLSERVER_17207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |
| :-------- | :---- |
|Nome prodotto|SQL Server|  
|ID evento|17207|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBLKIO_OS2DISKERROR|  
|Testo del messaggio|%ls: Errore del sistema operativo %ls durante la creazione o l'apertura del file '%ls'. Individuare e correggere l'errore del sistema operativo, quindi riprovare.|  


## <a name="explanation"></a>Spiegazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è riuscito ad aprire il file specificato a causa dell'errore del sistema operativo indicato.  

È possibile che venga visualizzato l'errore 17207 nell'evento dell'applicazione Windows o nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non riesce ad aprire un file di database e/o un log delle transazioni. L'errore può avere un aspetto simile al seguente.

``` 
Error: 17207, Severity: 16, State: 1.
FileMgr::StartSecondaryDataFiles: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'F:\MSSQL\DATA\MyDB_FG1_1.ndf'. Diagnose and correct the operating system error, and retry the operation.
```

È possibile che questi errori vengano visualizzati durante il processo di avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o per qualsiasi operazione del database che prova ad avviare il database, ad esempio ALTER DATABASE. In alcuni scenari è possibile che vengano visualizzati sia errori 17207 che errori 17204, in altre occasioni è possibile che ne venga visualizzato solo uno.

Se questi errori si verificano in un database utente, il database viene lasciato nello stato RECOVERY_PENDING e le applicazioni non potranno accedere al database stesso. Se questi errori si verificano in un database di sistema, l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene avviata e non è possibile connettersi a questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di conseguenza una risorsa in un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può andare offline.

Se il problema è correlato al filegroup FileStream [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si noterà che viene elencato solo il percorso completo della directory, anziché un nome file. Ecco un esempio. 
```
Error: 17207, Severity: 16, State: 1.
STREAMFCB::Startup: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\bpa_files_test_fs_1\bpa_files_test_fs_1'. Diagnose and correct the operating system error, and retry the operation.
```

## <a name="cause"></a>Causa
Prima di poter usare qualsiasi database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è necessario avviare il database. Il processo di avvio del database comporta l'inizializzazione di varie strutture di dati che rappresentano il database e i file di database, l'apertura di tutti i file che appartengono al database e infine l'esecuzione del ripristino sul database stesso. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa la funzione dell'API Windows [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) per aprire i file che appartengono a un database.
 
I messaggi 17207 (e 17204) indicano che si è verificato un errore quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha provato ad aprire i file di database durante il processo di avvio.
 
Questi messaggi di errore includono le informazioni seguenti:
1. Nome della funzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che sta provando ad aprire il file. Il nome della funzione che viene visualizzato normalmente in questi messaggi di errore è uno dei seguenti:
   - FCB::Open: si è verificato un errore nel file mentre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provava ad aprirlo
   - FileMgr::StartPrimaryDataFiles: un file di dati primario o un file appartenente al gruppo di file primario
   - FileMgr::StartSecondaryDataFiles: un file appartenente a un gruppo di file secondario
   - FileMgr::StartLogFiles: un file registro transazioni
   - STREAMFCB::Startup: un contenitore SQL FileStream
   - FCB::RemoveAlternateStreams
  
      
1. Le informazioni sullo stato distinguono più posizioni all'interno di una funzione che possono generare questo messaggio di errore.
1. Il percorso fisico completo del file.
1. L'ID file corrispondente al file.
1. Il codice di errore del sistema operativo e la descrizione dell'errore. In alcuni casi viene visualizzato solo il codice di errore.
 
Le informazioni sull'errore del sistema operativo visualizzate in questi messaggi di errore indicano la causa principale all'origine dell'errore 17204. Sono cause comuni di questi messaggi di errore un problema di autorizzazione o un percorso del file errato.


## <a name="user-action"></a>Azione utente  
1. Per la risoluzione dell'errore 17207 è necessario comprendere il codice di errore del sistema operativo associato e diagnosticare l'errore corrispondente. Dopo aver risolto la condizione di errore del sistema operativo, è possibile provare a riavviare il database (ad esempio usando ALTER DATABASE SET ONLINE) o l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per portare online il database interessato. In alcuni casi, può non essere possibile risolvere l'errore del sistema operativo, pertanto sarà necessario intraprendere specifiche azioni correttive. Le azioni sono illustrate in questa sezione.
1. Se il messaggio di errore 17207 contiene solo un codice di errore e non una descrizione dell'errore, è possibile provare a risolvere il codice di errore usando il comando da una shell del sistema operativo: net helpmsg <error code>. Se si riceve come codice di errore un codice di stato a 8 cifre, vedere fonti di informazioni come [How do I convert an HRESULT to a Win32 error code?](https://devblogs.microsoft.com/oldnewthing/20061103-07/?p=29133) (Come convertire uno stato HRESULT in un codice di errore Win32?) per convertire questi codici di stato in errori del sistema operativo.
1. Se si riceve l'errore del sistema operativo ```Access is Denied``` = 5, prendere in considerazione questi metodi:
   -  Controllare le autorizzazioni impostate per il file esaminando le proprietà del file in Esplora risorse. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa i gruppi di Windows per effettuare il provisioning del controllo di accesso sulle varie risorse di file. Verificare che il gruppo appropriato (con nomi come SQLServerMSSQLUser$ComputerName$MSSQLSERVER o SQLServerMSSQLUser$ComputerName$InstanceName) abbia le autorizzazioni necessarie per il file di database indicato nel messaggio di errore. Per altre informazioni, vedere [Configurare le autorizzazioni del file system per l'accesso al motore di database](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true). Verificare che il gruppo di Windows includa l'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il SID del servizio.
   -  Esaminare l'account utente in cui è attualmente in esecuzione il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile ottenere queste informazioni tramite Gestione attività di Windows. Cercare il valore "User Name" (Nome utente) nel file eseguibile "sqlservr.exe". Se è stato modificato di recente l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tenere presente che il metodo supportato per questa operazione è l'uso dell'utilità Gestione configurazione SQL Server. Per altre informazioni su questo aspetto, vedere [Gestione configurazione SQL Server](../sql-server-configuration-manager.md). 
   -  A seconda del tipo di operazione (apertura dei database durante l'avvio del server, collegamento di un database, ripristino del database e così via), l'account usato per la rappresentazione e l'accesso al file di database possono variare. Vedere l'argomento [Sicurezza dei dati e dei file di log](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) per informazioni sulle interazioni tra operazioni, autorizzazioni e account. Usare uno strumento come [Process Monitor](/sysinternals/downloads/procmon) di Windows SysInternals per determinare se l'accesso ai file avviene nel contesto di sicurezza dell'account di avvio del servizio dell'istanza di SQL Server (o SID del servizio) o di un account rappresentato.

      Se SQL Server rappresenta le credenziali utente dell'accesso che esegue l'operazione ALTER DATABASE o CREATE DATABASE, si noteranno ad esempio le informazioni seguenti nello strumento Process Monitor.

        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
1. Se si riceve l'errore del sistema operativo `The system cannot find the file specified` = 3:
   - Esaminare il percorso completo indicato nel messaggio di errore.
   - Verificare che l'unità disco e il percorso della cartella siano visibili e accessibili da Esplora risorse.
   - Esaminare il registro eventi di Windows per verificare se esistono problemi con questa unità disco.
   - Se il percorso non è corretto e questo database esiste già nel sistema, è possibile modificare i percorsi dei file di database usando i metodi descritti nell'articolo relativo allo [spostamento dei file di database](../databases/move-database-files.md). Può essere necessario usare questa procedura, in particolare per i file di database di sistema in cui si verificano gli errori 17204 o 17207 e se si usa uno scenario di ripristino di emergenza in cui le unità disco specificate non sono disponibili. In questo argomento viene inoltre illustrato come identificare il percorso corrente dei diversi database di sistema [master, model, tempdb, msdb e mssqlsystemresource].
   - Se questo errore viene visualizzato perché mancano i file di database, è necessario ripristinare il database da un backup valido:
     - Se il file di database associato all'errore appartiene a un filegroup secondario, è possibile contrassegnare il filegroup come offline, portare online il database e quindi eseguire un ripristino solo di quel filegroup. Per altre informazioni, vedere la sezione OFFLINE dell'argomento [Opzioni per file e filegroup ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).
     - Se il file che ha generato l'errore è un file di log delle transazioni, esaminare le informazioni riportate nelle sezioni "FOR ATTACH" e "FOR ATTACH_REBUILD_LOG" dell'argomento [CREATE DATABASE (Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md) per capire in che modo ricreare i file di log delle transazioni mancanti.
   - Verificare che il disco o il percorso di rete, ad esempio l'unità iSCSI, sia disponibile prima che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenti di accedere ai file del database in questi percorsi. Se necessario, creare le dipendenze richieste in Amministrazione del cluster o Gestione controllo servizi.

1. Se si riceve l'errore del sistema operativo `The process cannot access the file because it is being used by another process` = 32:
   - Usare uno strumento come [Esplora processi](/sysinternals/downloads/process-explorer) o [Handle](/sysinternals/downloads/handle) da Windows Sysinternals per verificare se un altro processo o servizio ha acquisito un blocco esclusivo per questo file di database.
   - Impedire al processo di accedere ai file di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esempi comuni includono i programmi antivirus, vedere il materiale sussidiario per le esclusioni di file nell'[articolo KB](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server) che segue.
   - In un ambiente cluster assicurarsi che il processo sqlservr.exe del nodo proprietario precedente abbia effettivamente rilasciato gli handle per i file di database. Normalmente questa situazione non si verifica, ma le configurazioni errate del cluster o dei percorsi I/O possono causare problemi di questo tipo.
