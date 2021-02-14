---
description: Registrazione di Integration Services (SSIS)
title: Registrazione di Integration Services (SSIS) | Microsoft Docs
ms.custom: supportability
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configuredtslogs.containers.f1
- sql13.dts.designer.configuredtslogs.loggingdetails.f1
- sql13.dts.designer.configuredtslogs.providersandlogs.f1
- SQL13.SSIS.SSMS.ISMANAGECUSTOMIZEDLOGGINGLEVEL.F1
helpviewer_keywords:
- containers [Integration Services], logs
- Windows Event log provider [Integration Services]
- XML File log provider [Integration Services]
- SQL Server Profiler, log provider
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- tasks [Integration Services], logs
- logs [Integration Services], log providers
- Integration Services packages, logs
- log providers [Integration Services]
- packages [Integration Services], logs
- Text File log provider
- SQL Server log provider
ms.assetid: 65e17889-371f-4951-9a7e-9932b2d0dcde
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ac0f913188b23b85548f87a4f458d1c2b754d15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347456"
---
# <a name="integration-services-ssis-logging"></a>Registrazione di Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sono disponibili provider di log che è possibile usare per implementare la registrazione in pacchetti, contenitori e attività. Tramite la registrazione è possibile acquisire informazioni di run-time su un pacchetto, che consentono di controllare e risolvere i problemi del pacchetto ogni volta che viene eseguito. Nel log è ad esempio possibile acquisire il nome dell'operatore che ha eseguito il pacchetto, nonché la data e l'ora di inizio e di fine dell'esecuzione.  
  
 È possibile configurare l'ambito di registrazione che si verifica durante l'esecuzione di un pacchetto nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per altre informazioni, vedere [Abilitare la registrazione per l'esecuzione di pacchetti nel server SSIS](#server_logging)  
  
 Si può anche includere la registrazione quando si esegue un pacchetto con l'utilità del prompt dei comandi **dtexec** . Per ulteriori informazioni sugli argomenti del prompt dei comandi che supportano la registrazione, vedere [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="configure-logging-in-sql-server-data-tools"></a>Configurare la registrazione in SQL Server Data Tools  
 I log sono associati ai pacchetti e vengono configurati a livello del pacchetto. Ogni attività o contenitore di un pacchetto può registrare informazioni in qualsiasi log del pacchetto. Le attività e i contenitori di un pacchetto possono essere abilitati per la registrazione anche se per il pacchetto la registrazione non è stata attivata. È possibile, ad esempio, abilitare la registrazione in un'attività Esegui SQL senza abilitarla nel pacchetto padre. Un pacchetto, un contenitore o un'attività possono registrare voci in più log. È possibile scegliere di abilitare la registrazione solo sul pacchetto oppure su ogni singolo contenitore o attività presente nel pacchetto.  
  
 Quando si aggiunge un log a un pacchetto, è necessario scegliere il provider di log e il percorso del log. Il provider di log specifica il formato dei dati del log, ad esempio un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un file di testo.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sono disponibili i provider di log seguenti:  
  
-   Provider di log File di testo, che scrive le voci di log in file di testo ASCII in formato CSV. L'estensione predefinita dei file per questo provider è log.  
  
-   Provider di log [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , che scrive tracce che è possibile visualizzare utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler. L'estensione predefinita dei file per questo provider è trc.  
  
    > [!NOTE]  
    >  In un pacchetto in esecuzione in modalità a 64 bit non è possibile usare il provider di log di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
-   Provider di log [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , che scrive le voci di log nella tabella **sysssislog** di un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Provider di log Registro eventi di Windows, che scrive le voci nel registro applicazioni del registro eventi di Windows sul computer locale.  
  
-   Provider di log File XML, che scrive file di log in formato XML. L'estensione predefinita dei file per questo provider è xml.  
  
 Se si aggiunge un provider di log a un pacchetto o si configura la registrazione a livello di codice, sarà possibile utilizzare un ProgID o un ClassID per identificare il provider di log, anziché utilizzare i nomi visualizzati in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , nella finestra di dialogo **Configura log SSIS** .  
  
 Nella tabella seguente sono elencati i ProgID e i ClassID per i provider di log disponibili in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e i percorsi dei log in cui scrivono i provider.  
  
|Provider di log|ProgID|ClassID|Location|  
|------------------|------------|-------------|--------------|  
|File di testo|DTS.LogProviderTextFile|{0A039101-ACC1-4E06-943F-279948323883}|La gestione connessione file utilizzata dal provider di log specifica il percorso del file di testo.|  
|SQL Server Profiler|DTS.LogProviderSQLProfiler|{E93F6300-AE0C-4916-A7BF-A8D0CE12C77A}|La gestione connessione file utilizzata dal provider di log specifica il percorso del file utilizzato da [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|SQL Server|DTS.LogProviderSQLServer|{94150B25-6AEB-4C0D-996D-D37D1C4FDEDA}|La gestione connessione OLE DB usata dal provider di log indica il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si trova la tabella sysssislog contenente le voci di log.|  
|Registro eventi di Windows|DTS.LogProviderEventLog|{071CC8EB-C343-4CFF-8D58-564B92FCA3CF}|Il registro applicazioni nel Visualizzatore eventi di Windows contiene le informazioni del log di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|File XML|DTS.LogProviderXMLFile|{440945A4-2A22-4F19-B577-EAF5FDDC5F7A}|La gestione connessione file utilizzata dal provider di log specifica il percorso del file XML.|  
  
 È inoltre possibile creare provider di log personalizzati. Per altre informazioni, vedere [Creating a Custom Log Provider](../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md).  
  
 I provider di log di un pacchetto sono membri della raccolta dei provider di log del pacchetto. Se si crea un pacchetto e si implementa la registrazione tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , sarà possibile visualizzare l'elenco dei membri della raccolta nelle cartelle **Provider di log** della scheda **Esplora pacchetti** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Per configurare un provider di log è necessario specificarne il nome, la descrizione e la gestione connessione utilizzata. Il provider di log [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza una gestione connessione OLE DB. I provider di log File di testo, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]e File XML utilizzano gestioni connessioni file. Il provider di log Registro eventi di Windows non utilizza invece una gestione connessione, perché scrive direttamente nel Registro eventi di Windows. Per ulteriori informazioni, vedere [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) e [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
### <a name="logging-customization"></a>Personalizzazione della registrazione  
 Per la personalizzazione della registrazione di un evento o la creazione di un messaggio personalizzato, in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è disponibile uno schema di informazioni comunemente registrate, che è possibile includere nelle voci di log. Lo schema del log di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] delinea le informazioni che è possibile registrare. È possibile selezionare elementi dallo schema per ogni voce di log.  
  
 Un pacchetto e le attività e i contenitori associati non devono necessariamente registrare le stesse informazioni. Inoltre le singole attività incluse in un pacchetto o contenitore possono registrare informazioni diverse. Ad esempio, un pacchetto potrebbe registrare informazioni relative agli operatori quando viene avviato, una delle attività potrebbe registrare la causa dell'esito negativo dell'attività e un'altra attività potrebbe registrare informazioni quando si verificano errori. Se un pacchetto e le attività e i contenitori associati utilizzano più log, in tutti i log vengono registrate le stesse informazioni.  
  
 È possibile selezionare il livello di registrazione più adatto alle proprie esigenze specificando gli eventi e le informazioni di ogni evento da registrare. A seconda delle specifiche esigenze le informazioni fornite da alcuni eventi potrebbero risultare più utili rispetto a quelle di altri eventi. Potrebbe risultare utile, ad esempio, registrare solo il nome del computer e dell'operatore dell'evento **PreExecute** , ma tutte le informazioni disponibili per l'evento **Error** .  
  
 Per impedire che i file di log utilizzino una quantità di spazio su disco elevata o per evitare un'attività di registrazione eccessiva che potrebbe influire negativamente sulle prestazioni, è possibile selezionare gli eventi e le informazioni da registrare. È possibile, ad esempio, configurare un log in modo che per ogni errore vengano registrati solo il nome del computer e la data.  
  
 Nella finestra di dialogo [!INCLUDE[ssIS](../../includes/ssis-md.md)] Configura log SSIS **di Progettazione** sono disponibili le opzioni di registrazione.  
  
#### <a name="log-schema"></a>Schema del log  
 Nella tabella seguente vengono descritti gli elementi dello schema del log.  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|Computer|Nome del computer in cui è stato generato l'evento.|  
|Operatore|Identifica l'utente che ha avviato il pacchetto.|  
|SourceName|Nome del contenitore o dell'attività in cui è stato generato l'evento.|  
|SourceID|Identificatore univoco del pacchetto, contenitore Ciclo For, Ciclo Foreach o Sequenza oppure attività in cui è stato generato l'evento.|  
|ExecutionID|GUID dell'istanza di esecuzione del pacchetto.<br /><br /> L'esecuzione di un singolo pacchetto potrebbe creare voci di log con valori diversi per l'elemento ExecutionID. Ad esempio, quando si esegue un pacchetto in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], la fase di convalida potrebbe creare voci di log con un elemento ExecutionID che corrisponde a [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. La fase di esecuzione potrebbe invece creare voci di log con un elemento ExecutionID che corrisponde a dtshost.exe. Per fornire un altro esempio, quando si esegue un pacchetto che contiene attività Esegui pacchetto, ognuna di queste attività esegue un pacchetto figlio. Questi pacchetti figlio potrebbero creare voci di log con un elemento ExecutionID diverso rispetto alle voci di log create dal pacchetto.|  
|MessageText|Messaggio associato alla voce di log.|  
|DataBytes|Matrice di byte specifica della voce di log. Il significato di questo campo varia a seconda della voce di log.|  
  
 Nella tabella seguente sono descritti tre elementi aggiuntivi dello schema del log che non sono disponibili nella scheda **Dettagli** della finestra di dialogo **Configura log SSIS** .  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|StartTime|Ora di inizio dell'esecuzione del contenitore o dell'attività.|  
|EndTime|Ora di arresto dell'esecuzione del contenitore o dell'attività.|  
|DataCode|Valore intero facoltativo che in genere contiene un valore dell'enumerazione <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> che indica il risultato dell'esecuzione del contenitore o dell'attività:<br /><br /> 0 - Esito positivo<br /><br /> 1 - Esito negativo<br /><br /> 2 - Esecuzione completata<br /><br /> 3 - Esecuzione annullata|  
  
#### <a name="log-entries"></a>Voci di log  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] supporta voci di log per gli eventi predefiniti e offre voci di log personalizzate per molti oggetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Tali eventi e voci di log personalizzate sono elencati nella finestra di dialogo **Configura log SSIS** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Nella tabella seguente vengono descritti gli eventi predefiniti che è possibile abilitare per scrivere voci di log quando si verificano eventi di run-time. Queste voci sono relative ai file eseguibili, al pacchetto e alle attività e ai contenitori inclusi nel pacchetto. Il nome della voce di log corrisponde al nome dell'evento di run-time che è stato generato e che ha causato la scrittura della voce.  
  
|Eventi|Descrizione|  
|------------|-----------------|  
|**OnError**|Viene inserita una voce del registro quando si verifica un errore.|  
|**OnExecStatusChanged**|Viene scritta una voce del registro quando un'attività (non un contenitore) viene sospesa o ripresa durante il debug.|  
|**OnInformation**|Viene scritta una voce del registro durante la convalida e l'esecuzione di un eseguibile per la segnalazione di informazioni.|  
|**OnPostExecute**|Viene registrata una voce di log non appena l'esecuzione del file eseguibile viene completata.|  
|**OnPostValidate**|Viene registrata una voce di log dopo la convalida del file eseguibile.|  
|**OnPreExecute**|Viene registrata una voce di log immediatamente prima dell'esecuzione del file eseguibile.|  
|**OnPreValidate**|Viene registrata una voce di log all'avvio della convalida del file eseguibile.|  
|**OnProgress**|Viene registrata una voce di log dopo un avanzamento percettibile dell'esecuzione del file eseguibile.|  
|**OnQueryCancel**|Viene registrata una voce di log in qualsiasi momento dell'elaborazione dell'attività in cui è possibile annullare l'esecuzione.|  
|**OnTaskFailed**|Viene registrata una voce di log quando un'attività ha esito negativo.|  
|**OnVariableValueChanged**|Viene registrata una voce di log quando il valore di una variabile viene modificato.|  
|**OnWarning**|Viene registrata una voce di log in corrispondenza di un avviso.|  
|**PipelineComponentTime**|Per ogni componente del flusso di dati, viene registrata una voce di log per ogni fase di convalida ed esecuzione. La voce di log specifica il tempo di elaborazione per ogni fase.|  
|**Diagnostic**<br /><br /> **DiagnosticEx**|Viene registrata una voce di log che fornisce informazioni diagnostiche.<br /><br /> È ad esempio possibile registrare un messaggio prima e dopo ogni chiamata a un provider di dati esterno. Per altre informazioni, vedere [Risoluzione dei problemi relativi agli strumenti per l'esecuzione del pacchetto](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).<br /><br /> Registrare l'evento **DiagnosticEx** quando si vuole trovare i nomi delle colonne per le colonne nel flusso di dati che contengono errori. Questo evento scrive una mappa di derivazione del flusso di dati nel log. È quindi possibile cercare il nome della colonna in questa mappa di derivazione usando l'identificatore della colonna acquisito da un output degli errori. Per altre informazioni, vedere [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> Notare che l'evento **DiagnosticEx** non mantiene gli spazi vuoti nel relativo output XML per ridurre le dimensioni del log. Per migliorare la leggibilità, copiare il log in un editor XML come Visual Studio, che supporta la formattazione XML e l'evidenziazione della sintassi.<br /><br /> Nota: se si registra l'evento **DiagnosticEx** con il provider di log di SQL Server, l'output potrebbe essere troncato. Il campo **message** del provider di log di SQL Server è di tipo nvarchar(2048). Per evitare il troncamento, usare un provider di log diverso quando si registra l'evento **DiagnosticEx** .|  
  
 Per il pacchetto e per molte attività sono disponibili voci di log personalizzate che è possibile abilitare per la registrazione. Per l'attività Invia messaggi è ad esempio disponibile la voce di log personalizzata **SendMailTaskBegin** , che registra informazioni quando l'attività viene avviata, ma prima che invii un messaggio di posta elettronica. Per altre informazioni, vedere [Custom Messages for Logging](#custom_messages).  
  
### <a name="differentiating-package-copies"></a>Differenziazione delle copie di un pacchetto  
 I dati del log includono il nome e il GUID del pacchetto a cui appartengono le voci di log. Se si crea un nuovo pacchetto copiando un pacchetto esistente, verranno copiati anche il nome e il GUID del pacchetto esistente. Possono essere pertanto presenti due pacchetti con nome e GUID uguali e questo può impedire di distinguere tali pacchetti nei dati del log.  
  
 Per eliminare questa ambiguità, è consigliabile modificare il nome e il GUID dei nuovi pacchetti. In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]è possibile rigenerare il GUID nella proprietà **ID** e modificare il valore della proprietà **Name** nella finestra Proprietà. È inoltre possibile modificare il GUID e il nome a livello di codice oppure eseguendo **dtutil** dal prompt dei comandi. Per altre informazioni, vedere [Impostazione delle proprietà di un pacchetto](../../integration-services/set-package-properties.md) e [Utilità dtutil](../../integration-services/dtutil-utility.md).  
  
### <a name="parent-logging-options"></a>Opzioni di registrazione padre  
 Spesso le opzioni di registrazione delle attività e dei contenitori Ciclo For, Ciclo Foreach e Sequenza corrispondono a quelle del pacchetto o di un contenitore padre. In questo caso è possibile configurarle in modo che ereditino l'impostazione delle opzioni di registrazione del contenitore padre. Ad esempio, in un contenitore Ciclo For che include un'attività Esegui SQL l'attività può utilizzare le opzioni di registrazione impostate nel contenitore. Per consentire l'utilizzo delle opzioni di registrazione padre, è necessario impostare la proprietà LoggingMode del contenitore su **UseParentSetting**. Questa impostazione può essere eseguita nella finestra **Proprietà** di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o nella finestra di dialogo **Configura log SSIS** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
### <a name="logging-templates"></a>Modelli di registrazione  
 Nella finestra di dialogo **Configura log SSIS** è inoltre possibile creare e salvare come modelli le configurazioni di registrazione utilizzate di frequente. I modelli possono essere quindi applicati in più pacchetti. Ciò consente di applicare una strategia di registrazione consistente tra più pacchetti e di modificare le impostazione di log dei pacchetti semplicemente aggiornando e applicando i modelli. I modelli vengono archiviati in file XML.  
  
 **Per configurare la registrazione tramite la finestra di dialogo Configura log SSIS**  
  
1.  Abilitare il pacchetto e le attività associate per la registrazione. La registrazione può venire eseguita a livello del pacchetto, del contenitore e dell'attività. È possibile specificare log diversi per pacchetti, contenitori e attività.  
  
2.  Selezionare un provider di log e aggiungere un log per il pacchetto. È possibile creare log solo a livello di pacchetto. Inoltre attività o contenitori devono utilizzare uno dei log creati per il pacchetto. I possibili provider di log a cui può essere associato un log sono file di testo, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], registro eventi di Windows o file XML. Per altre informazioni, vedere [Abilitare la registrazione di pacchetti in SQL Server Data Tools](#ssdt).  
  
3.  Selezionare gli eventi e le informazioni dello schema del registro relative a ogni evento che si desidera registrare. Per altre informazioni, vedere [Configurazione della registrazione tramite un file di configurazione salvato](#saved_config).  
  
### <a name="configuration-of-log-provider"></a>Configurazione del provider di log  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 La creazione e la configurazione del provider di log avviene in un passaggio dell'implementazione della registrazione in un pacchetto.  
  
 Dopo avere creato un provider di log è possibile visualizzarne e modificarne le proprietà nella finestra Proprietà di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Per informazioni sull'impostazione di queste proprietà a livello di programmazione, vedere la documentazione per la classe <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> .  
  
### <a name="logging-for-data-flow-tasks"></a>Registrazione per le attività Flusso di dati  
 L'attività Flusso di dati offre molte voci di log personalizzate che è possibile utilizzare per monitorare e regolare le prestazioni. È ad esempio possibile monitorare i componenti che potrebbero causare perdite di memoria o tenere traccia del tempo necessario per eseguire un componente specifico. Per un elenco di queste voci di log personalizzate e un output di registrazione di esempio, vedere [Data Flow Task](../../integration-services/control-flow/data-flow-task.md).  
  
#### <a name="capture-the-names-of-columns-in-which-errors-occur"></a>Acquisire i nomi delle colonne cui si verificano errori  
 Quando si configura un output di errore errori nel flusso di dati, per impostazione predefinita l'output di errore fornisce solo l'identificatore numerico della colonna in cui si è verificato l'errore. Per altre informazioni, vedere [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md).  
  
 È possibile trovare i nomi di colonna abilitando la registrazione e selezionando l'evento **DiagnosticEx** . Questo evento scrive una mappa di derivazione del flusso di dati nel log. È quindi possibile cercare il nome della colonna dal relativo identificatore in questa mappa di derivazione. Notare che l'evento **DiagnosticEx** non mantiene gli spazi vuoti nel relativo output XML per ridurre le dimensioni del log. Per migliorare la leggibilità, copiare il log in un editor XML come Visual Studio, che supporta la formattazione XML e l'evidenziazione della sintassi.  
  
#### <a name="use-the-pipelinecomponenttime-event"></a>Utilizzo dell'evento PipelineComponentTime  
 La voce di log personalizzata più utile è probabilmente l'evento PipelineComponentTime. Questa voce di log indica il numero di millisecondi che ogni componente del flusso di dati dedica a ognuno dei cinque passaggi principali dell'elaborazione. Nella tabella seguente vengono descritti i passaggi di elaborazione. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Gli sviluppatori di Integration Services riconosceranno tali passaggi come i metodi principali di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>.  
  
|Passaggio|Descrizione|  
|----------|-----------------|  
|Convalida|Il componente verifica la presenza di impostazioni di configurazione e valori di proprietà validi.|  
|PreExecute|Il componente esegue un'unica elaborazione prima di iniziare a elaborare le righe di dati.|  
|PostExecute|Il componente esegue un'unica elaborazione dopo avere elaborato tutte le righe di dati.|  
|ProcessInput|Il componente di destinazione o di trasformazione elabora le righe di dati in ingresso ricevute da un'origine o da una trasformazione a monte.|  
|PrimeOutput|Il componente di origine o di trasformazione riempie i buffer di dati da passare a un componente di destinazione o di trasformazione a valle.|  
  
 Quando si abilita l'evento PipelineComponentTime, in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene registrato un messaggio per ogni passaggio dell'elaborazione eseguito da ogni componente. Nelle voci di log seguenti viene illustrato un subset dei messaggi registrati dal pacchetto di esempio CalculatedColumns di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
 `The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`  
  
 `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`  
  
 `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`  
  
 `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`  
  
 `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`  
  
 `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`  
  
 Queste voci di log indicano che l'attività Flusso di dati ha dedicato la maggior parte del tempo ai passaggi seguenti, riportati in ordine decrescente:  
  
-   L'origine OLE DB denominata "Extract Data" ha dedicato 688 ms al caricamento dei dati.  
  
-   La trasformazione Colonna derivata denominata "Calculate LineItemTotalCost" ha dedicato 356 ms per l'esecuzione dei calcoli sulle righe in ingresso.  
  
-   La trasformazione Aggregazione denominata "Sum Quantity and LineItemTotalCost" ha dedicato in tutto 220 ms (141 per PrimeOutput e 79 per ProcessInput) per l'esecuzione di calcoli e il passaggio di dati alla trasformazione successiva.  

## <a name="enable-package-logging-in-sql-server-data-tools"></a><a name="ssdt"></a> Abilitare la registrazione di pacchetti in SQL Server Data Tools
  In questo argomento viene descritta la procedura per aggiungere log in un pacchetto, configurare la registrazione a livello di pacchetto e salvare la configurazione di registrazione in un file XML. È possibile aggiungere log solo a livello di pacchetto. Il pacchetto, tuttavia, non deve eseguire necessariamente la registrazione per consentire la registrazione nei contenitori del pacchetto.  
  
> [!IMPORTANT]  
>  Se si distribuisce il progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , il livello di registrazione impostato per l'esecuzione del pacchetto esegue l'override della registrazione del pacchetto configurata mediante [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Per impostazione predefinita, i contenitori del pacchetto utilizzano la stessa configurazione di registrazione del contenitore padre. Per informazioni sull'impostazione delle opzioni di registrazione per singoli contenitori, vedere [Configurazione della registrazione tramite un file di configurazione salvato](#saved_config).  
  
### <a name="to-enable-logging-in-a-package"></a>Per abilitare la registrazione in un pacchetto  
  
1.  In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  Scegliere **Registrazione** dal menu **SSIS**.  
  
3.  Selezionare un provider di log nell'elenco **Tipo provider** e quindi fare clic su **Aggiungi**.  
  
4.  Nella colonna **Configurazione** selezionare una gestione connessione oppure fare clic su **\<New connection>** per creare una nuova gestione connessione del tipo appropriato per il provider di log. A seconda del provider selezionato, utilizzare una delle gestioni connessioni seguenti:  
  
    -   Per file di testo utilizzare una gestione connessione file. Per altre informazioni, vedere [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
    -   Per [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]usare una gestione connessione file.  
  
    -   Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]utilizzare una gestione connessione OLE DB. Per altre informazioni, vedere [Gestione connessione OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
    -   Per il Registro eventi di Windows, non eseguire alcuna operazione. [!INCLUDE[ssIS](../../includes/ssis-md.md)] crea automaticamente il registro.  
  
    -   Per file XML utilizzare una gestione connessione file.  
  
5.  Ripetere i passaggi 3 e 4 per ogni log da utilizzare nel pacchetto.  
  
    > [!NOTE]  
    >  In un pacchetto possono venire utilizzati più log di ognuno dei tipi di log.  
  
6.  Facoltativamente, selezionare la casella di controllo a livello di pacchetto, selezionare i log da usare per la registrazione a livello di pacchetto e quindi fare clic sulla scheda **Dettagli** .  
  
7.  Nella scheda **Dettagli** selezionare **Eventi** per registrare tutte le voci di log oppure deselezionare l'opzione **Eventi** se si desidera selezionare singoli eventi.  
  
8.  Facoltativamente, fare clic su **Avanzate** per specificare le informazioni da registrare.  
  
    > [!NOTE]  
    >  Per impostazione predefinita vengono registrate tutte le informazioni.  
  
9. Nella scheda **Dettagli** fare clic su **Salva**. Verrà visualizzata la finestra di dialogo **Salva con nome** . Individuare la cartella in cui salvare la configurazione di registrazione, digitare un nome di file per la nuova configurazione e quindi fare clic su **Salva**.  
  
10. Fare clic su **OK**.  
  
11. Per salvare il pacchetto aggiornato, scegliere **Salva elementi selezionati** dal menu **File** .  

## <a name="configure-ssis-logs-dialog-box"></a><a name="configure_logs"></a> Finestra di dialogo Configura log SSIS
  Utilizzare la finestra di dialogo **Configura log SSIS** per definire le opzioni di registrazione per un pacchetto.  
  
 **Per saperne di più**  
  
1.  [Apertura della finestra di dialogo Configura log SSIS](#open_dialog)  
  
2.  [Configurazione delle opzioni nel riquadro Contenitori](#container)  
  
3.  [Configurazione delle opzioni nella scheda Provider e log](#provider)  
  
4.  [Configurazione delle opzioni nella scheda Dettagli](#detail)  
  
###  <a name="open-the-configure-ssis-logs-dialog-box"></a><a name="open_dialog"></a> Apertura della finestra di dialogo Configura log SSIS  
 **Per aprire la finestra di dialogo Configura log SSIS**  
  
-   In Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] scegliere **Registrazione** nel menu **SSIS** .  
  
###  <a name="configure-the-options-in-the-containers-pane"></a><a name="container"></a> Configurazione delle opzioni nel riquadro Contenitori  
 Utilizzare il riquadro **Contenitori** della finestra di dialogo **Configura log SSIS** per abilitare il pacchetto e i relativi contenitori per la registrazione.  
  
#### <a name="options"></a>Opzioni  
 **Contenitori**  
 Nella visualizzazione gerarchica selezionare le caselle di controllo in modo da abilitare il pacchetto e i relativi contenitori per la registrazione:  
  
-   Se deselezionato, il contenitore non è abilitato per la registrazione. Selezionarlo per abilitare la registrazione.  
  
-   Se visualizzato in grigio, il contenitore utilizza le opzioni di registrazione dell'elemento padre. Questa opzione non è disponibile per il pacchetto.  
  
-   Se selezionato, il contenitore definisce opzioni di registrazione specifiche.  
  
 Se si desidera impostare le opzioni di registrazione per un contenitore visualizzato in grigio, fare doppio clic sulla casella di controllo corrispondente al contenitore in questione. Al primo clic la casella di controllo viene deselezionata e al secondo clic viene selezionata, consentendo di scegliere il provider di log da utilizzare e di specificare le informazioni da registrare.  
  
###  <a name="configure-the-options-on-the-providers-and-logs-tab"></a><a name="provider"></a> Configurazione delle opzioni nella scheda Provider e log  
 Usare la scheda **Provider e log** della finestra di dialogo **Configura log SSIS** per creare e configurare log per l'acquisizione di eventi di runtime.  
  
#### <a name="options"></a>Opzioni  
 **Tipo provider**  
 Consente di selezionare un tipo di logger nell'elenco.  
  
 **Aggiungere**  
 Consente di aggiungere un log del tipo specificato alla raccolta di logger del pacchetto.  
  
 **Nome**  
 Consente di abilitare o disabilitare log per contenitori o attività selezionati nel riquadro **Contenitori** della finestra di dialogo **Configura log SSIS** usando le caselle di controllo. Il campo del nome è modificabile. Utilizzare il nome predefinito per il provider oppure digitare un nome descrittivo univoco.  
  
 **Descrizione**  
 Il campo della descrizione è modificabile. Fare clic nel campo e quindi modificare la descrizione predefinita del log.  
  
 **Configurazione**  
 Selezionare una gestione connessione esistente nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione. A seconda del tipo di logger, è possibile configurare una gestione connessione OLE DB o una gestione connessione file. Il logger per il registro eventi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] non necessita di connessioni.  
  
 Argomenti correlati: [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) e [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **Elimina**  
 Selezionare un provider di log e fare clic su **Elimina**.  
  
###  <a name="configure-the-options-on-the-details-tab"></a><a name="detail"></a> Configurazione delle opzioni nella scheda Dettagli  
 Utilizzare la scheda **Dettagli** della finestra di dialogo **Configura log SSIS** per indicare gli eventi da abilitare per la registrazione e specificare le informazioni da registrare. Le informazioni selezionate saranno valide per tutti i provider di log nel pacchetto. Non è ad esempio possibile scrivere informazioni nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diverse da quelle specificate in un file di testo.  
  
#### <a name="options"></a>Opzioni  
 **Eventi**  
 Consente di abilitare o disabilitare gli eventi per la registrazione.  
  
 **Descrizione**  
 Consente di visualizzare una descrizione dell'evento.  
  
 **Funzionalità avanzate**  
 Consente di selezionare o deselezionare gli eventi da registrare, nonché le informazioni da registrare per ogni evento. Fare clic su **Standard** per nascondere tutti i dettagli di registrazione, ad eccezione dell'elenco di eventi. Per la registrazione sono disponibili le informazioni seguenti:  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Computer**|Nome del computer in cui si è verificato l'evento registrato.|  
|**Operatore**|Nome utente dell'utente che ha avviato il pacchetto.|  
|**SourceName**|Nome del pacchetto, contenitore o attività in cui si è verificato l'evento registrato.|  
|**SourceID**|Identificatore univoco globale (GUID, Global Unique Identifier) del pacchetto, contenitore o attività in cui si è verificato l'evento registrato.|  
|**ExecutionID**|Identificatore univoco globale dell'istanza di esecuzione del pacchetto.|  
|**MessageText**|Messaggio associato alla voce di log.|  
|**DataBytes**|Riservato per utilizzi futuri.|  
  
 **Base**  
 Consente di selezionare o deselezionare gli eventi da registrare. Questa opzione può essere utilizzata per nascondere i dettagli di registrazione, ad eccezione dell'elenco di eventi. Se si seleziona un evento, per impostazione predefinita verranno selezionati anche tutti i relativi dettagli di registrazione. Fare clic su **Avanzate** per visualizzarli.  
  
 **Load**  
 Consente di specificare un file XML esistente da utilizzare come modello per l'impostazione delle opzioni di registrazione.  
  
 **Salva**  
 Consente di salvare i dettagli di configurazione come modello in un file XML.  

## <a name="configure-logging-by-using-a-saved-configuration-file"></a><a name="saved_config"></a> Configurazione della registrazione tramite un file di configurazione salvato
  In questo argomento viene descritta la procedura per configurare la registrazione per nuovi contenitori di un pacchetto semplicemente caricando un file di configurazione della registrazione.  
  
 Per impostazione predefinita, tutti i contenitori di un pacchetto utilizzano la stessa configurazione di registrazione del contenitore padre. Le attività del contenitore Ciclo Foreach, ad esempio, utilizzano la stessa configurazione di registrazione del contenitore.  
  
### <a name="to-configure-logging-for-a-container"></a>Per configurare la registrazione per un contenitore  
  
1.  In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  Scegliere **Registrazione** dal menu **SSIS**.  
  
3.  Espandere la visualizzazione albero dei pacchetti e selezionare il contenitore da configurare.  
  
4.  Nella scheda **Provider e log** selezionare i log da usare per il contenitore.  
  
    > [!NOTE]  
    >  È possibile creare log solo a livello di pacchetto. Per altre informazioni, vedere [Abilitare la registrazione di pacchetti in SQL Server Data Tools](#ssdt).  
  
5.  Nella scheda **Dettagli** fare clic su **Carica**.  
  
6.  Individuare il file di configurazione della registrazione desiderato e quindi fare clic su **Apri**.  
  
7.  Facoltativamente, selezionare una voce di log diversa selezionando la casella di controllo corrispondente nella colonna **Eventi** . Fare clic su **Avanzate** per selezionare il tipo di informazioni da registrare per tale voce.  
  
    > [!NOTE]  
    >  Il nuovo contenitore potrebbe includere voci di log aggiuntive non disponibili per il contenitore inizialmente utilizzato per creare la configurazione della registrazione. Se si desidera registrare queste voci, è necessario selezionarle in modo manuale.  
  
8.  Per salvare la configurazione della registrazione aggiornata, fare clic su **Salva**.  
  
9. Per salvare il pacchetto aggiornato, scegliere **Salva elementi selezionati** dal menu **File** .  

## <a name="enable-logging-for-package-execution-on-the-ssis-server"></a><a name="server_logging"></a> Abilitare la registrazione per l'esecuzione del pacchetto nel Server SSIS
  Questo argomento descrive come impostare o modificare il livello di registrazione per un pacchetto quando si esegue un pacchetto che è stato distribuito nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Il livello di registrazione impostato quando si esegue il pacchetto sostituisce il livello di registrazione del pacchetto configurato in fase di progettazione in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Vedere [Abilitare la registrazione di pacchetti in SQL Server Data Tools](#ssdt) per altre informazioni.  
  
 In **Proprietà server** di SQL Server, nella proprietà **Server logging level** (Livello di registrazione del server), è possibile selezionare un livello di registrazione predefinito per l'intero server. È possibile scegliere uno dei livelli di registrazione predefiniti descritti in questo argomento oppure è possibile selezionare un livello di registrazione personalizzato esistente. Il livello di registrazione selezionato viene applicato per impostazione predefinita a tutti i pacchetti distribuiti nel catalogo SSIS. Si applica anche per impostazione predefinita a un passaggio del processo di SQL Agent che esegue un pacchetto SSIS.  
  
 È anche possibile specificare il livello di registrazione per un singolo pacchetto con uno dei metodi indicati di seguito. In questo argomento viene illustrato il primo metodo.  
  
-   Configurazione di un'istanza di esecuzione di un pacchetto tramite la finestra di dialogo Esegui pacchetto  
  
-   Impostazione dei parametri per un'istanza di esecuzione usando il valore [catalog.set_execution_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
  
-   Configurazione di un processo di SQL Server Agent per l'esecuzione di un pacchetto tramite la finestra di dialogo Nuovo passaggio di processo.  
  
### <a name="set-the-logging-level-for-a-package-by-using-the-execute-package-dialog-box"></a>Per impostare il livello di registrazione per un pacchetto mediante la finestra di dialogo Esegui pacchetto  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], passare al pacchetto in Esplora oggetti.  
  
2.  Fare clic con il pulsante destro del mouse sul pacchetto e selezionare **Esegui**.  
  
3.  Selezionare la scheda **Avanzate** nella finestra di dialogo **Esecuzione pacchetto** .  
  
4.  In **Livello di registrazione**, selezionare il livello di registrazione. Questo argomento contiene una descrizione dei valori disponibili.  
  
5.  Completare le eventuali altre configurazione pacchetto, quindi fare clic su **OK** per eseguire il pacchetto.  
  
### <a name="select-a-logging-level"></a>Selezionare un livello di registrazione  
 Sono disponibili i livelli di registrazione predefiniti seguenti. È anche possibile selezionare un livello di registrazione personalizzato esistente. Questo argomento contiene una descrizione dei livelli di registrazione personalizzati.  
  
|Livello di registrazione|Descrizione|  
|-------------------|-----------------|  
|nessuno|La registrazione è disabilitata. Solo lo stato dell'esecuzione del pacchetto viene registrato.|  
|Basic|Tutti gli eventi sono registrati, ad eccezione di eventi personalizzati e di diagnostica. Si tratta del valore predefinito.|  
|RuntimeLineage|Raccoglie i dati necessari a tenere traccia delle informazioni di derivazione nel flusso di dati. È possibile analizzare queste informazioni di derivazione per mappare la relazione di derivazione tra attività. Gli ISV e sviluppatori possono creare strumenti personalizzati di mapping della derivazione con queste informazioni.|  
|Prestazioni|Vengono registrati solo le statistiche sulle prestazioni e gli eventi OnError e OnWarning.<br /><br /> Nel report **Prestazioni di esecuzione** vengono visualizzati il tempo di attività e il tempo totale per i componenti flusso di dati del pacchetto. Queste informazioni sono disponibili se il livello di registrazione dell'ultima esecuzione del pacchetto è stato impostato su **Prestazioni** o **Dettagliato**. Per altre informazioni, vedere [report per il server Integration Services](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).<br /><br /> La vista [catalog.execution_component_phases](../../integration-services/system-views/catalog-execution-component-phases.md) visualizza le ore di inizio e di fine per i componenti flusso di dati, per ogni fase di esecuzione. In questa vista vengono visualizzate le informazioni per i componenti solo quando il livello di registrazione dell'esecuzione del pacchetto è impostato su **Prestazioni** o **Dettagliato**.|  
|Dettagliato|Tutti gli eventi vengono registrati, inclusi gli eventi personalizzati e di diagnostica.<br /><br /> Gli eventi personalizzati includono quelli registrati dalle attività di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per altre informazioni sugli eventi personalizzati, vedere [Messaggi personalizzati per la registrazione](#custom_messages).<br /><br /> L'evento **DiagnosticEx** rappresenta un esempio di un evento di diagnostica. Ogni volta che un'attività Esegui pacchetto esegue un pacchetto figlio, l'evento acquisisce i valori dei parametri passati ai pacchetti figlio.<br /><br /> L'evento **DiagnosticEx** consente inoltre di ottenere i nomi delle colonne in cui si verificano errori a livello di riga. Questo evento scrive una mappa di derivazione del flusso di dati nel log. È quindi possibile cercare il nome della colonna in questa mappa di derivazione usando l'identificatore della colonna acquisito da un output degli errori.  Per altre informazioni, vedere [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> Il valore della colonna di messaggio per **DiagnosticEx** è testo XML. Per visualizzare il testo del messaggio per l'esecuzione del pacchetto, eseguire una query nella vista [catalog.operation_messages &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Notare che l'evento **DiagnosticEx** non mantiene gli spazi vuoti nel relativo output XML per ridurre le dimensioni del log. Per migliorare la leggibilità, copiare il log in un editor XML come Visual Studio, che supporta la formattazione XML e l'evidenziazione della sintassi.<br /><br /> Nella vista [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) viene visualizzata una riga ogni volta che un componente flusso di dati invia dati a un componente downstream, per l'esecuzione di un pacchetto. Il livello di registrazione deve essere impostato su **Dettagliato** per acquisire queste informazioni nella vista.|  
  
### <a name="create-and-manage-customized-logging-levels-by-using-the-customized-logging-level-management-dialog-box"></a>Creare e gestire i livelli di registrazione personalizzati con la finestra di dialogo Gestione del livello di registrazione personalizzato  
 È possibile creare livelli di registrazione personalizzati che raccolgono solo le statistiche e gli eventi desiderati. Facoltativamente, è anche possibile acquisire il contesto degli eventi, che include i valori delle variabili, le stringhe di connessione e le proprietà del componente. Quando si esegue un pacchetto, è possibile selezionare un livello di registrazione personalizzato in tutti i casi in cui è possibile selezionare un livello di registrazione predefinito.  
  
> [!TIP]  
>  Per acquisire i valori delle variabili del pacchetto, la proprietà **IncludeInDebugDump** delle variabili deve essere impostata su **True**.  
  
1.  Per creare e gestire i livelli di registrazione personalizzati, in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]fare clic con il pulsante destro del mouse sul database SSISDB e scegliere **Livello di registrazione personalizzato** per aprire la finestra di dialogo **Gestione del livello di registrazione personalizzato** . L'elenco **Livelli di registrazione personalizzati** contiene tutti i livelli di registrazione personalizzati esistenti.  
  
2.  Per **creare** un nuovo livello di registrazione personalizzato, fare clic su **Crea**, quindi specificare un nome e una descrizione. Nelle schede **Statistiche** ed **Eventi** selezionare le statistiche e gli eventi da raccogliere. Nella scheda **Eventi** selezionare facoltativamente **Includi contesto** per singoli eventi. Fare quindi clic su **Salva**.  
  
3.  Per **aggiornare** un livello di registrazione personalizzato esistente, selezionarlo nell'elenco, riconfigurarlo, quindi fare clic su **Salva**.  
  
4.  Per **eliminare** un livello di registrazione personalizzato esistente, selezionarlo nell'elenco, quindi fare clic su **Elimina**.  
  
 **Autorizzazioni per i livelli di registrazione personalizzati.**  
  
-   Tutti gli utenti del database SSISDB possono visualizzare i livelli di registrazione personalizzati e selezionare un livello di registrazione personalizzato quando eseguono i pacchetti.  
  
-   Solo gli utenti nel ruolo ssis_admin o sysadmin possono creare, aggiornare o eliminare i livelli di registrazione personalizzati.  

## <a name="custom-messages-for-logging"></a><a name="custom_messages"></a> Custom Messages for Logging
SQL Server Integration Services offre numerosi eventi personalizzati per la scrittura di voci di log per i pacchetti e per molte attività. È possibile utilizzare tali voci per salvare informazioni dettagliate su stato di esecuzione, risultati e problemi, tramite la registrazione di eventi predefiniti o messaggi definiti dall'utente da analizzare in un secondo momento. È ad esempio possibile registrare la data e l'ora di inizio e di fine di un'operazione di inserimento bulk per identificare problemi di prestazioni durante l'esecuzione del pacchetto.  
  
 Le voci di log personalizzate costituiscono un set diverso da quello degli eventi di registrazione standard, disponibili per i pacchetti e per tutti i contenitori e le attività. Le voci di log personalizzate vengono create appositamente per acquisire informazioni utili su specifiche attività di un pacchetto. Per l'attività Esegui SQL è ad esempio disponibile una voce di log personalizzata che registra nel log l'istruzione SQL eseguita dall'attività.  
  
 Tutte le voci di log includono informazioni di data e ora, comprese le voci di log scritte automaticamente all'inizio e alla fine dell'esecuzione di un pacchetto. Per molti eventi vengono scritte più voci nel log. Questo avviene in genere per gli eventi che includono varie fasi. Per l'evento **ExecuteSQLExecutingQuery** , ad esempio, vengono scritte tre voci di log: una dopo l'acquisizione di una connessione al database da parte dell'attività, una dopo l'inizio della preparazione dell'istruzione SQL da parte dell'attività e un'altra al termine dell'esecuzione dell'istruzione SQL.  
  
 Sono disponibili voci di log personalizzate per gli oggetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] seguenti:  
  
 [Pacchetto](#Package)  
  
 [Attività Inserimento bulk](#BulkInsert)  
  
 [Attività Flusso di dati](#DataFlow)  
  
 [Attività Esegui pacchetto DTS 2000](#ExecuteDTS200)  
  
 [Attività Esegui processo](#ExecuteProcess)  
  
 [Attività Esegui SQL](#ExecuteSQL)  
  
 [Attività File system](#FileSystem)  
  
 [Attività FTP](#FTP)  
  
 [Attività Message Queue](#MessageQueue)  
  
 [Attività Script](#Script)  
  
 [Attività Invia messaggi](#SendMail)  
  
 [Attività Trasferisci database](#TransferDatabase)  
  
 [Attività Trasferisci messaggi di errore](#TransferErrorMessages)  
  
 [Attività Trasferisci processi](#TransferJobs)  
  
 [Attività Trasferisci account di accesso](#TransferLogins)  
  
 [Attività Trasferisci stored procedure master](#TransferMasterStoredProcedures)  
  
 [Attività Trasferisci oggetti di SQL Server](#TransferSQLServerObjects)  
  
 [Attività Servizio Web](#WebServices)  
  
 [Attività Lettore di dati WMI](#WMIDataReader)  
  
 [Attività Monitoraggio eventi WMI](#WMIEventWatcher)  
  
 [Attività XML](#XML)  
  
### <a name="log-entries"></a>Voci di log  
  
####  <a name="package"></a><a name="Package"></a> Pacchetto  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per i pacchetti.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**PackageStart**|Indica che l'esecuzione del pacchetto è iniziata. Questa voce di log viene scritta automaticamente nel log e non può essere esclusa.|  
|**Fine pacchetto**|Indica che l'esecuzione del pacchetto è stata completata. Questa voce di log viene scritta automaticamente nel log e non può essere esclusa.|  
|**Diagnostic**|Offre informazioni sulla configurazione del sistema che influisce sull'esecuzione dei pacchetti, ad esempio il numero di file eseguibili che è possibile eseguire simultaneamente.<br /><br /> La voce di log **Diagnostic** include anche le voci precedenti e seguenti alle chiamate a provider di dati esterni.|  
  
####  <a name="bulk-insert-task"></a><a name="BulkInsert"></a> Attività Inserimento bulk  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Inserimento bulk.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**DTSBulkInsertTaskBegin**|Indica che l'inserimento bulk è iniziato.|  
|**DTSBulkInsertTaskEnd**|Indica che l'inserimento bulk è terminato.|  
|**DTSBulkInsertTaskInfos**|Offre informazioni descrittive sull'attività.|  
  
####  <a name="data-flow-task"></a><a name="DataFlow"></a> Data Flow Task  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Flusso di dati.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**BufferSizeTuning**|Indica che l'attività Flusso di dati ha modificato le dimensioni del buffer. In questa voce di log vengono indicati i motivi della modifica delle dimensioni del buffer e le nuove dimensioni temporanee del buffer.|  
|**OnPipelinePostEndOfRowset**|Indica che a un componente è stato inviato il segnale di fine del set di righe, che viene impostato dall'ultima chiamata al metodo **ProcessInput** . Viene scritta una voce per ogni componente del flusso di dati che elabora dati di input. Tale voce include il nome del componente.|  
|**OnPipelinePostPrimeOutput**|Indica che il componente ha completato l'ultima chiamata al metodo **PrimeOutput** . A seconda del flusso di dati, è possibile che vengano scritte più voci di log. Se il componente è un'origine, indica che tale componente ha terminato l'elaborazione delle righe.|  
|**OnPipelinePreEndOfRowset**|Indica che un componente sta per ricevere il segnale di fine del set di righe, che viene impostato dall'ultima chiamata al metodo **ProcessInput** . Viene scritta una voce per ogni componente del flusso di dati che elabora dati di input. Tale voce include il nome del componente.|  
|**OnPipelinePrePrimeOutput**|Indica che il componente sta per ricevere una chiamata dal metodo **PrimeOutput** . A seconda del flusso di dati, è possibile che vengano scritte più voci di log.|  
|**OnPipelineRowsSent**|Specifica il numero delle righe inviate all'input di un componente da una chiamata al metodo **ProcessInput** . La voce di log include il nome del componente.|  
|**PipelineBufferLeak**|Fornisce informazioni su tutti i componenti che hanno mantenuto attivi i buffer dopo la chiusura di Gestione buffer. Questo significa che le risorse dei buffer non sono state rilasciate e potrebbero verificarsi perdite di memoria. Nella voce di log vengono indicati il nome del componente e l'ID del buffer.|  
|**PipelineExecutionPlan**|Specifica il piano di esecuzione del flusso di dati. Fornisce informazioni sulle modalità di invio dei buffer ai componenti. Insieme alla voce PipelineExecutionTrees, queste informazioni illustrano ciò che avviene nell'ambito dell'attività.|  
|**PipelineExecutionTrees**|Specifica gli alberi di esecuzione del layout nel flusso di dati. L'utilità di pianificazione del motore flusso di dati utilizza tali alberi per compilare il piano di esecuzione del flusso di dati.|  
|**PipelineInitialization**|Fornisce le informazioni di inizializzazione relative all'attività, che includono le directory da utilizzare per l'archiviazione temporanea dei dati BLOB, le dimensioni predefinite del buffer e il numero di righe in un buffer. A seconda della configurazione dell'attività Flusso di dati, è possibile che vengano scritte più voci di log.|  
  
####  <a name="execute-dts-2000-task"></a><a name="ExecuteDTS200"></a> Attività Esegui pacchetto DTS 2000  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Esegui pacchetto DTS 2000.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**ExecuteDTS80PackageTaskBegin**|Indica che l'attività ha iniziato a eseguire un pacchetto DTS 2000.|  
|**ExecuteDTS80PackageTaskEnd**|Indica che l'attività è terminata.<br /><br /> Nota: l'esecuzione del pacchetto DTS 2000 può continuare anche dopo il termine dell'attività.|  
|**ExecuteDTS80PackageTaskTaskInfo**|Offre informazioni descrittive sull'attività.|  
|**ExecuteDTS80PackageTaskTaskResult**|Restituisce il risultato dell'esecuzione del pacchetto DTS 2000 eseguito dall'attività.|  
  
####  <a name="execute-process-task"></a><a name="ExecuteProcess"></a> Attività Esegui processo  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Esegui processo.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Fornisce informazioni sul processo di esecuzione del file eseguibile che l'attività dovrà eseguire.<br /><br /> Vengono scritte due voci di log. Una contiene informazioni sul nome e la posizione del file eseguibile eseguito dall'attività, l'altra registra l'uscita dall'eseguibile.|  
|**ExecuteProcessVariableRouting**|Fornisce informazioni sulle variabili indirizzate all'input e agli output del file eseguibile. Vengono scritte voci di log per stdin (l'input), stdout (l'output) e stderr (l'output degli errori).|  
  
####  <a name="execute-sql-task"></a><a name="ExecuteSQL"></a> Attività Esegui SQL  
 Nella tabella seguente è indicata la voce di log personalizzata disponibile per l'attività Esegui SQL.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**ExecuteSQLExecutingQuery**|Fornisce informazioni sulle fasi di esecuzione dell'istruzione SQL. Vengono scritte voci di log quando l'attività acquisisce la connessione al database, quando inizia a preparare l'istruzione SQL e al termine dell'esecuzione dell'istruzione SQL. La voce di log per la fase di preparazione include l'istruzione SQL utilizzata dall'attività.|  
  
####  <a name="file-system-task"></a><a name="FileSystem"></a> Attività File system  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività File system.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**FileSystemOperation**|Indica l'operazione eseguita dall'attività. Questa voce di log viene scritta all'inizio dell'operazione sul file system e include informazioni sull'origine e sulla destinazione.|  
  
####  <a name="ftp-task"></a><a name="FTP"></a> Attività FTP  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività FTP.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Indica che l'attività ha stabilito una connessione al server FTP.|  
|**FTPOperation**|Specifica l'inizio e il tipo dell'operazione FTP eseguita dall'attività.|  
  
####  <a name="message-queue-task"></a><a name="MessageQueue"></a> Attività Message Queue  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Message Queue.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Indica che l'attività ha terminato l'apertura della coda di messaggi.|  
|**MSMQBeforeOpen**|Indica che l'attività ha iniziato ad aprire la coda di messaggi.|  
|**MSMQBeginReceive**|Indica che l'attività ha iniziato a ricevere un messaggio.|  
|**MSMQBeginSend**|Indica che l'attività ha iniziato a inviare un messaggio.|  
|**MSMQEndReceive**|Indica che l'attività ha terminato la ricezione di un messaggio.|  
|**MSMQEndSend**|Indica che l'attività ha terminato l'invio di un messaggio.|  
|**MSMQTaskInfo**|Offre informazioni descrittive sull'attività.|  
|**MSMQTaskTimeOut**|Indica che si è verificato il timeout dell'attività.|  
  
####  <a name="script-task"></a><a name="Script"></a> Attività Script  
 Nella tabella seguente è indicata la voce di log personalizzata disponibile per l'attività Script.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Restituisce i risultati dell'implementazione della registrazione nell'ambito dello script. Viene scritta una voce di log per ogni chiamata al metodo **Log** dell'oggetto **Dts** . Tale voce viene scritta al momento dell'esecuzione del codice. Per altre informazioni, vedere [Registrazione nell'attività Script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
####  <a name="send-mail-task"></a><a name="SendMail"></a> Attività Invia messaggi  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Invia messaggi.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Indica che l'attività ha iniziato a inviare un messaggio di posta elettronica.|  
|**SendMailTaskEnd**|Indica che l'attività ha terminato l'invio di un messaggio di posta elettronica.|  
|**SendMailTaskInfo**|Offre informazioni descrittive sull'attività.|  
  
####  <a name="transfer-database-task"></a><a name="TransferDatabase"></a> Attività Trasferisci database  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Trasferisci database.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**SourceDB**|Specifica il database copiato dall'attività.|  
|**SourceSQLServer**|Specifica il computer da cui è stato copiato il database.|  
  
####  <a name="transfer-error-messages-task"></a><a name="TransferErrorMessages"></a> Attività Trasferisci messaggi di errore  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Trasferisci messaggi di errore.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**TransferErrorMessagesTaskFinishedTransferringObjects**|Indica che l'attività ha terminato il trasferimento dei messaggi di errore.|  
|**TransferErrorMessagesTaskStartTransferringObjects**|Indica che l'attività ha iniziato a trasferire messaggi di errore.|  
  
####  <a name="transfer-jobs-task"></a><a name="TransferJobs"></a> Attività Trasferisci processi  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Trasferisci processi.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**TransferJobsTaskFinishedTransferringObjects**|Indica che l'attività ha terminato il trasferimento dei processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|  
|**TransferJobsTaskStartTransferringObjects**|Indica che l'attività ha iniziato a trasferire processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|  
  
####  <a name="transfer-logins-task"></a><a name="TransferLogins"></a> Attività Trasferisci account di accesso  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Trasferisci account di accesso.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**TransferLoginsTaskFinishedTransferringObjects**|Indica che l'attività ha terminato il trasferimento degli account di accesso.|  
|**TransferLoginsTaskStartTransferringObjects**|Indica che l'attività ha iniziato a trasferire account di accesso.|  
  
####  <a name="transfer-master-stored-procedures-task"></a><a name="TransferMasterStoredProcedures"></a> Attività Trasferisci stored procedure master  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Trasferisci stored procedure master.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**TransferStoredProceduresTaskFinishedTransferringObjects**|Indica che l'attività ha terminato il trasferimento delle stored procedure definite dall'utente archiviate nel database **master** .|  
|**TransferStoredProceduresTaskStartTransferringObjects**|Indica che l'attività ha iniziato a trasferire le stored procedure definite dall'utente archiviate nel database **master** .|  
  
####  <a name="transfer-sql-server-objects-task"></a><a name="TransferSQLServerObjects"></a> Attività Trasferisci oggetti di SQL Server  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Trasferisci oggetti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**TransferSqlServerObjectsTaskFinishedTransferringObjects**|Indica che l'attività ha terminato il trasferimento degli oggetti di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TransferSqlServerObjectsTaskStartTransferringObjects**|Indica che l'attività ha iniziato a trasferire oggetti di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
####  <a name="web-services-task"></a><a name="WebServices"></a> Attività Servizio Web  
 Nella tabella seguente sono elencate le voci di log personalizzate che è possibile abilitare per l'attività Servizio Web.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**WSTaskBegin**|Indica che l'attività ha iniziato ad accedere a un servizio Web.|  
|**WSTaskEnd**|Indica che l'attività ha completato un metodo per il servizio Web.|  
|**WSTaskInfo**|Offre informazioni descrittive sull'attività.|  
  
####  <a name="wmi-data-reader-task"></a><a name="WMIDataReader"></a> Attività Lettore di dati WMI  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Lettore di dati WMI.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Indica che l'attività ha iniziato a leggere dati WMI.|  
|**WMIDataReaderOperation**|Specifica la query WQL eseguita dall'attività.|  
  
####  <a name="wmi-event-watcher-task"></a><a name="WMIEventWatcher"></a> Attività Monitoraggio eventi WMI  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Monitoraggio eventi WMI.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Indica che l'evento monitorato dall'attività si è verificato.|  
|**WMIEventWatcherTimedout**|Indica che si è verificato il timeout dell'attività.|  
|**WMIEventWatcherWatchingForWMIEvents**|Indica che l'attività ha iniziato a eseguire la query WQL. La voce include la query.|  
  
####  <a name="xml-task"></a><a name="XML"></a> Attività XML  
 Nella tabella seguente è indicata la voce di log personalizzata disponibile per l'attività XML.  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**XMLOperation**|Fornisce informazioni sull'operazione eseguita dall'attività.|  

## <a name="related-tasks"></a>Attività correlate  
 Nell'elenco seguente sono contenuti collegamenti ad argomenti che illustrano come eseguire attività correlate alla funzionalità di registrazione.  
  
-   [Eventi registrati da un pacchetto di Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
  
## <a name="related-content"></a>Contenuto correlato  
 [Strumento DTLoggedExec per la registrazione completa e dettagliata (progetto CodePlex)](https://go.microsoft.com/fwlink/?LinkId=150579)  
