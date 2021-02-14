---
description: Processi di SQL Server Agent per i pacchetti
title: Processi di SQL Server Agent per i pacchetti | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- jobs [Integration Services]
- automatic package execution
- scheduling packages [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: ecf7a5f9-b8a7-47f1-9ac0-bac07cb89e31
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 353acdd28f3611bcc387bd4122ca377a2d8d0ca5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346865"
---
# <a name="sql-server-agent-jobs-for-packages"></a>Processi di SQL Server Agent per i pacchetti

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile automatizzare e pianificare l'esecuzione dei pacchetti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. È possibile pianificare i pacchetti distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nell'archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] e nel file system.  
 
> [!NOTE]
> In questo articolo viene illustrato come pianificare i pacchetti SSIS a livello generale e come pianificare i pacchetti in locale. È anche possibile eseguire e pianificare pacchetti SSIS nelle piattaforme seguenti:
> - **Cloud di Microsoft Azure**. Per altre informazioni, vedere [Migrazione lift-and-shift dei carichi di lavoro di SQL Server Integration Services nel cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) e [Pianificare l'esecuzione di un pacchetto SSIS in Azure](../lift-shift/ssis-azure-schedule-packages.md).
> - **Linux**. Per altre informazioni, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md) e [Pianificare l'esecuzione dei pacchetti SSIS in Linux con cron](../../linux/sql-server-linux-schedule-ssis-packages.md).

## <a name="sections-in-this-topic"></a>Sezioni dell'argomento  
 In questo argomento sono incluse le sezioni seguenti:  
  
-   [Pianificazione dei processi in SQL Server Agent](#jobs)  
  
-   [Pianificazione dei pacchetti di Integration Services](#packages)  
  
-   [Risoluzione dei problemi dei pacchetti pianificati](#trouble)  
  
##  <a name="scheduling-jobs-in-sql-server-agent"></a><a name="jobs"></a> Scheduling Jobs in SQL Server Agent  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è il servizio installato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che consente di automatizzare e pianificare le attività eseguendo processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. È possibile eseguire automaticamente processi solo se il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è in esecuzione. Per altre informazioni, vedere [Configure SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md).  
  
 Il nodo **SQL Server Agent** viene visualizzato in Esplora oggetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] quando ci si connette a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Per automatizzare un'attività periodica, viene creato un processo usando la finestra di dialogo **Nuovo processo** . Per altre informazioni, vedere [Implementazione di processi](../../ssms/agent/implement-jobs.md).  
  
 Dopo aver creato il processo, è necessario aggiungere almeno un passaggio. In un processo possono essere inclusi più passaggi che consentono di effettuare attività diverse. Per altre informazioni, vedere [Gestire passaggi di processo](../../ssms/agent/manage-job-steps.md).  
  
 Dopo aver creato il processo e i relativi passaggi, è possibile creare una pianificazione per l'esecuzione del processo. È tuttavia possibile creare anche un processo non pianificato che viene eseguito manualmente. Per altre informazioni, vedere [Creare e collegare le pianificazioni ai processi](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
 È possibile migliorare il processo impostando opzioni di notifica, ad esempio aggiungendo avvisi o specificando l'operatore che deve inviare un messaggio di posta elettronica al completamento del processo. Per altre informazioni, vedere [Avvisi](../../ssms/agent/alerts.md).  
  
##  <a name="scheduling-integration-services-packages"></a><a name="packages"></a> Scheduling Integration Services Packages  
 Quando si crea un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per pianificare i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è necessario aggiungere almeno un passaggio e impostare il tipo di passaggio su **Pacchetto SQL Server Integration Services**. In un processo possono essere inclusi più passaggi che consentono di eseguire pacchetti diversi.  
  
 L'esecuzione di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] da un passaggio di processo è simile all'esecuzione di un pacchetto tramite le utilità **dtexec** (dtexec.exe) e **DTExecUI** (dtexecui.exe). Le opzioni di runtime per un pacchetto non vengono impostate tramite opzioni della riga di comando o nella finestra di dialogo **Utilità di esecuzione pacchetti** , ma nella finestra di dialogo **Nuovo passaggio di processo** . Per altre informazioni sulle opzioni per l'esecuzione di un pacchetto, vedere [Utilità dtexec](../../integration-services/packages/dtexec-utility.md).  
  
 Per altre informazioni, vedere [Pianificare un pacchetto tramite SQL Server Agent](#schedule).  
  
 Per visualizzare un video in cui viene illustrato come usare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per l'esecuzione di un pacchetto, vedere la home page del video [Procedura: Automazione dell'esecuzione di un pacchetto usando SQL Server Agent (video di SQL Server)](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100)) in MSDN Library.  
  
##  <a name="troubleshooting"></a><a name="trouble"></a> Risoluzione dei problemi  
 Un passaggio di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent potrebbe non riuscire ad avviare un pacchetto anche se il pacchetto viene eseguito correttamente in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] e dalla riga di comando. Per questo problema esistono alcuni motivi comuni e diverse soluzioni consigliate. Per ulteriori informazioni, vedere le risorse seguenti.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Articolo della Knowledge Base [Pacchetto SSIS non viene eseguito quando viene chiamato da un passaggio di processo SQL Server Agent](https://support.microsoft.com/kb/918760)  
  
-   Video [Risoluzione dei problemi: Esecuzione di un pacchetto con SQL Server Agent (video di SQL Server)](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100)) in MSDN Library.  
  
 Dopo l'avvio di un pacchetto tramite un passaggio di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, l'esecuzione del pacchetto potrebbe avere esito negativo oppure positivo ma con risultati imprevisti. È possibile utilizzare gli strumenti seguenti per risolvere questi problemi.  
  
-   Per i pacchetti archiviati nel database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] MSDB, nell'archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] o in una cartella del computer locale, è possibile usare **Visualizzatore file di log** , nonché qualsiasi log e file di dump del debug generato durante l'esecuzione del pacchetto.  
  
     **Per utilizzare Visualizzatore file di log, effettuare le operazioni seguenti.**  
  
    1.  Fare clic con il pulsante destro del mouse sul processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, quindi fare clic su **Visualizza cronologia**.  
  
    2.  Individuare l'esecuzione del processo nella casella **Riepilogo file di log** con il messaggio **Processo non riuscito** nella colonna **Messaggio** .  
  
    3.  Espandere il nodo del processo e fare clic sul passaggio di processo per visualizzare i dettagli del messaggio nell'area sotto la casella **Riepilogo file di log** .  
  
-   Per i pacchetti archiviati nel database SSISDB, è inoltre possibile usare **Visualizzatore file di log** , nonché qualsiasi log e file di dump del debug generato durante l'esecuzione del pacchetto. Inoltre, è possibile usare i report per il server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     **Per trovare informazioni nei report per l'esecuzione del pacchetto associata all'esecuzione del processo, effettuare le operazioni seguenti.**  
  
    1.  Attenersi ai passaggi precedenti per visualizzare i dettagli del messaggio per il passaggio di processo.  
  
    2.  Individuare l'ID esecuzione elencato nel messaggio.  
  
    3.  Espandere il nodo Catalogo di Integration Services in Esplora oggetti.  
  
    4.  Fare clic con il pulsante destro del mouse su SSISDB, scegliere Report, Report standard e quindi fare clic su Tutte le esecuzioni.  
  
    5.  Nel report **Tutte le esecuzioni** individuare l'ID esecuzione nella colonna **ID** . Fare clic su **Panoramica**, **Tutti i messaggi** o **Prestazioni di esecuzione** per visualizzare informazioni sull'esecuzione di questo pacchetto.  
  
    Per altre informazioni sui report Panoramica, Tutti i messaggi e Prestazioni di esecuzione, vedere [Report per il server Integration Services](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  

## <a name="schedule-a-package-by-using-sql-server-agent"></a><a name="schedule"></a> Pianificare un pacchetto tramite SQL Server Agent
  Nella procedura riportata di seguito vengono illustrati i passaggi per automatizzare l'esecuzione di un pacchetto tramite un passaggio di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per eseguire il pacchetto.  
  
### <a name="to-automate-package-execution-by-using-sql-server-agent"></a>Per automatizzare l'esecuzione dei pacchetti tramite SQL Server Agent  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si desidera creare un processo oppure all'istanza contenente il processo a cui si desidera aggiungere un passaggio.  
  
2.  Espandere il nodo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in Esplora oggetti ed eseguire una delle attività seguenti:  
  
    -   Per creare un nuovo processo, fare clic con il pulsante destro del mouse su **Processi** , quindi scegliere **Nuovo processo**.  
  
    -   Per aggiungere un passaggio a un processo esistente, espandere il nodo **Processi**, fare clic con il pulsante destro del mouse sul processo, quindi scegliere **Proprietà**.  
  
3.  Nella pagina **Generale** , se si crea un nuovo processo specificare un nome per il processo, selezionare un proprietario e una categoria di processo e, facoltativamente, fornire una descrizione.  
  
4.  Per rendere il processo disponibile per la pianificazione, selezionare **Abilitato**.  
  
5.  Per creare un passaggio di processo per il pacchetto che si desidera pianificare, fare clic su **Passaggi**, quindi su **Nuovo**.  
  
6.  Selezionare **Pacchetto di Integration Services** per il tipo di passaggio di processo.  
  
7.  Nell'elenco **Esegui come** selezionare **Account del servizio SQL Server Agent** oppure selezionare un account proxy che dispone delle credenziali che verranno utilizzate dal passaggio di processo. Per informazioni sulla creazione di un account proxy, vedere [Create a SQL Server Agent Proxy](../../ssms/agent/create-a-sql-server-agent-proxy.md).  

    L'utilizzo di un account proxy anziché dell' **Account del servizio SQL Server Agent** può risolvere i problemi comuni che possono verificarsi quando si esegue un pacchetto tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per ulteriori informazioni su questi problemi, vedere l'articolo della [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base relativo a [un pacchetto SSIS che non viene eseguito quando viene chiamato da un passaggio di processo di SQL Server Agent](https://support.microsoft.com/kb/918760). 
     
    - Quando si esegue un processo con un proxy, perché questo venga completato correttamente, è necessario aver predisposto gli elementi di sicurezza seguenti.

        L'account di accesso per le credenziali usato dal proxy, l'account che esegue SQL Server Agent e l'account che esegue il servizio SQL Server richiedono le autorizzazioni seguenti:

        - Attributo Criteri di sicurezza locali: Sostituire token a livello di processo
        - Controllo completo su %SYSTEMROOT%\Temp 

        La mancata predisposizione di questi elementi di sicurezza determina l'esito negativo del processo e un messaggio di errore simile al seguente: Il processo ha avuto esito negativo. Il client non dispone di un privilegio necessario.

        > **NOTA** Se viene modificata la password per le credenziali utilizzate dall'account proxy, è necessario aggiornare la password delle credenziali. In caso contrario, il passaggio di processo avrà esito negativo.  

        Per informazioni sulla configurazione dell'account del servizio SQL Server Agent, vedere [Impostazione dell'account di avvio del servizio SQL Server Agent &#40;Gestione configurazione SQL Server&#41;](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md).  

8.  Nella casella di riepilogo **Origine pacchetto** fare clic sull'origine del pacchetto e quindi configurare le opzioni per il passaggio di processo.  
  
    **Nella tabella seguente vengono descritte le possibili origini pacchetto.**  
  
    |Origine pacchetto|Descrizione|  
    |--------------------|-----------------|  
    |**Catalogo SSIS**|Pacchetti archiviati nel database SSISDB. I pacchetti sono contenuti nei progetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
    |**SQL Server**|Pacchetti archiviati nel database MSDB. Utilizzare il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per gestire i pacchetti.|  
    |**Archivio pacchetti SSIS**|Pacchetti archiviati nella cartella predefinita nel computer. La cartella predefinita è *\<drive>* :\Programmi\Microsoft SQL Server\110\DTS\Packages. Utilizzare il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per gestire i pacchetti.<br /><br /> Nota: È possibile specificare un'altra cartella o specificare cartelle aggiuntive nel file system da gestire tramite il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], modificando il file di configurazione per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Per altre informazioni, vedere [Servizio Integration Services &#40;servizio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).|  
    |**File system**|Pacchetti archiviati in qualsiasi cartella nel computer locale.|  
    |||
  
    **Nelle tabelle seguenti vengono descritte le opzioni di configurazione disponibili per il passaggio di processo in base all'origine del pacchetto selezionata.**  
  
    > **IMPORTANTE:** Se il pacchetto è protetto da password, quando si fa clic su una delle schede nella pagina **Generale** della finestra di dialogo **Nuovo passaggio di processo**, ad eccezione della scheda **Pacchetto**, è necessario immettere la password nella finestra di dialogo **Password pacchetto** che viene visualizzata. In caso contrario, il processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non sarà in grado di eseguire il pacchetto.  
  
    **Origine pacchetto**: Catalogo SSIS  
  
    |Scheda|Opzioni|  
    |---------|-------------|  
    |**Pacchetto**|**Server**<br /><br /> Digitare o selezionare il nome dell'istanza del server di database che ospita il catalogo SSISDB.<br /><br /> Quando **Catalogo SSIS** è l'origine del pacchetto, è possibile accedere al server utilizzando solo un account utente di Microsoft Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è disponibile.|  
    ||**Pacchetto**<br /><br /> Fare clic sul pulsante con i puntini di sospensione e selezionare un pacchetto.<br /><br /> Viene selezionato un pacchetto in una cartella nel nodo **Cataloghi di Integration Services** in **Esplora oggetti**.|  
    |**Parameters**<br /><br /> Si trova nella scheda **Configurazione** .|La **Conversione guidata progetto di Integration Services** consente di sostituire le configurazioni del pacchetto con i parametri.<br /><br /> Nella scheda **Parametri** sono visualizzati i parametri aggiunti dopo la progettazione del pacchetto, ad esempio tramite [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Nella scheda sono inoltre visualizzati i parametri aggiunti al pacchetto durante la conversione del progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dal modello di distribuzione del pacchetto nel modello di distribuzione del progetto. Immettere nuovi valori per i parametri contenuti nel pacchetto. È possibile immettere un valore letterale o utilizzare il valore contenuto in una variabile di ambiente server di cui è già stato eseguito il mapping al parametro.<br /><br /> Per immettere il valore letterale, fare clic sul pulsante con i puntini di sospensione accanto a un parametro. Viene visualizzata la finestra di dialogo **Modifica valore letterale per l'esecuzione** .<br /><br /> Per utilizzare una variabile di ambiente, fare clic su **Ambiente** e selezionare l'ambiente che contiene la variabile da utilizzare.<br /><br /> **\*\* Importante \*\*** Se è stato eseguito il mapping di più parametri e/o delle proprietà di gestione connessione alle variabili contenute in più ambienti, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent visualizza un messaggio di errore. Per un'esecuzione specifica, un pacchetto può essere eseguito solo con i valori contenuti in un ambiente server singolo.<br /><br /> Per informazioni sulla creazione di un ambiente server e il mapping di una variabile a un parametro, vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).|  
    |**Gestioni connessioni**<br /><br /> Si trova nella scheda **Configurazione** .|Modificare i valori per le proprietà di gestione connessione. Ad esempio, è possibile modificare il nome del server. I parametri vengono automaticamente generati nel server SSIS per le proprietà di gestione connessione. Per modificare il valore di una proprietà, è possibile immettere un valore letterale o utilizzare il valore contenuto in una variabile di ambiente server di cui è già stato eseguito il mapping alla proprietà di gestione connessione.<br /><br /> Per immettere il valore letterale, fare clic sul pulsante con i puntini di sospensione accanto a un parametro. Viene visualizzata la finestra di dialogo **Modifica valore letterale per l'esecuzione** .<br /><br /> Per utilizzare una variabile di ambiente, fare clic su **Ambiente** e selezionare l'ambiente che contiene la variabile da utilizzare.<br /><br /> **\*\* Importante \*\*** Se è stato eseguito il mapping di più parametri e/o delle proprietà di gestione connessione alle variabili contenute in più ambienti, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent visualizza un messaggio di errore. Per un'esecuzione specifica, un pacchetto può essere eseguito solo con i valori contenuti in un ambiente server singolo.<br /><br /> Per informazioni sulla creazione di un ambiente server e il mapping di una variabile a una proprietà di gestione connessione, vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).|  
    |**Funzionalità avanzate**<br /><br /> Si trova nella scheda **Configurazione** .|Configurare le impostazioni aggiuntive seguenti per l'esecuzione del pacchetto:|  
    ||**Override di proprietà**:<br /><br /> Fare clic su **Aggiungi** per immettere un nuovo valore per una proprietà del pacchetto, specificare il percorso della proprietà e indicare se il valore della proprietà è sensibile. Il server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crittografa i dati sensibili. Per modificare o rimuovere le impostazioni per una proprietà, fare clic su una riga nel contenitore di override **Proprietà** , quindi fare clic su **Modifica** o **Rimuovi**. È possibile trovare il percorso della proprietà con una delle operazioni seguenti:<br /><br /> -Copiare il percorso della proprietà dal file di configurazione XML (\*.dtsconfig). Il percorso è elencato nella sezione Configurazione del file, come valore dell'attributo Path. Di seguito è riportato un esempio del percorso per la proprietà MaximumErrorCount: \Package.Properties[MaximumErrorCount]<br /><br /> -Eseguire la **Configurazione guidata pacchetto** e copiare i percorsi delle proprietà dalla pagina finale **Completamento procedura guidata** . È possibile annullare la procedura guidata.<br /><br /> <br /><br /> Nota: L'opzione **Override di proprietà** è destinata ai pacchetti con configurazioni aggiornate da una versione precedente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. I pacchetti creati tramite [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] e distribuiti al server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilizzano parametri anziché configurazioni.|  
    ||**Livello di registrazione**<br /><br /> Selezionare uno dei livelli di registrazione seguenti per l'esecuzione del pacchetto. La selezione del livello di registrazione **Prestazioni** o **Dettagliato** può influire sulle prestazioni di esecuzione del pacchetto.<br /><br /> **Nessuna**:<br />                          La registrazione è disabilitata. Solo lo stato dell'esecuzione del pacchetto viene registrato.<br /><br /> **Basic**:<br />                          Tutti gli eventi sono registrati, ad eccezione di eventi personalizzati e di diagnostica. È il valore predefinito per il livello di registrazione.<br /><br /> **Prestazioni**:<br />                          Vengono registrati solo le statistiche sulle prestazioni e gli eventi OnError e OnWarning.<br /><br /> **Dettagliato**:<br />                          Tutti gli eventi vengono registrati, inclusi gli eventi personalizzati e di diagnostica.<br /><br /> Il livello di registrazione selezionato determina le informazioni visualizzate nelle viste SSISDB e nei report per il server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per altre informazioni, vedere [Registrazione di Integration Services (SSIS)](../../integration-services/performance/integration-services-ssis-logging.md).|  
    ||**Dump su errori**<br /><br /> Specificare se vengono generati file di dump del debug quando si verifica un errore durante l'esecuzione del pacchetto. I file contengono le informazioni sull'esecuzione del pacchetto che possono consentire di risolvere i problemi dell'esecuzione. Quando si seleziona questa opzione e si verifica un errore durante l'esecuzione, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea un file con estensione MDMP (file binario) e un file con estensione TMP (file di testo). Per impostazione predefinita, i file vengono archiviati da [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nella cartella *\<drive>:* \Programmi\Microsoft SQL Server\110\Shared\ErrorDumps.|  
    ||**Runtime a 32 bit**<br /><br /> Indicare se eseguire il pacchetto utilizzando la versione a 32 bit dell'utilità dtexec in un computer a 64 bit con la versione a 64 bit di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent installata.<br /><br /> Potrebbe essere necessario eseguire il pacchetto utilizzando la versione a 32 bit di dtexec se, ad esempio, il pacchetto utilizza un provider OLE DB nativo che non è disponibile in una versione a 64 bit. Per ulteriori informazioni, vedere [Considerazioni a 64r bit per Integration Services](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx).<br /><br /> Per impostazione predefinita, quando si seleziona il tipo di passaggio di processo **Pacchetto di SQL Server Integration Services** , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent esegue il pacchetto utilizzando la versione dell'utilità dtexec che è richiamata automaticamente dal sistema. Il sistema richiama la versione a 32 bit o la versione a 64 bit dell'utilità a seconda del processore del computer e la versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in esecuzione nel computer.|  
  
     **Origine pacchetto**:  SQL Server, archivio pacchetti SSIS o file system  
  
     Molte delle opzioni che è possibile impostare per i pacchetti archiviati in SQL Server, nell'archivio pacchetti SSIS o nel file system corrispondono alle opzioni della riga di comando per l'utilità del prompt dei comandi **dtexec** . Per ulteriori informazioni sull'utilità e sulle opzioni della riga di comando, vedere [Utilità dtexec](../../integration-services/packages/dtexec-utility.md).  
  
    |Scheda|Opzioni|  
    |---------|-------------|  
    |**Pacchetto**<br /><br /> Di seguito sono riportate le opzioni della scheda per i pacchetti archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nell'archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] .|**Server**<br /><br /> Digitare o selezionare il nome dell'istanza del server di database per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
    ||**Usa autenticazione di Windows**<br /><br /> Selezionare questa opzione per accedere al server mediante un account utente di Microsoft Windows.|  
    ||**Usa autenticazione di SQL Server**<br /><br /> Quando un utente si connette con un nome di account di accesso e una password da una connessione non trusted, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue l'autenticazione controllando se è stato impostato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e se la password specificata corrisponde a quella registrata in precedenza. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non riesce a trovare un account di accesso, l'autenticazione ha esito negativo e viene visualizzato un messaggio di errore.|  
    ||**Nome utente**|  
    ||**Password**|  
    ||**Pacchetto**<br /><br /> Fare clic sul pulsante con i puntini di sospensione e selezionare il pacchetto.<br /><br /> Viene selezionato un pacchetto in una cartella nel nodo **Pacchetti archiviati** in **Esplora oggetti**.|  
    |**Pacchetto**<br /><br /> Di seguito sono riportate le opzioni della scheda per i pacchetti archiviati nel file system.|**Pacchetto**<br /><br /> Digitare il percorso completo del file del pacchetto oppure fare clic sul pulsante con i puntini di sospensione per selezionare il pacchetto.|  
    |**Configurazioni**|Aggiungere un file di configurazione XML per eseguire il pacchetto con una configurazione specifica. Per aggiornare i valori delle proprietà del pacchetto in fase di esecuzione utilizzare una configurazione di pacchetto.<br /><br /> Questa opzione corrisponde all'opzione **/ConfigFile** per **dtexec**.<br /><br /> Per informazioni sull'applicazione delle configurazioni dei pacchetti, vedere [Package Configurations](./legacy-package-deployment-ssis.md). Per informazioni su come creare la configurazione di un pacchetto, vedere [Create Package Configurations](./legacy-package-deployment-ssis.md).|  
    |**File di comando**|Specificare le opzioni aggiuntive da eseguire con **dtexec**, in un file separato.<br /><br /> Ad esempio, è possibile includere un file contenente l'opzione /Dump *errorcode* per generare file di dump del debug quando uno o più eventi specificati si verificano durante l'esecuzione del pacchetto.<br /><br /> È possibile eseguire un pacchetto con diversi set di opzioni creando più file e specificando il file appropriato tramite l'opzione **File di comando** .<br /><br /> L'opzione **File di comando** corrisponde all'opzione **/CommandFile** per **dtexec**.|  
    |**Data Sources** (Origini dati)|Visualizzare le gestioni connessioni contenute nel pacchetto. Per modificare una stringa di connessione, fare clic sulla gestione connessione e quindi fare clic sulla stringa di connessione.<br /><br /> Questa opzione corrisponde all'opzione **/Connection** per **dtexec**.|  
    |**Opzioni di esecuzione**|**Interrompi il pacchetto in caso di avvisi di convalida**<br /> Indica se un messaggio di avviso viene considerato un errore. Se si seleziona questa opzione e viene generato un avviso durante la convalida, il pacchetto ha esito negativo durante la convalida. Questa opzione corrisponde all'opzione **/WarnAsError** per **dtexec**.<br /><br /> **Convalida pacchetto senza esecuzione**<br /> Indica se l'esecuzione del pacchetto viene arrestata dopo la fase di convalida, senza eseguire effettivamente il pacchetto. Questa opzione corrisponde all'opzione **/Validate** per **dtexec**.<br /><br /> **Esegui override proprietà MaxConcurrentExecutables**<br /> Consente di specificare il numero di file eseguibili che il pacchetto è in grado di eseguire contemporaneamente. Il valore -1 indica che il pacchetto può eseguire un numero massimo di file eseguibili uguale al numero totale di processori nel computer in cui è eseguito il pacchetto, più due. Questa opzione corrisponde all'opzione **/MaxConcurrent** per **dtexec**.<br /><br /> **Abilita checkpoint pacchetto**<br /> Indica se il pacchetto utilizzerà checkpoint durante l'esecuzione del pacchetto. Per ulteriori informazioni, vedere [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).<br /><br /> Questa opzione corrisponde all'opzione **/CheckPointing** per **dtexec**.<br /><br /> **Ignora opzioni di riavvio**<br /> Indica se è impostato un nuovo valore per la proprietà **CheckpointUsage** del pacchetto. Selezionare un valore nell'elenco a discesa **Opzione di avvio** .<br /><br /> Questa opzione corrisponde all'opzione **/Restart** per **dtexec**.<br /><br /> **Utilizza run-time a 32 bit**<br /> Indicare se eseguire il pacchetto utilizzando la versione a 32 bit dell'utilità dtexec in un computer a 64 bit con la versione a 64 bit di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent installata.<br /><br /> Potrebbe essere necessario eseguire il pacchetto utilizzando la versione a 32 bit di dtexec se, ad esempio, il pacchetto utilizza un provider OLE DB nativo che non è disponibile in una versione a 64 bit. Per ulteriori informazioni, vedere [Considerazioni a 64r bit per Integration Services](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx).<br /><br /> Per impostazione predefinita, quando si seleziona il tipo di passaggio di processo **Pacchetto di SQL Server Integration Services** , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent esegue il pacchetto utilizzando la versione dell'utilità dtexec che è richiamata automaticamente dal sistema. Il sistema richiama la versione a 32 bit o la versione a 64 bit dell'utilità a seconda del processore del computer e la versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in esecuzione nel computer.|  
    |**Logging**|Associare un provider di log all'esecuzione del pacchetto.<br /><br /> **Provider di log SSIS per file di testo**<br /> Scrive le voci di log in file di testo ASCII<br /><br /> **Provider di log SSIS per SQL Server**<br /> Scrive le voci di log nella tabella sysssislog nel database MSDB.<br /><br /> **Provider di log SSIS per SQL Server Profiler**<br /> Scrive tracce che è possibile visualizzare utilizzando SQL Server Profiler.<br /><br /> **Provider di log SSIS per il registro eventi di Windows**<br /> Scrive voci di log nel log applicazioni nel registro eventi di Windows.<br /><br /> **Provider di log SSIS per file XML**<br /> Scrive file di log in un file XML.<br /><br /> Per il file di testo, il file XML e i provider di log di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler, si selezionano gestioni connessione file contenute nel pacchetto. Per il provider di log di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si seleziona una gestione connessione OLE DB contenuta nel pacchetto.<br /><br /> Questa opzione corrisponde all'opzione **/Logger** per **dtexec**.|  
    |**Imposta valori**|Eseguire l'override dell'impostazione delle proprietà di un pacchetto. Nella casella **Proprietà** immettere i valori nelle colonne **Percorso proprietà** e **Valore** . Dopo avere immesso valori per una proprietà, viene visualizzata una riga vuota nella casella **Proprietà** che consente di immettere valori per un'altra proprietà.<br /><br /> Per rimuovere una proprietà dalla casella Proprietà, fare clic sulla riga e quindi su **Rimuovi**.<br /><br /> È possibile trovare il percorso della proprietà con una delle operazioni seguenti:<br /><br /> -Copiare il percorso della proprietà dal file di configurazione XML (\*.dtsconfig). Il percorso è elencato nella sezione Configurazione del file, come valore dell'attributo Path. Di seguito è riportato un esempio del percorso per la proprietà MaximumErrorCount: \Package.Properties[MaximumErrorCount]<br /><br /> -Eseguire la **Configurazione guidata pacchetto** e copiare i percorsi delle proprietà dalla pagina finale **Completamento procedura guidata** . È possibile annullare la procedura guidata.|  
    |**Verifica**|**Esegui solo pacchetti firmati**<br /> Indica se la firma del pacchetto è controllata. Se il pacchetto non è firmato o se la firma non è valida, il pacchetto ha esito negativo. Questa opzione corrisponde all'opzione **/VerifySigned** per **dtexec**.<br /><br /> **Verifica build pacchetto**<br /> Indica se il numero di build del pacchetto viene verificato rispetto al numero di build immesso nella casella **Compilazione** accanto all'opzione. Se i numeri non corrispondono, il pacchetto non verrà eseguito. Questa opzione corrisponde all'opzione **/VerifyBuild** per **dtexec**.<br /><br /> **Verifica ID pacchetto**<br /> Indica se il GUID del pacchetto viene verificato, confrontandolo con l'ID pacchetto immesso nella casella **ID pacchetto** accanto all'opzione. Questa opzione corrisponde all'opzione **/VerifyPackageID** per **dtexec**.<br /><br /> **Verifica ID versione**<br /> Indica se il GUID della versione del pacchetto viene verificato, confrontandolo con l'ID versione immesso nella casella **ID versione** accanto all'opzione. Questa opzione corrisponde all'opzione **/VerifyVersionID** per **dtexec**.|  
    |**Riga di comando**|Modificare le opzioni della riga di comando per dtexec. Per ulteriori informazioni sulle opzioni, vedere [dtexec Utility](../../integration-services/packages/dtexec-utility.md).<br /><br /> **Ripristina opzioni originali**<br /> Utilizzare le opzioni della riga di comando impostate nelle schede **Pacchetto**, **Configurazioni**, **File di comando**, **Origini dati**, **Opzioni di esecuzione**, **Registrazione**, **Imposta valori** e **Verifica** della finestra di dialogo **Proprietà set processo** .<br /><br /> **Modificare il comando manualmente**<br /> Digitare opzioni della riga di comando aggiuntive nella casella **Riga di comando** .<br /><br /> Prima di fare clic su **OK** per salvare le modifiche apportate al passaggio di processo, è possibile rimuovere tutte le opzioni aggiuntive digitate nella casella **Riga di comando** facendo clic su **Ripristina opzioni originali**.<br /><br /> **\*\* Suggerimento \*\*** È possibile copiare la riga di comando in una finestra del prompt dei comandi, aggiungere `dtexec`ed eseguire il pacchetto dalla riga di comando. Si tratta di un modo semplice per generare il testo della riga di comando.|  
  
9. Scegliere **OK** per salvare le impostazioni e chiudere la finestra di dialogo **Nuovo passaggio di processo** .  
  
    > [!NOTE]
    > Per i pacchetti archiviati nel **Catalogo SSIS** il pulsante **OK** è disabilitato se è presente un parametro o un'impostazione della proprietà di gestione connessione non risolto. Un'impostazione non risolta si verifica quando si usa un valore contenuto in una variabile di ambiente server per impostare il parametro o la proprietà e si verifica una delle condizioni seguenti:  
    >   
    >  La casella di controllo **Ambiente** nella scheda **Configurazione** non è selezionata.  
    >   
    >  L'ambiente server che contiene la variabile non è selezionato nella casella di riepilogo della scheda **Configurazione** .  
  
10. Per creare una pianificazione per un passaggio di processo, fare clic su **Pianificazioni** nel riquadro **Selezione pagina** . Per informazioni su come configurare una pianificazione, vedere [Schedule a Job](../../ssms/agent/schedule-a-job.md).  
  
    > [!TIP]  
    >  Quando si assegna un nome alla pianificazione, utilizzare un nome univoco e descrittivo in modo da distinguere più facilmente la pianificazione da altre pianificazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  

## <a name="see-also"></a>Vedere anche  
 [Esecuzione di progetti e pacchetti](deploy-integration-services-ssis-projects-and-packages.md)  

## <a name="external-resources"></a>Risorse esterne  
  
-   Articolo della Knowledge Base [Pacchetto SSIS non viene eseguito quando viene chiamato da un passaggio di processo SQL Server Agent](https://support.microsoft.com/kb/918760)nel sito Web di [!INCLUDE[msCoName](../../includes/msconame-md.md)]  
  
-   Video [Risoluzione dei problemi: Esecuzione di un pacchetto con SQL Server Agent (video di SQL Server)](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100)) in MSDN Library  
  
-   Video [Procedura: Automazione dell'esecuzione di un pacchetto usando SQL Server Agent (video di SQL Server)](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100)) in MSDN Library  
  
-   Articolo tecnico [Checking SQL Server Agent jobs using Windows PowerShell](https://go.microsoft.com/fwlink/?LinkId=165675)(Verifica dei processi di SQL Server Agent tramite Windows PowerShell) su mssqltips.com  
  
-   Articolo tecnico [Auto alert for SQL Agent jobs when they are enabled or disabled](https://go.microsoft.com/fwlink/?LinkId=165676)(Avviso automatico se i processi di SQL Agent sono abilitati o disabilitati) su mssqltips.com  
  
-   Intervento nel blog [Configuring SQL Agent Jobs to Write to Windows Event Log](https://go.microsoft.com/fwlink/?LinkId=220745)(Configurazione dei processi di SQL Agent per la scrittura nel Registro eventi di Windows) su mssqltips.com.  
  
