---
title: Installare tramite l'interfaccia utente grafica
description: Questo articolo offre istruzioni dettagliate per installare una nuova istanza di SQL Server usando l'Installazione guidata di SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, steps
- Setup [SQL Server], steps
- SQL Server, installing
ms.assetid: 6ad23de1-2bab-4933-9122-c09f5565028d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bb4b3b0e70850702c34ff1c69b9c2fcd5f3c85b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342318"
---
# <a name="install-sql-server-from-the-installation-wizard-setup"></a>Installare SQL Server dall'Installazione guidata (programma di installazione)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

In questo articolo viene descritto come installare SQL Server con l'Installazione guidata. Si applica a [!INCLUDE[SQLServer2016](../../includes/sssql16-md.md)] e a [!INCLUDE[SQLServer2017](../../includes/sssql17-md.md)].

Questo articolo fornisce istruzioni dettagliate per installare una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando l'Installazione guidata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Poiché nell'Installazione guidata è disponibile un unico albero delle funzionalità per l'installazione di tutti i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], non è necessario installarli singolarmente. Per installare i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] singolarmente, vedere [Installare SQL Server](../../database-engine/install-windows/install-sql-server.md#individual-component-installation).  

Per altri modi per installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere:  

* [Installare SQL Server al prompt dei comandi](./install-sql-server-from-the-command-prompt.md)  
* [Installare SQL Server tramite un file di configurazione](./install-sql-server-using-a-configuration-file.md)  
* [Installare SQL Server tramite SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)  
* [Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
* [Aggiornare SQL Server usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

## <a name="get-the-installation-media"></a>Ottenere il supporto di installazione

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]
  
## <a name="prerequisites"></a>Prerequisiti

Prima di installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Pianificazione di un'installazione di SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).  
  
> [!NOTE]  
> Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura ed esecuzione relative a tale condivisione.  

::: moniker range=">=sql-server-2016 <=sql-server-2017"

###  <a name="install-patch-requirement"></a><a name="bkmk_ga_instalpatch"></a> Requisito di installazione patch

Microsoft ha identificato un problema con i file binari di Microsoft Visual C++ 2013 Runtime che vengono installati come prerequisito da SQL Server 2016 e 2017. È disponibile un aggiornamento per risolvere questo problema. Se l'aggiornamento dei file binari di Visual C++ Runtime non viene installato, potrebbero verificarsi problemi di stabilità di SQL Server in determinati scenari. Prima di installare SQL Server, seguire le istruzioni in [Note sulla versione di SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) per vedere se il computer richiede una patch per i file binari di Visual C++ Runtime. 

Ciò non è applicabile a [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)].

## <a name="to-install-sql-server-2016-and-2017"></a>Per installare SQL Server 2016 e 2017  

1. Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic sul file **Setup.exe**. Per eseguire l'installazione da una condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file **Setup.exe**.  
  
1. L'Installazione guidata esegue Centro installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per creare una nuova installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], selezionare **Installazione** nell'area di navigazione sinistra, quindi selezionare **Nuova installazione autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aggiunta di funzionalità a un'installazione esistente**.  

1. Nella pagina relativa al **codice Product Key** selezionare un'opzione per indicare se si intende installare un'edizione gratuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o una versione di produzione con una chiave PID. Per altre informazioni, vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
   Per continuare, selezionare **Avanti**.

1. Nella pagina **Condizioni di licenza** leggere il contratto di licenza. Per accettare, selezionare la casella di controllo **Accetto le condizioni di licenza** e quindi selezionare **Avanti**.  
    
   > [!NOTE]
   > SQL Server trasmette le informazioni sull'esperienza di installazione, nonché altri dati sull'utilizzo e le prestazioni per aiutare Microsoft a migliorare il prodotto. Per altre informazioni sull'elaborazione dei dati e i controlli sulla privacy di SQL Server, vedere l'[informativa sulla privacy](https://privacy.microsoft.com/privacystatement) e [Configurare SQL Server per inviare commenti e suggerimenti a Microsoft](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md).

1. Nella pagina **Regole globali** la procedura di installazione passerà automaticamente alla pagina **Aggiornamenti prodotti** se non vi sono errori delle regole.  
  
1. La pagina di **Microsoft Update** verrà visualizzata successivamente se la casella di controllo **Microsoft Update** non è selezionata in **Pannello di controllo** > **Tutti gli elementi del Pannello di controllo** > **Windows Update** > **Modifica impostazioni**. Se si seleziona la casella di controllo **Microsoft Update**, le impostazioni del computer vengono modificate per includere gli aggiornamenti per tutti i prodotti Microsoft quando si effettua l'analisi degli aggiornamenti di Windows.  

1. Nella pagina **Aggiornamenti prodotto** vengono visualizzati gli aggiornamenti più recenti sul prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non viene individuato alcun aggiornamento del prodotto, durante l'installazione questa pagina non viene visualizzata e viene aperta automaticamente la pagina **Installazione dei file di installazione**.  

1. Nella pagina **Installazione dei file di installazione** viene mostrato lo stato del download, dell'estrazione e dell'installazione dei file di installazione. Se viene individuato un aggiornamento per il programma di installazione e si specifica di includerlo, verrà installato anche questo aggiornamento. Se non viene trovato alcun aggiornamento, il programma di installazione proseguirà automaticamente.
  
1. Nella pagina **Regole di installazione** il programma di installazione esegue un controllo dei potenziali problemi che potrebbero verificarsi durante l'esecuzione dell'installazione. Se si verificano errori, selezionare un elemento nella colonna **Stato** per altre informazioni. In caso contrario, selezionare **Avanti**.

1. Se questa è la prima installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer, il programma di installazione salta la pagina **Tipo di installazione** e passa direttamente alla pagina **Selezione funzionalità**. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è già installato nel sistema, è possibile usare la pagina **Tipo di installazione** per scegliere di eseguire una nuova installazione o aggiungere funzionalità a un'installazione esistente. Per continuare, selezionare **Avanti**.
  
1. Nella pagina **Selezione funzionalità** selezionare i componenti per l'installazione. Ad esempio, per installare una nuova istanza del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di [!INCLUDE[ssDE](../../includes/ssde-md.md)], selezionare **Servizi motore di database**.

    Una volta selezionato il nome della funzionalità desiderata, nel riquadro **Descrizione funzionalità** viene visualizzata una descrizione per ogni gruppo di componenti. È possibile selezionare qualsiasi combinazione di caselle di controllo. Per altre informazioni, vedere [Edizioni e componenti di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) o [Edizioni e componenti di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
  
     I prerequisiti per le funzionalità selezionate vengono visualizzati nel riquadro **Prerequisiti per le funzionalità selezionate** . Verranno installati i prerequisiti che non sono stati già installati durante la procedura di installazione descritta più avanti in questo argomento.  
  
     È inoltre possibile specificare una directory personalizzata per i componenti condivisi usando il campo nella parte inferiore della pagina **Selezione funzionalità**. Per modificare il percorso di installazione per i componenti condivisi, aggiornare il percorso nel campo visualizzato nella parte inferiore della finestra di dialogo oppure selezionare **Sfoglia** per passare a una directory di installazione. Il percorso di installazione predefinito è [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     > [!NOTE]
     > Il percorso specificato per i componenti condivisi deve essere un percorso assoluto. La cartella non deve essere compressa o crittografata. Le unità di cui è stato eseguito il mapping non sono supportate.  
  
     SQL Server usa due directory per le funzionalità condivise:
  
     * Directory funzionalità condivise  
     * Directory funzionalità condivise (x86)  
  
     > [!NOTE]
     > Il percorso specificato per ogni opzione descritta in precedenza deve essere diverso.  
  
1. Se tutte le regole vengono soddisfatte, si procede automaticamente dalla pagina **Regole funzionalità**.  
  
1. Nella pagina **Configurazione dell'istanza** specificare se installare un'istanza predefinita o un'istanza denominata. Per altre informazioni, vedere [Configurazione delle istanze](../../sql-server/install/instance-configuration.md#instance-configuration-page).  
  
     * **ID istanza**: per impostazione predefinita, come ID istanza viene usato il nome dell'istanza. Tale ID viene usato per identificare le directory di installazione e le chiavi del Registro di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lo stesso comportamento si verifica per le istanze predefinite e le istanze denominate. Per un'istanza predefinita, il nome dell'istanza e l'ID istanza sono MSSQLSERVER. Per usare un ID istanza non predefinito, specificare un valore differente nella casella di testo **ID istanza**.  
  
       > [!NOTE]  
       > Le normali istanze autonome di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], che si tratti di istanze predefinite o denominate, non usano un valore che non sia predefinito per l'ID istanza.  
  
       Tutti i Service Pack e gli aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno applicati a ogni componente di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     * **Istanze installate**: nella griglia vengono visualizzate le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presenti nel computer in cui viene eseguito il programma di installazione. Se nel computer è già installata un'istanza predefinita, è necessario installare un'istanza denominata di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     Il flusso di lavoro della parte rimanente dell'installazione dipende dalle funzionalità specificate per l'installazione. A seconda delle selezioni, potrebbero non essere visualizzate tutte le pagine. 

1. Se si sceglie di installare la funzionalità PolyBase, verrà aggiunta la pagina **Configurazione di PolyBase** per l'installazione di SQL Server, che viene visualizzata dopo la pagina **Configurazione dell'istanza**. PolyBase richiede almeno Oracle JRE 7 Update 51 e se questo componente non è già stato installato l'installazione verrà bloccata. Nella pagina **Configurazione di PolyBase** è possibile scegliere di usare SQL Server come istanza autonoma abilitata per PolyBase oppure è possibile usare SQL Server come parte di un gruppo con scalabilità orizzontale di PolyBase. Se si sceglie di usare il gruppo con scalabilità orizzontale, è necessario specificare un intervallo di porte fino a 6 o più porte. 


1. Usare la pagina **Configurazione server - Account di servizio** per specificare gli account di accesso per i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I servizi effettivamente configurati in questa pagina dipendono dalle funzionalità selezionate per l'installazione. Per altre informazioni sulle impostazioni di configurazione, vedere [Guida dell'installazione guidata](../../sql-server/install/instance-configuration.md#serverconfig).
  
     È possibile assegnare lo stesso account di accesso a tutti i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure configurare singolarmente l'account di ogni servizio. È inoltre possibile specificare se i servizi verranno avviati automaticamente, manualmente o se sono disabilitati. È consigliabile configurare gli account dei servizi singolarmente per assegnare i privilegi minimi per ogni servizio. Verificare che ai servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siano concesse le autorizzazioni minime necessarie per completare le attività. Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Per specificare lo stesso account di accesso per tutti gli account del servizio in questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], immettere le credenziali nei campi visualizzati nella parte inferiore della pagina.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > Iniziare con [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e selezionare la casella di controllo **Concedi il privilegio Esecuzione attività di manutenzione volume al servizio del motore di database di SQL Server** per consentire all'account di servizio del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] l'uso dell'[inizializzazione immediata dei file di database](../../relational-databases/databases/database-instant-file-initialization.md).
  
1. Usare la pagina **Configurazione server - Regole di confronto** per specificare regole di confronto non predefinite per il [!INCLUDE[ssDE](../../includes/ssde-md.md)] e [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].    

   L'impostazione di installazione predefinita è determinata dalle impostazioni locali del sistema operativo. Le regole di confronto a livello di server possono essere modificate durante l'installazione oppure modificando le impostazioni locali del sistema operativo prima dell'installazione. Le regole di confronto predefinite sono impostate sulla versione disponibile meno recente associata a impostazioni locali specifiche. Questa impostazione ha lo scopo di mantenere la compatibilità con le versioni precedenti. Non sempre, quindi, si tratta delle regole di confronto consigliate. Per sfruttare tutti i vantaggi delle funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], modificare le impostazioni di installazione predefinite per usare le regole di confronto di Windows. Ad esempio, per le impostazioni locali del sistema operativo **Inglese (Stati Uniti)** (tabella codici 1252), le regole di confronto predefinite durante l'installazione sono impostate su **SQL_Latin1_General_CP1_CI_AS** ed è possibile modificare l'impostazione scegliendo la controparte di Windows più simile **Latin1_General_100_CI_AS_SC**.

   Per altre informazioni, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
1. Usare la pagina **Configurazione del motore di database - Configurazione del server** per specificare le opzioni seguenti:  
  
    * **Modalità di sicurezza**: selezionare **Autenticazione di Windows** o **Autenticazione in modalità mista** per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si seleziona **Autenticazione in modalità mista**, è necessario specificare una password complessa per l'account amministratore di sistema predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
       Quando viene stabilita la connessione tra un dispositivo e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il meccanismo di sicurezza è lo stesso sia in modalità mista che di autenticazione di Windows. Per altre informazioni, vedere [Pagina Configurazione del motore di database - Configurazione del server](../../sql-server/install/instance-configuration.md#serverconfig).
  
    * **Amministratori di SQL Server**: è necessario specificare almeno un amministratore di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per aggiungere l'account usato per eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], selezionare **Aggiungi utente corrente**. Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, selezionare **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Usare la pagina **Configurazione del motore di database - Directory dati** per specificare directory di installazione non predefinite. Per eseguire l'installazione nelle directory predefinite, selezionare **Avanti**.  
  
    > [!IMPORTANT]  
    > Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Per altre informazioni, vedere [Pagina Configurazione del motore di database - Directory dati](../../sql-server/install/instance-configuration.md#datadir).

     Usare la pagina **Configurazione del motore di database - TempDB** per configurare le dimensioni dei file, il numero di file, le directory di installazione non predefinite e le impostazioni di aumento delle dimensioni dei file per **tempdb**. Per altre informazioni, vedere [Pagina Configurazione del motore di database - TempDB](../../sql-server/install/instance-configuration.md#tempdb). 

 
     Usare la pagina **Configurazione del motore di database - FILESTREAM** per abilitare la funzione FILESTREAM per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Pagina Configurazione del motore di database - FILESTREAM](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page).  
  
1. Usare la pagina **Configurazione di Analysis Services - Provisioning account** per specificare la modalità server e gli utenti o gli account che disporranno delle autorizzazioni di amministratore per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. La modalità server determina quali sottosistemi di memoria e archiviazione vengono usati nel server. Tipi di soluzione diversi eseguiti nelle modalità server diverse. Se si intende eseguire database di cubi multidimensionali nel server, selezionare l'opzione predefinita per la modalità server, ovvero **Modalità multidimensionale e di data mining**.

    È necessario specificare almeno un amministratore di sistema per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:

    * Per aggiungere l'account usato per eseguire il programma di installazione di SQL Server, selezionare **Aggiungi utente corrente**.

    * Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, selezionare **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].

    Per altre informazioni sulle autorizzazioni della modalità server e amministratore, vedere [Pagina Configurazione di Analysis Services - Provisioning account](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page).

    Dopo aver modificato l'elenco, selezionare **OK**. Verificare l'elenco di amministratori nella finestra di dialogo di configurazione. Quando l'elenco è completo, selezionare **Avanti**.

    Usare la pagina **Configurazione di Analysis Services - Directory dati** per specificare directory di installazione non predefinite. Per eseguire l'installazione nelle directory predefinite, selezionare **Avanti**.  

    > [!IMPORTANT]  
    > Quando si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se si specifica lo stesso percorso di directory per INSTANCEDIR e SQLUSERDBDIR, SQL Server Agent e la ricerca full-text non vengono avviati per la mancanza di autorizzazioni.  
    >  
    > Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

    Per altre informazioni, vedere [Pagina Configurazione di Analysis Services - Directory dati](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page).  

1. Usare la pagina di **configurazione del controller di Riesecuzione distribuita** per specificare gli utenti a cui si desidera concedere autorizzazioni amministrative per il servizio controller di Riesecuzione distribuita. Gli utenti che dispongono di autorizzazioni amministrative dispongono di accesso illimitato al servizio controller di Riesecuzione distribuita.  
  
     * Per concedere le autorizzazioni di accesso per il servizio controller di Riesecuzione distribuita all'utente che esegue l'installazione di SQL Server, selezionare il pulsante **Aggiungi utente corrente**.

     * Per concedere le autorizzazioni di accesso per il servizio controller di Riesecuzione distribuita ad altri utenti, selezionare il pulsante **Aggiungi**.

     * Per rimuovere le autorizzazioni di accesso dal servizio controller di Riesecuzione distribuita, selezionare il pulsante **Rimuovi**.

     * Per continuare, selezionare **Avanti**.  
  
1. Usare la pagina di **configurazione del client Riesecuzione distribuita** per specificare gli utenti a cui concedere autorizzazioni amministrative per il servizio client Riesecuzione distribuita. Gli utenti che dispongono di autorizzazioni amministrative dispongono di accesso illimitato al servizio client Riesecuzione distribuita.  
  
     * **Nome controller** è facoltativo. Il valore predefinito è \<*blank*>. Immettere il nome del controller con cui comunicherà il computer client per il servizio client Riesecuzione distribuita:  
  
       * Se è già stato configurato un controller, immettere il nome del controller durante la configurazione di ciascun client.  
  
       * In caso contrario, è possibile lasciare vuoto il nome del controller. Tuttavia, è necessario immettere manualmente il nome del controller nel file **configurazione client** .  
  
     * Specificare la **Directory di lavoro** per il servizio client Riesecuzione distribuita. La directory di lavoro predefinita è \<*drive letter*>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     * Specificare la **Directory dei risultati** per il servizio client Riesecuzione distribuita. La directory dei risultati predefinita è \<*drive letter*>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     * Per continuare, selezionare **Avanti**.  
  
1. Nella pagina **Inizio installazione** è disponibile una visualizzazione albero delle opzioni specificate durante l'installazione. In questa pagina, tramite il programma di installazione vengono indicati l'eventuale abilitazione o disabilitazione della funzionalità **Aggiornamento prodotto** e la versione dell'aggiornamento finale.  
  
     Per continuare, selezionare **Installa**. Il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installa innanzitutto i prerequisiti obbligatori per le funzionalità selezionate e quindi le funzionalità selezionate.  
  
1. Durante l'installazione, nella pagina **Stato dell'installazione** è possibile monitorare lo stato di avanzamento del processo.  
  
1. Al termine dell'installazione nella pagina **Operazione completata** viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti.
  
    > [!IMPORTANT]
    > Assicurarsi di leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

    Per completare il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], selezionare **Chiudi**.  
  
1. Se viene richiesto, riavviare il computer.

::: moniker-end

::: moniker range=">=sql-server-ver15"
## <a name="to-install-sql-server-2019"></a>Per installare SQL Server 2019 
  
1. Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic sul file **Setup.exe**. Per eseguire l'installazione da una condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file **Setup.exe**.  
  
1. L'Installazione guidata esegue Centro installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per creare una nuova installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], selezionare **Installazione** nell'area di navigazione sinistra, quindi selezionare **Nuova installazione autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aggiunta di funzionalità a un'installazione esistente**.  

1. Nella pagina relativa al **codice Product Key** selezionare un'opzione per indicare se si intende installare un'edizione gratuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o una versione di produzione con una chiave PID. Per altre informazioni, vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
   Per continuare, selezionare **Avanti**.
  
1. Nella pagina **Condizioni di licenza** leggere il contratto di licenza. Per accettare, selezionare la casella di controllo **Accetto le condizioni di licenza e [l'informativa sulla privacy](https://privacy.microsoft.com/privacystatement)** e quindi selezionare **Avanti**.  

   > [!NOTE]
   > Se viene immesso un codice Product Key per una licenza CAL di Enterprise Server e il computer ha più di 20 core fisici o 40 core logici quando Hyper-Threading è abilitato, viene visualizzato un avviso durante l'installazione. È comunque possibile continuare l'installazione selezionando la casella di controllo **Selezionare questa casella per confermare questa limitazione per continuare o fare clic su Indietro/Annulla per inserire una licenza di prodotto Enterprise Core che supporta fino al massimo consentito dal sistema operativo** oppure fare clic su **Indietro** e immettere un codice di licenza che supporti il numero massimo di processori del sistema operativo.

   > [!NOTE]
   > SQL Server trasmette le informazioni sull'esperienza di installazione, nonché altri dati sull'utilizzo e le prestazioni per aiutare Microsoft a migliorare il prodotto. Per altre informazioni sull'elaborazione dei dati e i controlli sulla privacy di SQL Server, vedere l'[informativa sulla privacy](https://privacy.microsoft.com/privacystatement) e [Configurare SQL Server per inviare commenti e suggerimenti a Microsoft](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md).

1. Nella pagina **Regole globali** la procedura di installazione passerà automaticamente alla pagina **Aggiornamenti prodotti** se non vi sono errori delle regole.  
  
1. La pagina di **Microsoft Update** verrà visualizzata successivamente se la casella di controllo **Microsoft Update** non è selezionata in **Pannello di controllo** > **Tutti gli elementi del Pannello di controllo** > **Windows Update** > **Modifica impostazioni**. Se si seleziona la casella di controllo **Microsoft Update**, le impostazioni del computer vengono modificate per includere gli aggiornamenti per tutti i prodotti Microsoft quando si effettua l'analisi degli aggiornamenti di Windows.  

1. Nella pagina **Aggiornamenti prodotto** vengono visualizzati gli aggiornamenti più recenti sul prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non viene individuato alcun aggiornamento del prodotto, durante l'installazione questa pagina non viene visualizzata e viene aperta automaticamente la pagina **Installazione dei file di installazione**.  

1. Nella pagina **Installazione dei file di installazione** viene mostrato lo stato del download, dell'estrazione e dell'installazione dei file di installazione. Se viene individuato un aggiornamento per il programma di installazione e si specifica di includerlo, verrà installato anche questo aggiornamento. Se non viene trovato alcun aggiornamento, il programma di installazione proseguirà automaticamente.
  
1. Nella pagina **Regole di installazione** il programma di installazione esegue un controllo dei potenziali problemi che potrebbero verificarsi durante l'esecuzione dell'installazione. Se si verificano errori, selezionare un elemento nella colonna **Stato** per altre informazioni. In caso contrario, selezionare **Avanti**.

1. Se questa è la prima installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer, il programma di installazione salta la pagina **Tipo di installazione** e passa direttamente alla pagina **Selezione funzionalità**. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è già installato nel sistema, è possibile usare la pagina **Tipo di installazione** per scegliere di eseguire una nuova installazione o aggiungere funzionalità a un'installazione esistente. Per continuare, selezionare **Avanti**.
  
1. Nella pagina **Selezione funzionalità** selezionare i componenti per l'installazione. Ad esempio, per installare una nuova istanza del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di [!INCLUDE[ssDE](../../includes/ssde-md.md)], selezionare **Servizi motore di database**.

    Una volta selezionato il nome della funzionalità desiderata, nel riquadro **Descrizione funzionalità** viene visualizzata una descrizione per ogni gruppo di componenti. È possibile selezionare qualsiasi combinazione di caselle di controllo. Per altre informazioni, vedere [Edizioni e componenti di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) o [Edizioni e componenti di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
  
     I prerequisiti per le funzionalità selezionate vengono visualizzati nel riquadro **Prerequisiti per le funzionalità selezionate** . Verranno installati i prerequisiti che non sono stati già installati durante la procedura di installazione descritta più avanti in questo argomento.  
  
     È inoltre possibile specificare una directory personalizzata per i componenti condivisi usando il campo nella parte inferiore della pagina **Selezione funzionalità**. Per modificare il percorso di installazione per i componenti condivisi, aggiornare il percorso nel campo visualizzato nella parte inferiore della finestra di dialogo oppure selezionare **Sfoglia** per passare a una directory di installazione. Il percorso di installazione predefinito è [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     > [!NOTE]
     > Il percorso specificato per i componenti condivisi deve essere un percorso assoluto. La cartella non deve essere compressa o crittografata. Le unità di cui è stato eseguito il mapping non sono supportate.  
  
     SQL Server usa due directory per le funzionalità condivise:
  
     * Directory funzionalità condivise  
     * Directory funzionalità condivise (x86)  
  
     > [!NOTE]
     > Il percorso specificato per ogni opzione descritta in precedenza deve essere diverso.  
  
1. Se tutte le regole vengono soddisfatte, si procede automaticamente dalla pagina **Regole funzionalità**.  
  
1. Nella pagina **Configurazione dell'istanza** specificare se installare un'istanza predefinita o un'istanza denominata. Per altre informazioni, vedere [Configurazione delle istanze](../../sql-server/install/instance-configuration.md#instance-configuration-page).  
  
     * **ID istanza**: per impostazione predefinita, come ID istanza viene usato il nome dell'istanza. Tale ID viene usato per identificare le directory di installazione e le chiavi del Registro di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lo stesso comportamento si verifica per le istanze predefinite e le istanze denominate. Per un'istanza predefinita, il nome dell'istanza e l'ID istanza sono MSSQLSERVER. Per usare un ID istanza non predefinito, specificare un valore differente nella casella di testo **ID istanza**.  
  
       > [!NOTE]  
       > Le normali istanze autonome di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], che si tratti di istanze predefinite o denominate, non usano un valore che non sia predefinito per l'ID istanza.  
  
       Tutti i Service Pack e gli aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno applicati a ogni componente di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     * **Istanze installate**: nella griglia vengono visualizzate le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presenti nel computer in cui viene eseguito il programma di installazione. Se nel computer è già installata un'istanza predefinita, è necessario installare un'istanza denominata di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     Il flusso di lavoro della parte rimanente dell'installazione dipende dalle funzionalità specificate per l'installazione. A seconda delle selezioni, potrebbero non essere visualizzate tutte le pagine. 

1. A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], non è più necessario che Oracle JRE 7 Update 51 (come versione minima) sia pre-installato nel computer prima dell'installazione della funzionalità PolyBase. Se si sceglie di installare la funzionalità Polybase, verrà aggiunta la pagina **Percorso di installazione di Java** per l'installazione di SQL Server, che viene visualizzata dopo la pagina **Configurazione dell'istanza**. Nella pagina Percorso di installazione di Java è possibile scegliere di installare Azul Zulu Open JRE, incluso con l'installazione di [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], oppure specificare un percorso di un componente JRE o JDK diverso già installato nel computer.

1. A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], Java è stato aggiunto con le estensioni del linguaggio. Se si sceglie di installare la funzionalità Java, verrà aggiunta la pagina **Percorso di installazione di Java** nella finestra di dialogo per l'installazione di SQL Server, che viene visualizzata dopo la pagina **Configurazione dell'istanza**. Nella pagina **Percorso di installazione di Java** è possibile scegliere di installare Zulu Open JRE, incluso con l'installazione di [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], oppure specificare un percorso di un componente JRE o JDK diverso già installato nel computer.

1. Usare la pagina **Configurazione server - Account di servizio** per specificare gli account di accesso per i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I servizi effettivamente configurati in questa pagina dipendono dalle funzionalità selezionate per l'installazione. Per altre informazioni sulle impostazioni di configurazione, vedere [Guida dell'installazione guidata](../../sql-server/install/instance-configuration.md#serverconfig).
  
     È possibile assegnare lo stesso account di accesso a tutti i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure configurare singolarmente l'account di ogni servizio. È inoltre possibile specificare se i servizi verranno avviati automaticamente, manualmente o se sono disabilitati. È consigliabile configurare gli account dei servizi singolarmente per assegnare i privilegi minimi per ogni servizio. Verificare che ai servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siano concesse le autorizzazioni minime necessarie per completare le attività. Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Per specificare lo stesso account di accesso per tutti gli account del servizio in questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], immettere le credenziali nei campi visualizzati nella parte inferiore della pagina.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > Iniziare con [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e selezionare la casella di controllo **Concedi il privilegio Esecuzione attività di manutenzione volume al servizio del motore di database di SQL Server** per consentire all'account di servizio del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] l'uso dell'[inizializzazione immediata dei file di database](../../relational-databases/databases/database-instant-file-initialization.md).
  
     Usare la pagina **Configurazione server - Regole di confronto** per specificare regole di confronto non predefinite per il [!INCLUDE[ssDE](../../includes/ssde-md.md)] e [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Per altre informazioni, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
1. Usare la pagina **Configurazione del motore di database - Configurazione del server** per specificare le opzioni seguenti:  
  
    * **Modalità di sicurezza**: selezionare **Autenticazione di Windows** o **Autenticazione in modalità mista** per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si seleziona **Autenticazione in modalità mista**, è necessario specificare una password complessa per l'account amministratore di sistema (sa) predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
       Quando viene stabilita la connessione tra un dispositivo e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il meccanismo di sicurezza è lo stesso sia in modalità mista che di autenticazione di Windows. Per altre informazioni, vedere [Pagina Configurazione del motore di database - Configurazione del server](../../sql-server/install/instance-configuration.md#serverconfig).
  
    * **Amministratori di SQL Server**: è necessario specificare almeno un amministratore di sistema per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per aggiungere l'account usato per eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], selezionare **Aggiungi utente corrente**. Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, selezionare **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  È anche possibile aggiungere un gruppo di dominio Windows per stabilire un gruppo di amministratori SQL condiviso in Active Directory con accesso sysadmin a SQL Server. 
  
     Usare la pagina **Configurazione del motore di database - Directory dati** per specificare directory di installazione non predefinite. Per eseguire l'installazione nelle directory predefinite, selezionare **Avanti**.  
  
    > [!IMPORTANT]  
    > Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Per altre informazioni, vedere [Pagina Configurazione del motore di database - Directory dati](../../sql-server/install/instance-configuration.md#datadir).

     Usare la pagina **Configurazione del motore di database - TempDB** per configurare le dimensioni dei file, il numero di file, le directory di installazione non predefinite e le impostazioni di aumento delle dimensioni dei file per **tempdb**. Per altre informazioni, vedere [Pagina Configurazione del motore di database - TempDB](../../sql-server/install/instance-configuration.md#tempdb).

     Usare la pagina **Configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)] - MaxDOP** per specificare l'opzione max degree of parallelism. Questa impostazione determina il numero di processori che possono essere usati da una singola istruzione durante l'esecuzione. Il valore consigliato viene calcolato automaticamente durante l'installazione. 
     
    > [!NOTE]  
    > Questa pagina è disponibile solo nel programma di installazione a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. 
    
    Per altre informazioni, vedere [Pagina Configurazione del motore di database - MaxDOP](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#maxdop). 

     Usare la pagina **Configurazione del motore di database - Memoria** per specificare i valori di **min server memory** e **max server memory** che verranno usati da questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dopo l'avvio. È possibile usare i valori predefiniti, usare i valori consigliati calcolati oppure specificare manualmente i valori personalizzati dopo aver scelto l'opzione **Consigliato**.
     
    > [!NOTE]  
    > Questa pagina è disponibile solo nel programma di installazione a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. 
    
    Per altre informazioni, vedere [Pagina Configurazione del motore di database - Memoria](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#memory). 

     Usare la pagina **Configurazione del motore di database - FILESTREAM** per abilitare la funzione FILESTREAM per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Pagina Configurazione del motore di database - FILESTREAM](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page).  
  
1. Usare la pagina **Configurazione di Analysis Services - Provisioning account** per specificare la modalità server e gli utenti o gli account che disporranno delle autorizzazioni di amministratore per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. La modalità server determina quali sottosistemi di memoria e archiviazione vengono usati nel server. Tipi di soluzione diversi eseguiti nelle modalità server diverse. Se si intende eseguire database di cubi multidimensionali nel server, selezionare l'opzione predefinita per la modalità server, ovvero **Modalità multidimensionale e di data mining**.

    È necessario specificare almeno un amministratore di sistema per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:

    * Per aggiungere l'account usato per eseguire il programma di installazione di SQL Server, selezionare **Aggiungi utente corrente**.

    * Per aggiungere o rimuovere account dall'elenco degli amministratori di sistema, selezionare **Aggiungi** o **Rimuovi**, quindi modificare l'elenco di utenti, gruppi o computer con privilegi di amministratore per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].

    Per altre informazioni sulle autorizzazioni della modalità server e amministratore, vedere [Pagina Configurazione di Analysis Services - Provisioning account](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page).

    Dopo aver modificato l'elenco, selezionare **OK**. Verificare l'elenco di amministratori nella finestra di dialogo di configurazione. Quando l'elenco è completo, selezionare **Avanti**.

    Usare la pagina **Configurazione di Analysis Services - Directory dati** per specificare directory di installazione non predefinite. Per eseguire l'installazione nelle directory predefinite, selezionare **Avanti**.  

    > [!IMPORTANT]  
    > Quando si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se si specifica lo stesso percorso di directory per INSTANCEDIR e SQLUSERDBDIR, SQL Server Agent e la ricerca full-text non vengono avviati per la mancanza di autorizzazioni.  
    >  
    > Se si specificano directory di installazione non predefinite, verificare che le cartelle di installazione siano univoche per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nessuna delle directory presenti in questa finestra di dialogo deve essere condivisa con le directory di altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

    Per altre informazioni, vedere [Pagina Configurazione di Analysis Services - Directory dati](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page).  

1. Usare la pagina di **configurazione del controller di Riesecuzione distribuita** per specificare gli utenti a cui si desidera concedere autorizzazioni amministrative per il servizio controller di Riesecuzione distribuita. Gli utenti che dispongono di autorizzazioni amministrative dispongono di accesso illimitato al servizio controller di Riesecuzione distribuita.  
  
     * Per concedere le autorizzazioni di accesso per il servizio controller di Riesecuzione distribuita all'utente che esegue l'installazione di SQL Server, selezionare il pulsante **Aggiungi utente corrente**.

     * Per concedere le autorizzazioni di accesso per il servizio controller di Riesecuzione distribuita ad altri utenti, selezionare il pulsante **Aggiungi**.

     * Per rimuovere le autorizzazioni di accesso dal servizio controller di Riesecuzione distribuita, selezionare il pulsante **Rimuovi**.

     * Per continuare, selezionare **Avanti**.  
  
1. Usare la pagina di **configurazione del client Riesecuzione distribuita** per specificare gli utenti a cui concedere autorizzazioni amministrative per il servizio client Riesecuzione distribuita. Gli utenti che dispongono di autorizzazioni amministrative dispongono di accesso illimitato al servizio client Riesecuzione distribuita.  
  
     * **Nome controller** è facoltativo. Il valore predefinito è \<*blank*>. Immettere il nome del controller con cui comunicherà il computer client per il servizio client Riesecuzione distribuita:  
  
       * Se è già stato configurato un controller, immettere il nome del controller durante la configurazione di ciascun client.  
  
       * In caso contrario, è possibile lasciare vuoto il nome del controller. Tuttavia, è necessario immettere manualmente il nome del controller nel file **configurazione client** .  
  
     * Specificare la **Directory di lavoro** per il servizio client Riesecuzione distribuita. La directory di lavoro predefinita è \<*drive letter*>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     * Specificare la **Directory dei risultati** per il servizio client Riesecuzione distribuita. La directory dei risultati predefinita è \<*drive letter*>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     * Per continuare, selezionare **Avanti**.  
  
1. Nella pagina **Inizio installazione** è disponibile una visualizzazione albero delle opzioni specificate durante l'installazione. In questa pagina, tramite il programma di installazione vengono indicati l'eventuale abilitazione o disabilitazione della funzionalità **Aggiornamento prodotto** e la versione dell'aggiornamento finale.  
  
     Per continuare, selezionare **Installa**. Il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installa innanzitutto i prerequisiti obbligatori per le funzionalità selezionate e quindi le funzionalità selezionate.  
  
1. Durante l'installazione, nella pagina **Stato dell'installazione** è possibile monitorare lo stato di avanzamento del processo.  
  
1. Al termine dell'installazione nella pagina **Operazione completata** viene visualizzato un collegamento al file di log di riepilogo del processo di installazione e ad altre note importanti.
  
    > [!IMPORTANT]
    > Assicurarsi di leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

    Per completare il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], selezionare **Chiudi**.  
  
1. Se viene richiesto, riavviare il computer.

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

[Configurare la nuova installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](../../database-engine/configure-windows/database-engine-instances-sql-server.md).  
  
Per ridurre la superficie di attacco di un sistema, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono installati e abilitati in modo selettivo i servizi e le funzionalità principali. Per altre informazioni, vedere [Configurazione superficie di attacco](../../relational-databases/security/surface-area-configuration.md).  
  
## <a name="see-also"></a>Vedere anche
  
* [Convalidare un'installazione di SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)  
* [Ripristinare un'installazione non riuscita di SQL Server](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
* [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
* [Eseguire l'aggiornamento a SQL Server usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [Installare SQL Server al prompt dei comandi](./install-sql-server-from-the-command-prompt.md)