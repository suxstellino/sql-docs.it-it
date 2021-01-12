---
title: Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server
description: Informazioni su come avviare, arrestare, sospendere, riprendere o riavviare diversi servizi di SQL Server. Scoprire come usare Transact-SQL, PowerShell e altri strumenti per queste azioni.
ms.custom: ''
ms.date: 03/05/2020
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 697cf043c4582796015b061c857145c214d105fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091770"
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo argomento descrive come avviare, arrestare, sospendere, riprendere o riavviare il motore di database di SQL Server, SQL Server Agent o il servizio SQL Server Browser usando Gestione configurazione SQL Server, SQL Server Management Studio (SSMS), i comandi net da un prompt dei comandi, Transact-SQL o PowerShell.

## <a name="identify-the-service"></a>Identificare il servizio

I componenti di SQL Server sono programmi eseguibili che vengono eseguiti come servizi Windows. I programmi che vengono eseguiti come servizi Windows rimangono in esecuzione anche se sullo schermo del computer non viene rilevata alcuna attività.

### <a name="database-engine-service"></a>Servizio del motore di database

Il processo eseguibile corrispondente al motore di database di SQL Server. Il motore di database può essere l'istanza predefinita (una per computer) o una delle molte istanze denominate del motore di database. Usare Gestione configurazione SQL Server per determinare quali istanze del motore di database sono installate nel computer. L'istanza predefinita (se installata) è indicata come **SQL Server (MSSQLSERVER)** . Le istanze denominate (se installate) sono indicate come **SQL Server (<nome_istanza>)** . Per impostazione predefinita, SQL Server Express è installato come **SQL Server (SQLEXPRESS)** .

### <a name="sql-server-agent-service"></a>servizio SQL Server Agent

Servizio di Windows che esegue attività amministrative pianificate, ovvero processi e avvisi. Per altre informazioni, vedere [SQL Server Agent](../../ssms/agent/sql-server-agent.md). SQL Server Agent non è disponibile in tutte le edizioni di SQL Server. Per un elenco delle funzionalità supportate dalle edizioni di SQL Server, vedere [Funzionalità supportate dalle edizioni di SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

### <a name="sql-server-browser-service"></a>Servizio SQL Server Browser

Servizio di Windows che rimane in ascolto delle richieste in arrivo per le risorse di SQL Server e fornisce informazioni client sulle istanze di SQL Server installate nel computer. Viene usata una singola istanza del servizio SQL Server Browser per tutte le istanza di SQL Server installate nel computer.

### <a name="additional-information"></a>Informazioni aggiuntive

- Sospendendo il servizio motore di database si impedisce a nuovi utenti di connettersi al motore di database, ma si consente a quelli già connessi di continuare a lavorare finché le connessioni non vengono interrotte. Sospendere il servizio quando si desidera attendere che gli utenti completino il loro lavoro prima di arrestare il servizio. In questo modo, gli utenti possono completare le transazioni in corso. *Riprendi consente* al motore di database di accettare nuove connessioni. Non è possibile sospendere né riprendere il servizio SQL Server Agent.  

- In Gestione configurazione SQL Server e SSMS viene visualizzato lo stato corrente dei servizi tramite le icone seguenti.  

    **Gestione configurazione SQL Server**

  - Una freccia verde sull'icona accanto al nome del servizio indica che il servizio è stato avviato.

  - Un quadrato rosso sull'icona accanto al nome del servizio indica che il servizio è stato arrestato.

  - Due linee blu verticali sull'icona accanto al nome del servizio indicano che il servizio è stato sospeso.

  - Quando il motore di database viene riavviato, un quadrato rosso indica che il servizio è stato arrestato, dopodiché una freccia verde indica che il servizio è stato avviato correttamente.

    **SQL Server Management Studio (SSMS)**

  - Una freccia bianca su un cerchio verde accanto al nome del servizio indica che il servizio è stato avviato.  

  - Un quadrato bianco su un cerchio rosso accanto al nome del servizio indica che il servizio è stato arrestato.  

  - Due linee bianche verticali su un cerchio blu accanto al nome del servizio indicano che il servizio è stato sospeso.  

- Quando si usa Gestione configurazione SQL Server o SSMS, sono disponibili solo le opzioni possibili. Ad esempio, se il servizio è già avviato, l'opzione **Avvia** non è disponibile.

- Se eseguito in un cluster, il servizio motore di database di SQL Server verrà gestito al meglio tramite Amministrazione cluster.  

### <a name="security"></a>Sicurezza

#### <a name="permissions"></a>Autorizzazioni

Per impostazione predefinita, solo i membri del gruppo di amministratori locale possono avviare, arrestare, sospendere, riprendere o riavviare un servizio. Per concedere a utenti non amministratori la possibilità di gestire servizi, vedere [Concedere agli utenti i privilegi per gestire i servizi in Windows Server 2003](https://support.microsoft.com/kb/325349). Il processo è analogo in altre versioni di Windows Server.

L'arresto del motore di database con il comando **SHUTDOWN** Transact-SQL richiede l'appartenenza ai ruoli predefiniti del server **sysadmin** o **serveradmin** e non è trasferibile.

## <a name="sql-server-configuration-manager"></a>Gestione configurazione SQL Server

### <a name="starting-sql-server-configuration-manager"></a>Avvio di Gestione configurazione SQL Server

Fare clic sul menu **Start** , scegliere **Tutti i programmi**, **Microsoft SQL Server**, **Strumenti di configurazione** e quindi fare clic su **Gestione configurazione SQL Server**.

Poiché Gestione configurazione SQL Server è uno snap-in per il programma Microsoft Management Console e non un programma autonomo, Gestione configurazione SQL Server non viene visualizzato come applicazione nelle versioni più recenti di Windows. Ecco i percorsi per le ultime quattro versioni, con Windows installato nell'unità C.  

|Versione|Path|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|SQL Server 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|SQL Server 2014|C:\Windows\SysWOW64\SQLServerManager12.msc|
|SQL Server 2012|C:\Windows\SysWOW64\SQLServerManager11.msc|

#### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="configmande"></a> Per avviare, arrestare, sospendere, riprendere o riavviare un'istanza del motore di database di SQL Server

1. Avviare Gestione configurazione SQL Server seguendo le istruzioni sopra riportate.

2. Se viene visualizzata la finestra di dialogo **Controllo account utente** fare clic su **Sì**.

3. Nel riquadro sinistro di Gestione configurazione SQL Server fare clic su **Servizi di SQL Server**.

4. Nel riquadro dei risultati fare clic con il pulsante destro del mouse su **SQL Server (MSSQLServer)** o su un'istanza denominata, quindi fare clic su **Avvia**, **Arresta**, **Sospendi**, **Riprendi** o **Riavvia**.

5. Fare clic su **OK** per chiudere lo strumento Gestione configurazione SQL Server.

> [!NOTE]
> Per avviare un'istanza del motore di database di SQL Server con le opzioni di avvio, vedere [Configurare le opzioni di avvio del server &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  

#### <a name="to-start-stop-pause-resume-or-restart-the-sql-server-browser-or-an-instance-of-the-sql-server-agent"></a>Per avviare, arrestare, sospendere, riprendere o riavviare SQL Server Browser o un'istanza di SQL Server Agent

1. Avviare Gestione configurazione SQL Server seguendo le istruzioni sopra riportate.

2. Se viene visualizzata la finestra di dialogo **Controllo account utente** fare clic su **Sì**.

3. Nel riquadro sinistro di Gestione configurazione SQL Server fare clic su **Servizi di SQL Server**.

4. Nel riquadro dei risultati fare clic con il pulsante destro del mouse su **SQL Server Browser**, **SQL Server Agent (MSSQLServer)** o **SQL Server Agent (<nome_istanza>)** per un'istanza denominata e quindi fare clic su **Avvia**, **Arresta**, **Sospendi**, **Riprendi** o **Riavvia**.  

5. Fare clic su **OK** per chiudere lo strumento Gestione configurazione SQL Server.  

> [!NOTE]
> Non è possibile sospendere SQL Server Agent.

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="ssmsde"></a> Per avviare, arrestare, sospendere, riprendere o riavviare un'istanza del motore di database di SQL Server

1. In Esplora oggetti connettersi all'istanza del motore di database, fare clic con il pulsante destro del mouse sull'istanza del motore di database da avviare e quindi scegliere **Avvia**, **Arresta**, **Sospendi**, **Riprendi** o **Riavvia**.

    Oppure, in Server registrati fare clic con il pulsante destro del mouse sull'istanza del motore di database da avviare, selezionare **Controllo servizi** e quindi fare clic su **Avvia**, **Arresta**, **Sospendi**, **Riprendi** o **Riavvia**.

2. Se viene visualizzata la finestra di dialogo **Controllo account utente** fare clic su **Sì**.

3. Quando viene richiesto se si vuole intervenire, fare clic su **Sì**.  

#### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-agent"></a>Per avviare, arrestare o riavviare un'istanza di SQL Server Agent

1. In Esplora oggetti connettersi all'istanza del motore di database, fare clic con il pulsante destro del mouse su **SQL Server Agent** e quindi fare clic su **Avvia**, **Arresta** o **Riavvia**.

2. Se viene visualizzata la finestra di dialogo **Controllo account utente** fare clic su **Sì**.

3. Quando viene richiesto se si vuole intervenire, fare clic su **Sì**.

## <a name="command-prompt-window-using-net-commands"></a><a name="CommandPrompt"></a> Uso dei comandi net dalla finestra del prompt dei comandi

I servizi di Microsoft SQL Server possono essere avviati, arrestati o sospesi usando i comandi **net** di Microsoft Windows.

### <a name="to-start-the-default-instance-of-the-database-engine"></a><a name="dbDefault"></a> Per avviare l'istanza predefinita del motore di database

- Al prompt dei comandi digitare uno dei comandi seguenti:  
  
    **net start "SQL Server (MSSQLSERVER)"**

   -oppure-  

    **net start MSSQLSERVER**

### <a name="to-start-a-named-instance-of-the-database-engine"></a><a name="dbNamed"></a> Per avviare un'istanza denominata del motore di database

- Al prompt dei comandi digitare uno dei comandi seguenti. Sostituire *\<instancename>* con il nome dell'istanza da gestire.  
  
    **net start "SQL Server (** *instancename* **)"**
  
   -oppure-  
  
    **net start MSSQL$** *instancename*  
  
### <a name="to-start-the-database-engine-with-startup-options"></a><a name="dbStartup"></a> Per avviare il motore di database con le opzioni di avvio  

- Aggiungere le opzioni di avvio alla fine dell'istruzione **net start "SQL Server (MSSQLSERVER)"** , separate da uno spazio. Quando vengono avviate con **net start**, le opzioni di avvio usano una barra (/) anziché un trattino (-).  
  
    **net start "SQL Server (MSSQLSERVER)" /f /m**
  
   -oppure-  
  
    **net start MSSQLSERVER /f /m**
  
  > [!NOTE]
  >  Per altre informazioni sulle opzioni di avvio, vedere [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
###  <a name="to-start-the-sql-server-agent-on-the-default-instance-of-sql-server"></a><a name="agDefault"></a> Per avviare SQL Server Agent nell'istanza predefinita di SQL Server
  
- Al prompt dei comandi digitare uno dei comandi seguenti:  
  
    **net start "SQL Server Agent (MSSQLSERVER)"**
  
   -oppure-  
  
    **net start SQLSERVERAGENT**
  
###  <a name="to-start-the-sql-server-agent-on-a-named-instance-of-sql-server"></a><a name="agNamed"></a> Per avviare SQL Server Agent in un'istanza denominata di SQL Server  
  
- Al prompt dei comandi digitare uno dei comandi seguenti. Sostituire *instancename* con il nome dell'istanza da gestire.  
  
    **net start "SQL Server Agent(** *instancename* **)"**
  
   -oppure-  
  
    **net start SQLAgent$** *instancename*  
  
 Per informazioni sull'esecuzione di SQL Server Agent in modalità dettagliata per la risoluzione dei problemi, vedere [Applicazione sqlagent90](../../tools/sqlagent90-application.md).  

### <a name="to-start-the-sql-server-browser"></a><a name="Browser"></a> Per avviare il servizio SQL Server Browser  

- Al prompt dei comandi digitare uno dei comandi seguenti:  
  
    **net start "SQL Server Browser"**
  
   -oppure-  
  
    **net start SQLBrowser**
  
### <a name="to-pause-or-stop-services-from-the-command-prompt-window"></a><a name="pauseStop"></a> Per sospendere o arrestare servizi dalla finestra del prompt dei comandi  

- Per sospendere o arrestare servizi, modificare i comandi nei modi seguenti.  

- Per sospendere un servizio, sostituire **net start** con **net pause**.  

- Per arrestare un servizio, sostituire **net start** con **net stop**.  

## <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL

È possibile arrestare il motore di database tramite l'istruzione **SHUTDOWN**.  

### <a name="to-stop-the-database-engine-using-transact-sql"></a>Per arrestare il motore di database usando Transact-SQL

- Per attendere il completamento delle stored procedure e delle istruzioni Transact-SQL attualmente in esecuzione e quindi arrestare il motore di database, eseguire l'istruzione seguente.  
  
    ```sql
    SHUTDOWN;
    ```
  
- Per arrestare immediatamente il motore di database, eseguire l'istruzione seguente.  
  
    ```sql
    SHUTDOWN WITH NOWAIT;
    ```

Per altre informazioni sull'istruzione **SHUTDOWN**, vedere [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md).  

## <a name="powershell"></a><a name="PowerShellProcedure"></a> PowerShell

### <a name="to-start-and-stop-database-engine-services"></a>Per avviare e arrestare i servizi del motore di database

1. In una finestra del prompt dei comandi avviare SQL Server PowerShell eseguendo il comando seguente.  

    ```cmd
    sqlps
    ```

2. Al prompt dei comandi di SQL Server PowerShell, eseguire il comando seguente. Sostituire `computername` con il nome del computer.  

    ```powershell
    # Get a reference to the ManagedComputer class.
    CD SQLSERVER:\SQL\computername
    $Wmi = (get-item .).ManagedComputer
    ```

3. Identificare il servizio che si desidera arrestare o avviare. Selezionare una delle righe seguenti. Sostituire `instancename` con il nome dell'istanza denominata.

    - Per ottenere un riferimento all'istanza predefinita del motore di database.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQLSERVER']
        ```

    - Per ottenere un riferimento a un'istanza denominata del motore di database.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQL$instancename']
        ```

    - Per ottenere un riferimento al servizio SQL Server Agent nell'istanza predefinita del motore di database.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']
        ```

    - Per ottenere un riferimento al servizio SQL Server Agent in un'istanza denominata del motore di database.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']
        ```

    - Per ottenere un riferimento al servizio SQL Server Browser.

        ```powershell
        $DfltInstance = $Wmi.Services['SQLBROWSER']
        ```

4. Completare l'esempio per avviare e quindi arrestare il servizio selezionato.
  
    ```powershell
    # Display the state of the service.
    $DfltInstance
    # Start the service.
    $DfltInstance.Start();
    # Wait until the service has time to start.
    # Refresh the cache.  
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    # Stop the service.
    $DfltInstance.Stop();
    # Wait until the service has time to stop.
    # Refresh the cache.
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    ```  
  
##  <a name="using-service-controller-class"></a><a name="ServiceController"></a> Uso della classe ServiceController

È possibile usare la classe ServiceController per controllare il servizio SQL Server o qualsiasi altro servizio Windows. Per un esempio di esecuzione di questa operazione, vedere [Classe ServiceController](/dotnet/api/system.serviceprocess.servicecontroller).

## <a name="manage-the-sql-server-service-on-linux"></a>Gestire il servizio SQL Server in Linux

### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-database-engine"></a>Per avviare, arrestare o riavviare un'istanza del motore di database di SQL Server

Le sezioni seguenti illustrano come avviare, arrestare, riavviare e controllare lo stato del [servizio SQL Server in Linux](../../linux/sql-server-linux-troubleshooting-guide.md#manage-the-sql-server-service).

Controllare lo stato del servizio SQL Server usando questo comando:

   ```bash
   sudo systemctl status mssql-server
   ```

È possibile arrestare, avviare o riavviare il servizio SQL Server in base alle esigenze usando i comandi seguenti:

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

## <a name="next-steps"></a>Passaggi successivi

- [Panoramica della documentazione del programma di installazione di SQL Server](../install-windows/install-sql-server.md)
- [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md)
- [Avvio di SQL Server con la configurazione minima](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)
- [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)