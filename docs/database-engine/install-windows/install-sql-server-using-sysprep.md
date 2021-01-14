---
title: Installare SQL Server mediante SysPrep | Microsoft Docs
description: Questo articolo descrive come preparare e completare immagini usando SysPrep durante l'installazione di SQL Server.
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 11f4ed8a-aaa9-417b-bdd5-204f551c6bb6
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: bcf581a18af1f8f3024cd94d712359396f64bbfd
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975446"
---
# <a name="install-sql-server-with-sysprep"></a>Installare SQL Server con SysPrep

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Le azioni di installazione correlate a SysPrep sono accessibili tramite Centro installazione. Nella pagina **Avanzate** di **Centro installazione** sono disponibili due opzioni: **Preparazione immagine di un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** e **Completamento immagine di un'istanza autonoma predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Nelle sezioni di [preparazione](#prepare) e [completamento](#complete) viene descritto in modo dettagliato il processo di installazione. Per altre informazioni, vedere [Considerazioni sull'installazione di SQL Server tramite SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
È possibile inoltre preparare e completare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il prompt dei comandi o un file di configurazione. Per altre informazioni, vedere:  
  
- [Installare SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
- [Installare SQL Server tramite un file di configurazione](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], rivedere gli articoli in [Pianificazione di un'installazione di SQL Server](../../sql-server/install/planning-a-sql-server-installation.md). 
  
Per altre informazioni sulle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e sui requisiti hardware e software, vedere [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 
    
##  <a name="ssnoversion-sysprep-cluster-support"></a><a name="sysprep"></a> Supporto dei cluster di SysPrep in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], SysPrep supporta le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cluster nelle installazioni dalla riga di comando. Per altre informazioni, vedere [Che cos'è SysPrep](https://msdn.microsoft.com/library/cc721940\(v=WS.10\).aspx). 
  
### <a name="to-prepare-a-ssnoversion-failover-cluster-unattended"></a>Per preparare un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (esecuzione automatica)  
  
1. Preparare l'immagine come descritto in [Considerazioni sull'installazione di SQL Server tramite SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)e acquisire l'immagine Windows tramite SysPrep Generalization. Nell'esempio seguente viene preparata l'immagine:  
  
    ```  
    Setup.exe /q /ACTION=PrepareImage l /FEATURES=SQLEngine /InstanceID =<MYINST> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Eseguire quindi Windows SysPrep Generalization. 
  
2. Distribuire l'immagine eseguendo Windows SysPrep Specialize. 
  
3. Creare il cluster di failover di Windows. 
  
4. Eseguire setup.exe con **/ACTION=PrepareFailoverCluster** su tutti i nodi. Ad esempio:  
  
    ```  
    setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
### <a name="complete-a-ssnoversion-failover-cluster-unattended"></a>Completare un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (esecuzione automatica)  
  
1. Eseguire setup.exe con **/ACTION=CompleteFailoverCluster** sul nodo a cui appartiene il gruppo di archiviazione disponibile:  
  
    ```  
    setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName=<InstanceName>  /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>:" /FAILOVERCLUSTERNETWORKNAME="<Insert FOI Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\UserName>"  
    ```  
  
### <a name="adding-a-node-to-an-existing-ssnoversion-failover-cluster-unattended"></a>Aggiunta di un nodo a un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esistente (esecuzione automatica)  
  
1. Distribuire l'immagine eseguendo Windows SysPrep Specialize. 
  
2. Aggiungere il cluster di failover di Windows. 
  
3. Eseguire setup.exe con **/ACTION=AddNode** su tutti i nodi:  
  
    ```  
    setup.exe /q /ACTION=AddNode /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
##  <a name="prepare-image"></a><a name="prepare"></a> Preparare l'immagine  
  
### <a name="prepare-a-stand-alone-instance-of-ssnoversion"></a>Preparare un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
1. Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic sul file Setup.exe. Per eseguire l'installazione da una condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe. 
  
2. L'Installazione guidata esegue Centro installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per preparare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fare clic su **Preparazione immagine di un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nella pagina **Avanzate**. 
  
3. Controllo configurazione sistema consente di eseguire un'operazione di individuazione nel computer. Per continuare, fare clic su **OK**. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
4. Nella pagina Aggiornamenti prodotto vengono visualizzati gli aggiornamenti più recenti sul prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non si vogliono includere gli aggiornamenti, deselezionare la casella di controllo **Includi aggiornamenti prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Se non viene individuato alcun aggiornamento del prodotto, durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa pagina non viene visualizzata e viene aperta automaticamente la pagina **Installazione dei file di installazione** . 
  
5. Nella pagina Installa i file di installazione viene mostrato lo stato di avanzamento del download, dell'estrazione e dell'installazione dei file di installazione. Se viene individuato un aggiornamento per il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e ne viene specificata l'inclusione, verrà installato anche questo aggiornamento. 
  
6. Controllo configurazione sistema verifica lo stato del sistema del computer prima che l'installazione continui. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
7. Nella pagina **Prepara tipo di immagine** selezionare **Prepara una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . 
  
     La pagina **Prepara tipo di immagine** viene visualizzata solo quando si ha un'istanza predisposta non configurata esistente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer. È possibile scegliere di preparare una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o di aggiungere funzionalità supportate da SysPrep a un'istanza predisposta esistente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer. Per altre informazioni su come aggiungere funzionalità a un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Aggiungere funzionalità a un'istanza predisposta](#AddFeatures). 
  
8. Nella pagina **Condizioni di licenza** leggere il contratto di licenza, quindi selezionare la casella di controllo per accettarne le condizioni. Per migliorare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è inoltre possibile abilitare l'opzione relativa all'utilizzo delle funzionalità e inviare report a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 
  
9. Nella pagina **Selezione funzionalità** selezionare i componenti per l'installazione:  
  
    |Installazione|Componenti|  
    |-|-|  
    |[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] SysPrep|[!INCLUDE[ssDE](../../includes/ssde-md.md)]<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replica<br /><br /> Funzionalità full-text<br /><br /> Data Quality Services<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità nativa<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Funzionalità ridistribuibili<br /><br /> Funzionalità condivise|  
  
     Quando si evidenzia il nome della funzionalità desiderata, nel riquadro a destra verrà visualizzata una descrizione per ogni gruppo di componenti. È possibile selezionare qualsiasi combinazione di caselle di controllo. Per altre informazioni, vedere [Funzionalità supportate dalle edizioni di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) e [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)
  
     I prerequisiti per le funzionalità selezionate vengono visualizzati nel riquadro di destra. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno installati i prerequisiti che non sono stati ancora installati durante la procedura di installazione descritta più avanti in questo argomento. 
  
10. Nella pagina **Regole di preparazione immagine** Controllo configurazione sistema consente di verificare lo stato del sistema del computer prima che l'installazione continui. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
11. Nella pagina Configurazione dell'istanza specificare l'ID istanza per l'istanza. Fare clic su **Avanti** per continuare. 
  
     **ID istanza**: ID istanza usato per identificare le directory di installazione e le chiavi del Registro di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si tratta del caso delle istanze predefinite e delle istanze denominate. Se durante il passaggio per il completamento l'istanza predisposta viene completata come istanza predefinita, il nome dell'istanza viene sovrascritto come MSSQLSERVER. L'ID istanza rimane lo stesso di quello specificato. 
  
     **Directory radice istanza**: per impostazione predefinita, la directory radice dell'istanza è [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]. Per specificare una directory radice non predefinita, usare il campo disponibile o fare clic su **Sfoglia** per individuare una cartella di installazione. La directory specificata nel passaggio relativo alla preparazione sarà utilizzata durante la configurazione nel passaggio per il completamento. 
  
     Tutti i Service Pack e gli aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno applicati a ogni componente di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     **Istanze installate**: nella griglia vengono visualizzate le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presenti nel computer in cui viene eseguito il programma di installazione. 
  
12. Nella pagina **Requisiti di spazio su disco** viene calcolato lo spazio su disco necessario per le funzionalità specificate. Tale spazio viene quindi confrontato con lo spazio su disco disponibile. 
  
13. Controllo configurazione sistema eseguirà regole di preparazione immagine per convalidare la configurazione del computer con le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificate. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
14. Nella pagina **Inizio preparazione immagine** viene illustrata una visualizzazione albero delle opzioni di installazione specificate durante l'installazione. In questa pagina, tramite il programma di installazione vengono indicati l'eventuale abilitazione o disabilitazione della funzionalità di aggiornamento del prodotto e la versione dell'aggiornamento finale. Per continuare fare clic su **Prepara**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno innanzitutto installati i prerequisiti obbligatori per le funzionalità selezionate e, successivamente, le funzionalità stesse. 
  
15. Durante l'installazione, nella pagina **Avanzamento preparazione immagine** è possibile monitorare lo stato di avanzamento del processo. 
  
16. Al termine dell'installazione nella pagina **Operazione completata** viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti. Per completare il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , fare clic su **Chiudi**. 
  
17. Se viene richiesto, riavviare il computer. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
18. A questo punto, la procedura di preparazione è stata completata. È possibile completare l'immagine o distribuire l'immagine preparata come descritto in [Considerazioni sull'installazione di SQL Server tramite SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
##  <a name="complete-image"></a><a name="complete"></a> Completare l'immagine  
  
### <a name="complete-a-prepared-instance-of-ssnoversion"></a>Completare un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Se si dispone di un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclusa nell'immagine del computer, verrà visualizzato un collegamento nel menu Start. È anche possibile avviare Centro installazione e fare clic su **Completamento immagine di un'istanza autonoma predisposta** nella pagina **Avanzate** . 
  
2. Controllo configurazione sistema consente di eseguire un'operazione di individuazione nel computer. Per continuare, fare clic su **OK**. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
3. Nella pagina **File di supporto per l'installazione** fare clic su **Installa** per installare i file specifici. 
  
4. Controllo configurazione sistema verifica lo stato del sistema del computer prima che l'installazione continui. Al termine della verifica, fare clic su **Avanti** per continuare. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
5. Nella pagina **Product Key** selezionare un pulsante di opzione per indicare se si intende installare una versione gratuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o una versione di produzione del prodotto con una chiave PID. Per altre informazioni, vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md). Se si installa Evaluation Edition il periodo di prova di 180 giorni inizia al termine di questo passaggio. 
  
6. Nella pagina **Condizioni di licenza** leggere il contratto di licenza, quindi selezionare la casella di controllo per accettarne le condizioni. Per migliorare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è inoltre possibile abilitare l'opzione relativa all'utilizzo delle funzionalità e inviare report a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 
  
7. Nella pagina **Seleziona istanza predisposta** selezionare l'istanza predisposta che si vuole completare dalla casella di riepilogo a discesa. Selezionare l'istanza non configurata nell'elenco **ID istanza** . 
  
     **Istanze installate:** la griglia mostra tutte le istanze, incluse tutte le istanze preparate nel computer. 
  
8. Nella pagina **Esame funzionalità** verranno visualizzate le funzionalità selezionate e i componenti inclusi nell'istallazione durante il passaggio relativo alla preparazione. Se si vogliono aggiungere più funzionalità all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza includerle nell'istanza predisposta, è prima necessario completare questo passaggio per completare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , quindi aggiungere le funzionalità da **Aggiungi funzionalità** in **Centro installazione**. 
  
    > [!NOTE]  
    >  È possibile aggiungere funzionalità disponibili per la versione del prodotto che si sta installando. Per altre informazioni, vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
9. Nella pagina Configurazione dell'istanza specificare il nome dell'istanza per l'istanza predisposta. Questo nome corrisponde a quello dell'istanza una volta che è stata completata la configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Fare clic su **Avanti** per continuare. 
  
     **ID istanza**: ID istanza usato per identificare le directory di installazione e le chiavi del Registro di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si tratta del caso delle istanze predefinite e delle istanze denominate. Se durante il passaggio per il completamento l'istanza predisposta viene completata come istanza predefinita, il nome dell'istanza viene sovrascritto come MSSQLSERVER. L'ID istanza rimane lo stesso di quello specificato durante il passaggio relativo alla preparazione. 
  
     **Directory radice istanza**: verrà usata la directory specificata nel passaggio della preparazione, che non potrà essere modificata in questo passaggio. 
  
     Tutti i Service Pack e gli aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno applicati a ogni componente di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     **Istanze installate** : nella griglia vengono visualizzate le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presenti nel computer in cui viene eseguito il programma di installazione. 
  
10. Il flusso di lavoro relativo alla parte restante di questo articolo dipende dalle funzionalità selezionate durante il passaggio relativo alla preparazione. Le pagine visualizzate dipendono dalle selezioni effettuate. 
  
11. Nella pagina **Configurazione server** - Account di servizio specificare gli account di accesso per i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I servizi effettivamente configurati in questa pagina dipendono dalle funzionalità selezionate per l'installazione. 
  
     È possibile assegnare lo stesso account di accesso a tutti i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure configurare singolarmente l'account di ogni servizio. È inoltre possibile specificare se i servizi verranno avviati automaticamente, manualmente o se sono disabilitati. [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di configurare gli account del servizio singolarmente per assegnare i privilegi minimi a ogni servizio, in modo che ai servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengano concesse le autorizzazioni minime necessarie per completare le attività. Per altre informazioni, vedere [Configurazione Server - Account di servizio](./install-sql-server.md) e [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). 
  
     Per specificare lo stesso account di accesso per tutti gli account del servizio in questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], immettere le credenziali nei campi visualizzati nella parte inferiore della pagina. 
  
     **Nota sulla sicurezza** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Dopo aver specificato le informazioni di accesso per i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , fare clic su **Avanti**. 
  
12. Usare la scheda **Configurazione server - Regole di confronto** per specificare regole di confronto non predefinite per il [!INCLUDE[ssDE](../../includes/ssde-md.md)] e [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Per altre informazioni, vedere [Configurazione del server - Regole di confronto](./install-sql-server.md). 
  
13. Utilizzare la pagina Configurazione [!INCLUDE[ssDE](../../includes/ssde-md.md)] - Provisioning account per specificare gli elementi seguenti:  
  
    - Modalità di sicurezza: selezionare l'autenticazione di Windows o l'autenticazione mista per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si seleziona l'autenticazione Modalità mista, è necessario specificare una password complessa per l'account amministratore di sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] predefinito. 
  
         Quando viene stabilita la connessione tra un dispositivo e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il meccanismo di sicurezza è lo stesso sia in modalità mista che di autenticazione di Windows. Per altre informazioni, vedere [Configurazione del motore di database - Configurazione del server](./install-sql-server.md). 
  
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Amministratori: è necessario specificare almeno un amministratore di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per aggiungere l'account usato per eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , fare clic su **Aggiungi utente corrente**. Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, fare clic su **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Configurazione del motore di database - Configurazione del server](./install-sql-server.md). 
  
     Dopo aver modificato l'elenco, fare clic su **OK**. Verificare l'elenco di amministratori nella finestra di dialogo di configurazione. Quando l'elenco è completo, fare clic su **Avanti**. 
  
14. Utilizzare la pagina Configurazione di [!INCLUDE[ssDE](../../includes/ssde-md.md)] - Directory dati per specificare directory di installazione non predefinite. Per eseguire l'installazione in directory predefinite, fare clic su **Avanti**. 
  
    > [!IMPORTANT]  
    >  Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     Per altre informazioni, vedere [Configurazione del motore di database - Directory dati](./install-sql-server.md). 
  
15. Utilizzare la pagina Configurazione [!INCLUDE[ssDE](../../includes/ssde-md.md)] - FILESTREAM per abilitare la funzione FILESTREAM per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Configurazione del Motore di database - Filestream](./install-sql-server.md). 
  
16. Utilizzare la pagina Configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per specificare il tipo di installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da creare. Per altre informazioni sulle modalità di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Opzioni di configurazione di Reporting Services &#40;SSRS&#41;](./install-sql-server.md). 
  
17. Nella pagina **Segnalazione errori** specificare le informazioni da inviare a [!INCLUDE[msCoName](../../includes/msconame-md.md)] per contribuire a migliorare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per impostazione predefinita, l'opzione per la segnalazione di errori è abilitata. 
  
18. Nella pagina **Regole di completamento immagine** Controllo configurazione sistema consente di eseguire le regole di completamento immagine per convalidare la configurazione del computer con le configurazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificate. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
19. Nella pagina **Inizio completamento immagine** viene illustrata una visualizzazione albero delle opzioni di installazione specificate durante l'installazione. Per continuare, fare clic su **Installa**. 
  
20. Durante l'installazione, nella pagina **Avanzamento completamento immagine** è possibile monitorare lo stato di avanzamento del processo. 
  
21. Al termine dell'installazione nella pagina **Operazione completata** viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti. Per completare il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , fare clic su **Chiudi**. 
  
22. Se viene richiesto, riavviare il computer. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
23. Questo passaggio consente di completare la configurazione dell'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di completare l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
##  <a name="add-features-to-a-prepared-instance"></a><a name="AddFeatures"></a> Add Features to a Prepared Instance  
  
### <a name="add-features-to-a-prepared-instance-of-ssnoversion"></a>Aggiungere funzionalità a un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic sul file Setup.exe. Per eseguire l'installazione da una condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe. 
  
2. L'Installazione guidata esegue Centro installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per aggiungere funzionalità a un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fare clic su **Preparazione immagine di un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nella pagina **Avanzate**. 
  
3. Controllo configurazione sistema consente di eseguire un'operazione di individuazione nel computer. Per continuare, fare clic su **OK**. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
4. Nella pagina File di supporto per l'installazione fare clic su **Installa** per installare i file specifici. 
  
5. Nella pagina **Prepara tipo di immagine** selezionare l'opzione **Aggiungi funzionalità a un'istanza predisposta esistente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Selezionare l'istanza predisposta specifica a cui si desidera aggiungere funzionalità dall'elenco a discesa delle istanze predisposte disponibili. 
  
6. Nella pagina **Selezione funzionalità** specificare le funzionalità che si vogliono aggiungere all'istanza predisposta specificata. 
  
     I prerequisiti per le funzionalità selezionate vengono visualizzati nel riquadro di destra. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno installati i prerequisiti che non sono stati ancora installati durante la procedura di installazione descritta più avanti in questo argomento. 
  
7. Nella pagina **Regole di preparazione immagine** Controllo configurazione sistema consente di verificare lo stato del sistema del computer prima che l'installazione continui. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
8. Nella pagina Requisiti di spazio su disco viene calcolato lo spazio su disco necessario per le funzionalità specificate. Tale spazio viene quindi confrontato con lo spazio su disco disponibile. 
  
9. Nella pagina **Regole di preparazione immagine** Controllo configurazione sistema consente di eseguire le regole di preparazione immagine per convalidare la configurazione del computer rispetto alle funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificate. È possibile fare clic su **Mostra dettagli** per visualizzare i dettagli sullo schermo oppure su **Visualizza report dettagliato** per visualizzarlo come report HTML. 
  
10. Nella pagina **Inizio preparazione immagine** viene illustrata una visualizzazione albero delle opzioni di installazione specificate durante l'installazione. Per continuare, fare clic su **Installa**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno innanzitutto installati i prerequisiti obbligatori per le funzionalità selezionate e, successivamente, le funzionalità stesse. 
  
11. Durante l'installazione, nella pagina **Avanzamento preparazione immagine** è possibile monitorare lo stato di avanzamento del processo. 
  
12. Al termine dell'installazione nella pagina **Operazione completata** viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti. Per completare il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , fare clic su **Chiudi**. 
  
13. Se viene richiesto, riavviare il computer. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
##  <a name="remove-features-from-a-prepare-instance"></a><a name="RemoveFeatures"></a> Rimuovere funzionalità da un'istanza predisposta  
  
### <a name="removing-features-from-a-prepared-instance-of-ssnoversion"></a>Rimozione di funzionalità da un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Per avviare il processo di disinstallazione, dal menu **Start** scegliere **Pannello di controllo** e fare doppio clic su **Programmi e funzionalità**. 
  
2. Fare doppio clic sul componente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da disinstallare, quindi fare clic su **Rimuovi**. 
  
3. Verranno eseguite le regole di supporto dell'installazione per verificare la configurazione del computer. Per continuare scegliere **OK** . 
  
4. Nella pagina **Seleziona istanza** selezionare l'istanza predisposta che si vuole modificare. Il nome dell'istanza predisposta verrà visualizzato come "PreparedInstanceID non configurata" dove PreparedInstanceID è l'istanza selezionata. 
  
5. Nella pagina **Seleziona funzionalità** specificare le funzionalità da rimuovere dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificata. Fare clic su **Avanti** per continuare. 
  
6. Verranno eseguite le regole di rimozione per verificare la corretta esecuzione dell'operazione. 
  
7. Nella pagina **Inizio rimozione** esaminare l'elenco dei componenti e delle funzionalità che verranno disinstallate. 
  
8. Nella pagina **Stato rimozione** verrà visualizzato lo stato dell'operazione. 
  
9. Nella pagina **Operazione completata** è possibile esaminare lo stato di completamento dell'operazione. Per uscire dall'installazione guidata, fare clic su **Chiudi** . 
  
##  <a name="uninstalling-a-prepared-instance"></a><a name="Uninstall"></a> Disinstallazione di un'istanza predisposta  
  
### <a name="uninstall-a-prepared-instance-of-ssnoversion"></a>Disinstallare un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Per avviare il processo di disinstallazione, dal menu **Start** scegliere **Pannello di controllo** e fare doppio clic su **Programmi e funzionalità**. 
  
2. Fare doppio clic sul componente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da disinstallare, quindi fare clic su **Rimuovi**. 
  
3. Verranno eseguite le regole di supporto dell'installazione per verificare la configurazione del computer. Per continuare scegliere **OK** . 
  
4. Nella pagina **Seleziona istanza** selezionare l'istanza predisposta che si vuole modificare. Il nome dell'istanza predisposta verrà visualizzato come "PreparedInstanceID non configurata" dove PreparedInstanceID è l'istanza selezionata. 
  
5. Nella pagina **Seleziona funzionalità** specificare le funzionalità da rimuovere dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificata. Fare clic su **Avanti** per continuare. 
  
6. Nella pagina **Regole di rimozione** verranno eseguite le regole per verificare la corretta esecuzione dell'operazione. 
  
7. Nella pagina **Inizio rimozione** esaminare l'elenco dei componenti e delle funzionalità che verranno disinstallate. 
  
8. Nella pagina **Stato rimozione** verrà visualizzato lo stato dell'operazione. 
  
9. Nella pagina **Operazione completata** è possibile esaminare lo stato di completamento dell'operazione. Per uscire dall'installazione guidata, fare clic su **Chiudi** . 
  
10. Ripetere i passaggi da 1 a 9 fino a rimuovere tutti i componenti di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . 
  
##  <a name="modifying-or-uninstalling-a-completed-instance-of-ssnoversion"></a><a name="bk_Modifying_Uninstalling"></a> Modifica o disinstallazione di un'istanza completata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
 Il processo per l'aggiunta o la rimozione di funzionalità o per la disinstallazione di un'istanza completata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è simile al processo per un'istanza installata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere gli articoli seguenti:  
  
- [Aggiungere funzionalità a un'istanza di SQL Server &#40;programma di installazione&#41;](./add-features-to-an-instance-of-sql-server-setup.md)  
  
- [Disinstallare un'istanza esistente di SQL Server &#40;Setup&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni su Windows SysPrep](/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10))   
 [Funzionamento di Windows SysPrepWork](/previous-versions/windows/it-pro/windows-vista/cc766514(v=ws.10))  
  
