---
title: SQLdiag - utilità
description: Usare l'utilità SQLdiag per raccogliere i log e i file di dati da SQL Server e altri tipi di server e per monitorare i server nel tempo o individuare e risolvere problemi.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], SQLdiag
- stopping diagnostic collection
- storing diagnostic information
- performance [SQL Server], diagnostic collection
- diagnostic records [SQL Server]
- scripts [SQL Server], diagnostic collection
- logs [SQL Server], diagnostic collection
- starting diagnostic collection
- clustered instance of SQL Server
- monitoring performance [SQL Server], diagnostic collection
- security [SQL Server], diagnostic collection
- SQLDIAG service
- space [SQL Server], diagnostic collection
- SQLdiag utility
- disk space [SQL Server], diagnostic collection
- configuration files [SQL Server]
- automatic diagnostic collection
- clusters [SQL Server], diagnostic collection
ms.assetid: 45ba1307-33d1-431e-872c-a6e4556f5ff2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c9f3528ac4f377e379afd26e6583df3fe63fb2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353337"
---
# <a name="sqldiag-utility"></a>SQLdiag - utilità
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  **SQLdiag** è un'utilità di raccolta di dati diagnostici generica che può essere eseguita come applicazione console o come servizio. È possibile usare **SQLdiag** per raccogliere i log e i file di dati da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e altri tipi di server, nonché per monitorare i server in un intervallo di tempo oppure risolvere problemi specifici dei server. L'utilità **SQLdiag** è stata creata per velocizzare e semplificare la raccolta delle informazioni diagnostiche necessarie per il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../includes/msconame-md.md)] .  
  
> [!NOTE]  
>  Poiché questa utilità è soggetta a modifiche, le applicazioni o gli script che ne utilizzano gli argomenti della riga di comando o che dipendono dal suo comportamento potrebbero non funzionare correttamente nelle versioni future.  
  
 L'utilità **SQLdiag** può raccogliere i tipi di informazioni diagnostiche riportate di seguito:  
  
-   Registri di prestazioni di Windows  
  
-   Log eventi di Windows  
  
-   [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] tracce  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] informazioni di blocco  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] informazioni di configurazione  
  
 Per specificare i tipi di informazione che si vogliono raccogliere con l'utilità **SQLdiag** , modificare il file di configurazione SQLDiag.xml, descritto nella sezione seguente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqldiag   
     { [/?] }  
     |  
     { [/I configuration_file]  
       [/O output_folder_path]  
       [/P support_folder_path]  
       [/N output_folder_management_option]  
       [/M machine1 [ machine2 machineN]| @machinelistfile]  
       [/C file_compression_type]  
       [/B [+]start_time]  
       [/E [+]stop_time]  
       [/A SQLdiag_application_name]  
       [/T { tcp [ ,port ] | np | lpc } ]  
       [/Q] [/G] [/R] [/U] [/L] [/X] }  
     |  
     { [START | STOP | STOP_ABORT] }  
     |  
     { [START | STOP | STOP_ABORT] /A SQLdiag_application_name }  
```  
  
## <a name="arguments"></a>Argomenti  
 **/?**  
 Visualizza le informazioni sull'utilizzo.  
  
 **/I** _configuration_file_  
 Imposta il file di configurazione usato da **SQLdiag** . Per impostazione predefinita, **/I** è impostato su SQLDiag.Xml.  
  
 **/O** _output_folder_path_  
 Reindirizza l'output dell'utilità **SQLdiag** sulla cartella specificata. Se si omette l'opzione **/O** , l'output dell'utilità **SQLdiag** viene scritto in una sottocartella denominata SQLDIAG nella cartella di avvio **SQLdiag** . Se la cartella SQLDIAG non esiste, l'utilità **SQLdiag** cercherà di crearla.  
  
> [!NOTE]  
>  La posizione della cartella di output è relativa alla posizione della cartella di supporto che è possibile specificare mediante **/P**. Per impostare una cartella di output completamente diversa, specificare il percorso completo della directory per **/O**.  
  
 **/P** _support_folder_path_  
 Imposta il percorso della cartella di supporto. Per impostazione predefinita, **/P** viene impostata sulla cartella nella quale risiede il file eseguibile di **SQLdiag** . La cartella di supporto contiene i file di supporto di **SQLdiag** , ad esempio il file di configurazione XML, gli script di Transact-SQL e altri file che l'utilità usa durante la raccolta dei dati diagnostici. Se si usa questa opzione per specificare un percorso alternativo per i file di supporto, **SQLdiag** copierà automaticamente i file di supporto necessari nella cartella specificata, se non esistono già.  
  
> [!NOTE]  
>  Per impostare la cartella corrente come percorso di supporto, specificare **%cd%** nella riga di comando come segue:  
>   
>  **SQLDIAG /P %cd%**  
  
 **/N** _output_folder_management_option_  
 Imposta la sovrascrittura o la ridenominazione della cartella di output all'avvio dell'utilità **SQLdiag** . Opzioni disponibili:  
  
 1 = sovrascrive la cartella di output (impostazione predefinita).  
  
 2 = all'avvio di **SQLdiag** , rinomina la cartella di output in SQLDIAG_00001, SQLDIAG_00002 e così via. Dopo aver rinominato la cartella di output corrente, **SQLdiag** scrive l'output nella cartella di output predefinita SQLDIAG.  
  
> [!NOTE]  
>  Con **SQLdiag** l'output non viene accodato alla cartella di output corrente al momento dell'avvio. È possibile soltanto sovrascrivere la cartella di output predefinita (opzione 1) o rinominare la cartella (opzione 2), quindi l'output viene scritto nella nuova cartella di output predefinita denominata SQLDIAG.  
  
 **/M** _machine1_ [ *machine2* *machineN*] | *\@machinelistfile*  
 Esegue l'override del computer specificato nel file di configurazione. Per impostazione predefinita il file di configurazione è SQLDiag.Xml o è impostato con il parametro **/I** . Quando si specifica più di un computer, separare ogni nome di computer con uno spazio.  
  
 Con *\@machinelistfile* si specifica un nome di file per un elenco di computer da archiviare nel file di configurazione.  
  
 **/C** _file_compression_type_  
 Imposta il tipo di compressione usato per i file nella cartella di output **SQLdiag** . Opzioni disponibili:  
  
 0 = nessuna compressione (impostazione predefinita)  
  
 1 = compressione NTFS  
  
 **/B** [ **+** ]*start_time*  
 Specifica la data e l'ora di inizio della raccolta di dati diagnostici nel formato seguente:  
  
 AAAAMMGG_HH:MM:SS  
  
 L'ora viene specificata nel formato 24 ore. Per specificare ad esempio le 2 del pomeriggio deve essere specificato come **14:00:00**.  
  
 Usare **+** senza specificare la data, ovvero solo HH:MM:SS, per specificare un'ora in relazione alla data e all'ora correnti. Ad esempio, se si specifica **/B +02:00:00**, **SQLdiag** attenderà due ore prima di iniziare a raccogliere informazioni.  
  
 Non inserire spazi tra **+** e il valore specificato per *start_time*.  
  
 Se si specifica un'ora di inizio già trascorsa, **SQLdiag** modifica forzatamente la data di inizio in modo che faccia riferimento a un periodo futuro. Se ad esempio si specifica **/B 01:00:00** e l'ora corrente è 08:00:00, **SQLdiag** modifica forzatamente la data di inizio in modo che venga impostata in corrispondenza del giorno successivo.  
  
 L'utilità **SQLdiag** usa l'ora locale del computer in cui è in esecuzione.  
  
 **/E** [ **+** ]*stop_time*  
 Specifica la data e l'ora di arresto della raccolta di dati diagnostici nel formato seguente:  
  
 AAAAMMGG_HH:MM:SS  
  
 L'ora viene specificata nel formato 24 ore. Per specificare ad esempio le 2 del pomeriggio deve essere specificato come **14:00:00**.  
  
 Usare **+** senza specificare la data, ovvero solo HH:MM:SS, per specificare un'ora in relazione alla data e all'ora correnti. Se ad esempio si specifica un'ora di inizio e un'ora di fine usando **/B +02:00:00 /E +03:00:00**, **SQLdiag** attenderà 2 ore prima di iniziare a raccogliere informazioni. I dati verranno raccolti per 3 ore prima che l'utilità venga arrestata e chiusa. Se si omette **/B** , **SQLdiag** inizia a raccogliere subito i dati diagnostici e termina alla data e all'ora specificate da **/E**.  
  
 Non inserire spazi tra **+** e il valore specificato per *start_time* o *end_time*.  
  
 L'utilità **SQLdiag** usa l'ora locale del computer in cui è in esecuzione.  
  
 **/A**  _SQLdiag_application_name_  
 Abilita l'esecuzione di più istanze dell'utilità **SQLdiag** sulla base della stessa istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Ogni *SQLdiag_application_name* identifica un'istanza diversa di **SQLdiag**. Non esiste alcuna relazione tra un'istanza *SQLdiag_application_name* e il nome di un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 È possibile usare *SQLdiag_application_name* per avviare o arrestare un'istanza specifica del servizio **SQLdiag** .  
  
 Ad esempio:  
  
 **SQLDIAG START /A**  _SQLdiag_application_name_  
  
 È anche possibile usare tale parametro con l'opzione **/R** per registrare un'istanza specifica di **SQLdiag** come servizio. Ad esempio:  
  
 **SQLDIAG /R /A** _SQLdiag_application_name_  
  
> [!NOTE]  
>  **SQLdiag** aggiunge automaticamente il prefisso DIAG$ al nome dell'istanza specificato per *SQLdiag_application_name*. In questo modo viene messo a disposizione un nome di servizio appropriato se si registra **SQLdiag** come servizio.  
  
 /T { tcp [ ,*port* ] | np | lpc }  
 Consente di connettersi a un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando il protocollo specificato.  
  
 tcp [,*port*]  
 Protocollo TCP/IP (Transmission Control Protocol/Internet Protocol). È possibile specificare facoltativamente un numero di porta per la connessione.  
  
 np  
 Named pipe. Per impostazione predefinita, l'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] resta in attesa sulla named pipe `\\.\pipe\sql\query` e `\\.\pipe\MSSQL$<instancename>\sql\query` per un'istanza denominata. Non è possibile connettersi a un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tramite un nome della pipe alternativo.  
  
 lpc  
 Chiamata di procedura locale. Il protocollo Shared Memory è disponibile se il client si connette a un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nello stesso computer.  
  
 **/Q**  
 Esegue **SQLdiag** in modalità non interattiva. **/Q** disattiva tutti i prompt, ad esempio i prompt delle password.  
  
 **/G**  
 Esegue **SQLdiag** in modalità generica. Se si specifica **/G** , all'avvio dell'utilità **SQLdiag** non vengono eseguiti i controlli della connettività di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] né viene verificata l'appartenenza dell'utente al ruolo predefinito del server **sysadmin** . L'utilità **SQLdiag** usa invece Windows per determinare se un utente ha le autorizzazioni sufficienti per raccogliere i dati diagnostici richiesti.  
  
 Se si omette **/G** , **SQLdiag** verifica se l'utente è membro del gruppo **Administrators** di Windows e non procede alla raccolta dei dati diagnostici di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se l'utente non appartiene al gruppo **Administrators** .  
  
 **/R**  
 Registra **SQLdiag** come servizio. Gli argomenti della riga di comando specificati per registrare l'utilità **SQLdiag** come servizio vengono conservati per le esecuzioni future del servizio stesso.  
  
 Se l'utilità **SQLdiag** viene registrata come servizio, il nome del servizio predefinito è SQLDIAG. È possibile modificare il nome del servizio usando l'argomento **/A** .  
  
 Utilizzare l'argomento della riga di comando **START** per avviare il servizio:  
  
 **SQLDIAG START**  
  
 È inoltre possibile utilizzare il comando **net start** per avviare il servizio:  
  
 **net  start SQLDIAG**  
  
 **/U**  
 Annulla la registrazione di **SQLdiag** come servizio.  
  
 Usare l'argomento **/A** anche se si annulla la registrazione di un'istanza denominata di **SQLdiag** .  
  
 **/L**  
 Esegue l'utilità **SQLdiag** in modalità continua quando si specificano un'ora di inizio o fine rispettivamente con gli argomenti **/B** o **/E** . L'utilità **SQLdiag** viene automaticamente riavviata in seguito all'arresto della raccolta di dati diagnostici a causa di una chiusura pianificata impostata tramite, ad esempio, l'argomento **/E** o **/X** .  
  
> [!NOTE]  
>  **SQLdiag** ignora l'argomento **/L** se non si specifica un'ora di inizio e fine con gli argomenti della riga di comando **/B** e **/E** .  
  
 L'uso dell'argomento **/L** non implica la definizione della modalità del servizio. Per usare **/L** per l'esecuzione dell'utilità **SQLdiag** come servizio, specificare l'argomento nella riga di comando quando si registra il servizio.  
  
 **/X**  
 Esegue **SQLdiag** in modalità snapshot. L'utilità **SQLdiag** esegue uno snapshot di tutti i dati diagnostici configurati, quindi viene chiusa automaticamente.  
  
 **START** | **STOP** | **STOP_ABORT**  
 Avvia o arresta il servizio **SQLdiag** . **STOP_ABORT** forza il servizio di arrestarsi al più presto senza portare a termine la raccolta dei dati diagnostici in corso.  
  
 Questi argomenti di controllo del servizio devono essere i primi nella riga di comando quando vengono utilizzati. Ad esempio:  
  
 **SQLDIAG START**  
  
 L' argomento **/A** , che specifica un'istanza denominata di **SQLdiag**, è l'unico che può essere usato con **START**, **STOP** o **STOP_ABORT** per il controllo di un'istanza specifica del servizio **SQLdiag** . Ad esempio:  
  
 **SQLDIAG START /A** _SQLdiag_application_name_  
  
## <a name="security-requirements"></a>Requisiti di sicurezza  
 Se l'utilità **SQLdiag** non viene eseguita in modalità generica specificando l'argomento della riga di comando **/G**, l'utente che esegue **SQLdiag** deve essere membro del gruppo **Administrators** di Windows e membro del ruolo predefinito del server **sysadmin** di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per impostazione predefinita, l'utilità **SQLdiag** esegue la connessione a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando l'autenticazione di Windows, ma supporta anche l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="performance-considerations"></a>Considerazioni sulle prestazioni  
 Le prestazioni relative all'esecuzione dell'utilità **SQLdiag** dipendono dal tipo di dati diagnostici configurati per la raccolta. Se ad esempio l'utilità **SQLdiag** è stata configurata per raccogliere informazioni di traccia di [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , maggiore è il numero di classi degli eventi selezionate per la raccolta delle tracce, maggiore sarà l'impatto sulle prestazioni del server.  
  
 L'impatto sulle prestazioni dell'esecuzione dell'utilità **SQLdiag** è pari circa alla somma dei costi di raccolta dei dati diagnostici configurati separatamente. La raccolta di una traccia con l'utilità **SQLdiag** , ad esempio, è caratterizzata dagli stessi costi a livello di prestazioni della raccolta degli stessi dati con [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. L'impatto dell'uso dell'utilità **SQLdiag** sulle prestazioni è irrilevante.  
  
## <a name="required-disk-space"></a>Spazio su disco richiesto  
 Poiché l'utilità **SQLdiag** può raccogliere tipi diversi di dati diagnostici, lo spazio libero su disco richiesto per l'esecuzione di **SQLdiag** può variare. La quantità di dati diagnostici raccolti dipende dalla natura e dal volume del carico di lavoro che il server sta elaborando e può variare da pochi megabyte a una quantità rilevante di gigabyte.  
  
## <a name="configuration-files"></a>File di configurazione  
 All'avvio dell'utilità **SQLdiag** vengono letti il file di configurazione e gli argomenti della riga di comando specificati. Nel file di configurazione vengono specificati i tipi di dati diagnostici raccolti dall'utilità **SQLdiag** . Per impostazione predefinita, l'utilità **SQLdiag** usa il file di configurazione SQLDiag.Xml, disponibile a ogni esecuzione nella cartella di avvio dell'utilità **SQLdiag** . Il file di configurazione usa l'XML Schema SQLDiag_schema.xsd estratto dalla directory di avvio dell'utilità dal file eseguibile ad ogni esecuzione di **SQLdiag** .  
  
### <a name="editing-the-configuration-files"></a>Modifica dei file di configurazione  
 È possibile copiare e modificare il file SQLDiag.Xml per modificare i tipi di dati diagnostici raccolti dall'utilità **SQLdiag** . Durante la modifica del file di configurazione utilizzare sempre un editor XML in grado di convalidare il file in base al relativo XML Schema, ad esempio [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]. Non è consigliabile modificare SQLDiag.Xml direttamente. Creare invece una copia del file SQLDiag.Xml e ridenominarlo con un nome file diverso nella stessa cartella. A questo punto, modificare il nuovo file e usare l'argomento **/I** per passarlo all'utilità **SQLdiag**.  
  
#### <a name="editing-the-configuration-file-when-sqldiag-runs-as-a-service"></a>Modifica del file di configurazione in caso di esecuzione di SQLdiag come servizio  
 Se l'utilità **SQLdiag** è già stata eseguita come servizio ed è necessario modificare il file di configurazione, annullare la registrazione del servizio SQLDIAG specificando l'argomento della riga di comando **/U** e quindi rieseguendo la registrazione del servizio usando l'argomento della riga di comando **/R** . L'annullamento della registrazione e la successiva nuova registrazione comportano la rimozione delle vecchie informazioni di configurazione memorizzate nella cache del Registro di sistema di Windows.  
  
## <a name="output-folder"></a>Cartella di output  
 Se non si specifica una cartella di output con l'argomento **/O** , l'utilità **SQLdiag** crea una sottocartella denominata SQLDIAG nella cartella di avvio **SQLdiag** . Per la raccolta di dati diagnostici che comportano un elevato numero di tracce, ad esempio [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , verificare che la cartella di output si trovi in un'unità locale dotata di spazio sufficiente per archiviare l'output di dati diagnostici richiesto.  
  
 Quando l'utilità **SQLdiag** viene riavviata, i contenuti della cartella di output vengono sovrascritti. Per evitare la sovrascrittura, specificare **/N 2** nella riga di comando.  
  
## <a name="data-collection-process"></a>Processo di raccolta dei dati  
 Quando viene avviata, l'utilità **SQLdiag** esegue i controlli di inizializzazione necessari per raccogliere i dati diagnostici specificati nel file SQLDiag.Xml. Questo processo potrebbe richiedere alcuni secondi. Dopo che l'utilità **SQLdiag** eseguita come applicazione console ha iniziato a raccogliere i dati diagnostici, viene visualizzato un messaggio indicante che la raccolta di **SQLdiag** è iniziata e che è possibile premere CTRL+C per arrestarla. Se l'utilità **SQLdiag** viene eseguita come servizio, un messaggio simile viene scritto nel registro eventi di Windows.  
  
 In caso di uso dell'utilità **SQLdiag** per diagnosticare un problema che è possibile riprodurre, attendere la visualizzazione di questo messaggio prima di riprodurre il problema nel server.  
  
 L'utilità **SQLdiag** raccoglie la maggior parte dei dati diagnostici in parallelo. Tutti i dati diagnostici vengono raccolti con la connessione a strumenti, ad esempio l'utilità **sqlcmd** di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] oppure il processore dei comandi di Windows, tranne quando le informazioni vengono raccolte dai registri di prestazioni e dai registri eventi di Windows. L'utilità **SQLdiag** usa un thread di lavoro per computer per monitorare la raccolta di dati diagnostici da questi strumenti, spesso attendendo contemporaneamente il completamento della raccolta da parte di più strumenti. Durante il processo di raccolta l'utilità **SQLdiag** instrada l'output di ogni dato diagnostico alla cartella di output.  
  
## <a name="stopping-data-collection"></a>Arresto della raccolta di dati  
 Dopo l'inizio della raccolta di dati diagnostici, l'utilità **SQLdiag** continua a raccogliere informazioni finché non viene arrestata manualmente oppure in base all'ora di arresto specificata. Per configurare l'arresto dell'esecuzione dell'utilità **SQLdiag** a un'ora specifica, usare l'argomento **/E** , che consente di specificare l'ora di arresto desiderata, oppure l'argomento **/X** , che imposta l'esecuzione dell'utilità **SQLdiag** in modalità snapshot.  
  
 Quando viene arrestata, l'utilità **SQLdiag** arresta tutti i processi di diagnostica avviati. Vengono ad esempio arrestati la raccolta delle tracce di [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , gli script [!INCLUDE[tsql](../includes/tsql-md.md)] in esecuzione e qualsiasi processo secondario generato durante la raccolta dei dati. Quando la raccolta di dati diagnostici è completata, l'utilità **SQLdiag** viene chiusa.  
  
> [!NOTE]  
>  La sospensione del servizio dell'utilità **SQLdiag** non è supportata. Se si prova a sospendere il servizio **SQLdiag** , l'arresto si verifica al termine della raccolta dei dati diagnostici in corso al momento della sospensione. Se si riavvia l'utilità **SQLdiag** dopo averla arrestata, l'applicazione viene riavviata e sovrascrive la cartella di output. Per evitare la sovrascrittura della cartella di output, specificare **/N 2** nella riga di comando.  
  
 **Per arrestare l'esecuzione dell'utilità SQLdiag come applicazione console**  
  
 Se l'utilità **SQLdiag** è eseguita come applicazione console, premere CTRL+C nella finestra della console in cui l'utilità **SQLdiag** è in esecuzione per arrestarla. Dopo aver premuto CTRL+C, nella finestra della console viene visualizzato un messaggio indicante che la raccolta di dati da parte di **SQLDiag** sta per essere interrotta, che è necessario attendere che il processo venga interrotto e che questa operazione potrebbe richiedere alcuni minuti.  
  
 Premere Ctrl+C due volte per terminare tutti i processi figlio di raccolta dei dati diagnostici e terminare l'applicazione immediatamente.  
  
 **Per arrestare l'esecuzione dell'utilità SQLdiag come servizio**  
  
 Se si esegue l'utilità **SQLdiag** come servizio, eseguire **SQLDiag STOP** nella cartella di avvio di **SQLdiag** per arrestarlo.  
  
 Se si eseguono più istanze di **SQLdiag** nello stesso computer, è anche possibile passare il nome dell'istanza di **SQLdiag** nella riga di comando quando si arresta il servizio. Per arrestare un'istanza di **SQLdiag** denominata Instance1, ad esempio, usare la sintassi seguente:  
  
```  
SQLDIAG STOP /A Instance1  
```  
  
> [!NOTE]  
>  **/A** è il solo argomento della riga di comando che può essere utilizzato con **START**, **STOP** o **STOP_ABORT**. Se è necessario specificare un'istanza denominata di **SQLdiag** con uno dei verbi di controllo del servizio, specificare **/A** dopo il verbo di controllo nella riga di comando, come indicato nell'esempio di sintassi precedente. I verbi di controllo del servizio devono essere il primo argomento nella riga di comando quando vengono utilizzati.  
  
 Per arrestare il servizio al più presto, eseguire **SQLDIAG STOP_ABORT** nella cartella di avvio dell'utilità. Questo comando annulla tutte le raccolte di dati diagnostici in corso senza attenderne il completamento.  
  
> [!NOTE]  
>  Usare **SQLDiag STOP** o **SQLDIAG STOP_ABORT** per arrestare il servizio **SQLdiag** . Non usare la console dei servizi Windows per arrestare **SQLdiag** o gli altri servizi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="automatically-starting-and-stopping-sqldiag"></a>Avvio e arresto automatici dell'utilità SQLdiag  
 Per avviare e arrestare automaticamente la raccolta di dati diagnostici a un'ora specificata, usare gli argomenti **/B**_start\_time_ e **/E**_stop\_time_ nel formato 24 ore. Se, ad esempio, si sta cercando di risolvere un problema che si verifica regolarmente alle 02:00:00 circa, è possibile configurare l'utilità **SQLdiag** in modo che inizi automaticamente a raccogliere dati diagnostici alle 01:00 e si arresti automaticamente alle 03:00:00. Usare gli argomenti **/B** e **/E** per specificare l'ora di inizio e di fine. Utilizzare il formato 24 ore per specificare la data e l'ora di inizio e fine corrette nel formato AAAAMMGG_HH:MM:SS. Per specificare una data di inizio o fine relativa, anteporre il segno **+** all'ora di inizio e fine omettendo la parte relativa alla data (AAAAMMGG_) come illustrato nell'esempio seguente. In questo modo, l'utilità **SQLdiag** attende un'ora prima di iniziare a raccogliere le informazioni. I dati vengono raccolti per 3 ore prima che l'utilità venga arrestata e chiusa.  
  
```  
sqldiag /B +01:00:00 /E +03:00:00  
```  
  
 Se si specifica un valore relativo per *start_time* , l'utilità **SQLdiag** viene avviata a un'ora relativa rispetto alla data e all'ora correnti. Se si specifica un valore relativo per *end_time* , l'utilità **SQLdiag** viene interrotta a un'ora relativa rispetto al valore specificato per *start_time*. Se la data o l'ora di inizio e fine specificate sono già trascorse, l'utilità **SQLdiag** modifica forzatamente la data e l'ora di inizio in modo che facciano riferimento a un periodo futuro.  
  
 Ciò ha implicazioni molto importanti sulle date di inizio e fine selezionate. Prendere in considerazione gli esempi seguenti:  
  
```  
sqldiag /B +01:00:00 /E 08:30:00  
```  
  
 Se l'ora corrente è 08:00, l'ora di fine è già trascorsa prima dell'inizio della raccolta di dati diagnostici. Poiché l'utilità **SQLDiag** modifica automaticamente le date di inizio e fine riferendole al giorno successivo qualora siano già trascorse, in questo esempio la raccolta di dati diagnostici inizia alle 09:00 della data corrente (è stata specificata un'ora di inizio relativa con il prefisso **+** ) e continua fino alle 08:30 del giorno successivo.  
  
### <a name="stopping-and-restarting-sqldiag-to-collect-daily-diagnostics"></a>Arresto e riavvio di SQLdiag per la raccolta giornaliera di dati diagnostici  
 Per raccogliere un set specifico di dati diagnostici su base giornaliera senza dover avviare e arrestare manualmente l'utilità **SQLdiag**, usare l'argomento **/L** . L'argomento **/L** consente di impostare l'esecuzione continua dell'utilità **SQLdiag** grazie all'impostazione del riavvio automatico dell'utilità dopo un'interruzione pianificata. Se si specifica l'argomento **/L** e l'utilità **SQLdiag** viene arrestata perché ha raggiunto l'ora di fine specificata dall'argomento **/E** oppure perché viene eseguita in modalità snapshot specificata con l'argomento **/X** , l'utilità **SQLdiag** non viene chiusa, bensì riavviata.  
  
 Nell'esempio seguente l'utilità **SQLdiag** eseguita in modalità continua viene automaticamente riavviata dopo che la raccolta di dati diagnostici è stata eseguita dalle 03:00:00 alle 05:00:00.  
  
```  
sqldiag /B 03:00:00 /E 05:00:00 /L  
```  
  
 Nell'esempio seguente l'utilità **SQLdiag** eseguita in modalità continua viene riavviata automaticamente dopo l'esecuzione di uno snapshot dei dati diagnostici alle 03:00:00.  
  
```  
sqldiag /B 03:00:00 /X /L  
```  
  
## <a name="running-sqldiag-as-a-service"></a>Esecuzione dell'utilità SQLdiag come servizio  
 Se si vuole usare l'utilità **SQLdiag** per raccogliere dati diagnostici per lunghi periodi di tempo durante i quali potrebbe essere necessario disconnettersi dal computer sul quale l'utilità **SQLdiag** è in esecuzione, è possibile eseguire l'utilità come servizio.  
  
 **Per registrare l'utilità SQLDiag in modo che venga eseguita come servizio**  
  
 È possibile registrare l'utilità **SQLdiag** in modo che venga eseguita come servizio specificando l'argomento **/R** nella riga di comando. L'utilità **SQLdiag** viene registrata in modo che venga eseguita come servizio. Il nome del servizio dell'utilità **SQLdiag** è SQLDIAG. Qualsiasi altro argomento specificato nella riga di comando durante la registrazione dell'utilità **SQLDiag** come servizio viene memorizzato e riutilizzato quando il servizio viene avviato.  
  
 Per modificare il nome del servizio SQLDIAG predefinito, usare l'argomento della riga di comando **/A** per specificare un altro nome. **SQLdiag** aggiunge automaticamente il prefisso DIAG$ a qualsiasi nome dell'istanza di **SQLdiag** specificato con **/A** per creare nomi di servizio appropriati.  
  
 **Per annullare la registrazione del servizio SQLDIAG**  
  
 Per annullare la registrazione del servizio, specificare l'argomento **/U** . L'annullamento della registrazione dell'utilità **SQLdiag** come servizio comporta l'eliminazione anche delle chiavi del Registro di sistema di Windows.  
  
 **Per avviare o riavviare il servizio SQLDIAG**  
  
 Per avviare o riavviare il servizio SQLDIAG, eseguire **SQLDiag START** dalla riga di comando.  
  
 Se si eseguono più istanze di **SQLdiag** usando l'argomento **/A** , è anche possibile passare il nome dell'istanza di **SQLdiag** nella riga di comando quando si avvia il servizio. Per avviare un'istanza di **SQLdiag** denominata Instance1, ad esempio, usare la sintassi seguente:  
  
```  
SQLDIAG START /A Instance1  
```  
  
 È inoltre possibile utilizzare il comando **net start** per avviare il servizio SQLDIAG.  
  
 Quando **SQLdiag** viene riavviato, i contenuti della cartella di output vengono sovrascritti. Per evitare la sovrascrittura, specificare **/N 2** nella riga di comando per rinominare la cartella di output all'avvio dell'utilità.  
  
 La sospensione del servizio dell'utilità **SQLdiag** non è supportata.  
  
## <a name="running-multiple-instances-of-sqldiag"></a>Esecuzione di più istanze di SQLdiag  
 È possibile eseguire più istanze di **SQLdiag** nello stesso computer specificando **/A**_SQLdiag\_application\_name_ nella riga di comando. Ciò consente di raccogliere diversi set di dati diagnostici contemporaneamente dalla stessa istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . È ad esempio possibile configurare un'istanza denominata di **SQLdiag** per eseguire in modo continuo una raccolta di dati lightweight. Se quindi si verifica un problema specifico relativo a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], è possibile eseguire l'istanza di **SQLdiag** predefinita per raccogliere i dati diagnostici relativi a quel problema o per la raccolta di un set di dati diagnostici su richiesta del Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../includes/msconame-md.md)] per elaborare una diagnosi relativa al problema.  
  
## <a name="collecting-diagnostic-data-from-clustered-sql-server-instances"></a>Raccolta dei dati diagnostici dalle istanze di SQL Server del cluster  
 L'utilità **SQLdiag** supporta la raccolta di dati diagnostici dalle istanze cluster di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per raccogliere dati diagnostici dalle istanze cluster di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], verificare di aver specificato **"."** per l'attributo **name** dell'elemento **\<Machine>** nel file di configurazione SQLDiag.Xml e di non aver specificato l'argomento **/G** nella riga di comando. Per impostazione predefinita, viene specificato **"."** per l'attributo **name** nel file di configurazione e l'argomento **/G** è disabilitato. Non è in genere necessario modificare il file di configurazione o gli argomenti della riga di comando durante la raccolta di dati da un'istanza cluster di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Se si specifica **"."** come nome del computer, l'utilità **SQLdiag** rileva che è in esecuzione in un cluster e contemporaneamente recupera i dati diagnostici da tutte le istanze virtuali di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] installate nel cluster. Se si desidera raccogliere dati diagnostici da una sola istanza virtuale di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un computer, specificare l'istanza virtuale di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per l'attributo **name** dell'elemento **\<Machine>** nel file SQLDiag.Xml.  
  
> [!NOTE]  
>  Per raccogliere le informazioni di traccia di [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] dalle istanze cluster di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , è necessario abilitare le condivisioni amministrative (ADMIN$) nel cluster.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento alle utilità del prompt dei comandi &#40;motore di database&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
