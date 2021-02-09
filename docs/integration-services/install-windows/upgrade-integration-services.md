---
description: Aggiornare Integration Services
title: Aggiornare Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, upgrading
- SSIS, upgrading
- SQL Server Integration Services, upgrading
- upgrading Integration Services
ms.assetid: 04f9863c-ba0b-47c5-af91-f2d41b078a23
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: cd258e7680e2f70693ea6e6c36355c0beacbeca8
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835859"
---
# <a name="upgrade-integration-services"></a>Aggiornare Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Se nel computer è installato [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o una versione successiva, è possibile eseguire l'aggiornamento a [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)].  
  
 Se si esegue l'aggiornamento a [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] in un computer con una di queste versioni precedenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] verrà installato side-by-side con la versione precedente.  
  
 Con l'installazione side-by-side, vengono installate più versioni dell'utilità dtexec. Per accertarsi di eseguire la versione corretta dell'utilità, al prompt dei comandi eseguire l'utilità immettendo il percorso completo (\<drive>:\Programmi\Microsoft SQL Server\\<versione\>\DTS\Binn). Per ulteriori informazioni su dtexec, vedere [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
> [!NOTE]  
>  Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], per impostazione predefinita quando si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tutti gli utenti nel gruppo Utenti dispongono di accesso al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Quando si installa [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] e versioni successive, gli utenti non dispongono dell'accesso al [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] servizio. Il servizio è protetto per impostazione predefinita. Dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'amministratore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve eseguire lo strumento Configurazione DCOM (Dcomcnfg.exe) per concedere a utenti specifici l'accesso al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per altre informazioni, vedere [Servizio Integration Services &#40;servizio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
## <a name="before-upgrading-integration-services"></a>Operazioni preliminari per l'aggiornamento di Integration Services  
 Prima di procedere all'aggiornamento a [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], è consigliabile eseguire Preparazione aggiornamento. Preparazione aggiornamento segnala i problemi che potrebbero verificarsi se si esegue la migrazione dei pacchetti esistenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al nuovo formato dei pacchetti utilizzato da [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] .  
  
> [!NOTE]
>  Il supporto per la migrazione o l'esecuzione di pacchetti di Data Transformation Services (DTS) non è più disponibile in SQL Server 2012. Le seguenti funzionalità DTS non sono più utilizzate.  
> 
>  -   DTS Runtime  
> -   API DTS  
> -   Migrazione guidata pacchetti per la migrazione dei pacchetti DTS alla versione successiva di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
> -   Supporto per la gestione di pacchetti DTS in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
> -   Attività Esegui pacchetto DTS 2000  
> -   Analisi di pacchetti DTS in Preparazione aggiornamento.  
> 
>  Per informazioni sulle altre funzionalità sospese, vedere [Funzionalità di Integration Services non più supportate in SQL Server 2016](/previous-versions/sql/sql-server-2016/bb500429(v=sql.130)).  
  
## <a name="upgrading-integration-services"></a>aggiornamento di Integration Services  
 È possibile eseguire l'aggiornamento utilizzando uno dei metodi seguenti:  
  
-   Eseguire il programma di installazione di [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] e selezionare l'opzione **Aggiorna da SQL Server 2008, SQL Server 2008 R2 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
-   Eseguire **setup.exe** al prompt dei comandi e specificare l'opzione **/ACTION=upgrade** . Per altre informazioni, vedere la sezione "Script di installazione per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]" in [Installazione di SQL Server 2016 dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
 Non è possibile utilizzare l'aggiornamento per effettuare le azioni seguenti:  
  
-   Riconfigurare un'installazione esistente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   Passare da una versione a 32 bit a una versione a 64 bit di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o da una versione a 64 bit a una versione a 32 bit.  
  
-   Passare da una versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un'altra versione localizzata.  
  
 È possibile scegliere di eseguire l'aggiornamento sia di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che del [!INCLUDE[ssDE](../../includes/ssde-md.md)], solo del [!INCLUDE[ssDE](../../includes/ssde-md.md)]o solo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se si esegue l'aggiornamento solo del [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o una versione successiva continuerà a essere funzionante ma la funzionalità di [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)]non sarà disponibile. Se si esegue l'aggiornamento solo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] sarà completamente funzionante ma sarà possibile solo archiviare i pacchetti nel file system, a meno che in un altro computer non sia disponibile un'istanza del [!INCLUDE[ssDECurrent](../../includes/ssdecurrent-md.md)] .  
  
## <a name="upgrading-both-integration-services-and-the-database-engine-to-sssql19"></a>Aggiornamento di Integration Services e del Motore di database a [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]  
 In questa sezione vengono descritti gli effetti di un aggiornamento che utilizza i criteri seguenti:  
  
-   Si aggiornano a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sia [!INCLUDE[ssDE](../../includes/ssde-md.md)] sia un'istanza del [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)].  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] risiedono nello stesso computer.  
  
### <a name="what-the-upgrade-process-does"></a>Operazioni eseguite durante l'aggiornamento  
 Durante il processo di aggiornamento vengono eseguite le attività seguenti:  
  
-   Installazione dei file, del servizio e degli strumenti di [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] ([!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]). Se sono presenti più istanze di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] nello stesso computer, al primo aggiornamento delle istanze a [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]verranno installati i file, il servizio e gli strumenti di [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] .  
  
-   Aggiornamento dell'istanza di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] alla versione [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] .  
  
-   Spostamento dei dati dalle tabelle di sistema di [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o versioni successive alle tabelle di sistema di [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , come indicato di seguito:  
  
    -   Spostamento dei pacchetti senza modifiche dalla tabella di sistema msdb.dbo.sysdtspackages90 alla tabella di sistema msdb.dbo.sysssispackages.  
  
        > [!NOTE]  
        >  Benché i dati vengano spostati in una tabella di sistema diversa, il processo di aggiornamento non esegue la migrazione dei pacchetti al nuovo formato.  
  
    -   Spostamento dei metadati delle cartelle dalla tabella di sistema msdb.sysdtsfolders90 alla tabella di sistema msdb.sysssisfolders.  
  
    -   Spostamento dei dati del log dalla tabella di sistema msdb.sysdtslog90 alla tabella di sistema msdb.sysssislog.  
  
-   Rimozione delle tabelle di sistema msdb.sysdts*90 e delle stored procedure usate per accedervi in seguito allo spostamento dei dati nelle nuove tabelle msdb.sysssis\* . Tuttavia, durante l'aggiornamento la tabella sysdtslog90 viene sostituita con una vista denominata anche sysdtslog90. Questa nuova vista sysdtslog90 espone la nuova tabella di sistema msdb.sysssislog. In questo modo, l'esecuzione dei report basati sulla tabella di log continuerà senza interruzione.  
  
-   Creazione di tre nuovi ruoli predefiniti a livello di database, db_ssisadmin, db_ssisltduser e db_ssisoperator, per il controllo dell'accesso ai pacchetti. I ruoli db_dtsadmin, db_dtsltduser e db_dtsoperator di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] non vengono rimossi ma vengono inseriti come membri dei nuovi ruoli corrispondenti.  
  
-   Se l'archivio pacchetti di [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ovvero la posizione del file system gestita dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , corrisponde al percorso predefinito **\SQL Server\90**, **\SQL Server\100**, **\SQL Server\110** o **\SQL Server\120** , viene eseguito lo spostamento di tali pacchetti nel nuovo percorso predefinito **\SQL Server\130**.  
  
-   Aggiornamento del file di configurazione del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in modo da puntare all'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
### <a name="what-the-upgrade-process-does-not-do"></a>Operazioni non eseguite durante l'aggiornamento  
 Durante il processo di aggiornamento non vengono eseguite le attività seguenti:  
  
-   **Rimozione** del servizio [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o versioni successive.  
  
-   Migrazione dei pacchetti esistenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al nuovo formato dei pacchetti utilizzato da [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] . Per informazioni su come eseguire la migrazione dei pacchetti, vedere [Aggiornare pacchetti di Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
-   Spostamento dei pacchetti da percorsi del file system diversi dal percorso predefinito aggiunti al file di configurazione del servizio. Se in precedenza il file di configurazione del servizio è stato modificato per aggiungervi altre cartelle del file system, i pacchetti archiviati in tali cartelle non verranno spostati nel nuovo percorso.  
  
-   Aggiornamento del percorso del file system per l'utilità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dtexec **(dtexec.exe) nei passaggi del processo di** che chiamano direttamente l'utilità **dtexec** . È necessario modificare manualmente questi passaggi del processo per aggiornare il percorso del file system specificando il percorso di [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] per l'utilità **dtexec** .  
  
### <a name="what-you-can-do-after-upgrading"></a>Operazioni possibili in seguito all'aggiornamento  
 Al termine del processo di aggiornamento, è possibile effettuare le attività seguenti:  
  
-   Eseguire processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per l'esecuzione di pacchetti.  
  
-   Usare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] per gestire i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] archiviati in un'istanza di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. È necessario modificare il file di configurazione del servizio per aggiungere l'istanza di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] all'elenco dei percorsi gestiti dal servizio.  
  
    > [!NOTE]  
    >  Nelle versioni precedenti di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] non è possibile eseguire la connessione al servizio [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] .  
  
-   Identificare la versione dei pacchetti nella tabella di sistema msdb.dbo.sysssispackages controllando il valore nella colonna packageformat. La colonna packageformat della tabella identifica la versione di ogni pacchetto. Il valore 3 indica un pacchetto [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] . Fino a quando non si esegue la migrazione dei pacchetti al nuovo formato, il valore nella colonna packageformat non cambia.  
  
-   Non è possibile usare gli strumenti [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] per progettare, eseguire o gestire pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Gli strumenti di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] includono le rispettive versioni di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], l'Importazione/Esportazione guidata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'Utilità di esecuzione pacchetti (dtexecui.exe). Il processo di aggiornamento non comporta la rimozione degli strumenti di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Non sarà tuttavia possibile usare questi strumenti per continuare a lavorare con i pacchetti di [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o versioni successive in un server aggiornato.  
  
-   Per impostazione predefinita, in un'installazione dell'aggiornamento, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene configurato in modo da registrare gli eventi correlati all'esecuzione dei pacchetti nel registro eventi delle applicazioni. Questa impostazione potrebbe generare un numero eccesivo di voci nel registro eventi quando si utilizza la funzionalità di raccolta dati di [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]. Gli eventi registrati includono EventID 12288, che indica che il pacchetto è stato avviato ed EventID 12289 che indica che il pacchetto è stato completato. Per arrestare la registrazione di questi due eventi nel registro eventi dell'applicazione, aprire il Registro di sistema per la modifica. Nel Registro di sistema individuare il nodo HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS e modificare il valore DWORD dell'impostazione LogPackageExecutionToEventLog da 1 a 0.  
  
## <a name="upgrading-only-the-database-engine-to-sssql19"></a>Aggiornamento solo del Motore di database a [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]  
 In questa sezione vengono descritti gli effetti di un aggiornamento che utilizza i criteri seguenti:  
  
-   Si aggiorna solo un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Di conseguenza, l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] ora è un'istanza di [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], mentre l'istanza di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti client sono di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
-   L'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] risiede in un computer, mentre [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti client risiedono in un altro computer.  
  
### <a name="what-you-can-do-after-upgrading"></a>Operazioni possibili in seguito all'aggiornamento  
 Le tabelle di sistema in cui sono archiviati i pacchetti nell'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)] non sono le stesse di quelle utilizzate in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. Le versioni [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] , pertanto, non sono in grado di individuare i pacchetti nelle tabelle di sistema dell'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Poiché tali pacchetti non possono essere individuati, vi sono alcune limitazioni relative alle operazioni che è possibile eseguire:  
  
-   Non è possibile utilizzare gli strumenti di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], in altri computer per caricare o gestire i pacchetti dall'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
    > [!NOTE]  
    >  Anche se non è stata ancora eseguita la migrazione al nuovo formato dei pacchetti nell'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , tali pacchetti non possono essere individuati dagli strumenti di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . I pacchetti, pertanto, non possono essere utilizzati dagli strumenti di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .  
  
-   Non è possibile utilizzare [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] in altri computer per eseguire i pacchetti archiviati in msdb nell'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Non è possibile usare i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in computer con [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] per eseguire pacchetti di [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] archiviati nell'istanza aggiornata del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="external-resources"></a>Risorse esterne  
 Intervento nel blog relativo all' [utilizzo delle applicazioni e delle estensioni SSIS personalizzate esistenti in Denali](https://techcommunity.microsoft.com/t5/sql-server-integration-services/making-your-existing-custom-ssis-extensions-and-applications/ba-p/387951)sul sito blogs.msdn.com.  
  
