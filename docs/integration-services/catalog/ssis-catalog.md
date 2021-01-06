---
description: Catalogo SSIS
title: Catalogo SSIS | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.iscreatecatalog.f1
- sql13.ssis.ssms.iscatalogprop.general.f1
- sql13.ssis.dbupgradewizard.f1
ms.assetid: 24bd987e-164a-48fd-b4f2-cbe16a3cd95e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a8ed14b14198bb95db838e76ca07bffe5875ce69
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642596"
---
# <a name="ssis-catalog"></a>Catalogo SSIS

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Il catalogo **SSISDB** è il punto centrale dell'utilizzo di progetti [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS) che sono stati distribuiti nel server [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)]. Ad esempio, è possibile impostare parametri di progetti e pacchetti, configurare ambienti per specificare valori di runtime per i pacchetti, eseguire e risolvere i problemi dei pacchetti e gestire le operazioni del server [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] .  
 
> [!NOTE]
> In questo articolo viene illustrato il catalogo SSIS a livello generale e il catalogo SSIS eseguito in locale. È possibile creare il catalogo SSIS anche nel database SQL di Azure e distribuire ed eseguire i pacchetti SSIS in Azure. Per altre informazioni, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
>
> Anche se è possibile eseguire i pacchetti SSIS in Linux, quest'ultimo non supporta il catalogo SSIS. Per altre informazioni, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md).
 
 Tra gli oggetti archiviati nel catalogo **SSISDB** sono inclusi progetti, pacchetti, parametri, ambienti e cronologia operativa.  
  
 È possibile eseguire una query sulle viste nel database **SSISDB** per verificare oggetti, impostazioni e dati operativi archiviati nel catalogo **SSISDB** . È possibile gestire gli oggetti chiamando le stored procedure nel database **SSISDB** o usando l'interfaccia utente del catalogo **SSISDB** . In molti casi è possibile eseguire la stessa attività nella UI o chiamando una stored procedure.  
  
 Per gestire il database **SSISDB** , si consiglia di applicare criteri aziendali standard per la gestione di database utente. Per informazioni sulla creazione dei piani di manutenzione, vedere [Maintenance Plans](../../relational-databases/maintenance-plans/maintenance-plans.md).  
  
 Il catalogo **SSISDB** e il database **SSISDB** supportano Windows PowerShell. Per altre informazioni sull'uso di SQL Server con Windows PowerShell, vedere [SQL Server PowerShell](../../powershell/sql-server-powershell.md). Per gli esempi di come usare Windows PowerShell per completare attività quali la distribuzione di un progetto, vedere l'intervento sul blog relativo a [SSIS e PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015)sul sito blogs.msdn.com.  
  
 Per altre informazioni sulla visualizzazione dei dati delle operazioni, vedere [Esecuzione di pacchetti e altre operazioni di monitoraggio](../../integration-services/performance/monitor-running-packages-and-other-operations.md).  
  
 È possibile accedere al catalogo **SSISDB** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eseguendo una connessione al motore di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quindi espandendo il nodo **Cataloghi di Integration Services** in Esplora oggetti. È possibile accedere al database **SSISDB** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] espandendo il nodo Database in Esplora oggetti.  
  
> [!NOTE]
> Non è possibile rinominare il database **SSISDB** .  
  
> [!NOTE]
> Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui è collegato il database **SSISDB** viene arrestata o non risponde, il processo ISServerExec.exe termina. Un messaggio verrà scritto nel log eventi di Windows.  
>   
>  Se si verifica un failover delle risorse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come parte del failover di un cluster, i pacchetti in esecuzione non vengono riavviati. È possibile usare i checkpoint per riavviare i pacchetti. Per ulteriori informazioni, vedere [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
## <a name="features-and-capabilities"></a>Funzionalità e caratteristiche  
  
-   [Identificatori dell'oggetto catalogo](../../integration-services/catalog/ssis-catalog.md#CatalogObjectIdentifiers)  
  
-   [Configurazione del catalogo](../../integration-services/catalog/ssis-catalog.md#Configuration)  
  
-   [Autorizzazioni](../../integration-services/catalog/ssis-catalog.md#Permissions)  
  
-   [Cartelle](../../integration-services/catalog/ssis-catalog.md#Folders)  
  
-   [Progetti e pacchetti](../../integration-services/catalog/ssis-catalog.md#ProjectsAndPackages)  
  
-   [Parameters](../../integration-services/catalog/ssis-catalog.md#Parameters)  
  
-   [Ambienti server, variabili del server e riferimenti all'ambiente del server](../../integration-services/catalog/ssis-catalog.md#ServerEnvironments)  
  
-   [Esecuzioni e convalide](../../integration-services/catalog/ssis-catalog.md#Executions)  

##  <a name="catalog-object-identifiers"></a><a name="CatalogObjectIdentifiers"></a> Identificatori dell'oggetto catalogo  
 Quando si crea un nuovo oggetto nel catalogo, assegnare un nome all'oggetto. Il nome di un oggetto costituisce l'identificatore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definisce le regole per i caratteri che possono essere usati in un identificatore. I nomi degli oggetti seguenti devono rispettare le regole per gli identificatori.  
  
-   Cartella  
  
-   Project  
  
-   Environment  
  
-   Parametro  
  
-   Variabile di ambiente  
  
###  <a name="folder-project-environment"></a><a name="Folder"></a> Cartella, progetto, ambiente  
 Quando si rinomina una cartella, un progetto o un ambiente, considerare le regole riportate di seguito.  
  
-   I caratteri non validi includono i caratteri ASCII/Unicode compresi tra 1 e 31, le virgolette ("), il simbolo minore di (\<), greater than (>), la barra verticale (|), il carattere backspace (\b), il valore Null (\0) e la tabulazione (\t).  
  
-   Nel nome potrebbero non essere contenuti spazi iniziali o finali.  
  
-   Il simbolo \@ non è consentito come primo carattere, ma i caratteri successivi potrebbero usare \@.  
  
-   La lunghezza del nome deve essere maggiore di 0 e minore o uguale a 128.  
  
###  <a name="parameter"></a><a name="Parameter"></a>Parametro  
 Quando si rinomina un parametro, considerare le regole seguenti:  
  
-   Il primo carattere del nome deve essere una lettera, come definito nello standard Unicode 2.0, o un carattere di sottolineatura (_).  
  
-   I caratteri successivi possono includere lettere o numeri, come definito nello standard Unicode 2.0, o un carattere di sottolineatura (_).  
  
###  <a name="environment-variable"></a><a name="EnvironmentVariable"></a> Variabile di ambiente  
 Quando si rinomina una variabile di ambiente, considerare le regole seguenti:  
  
-   I caratteri non validi includono i caratteri ASCII/Unicode compresi tra 1 e 31, le virgolette ("), il simbolo minore di (\<), greater than (>), la barra verticale (|), il carattere backspace (\b), il valore Null (\0) e la tabulazione (\t).  
  
-   Nel nome potrebbero non essere contenuti spazi iniziali o finali.  
  
-   Il simbolo \@ non è consentito come primo carattere, ma i caratteri successivi potrebbero usare \@.  
  
-   La lunghezza del nome deve essere maggiore di 0 e minore o uguale a 128.  
  
-   Il primo carattere del nome deve essere una lettera, come definito nello standard Unicode 2.0, o un carattere di sottolineatura (_).  
  
-   I caratteri successivi possono includere lettere o numeri, come definito nello standard Unicode 2.0, o un carattere di sottolineatura (_).  
  
##  <a name="catalog-configuration"></a><a name="Configuration"></a> Configurazione del catalogo  
 È possibile ottimizzare la modalità di comportamento del catalogo modificandone le relative proprietà. Le proprietà del catalogo consentono di definire come vengono crittografati i dati sensibili e come vengono mantenuti i dati del controllo delle versioni dei progetti. Per impostare le proprietà del catalogo, usare la finestra di dialogo **Proprietà catalogo** o chiamare la stored procedure [catalog.configure_catalog &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md). Per visualizzare le proprietà, usare la finestra di dialogo o eseguire una query su [catalog.catalog_properties &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). È possibile accedere alla finestra di dialogo facendo clic con il pulsante destro del mouse su **SSISDB** in Esplora oggetti.  
  
###  <a name="operations-and-project-version-cleanup"></a><a name="Cleanup"></a> Operazioni e pulizia della versione del progetto  
 I dati dello stato per molte delle operazioni nel catalogo vengono archiviati nelle tabelle di database interne. Ad esempio, tramite il catalogo si tiene traccia dello stato delle esecuzioni dei pacchetti e delle distribuzioni dei progetti. Per gestire le dimensioni dei dati delle operazioni, è possibile usare **Processo di manutenzione del server SSIS** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per rimuovere i dati vecchi. Questo processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene creato quando viene installato [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 È possibile aggiornare o ridistribuire un progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] distribuendolo con lo stesso nome nella stessa cartella del catalogo. Per impostazione predefinita, ogni volta che si ridistribuisce un progetto, il catalogo **SSISDB** mantiene la versione precedente del progetto. Per gestire le dimensioni dei dati delle operazioni, è possibile usare **Processo di manutenzione del server SSIS** per rimuovere le versioni precedenti dei progetti.  
 
Per eseguire il **processo di manutenzione del server SSIS**, SSIS crea l'accesso di SQL Server **##MS_SSISServerCleanupJobLogin##** . Questo accesso è destinato esclusivamente all'uso interno da parte di SSIS.
  
 Tramite le seguenti proprietà del catalogo **SSISDB** viene definito il comportamento di questo processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. È possibile visualizzare e modificare le proprietà tramite la finestra di dialogo **Proprietà catalogo** oppure usando [catalog.catalog_properties &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) e [catalog.configure_catalog &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Pulisci log periodicamente**  
 Il passaggio del processo per la pulizia delle operazioni viene eseguito quando questa proprietà è impostata su **True**.  
  
 **Periodo di memorizzazione (giorni)**  
 Definisce la validità massima di dati di operazioni consentiti (in giorni). I dati più obsoleti vengono rimossi.  
  
 Il valore minimo è 1 giorno. Il valore massimo è limitato al valore massimo dei dati **int** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per informazioni su questo tipo di dati, vedere [int, bigint, smallint e tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).  
  
 **Rimuovi periodicamente versioni precedenti**  
 Il passaggio del processo per la pulizia della versione del progetto viene eseguito quando questa proprietà è impostata su **True**.  
  
 **Numero massimo di versioni per progetto**  
 Viene definito il numero di versioni di un progetto che vengono archiviate nel catalogo. Le versioni precedente dei progetti vengono rimosse.  
  
###  <a name="encryption-algorithm"></a><a name="Encryption"></a> Algoritmo di crittografia  
 La proprietà **Algoritmo di crittografia** consente di specificare il tipo di crittografia usato per crittografare i valori dei parametri sensibili. È possibile scegliere tra i tipi di crittografia seguenti:  
  
-   AES_256 (predefinito)  
  
-   AES_192  
  
-   AES_128  
  
-   DESX  
  
-   TRIPLE_DES_3KEY  
  
-   TRIPLE_DES  
  
-   DES  
  
 Quando si distribuisce un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , il catalogo crittografa automaticamente i dati e i valori sensibili del pacchetto. Il catalogo inoltre decrittografa automaticamente i dati quando viene recuperato. Il catalogo SSISDB usano il livello di protezione **ServerStorage** . Per altre informazioni, vedere [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 La modifica dell'algoritmo di crittografia è un'operazione che richiede molto tempo. Innanzitutto, nel server deve essere usato l'algoritmo specificato in precedenza per decrittografare tutti i valori di configurazione. Successivamente, deve essere usato il nuovo algoritmo per crittografare nuovamente i valori. Durante questa fase, nel server non è possibile eseguire altre operazioni usando [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pertanto, per consentire il funzionamento di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] senza interruzioni, l'algoritmo di crittografia è un valore di sola lettura nella finestra di dialogo di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 Per modificare l'impostazione della proprietà **Algoritmo di crittografia** , impostare il database **SSISDB** sulla modalità utente singolo e quindi chiamare la stored procedure catalog.configure_catalog. Usare ENCRYPTION_ALGORITHM per l'argomento *property_name* . Per i valori di proprietà supportati, vedere [catalog.catalog_properties &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). Per altre informazioni sulla stored procedure, vedere [catalog.configure_catalog &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 Per altre informazioni sulla modalità utente singolo, vedere [Impostare un database in modalità utente singolo](../../relational-databases/databases/set-a-database-to-single-user-mode.md). Per informazioni sulla crittografia e sui relativi algoritmi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere gli argomenti della sezione [Crittografia di SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md).  
  
 Per la crittografia viene usata una chiave master del database. La chiave viene creata durante la creazione del catalogo.  
  
 Nella tabella seguente vengono elencati i nomi delle proprietà visualizzati nella finestra di dialogo **Proprietà catalogo** e le proprietà corrispondenti nella vista del database.  
  
|Nome proprietà (finestra di dialogo **Proprietà catalogo** )|Nome proprietà (vista di database)|  
|---------------------------------------------------------|-------------------------------------|  
|Nome algoritmo di crittografia|ENCRYPTION_ALGORITHM|  
|Pulisci log periodicamente|OPERATION_CLEANUP_ENABLEDâ€‹|  
|Periodo di memorizzazione (giorni)|RETENTION_WINDOW|  
|Rimuovi periodicamente versioni precedenti|VERSION_CLEANUP_ENABLED|  
|Numero massimo di versioni per progetto|MAX_PROJECT_VERSIONS|  
|Livello di registrazione predefinito per l'intero server|SERVER_LOGGING_LEVEL|  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 I progetti, gli ambienti e i pacchetti sono contenuti in cartelle che sono oggetti a protezione diretta. È possibile concedere le autorizzazioni a una cartella, inclusa l'autorizzazione MANAGE_OBJECT_PERMISSIONS. L'autorizzazione MANAGE_OBJECT_PERMISSIONS consente di delegare l'amministrazione del contenuto di una cartella a un utente senza dover concedere all'utente l'appartenenza al ruolo ssis_admin. È inoltre possibile concedere autorizzazioni per progetti, ambienti e operazioni. Le operazioni includono l'inizializzazione di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], la distribuzione di progetti, la creazione e l'avvio di esecuzioni, la convalida di progetti e pacchetti e la configurazione del catalogo **SSISDB** .  
  
 Per altre informazioni sui ruoli di database, vedere [Ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Il catalogo SSISDB usano un trigger DDL, ddl_cleanup_object_permissions, per applicare l'integrità delle informazioni sulle autorizzazioni per le entità a protezione diretta di SSIS. Il trigger viene attivato quando un'entità di database, ad esempio un utente del database, un ruolo del database o un ruolo applicazione di database, viene rimossa dal database SSISDB.  
  
 Se l'entità ha concesso o negato le autorizzazioni ad altre entità, è necessario revocare le autorizzazioni fornite dall'utente che concede le autorizzazioni, prima di poter rimuovere l'entità. In caso contrario, viene restituito un messaggio di errore quando il sistema tenta di rimuovere l'entità. Tramite il trigger vengono rimossi tutti i record di autorizzazione in cui l'entità di database è un utente autorizzato.  
  
 È consigliabile che il trigger non sia disabilitato perché in questo modo viene assicurata l'assenza di record di autorizzazione orfani dopo l'eliminazione di un'entità di database dal database **SSISDB** .  
  
### <a name="managing-permissions"></a>Gestione delle autorizzazioni  
 È possibile gestire le autorizzazioni tramite l'interfaccia utente di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , le stored procedure e lo spazio dei nomi <xref:Microsoft.SqlServer.Management.IntegrationServices> .  
  
 Per gestire le autorizzazioni con l'interfaccia utente di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], usare le finestre di dialogo seguenti: 
  
-   Per una cartella, usare la pagina **Autorizzazioni** in [Folder Properties Dialog Box](../../integration-services/catalog/folder-properties-dialog-box.md).  
  
-   Per un progetto, usare la pagina **Autorizzazioni** in [Project Properties Dialog Box](../../integration-services/catalog/project-properties-dialog-box.md).  

 Per gestire le autorizzazioni usando Transact-SQL, chiamare [catalog.grant_permission &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-grant-permission-ssisdb-database.md), [catalog.deny_permission &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deny-permission-ssisdb-database.md) e [catalog.revoke_permission &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-revoke-permission-ssisdb-database.md). Per visualizzare le autorizzazioni valide per l'entità corrente per tutti gli oggetti, eseguire una query su [catalog.effective_object_permissions &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md). In questo argomento vengono fornite le descrizioni dei diversi tipi di autorizzazioni. Per visualizzare le autorizzazioni assegnate in modo esplicito all'utente, eseguire una query su [catalog.explicit_object_permissions &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md).  
  
##  <a name="folders"></a><a name="Folders"></a> Cartelle  
 Nel catalogo di **SSISDB** della cartella sono contenuti uno o più pacchetti e ambienti. È possibile usare la vista [catalog.folders &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md) per accedere alle informazioni sulle cartelle del catalogo. È possibile usare le stored procedure seguenti per gestire le cartelle:  
  
-   [catalog.create_folder &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-create-folder-ssisdb-database.md)  
  
-   [catalog.delete_folder &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-delete-folder-ssisdb-database.md)  
  
-   [catalog.rename_folder &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-rename-folder-ssisdb-database.md)  
  
-   [catalog.set_folder_description &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-folder-description-ssisdb-database.md)  
  
##  <a name="projects-and-packages"></a><a name="ProjectsAndPackages"></a> Progetti e pacchetti  
 Ogni progetto può contenere più pacchetti. I progetti e i pacchetti possono contenere entrambi i parametri e i riferimenti agli ambienti. È possibile accedere ai parametri e ai riferimenti agli ambienti tramite [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md).  
  
 È possibile eseguire altre attività sui progetti chiamando le stored procedure seguenti: 
  
-   [catalog.delete_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-delete-project-ssisdb-database.md)  
  
-   [catalog.deploy_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)  
  
-   [catalog.get_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)  
  
-   [catalog.move_project &#40;&#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-move-project-ssisdb-database.md)  
  
-   [catalog.restore_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md)  
  
 Queste viste forniscono i dettagli su pacchetti, progetti e versioni di progetto.  
  
-   [catalog.projects &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
  
-   [catalog.packages &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
  
-   [catalog.object_versions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parametri  
 È possibile usare i parametri per assegnare i valori alle proprietà dei pacchetti durante la fase di esecuzione. Per impostare il valore di un parametro del pacchetto o del progetto e per cancellare il valore, chiamare [catalog.set_object_parameter_value &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md) e [catalog.clear_object_parameter_value &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md). Per impostare il valore di un parametro per un'istanza di esecuzione, chiamare [catalog.set_execution_parameter_value &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md). È possibile recuperare i valori di parametro predefiniti chiamando [catalog.get_parameter_values &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md).  
  
 Queste viste mostrano i parametri per tutti i pacchetti e i progetti nonché i valori del parametro usati per un'istanza di esecuzione.  
  
-   [catalog.object_parameters &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
  
-   [catalog.execution_parameter_values &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
  
##  <a name="server-environments-server-variables-and-server-environment-references"></a><a name="ServerEnvironments"></a> Ambienti server, variabili del server e riferimenti all'ambiente del server  
 Gli ambienti del server contengono le variabili del server. I valori delle variabili possono essere usati quando un pacchetto viene eseguito o convalidato nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Le stored procedure seguenti consentono di effettuare molte altre attività di gestione per ambienti e variabili.  
  
-   [catalog.create_environment &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-ssisdb-database.md)  
  
-   [catalog.delete_environment &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-ssisdb-database.md)  
  
-   [catalog.move_environment &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-move-environment-ssisdb-database.md)  
  
-   [catalog.rename_environment &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-rename-environment-ssisdb-database.md)  
  
-   [catalog.set_environment_property &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-property-ssisdb-database.md)  
  
-   [catalog.create_environment_variable &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-variable-ssisdb-database.md)  
  
-   [catalog.delete_environment_variable &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-variable-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_property &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-property-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-value-ssisdb-database.md)  
  
 Chiamando la stored procedure [catalog.set_environment_variable_protection &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-protection-ssisdb-database.md) è possibile impostare il bit di importanza per una variabile.  
  
 Per usare il valore di una variabile del server, specificare il riferimento tra il progetto e l'ambiente del server. È possibile usare le stored procedure seguenti per creare ed eliminare riferimenti. È anche possibile indicare se l'ambiente può essere individuato nella stessa cartella del progetto o in una cartella diversa.  
  
-   [catalog.create_environment_reference &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-reference-ssisdb-database.md)  
  
-   [catalog.delete_environment_reference &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-reference-ssisdb-database.md)  
  
-   [catalog.set_environment_reference_type &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-reference-type-ssisdb-database.md)  
  
 Per ulteriori dettagli sugli ambienti e le variabili, eseguire una query su queste viste.  
  
-   [catalog.environments &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
  
-   [catalog.environment_variables &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
  
-   [catalog.environment_references &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
  
##  <a name="executions-and-validations"></a><a name="Executions"></a> Esecuzioni e convalide  
 Un'esecuzione è un'istanza di un'esecuzione del pacchetto. Chiamare [catalog.create_execution &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) e [catalog.start_execution &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) per creare e avviare un'esecuzione. Per interrompere un'esecuzione o una convalida del pacchetto/progetto, chiamare [catalog.stop_operation &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md).  
  
 Per interrompere un pacchetto in esecuzione e creare un file di dump, chiamare la stored procedure catalog.create_execution_dump. Un file di dump fornisce le informazioni sull'esecuzione di un pacchetto che possono consentire di risolvere i problemi dell'esecuzione. Per altre informazioni sulla generazione e sulla configurazione dei file di dump, vedere [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
 Per i dettagli sulle esecuzioni, le convalide, i messaggi registrati durante le operazioni e le informazioni contestuali correlate agli errori, eseguire una query su queste viste.  
  
-   [catalog.executions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
-   [catalog.operations &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
  
-   [catalog.operation_messages &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
  
-   [catalog.extended_operation_info &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
  
-   [catalog.event_messages](../../integration-services/system-views/catalog-event-messages.md)  
  
-   [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
 È possibile convalidare i progetti e i pacchetti chiamando le stored procedure [catalog.validate_project &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-validate-project-ssisdb-database.md) e [catalog.validate_package &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-validate-package-ssisdb-database.md). La vista [catalog.validations &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) fornisce dettagli sulle convalide, ad esempio i riferimenti all'ambiente server considerati nella convalida, se si tratta di una convalida della dipendenza o di una convalida completa e se viene usato il runtime a 32 bit o a 64 bit per eseguire il pacchetto.  

## <a name="create-the-ssis-catalog"></a>Creare il catalogo SSIS
  Dopo avere progettato e testato i pacchetti in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], è possibile distribuire i progetti che contengono i pacchetti in un server di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Prima di poter distribuire i progetti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è necessario che il server contenga il catalogo **SSISDB** . Tramite il programma di installazione per [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] non viene creato automaticamente il catalogo. Sarà necessario crearlo manualmente usando le istruzioni seguenti.  
  
 Il catalogo SSISDB può essere creato in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Il catalogo può essere creato anche a livello di programmazione utilizzando Windows PowerShell.  
  
### <a name="to-create-the-ssisdb-catalog-in-sql-server-management-studio"></a>Per creare il catalogo SSISDB in SQL Server Management Studio  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Connettersi al motore di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  In Esplora oggetti espandere il nodo del server, fare clic con il pulsante destro del mouse sul nodo **Cataloghi di Integration Services** e quindi fare clic su **Creazione catalogo**.  
  
4.  Fare clic su **Abilitazione integrazione con CLR**.  
  
     Nel catalogo vengono utilizzate stored procedure CLR.  
  
5.  Fare clic su **Abilita l'esecuzione automatica della stored procedure di Integration Services all'avvio di SQL Server** per abilitare l'esecuzione della stored procedure [catalog.startup](../../integration-services/system-stored-procedures/catalog-startup.md) a ogni riavvio dell’istanza del server [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
     La stored procedure esegue la manutenzione dello stato delle operazioni per il catalogo SSISDB. Corregge lo stato di eventuali pacchetti in esecuzione in caso di arresto dell'istanza del server [!INCLUDE[ssIS](../../includes/ssis-md.md)].  
  
6.  Immettere una password quindi fare clic su **Ok**.  
  
     La password consente di proteggere la chiave del database master utilizzata per crittografare i dati del catalogo. Salvare la password in un percorso sicuro. È consigliabile eseguire inoltre il backup della chiave master del database. Per altre informazioni, vedere [Backup della chiave master di un database](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
### <a name="to-create-the-ssisdb-catalog-programmatically"></a>Per creare il catalogo SSISDB a livello di programmazione  
  
1.  Eseguire lo script di PowerShell seguente:  
  
    ```  
    # Load the IntegrationServices Assembly  
    [Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices")  
  
    # Store the IntegrationServices Assembly namespace to avoid typing it every time  
    $ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"  
  
    Write-Host "Connecting to server ..."  
  
    # Create a connection to the server  
    $sqlConnectionString = "Data Source=localhost;Initial Catalog=master;Integrated Security=SSPI;"  
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString  
  
    # Create the Integration Services object  
    $integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection  
  
    # Provision a new SSIS Catalog  
    $catalog = New-Object $ISNamespace".Catalog" ($integrationServices, "SSISDB", "P@assword1")  
    $catalog.Create()  
  
    ```  
  
     Per altri esempi di come usare Windows PowerShell e lo spazio dei nomi <xref:Microsoft.SqlServer.Management.IntegrationServices>, vedere l'intervento sul blog relativo a [SSIS e PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015) nel sito blogs.msdn.com. Per una panoramica dello spazio dei nomi e degli esempi di codice, vedere l'intervento sul blog relativo a [uno sguardo rapido del modello a oggetti gestito del catalogo SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)sul sito blogs.msdn.com.  

## <a name="catalog-properties-dialog-box"></a>Finestra di dialogo Proprietà catalogo
  Utilizzare la finestra di dialogo Proprietà catalogo per configurare il catalogo di SSISDB. Le proprietà del catalogo consentono di definire come vengono crittografati i dati sensibili, come vengono mantenuti i dati del controllo delle versioni dei progetti e quando si verifica il timeout delle operazioni di convalida. Il catalogo di SSISDB rappresenta un punto centrale di archiviazione e amministrazione di progetti, pacchetti, parametri e ambienti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 È anche possibile visualizzare le proprietà del catalogo nella vista `catalog.catalog_properties` e impostare le proprietà tramite la stored procedure `catalog.configure_catalog`. Per altre informazioni, vedere [catalog.catalog_properties &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) e [catalog.configure_catalog &#40;database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Per saperne di più**  
  
-   [Aprire la finestra di dialogo Proprietà catalogo](#open_dialog)  
  
-   [Configurare le opzioni](#options)  
  
###  <a name="open-the-catalog-properties-dialog-box"></a><a name="open_dialog"></a> Aprire la finestra di dialogo Proprietà catalogo  
  
1.  Aprire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Connettersi al motore di database di Microsoft SQL Server.  
  
3.  In Esplora oggetti espandere il nodo **Integration Services** , fare clic con il pulsante destro del mouse su **SSISDB**, quindi fare clic su **Proprietà**.  
  
###  <a name="configure-the-options"></a><a name="options"></a> Configurare le opzioni  
  
#### <a name="options"></a>Opzioni  
 La tabella seguente illustra determinate proprietà nella finestra di dialogo e le proprietà corrispondenti nella vista `catalog.catalog_properties`.  
  
|Nome proprietà (finestra di dialogo Proprietà catalogo)|Nome proprietà (vista catalog.catalog_properties)|Descrizione|  
|-----------------------------------------------------|------------------------------------------------------|-----------------|  
|Nome algoritmo di crittografia|ENCRYPTION_ALGORITHM|Consente di specificare il tipo di crittografia utilizzato per crittografare i valori di parametro sensibili nel catalogo. Di seguito sono indicati i valori possibili:<br /><br /> DES<br /><br /> TRIPLE_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESPX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256 (predefinito)|  
|Numero massimo di versioni per progetto|MAX_PROJECT_VERSIONS|Specificare il numero di versioni di un progetto archiviate nel catalogo. Le versioni precedenti di progetti che superano il numero massimo vengono rimosse quando viene eseguito il processo di pulizia della versione del progetto.|  
|Pulisci log periodicamente|OPERATION_CLEANUP_ENABLED|Impostare la proprietà su True per indicare l'esecuzione della pulizia di operazioni da parte del processo di SQL Server Agent. In caso contrario, impostare la proprietà su False.|  
|Periodo di memorizzazione (giorni)|RETENTION_WINDOW|Specificare la validità massima di dati di operazioni consentiti (in giorni). I dati che superano il numero di giorni specificato vengono rimossi dalla pulizia delle operazioni del processo di SQL Agent.|  

## <a name="back-up-restore-and-move-the-ssis-catalog"></a>Backup, ripristino e spostamento del catalogo SSIS
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] è incluso il database SSISDB. È possibile eseguire una query sulle viste nel database SSISDB per verificare oggetti, impostazioni e dati operativi archiviati nel catalogo **SSISDB** . In questo argomento vengono fornite istruzioni per l'esecuzione del backup e del ripristino del database.  
  
 Nel catalogo **SSISDB** sono archiviati i pacchetti distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Per ulteriori informazioni sul catalogo, vedere [Catalogo SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
###  <a name="to-back-up-the-ssis-database"></a><a name="backup"></a> Per eseguire il backup del database SSIS  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Eseguire il backup della chiave master per il database SSISDB tramite l'istruzione Transact-SQL BACKUP MASTER KEY. La chiave viene archiviata in un file specificato. Utilizzare una password per crittografare la chiave master nel file.  
  
     Per altre informazioni sull'istruzione, vedere [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-master-key-transact-sql.md).  
  
     Nell'esempio seguente la chiave master viene esportata nel file `c:\temp directory\RCTestInstKey` . Per crittografare la chiave master viene utilizzata la password `LS2Setup!` .  
  
    ```  
    backup master key to file = 'c:\temp\RCTestInstKey'  
           encryption by password = 'LS2Setup!'  
  
    ```  
  
3.  Eseguire il backup del database SSISDB tramite la finestra di dialogo **Backup database** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere [Procedura: Eseguire il backup del database (SQL Server Management Studio)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
4.  Generare lo script CREATE LOGIN per ##MS_SSISServerCleanupJobLogin##, effettuando le operazioni riportate di seguito. Per altre informazioni, vedere [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
    1.  In Esplora oggetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]espandere il nodo **Sicurezza** , quindi espandere il nodo **Account di accesso** .  
  
    2.  Fare clic con il pulsante destro del mouse su **##MS_SSISServerCleanupJobLogin##** , quindi fare clic su **Crea script per account di accesso** > **Genera codice per istruzione CREATE in** > **Nuova finestra editor di query**.  
  
5.  Se si ripristina il database SSISDB in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il catalogo SSISDB non è mai stato creato, generare lo script CREATE PROCEDURE per sp_ssis_startup, effettuando le operazioni riportate di seguito. Per altre informazioni, vedere [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
    1.  In Esplora oggetti espandere il nodo **Database**, quindi espandere **master** > **Programmabilità** > nodo **Stored procedure**.  
  
    2.  Fare clic con il pulsante destro del mouse su **dbo.sp_ssis_startup**, quindi fare clic su **Crea script per stored procedure** > **Genera codice per istruzione CREATE in** > **Nuova finestra editor di query**.  
  
6.  Verificare che SQL Server Agent sia stato avviato.  
  
7.  Se si ripristina il database SSISDB in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il catalogo SSISDB non è mai stato creato, generare uno script per il processo di manutenzione del server SSIS, effettuando le operazioni riportate di seguito. Lo script viene creato automaticamente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent quando viene creato il catalogo SSISDB. Il processo consente di pulire i log operazioni di pulizia al di fuori del periodo di memorizzazione e di rimuovere le versioni precedenti dei progetti.  
  
    1.  In Esplora oggetti espandere il nodo **SQL Server Agent** , quindi espandere il nodo **Processi** .  
  
    2.  Fare clic con il pulsante destro del mouse sul processo di manutenzione del server SSIS e quindi scegliere su **Crea script per processo** > **Genera codice per istruzione CREATE in** > **Nuova finestra editor di query**.  
  
### <a name="to-restore-the-ssis-database"></a>Per ripristinare il database SSIS  
  
1.  Se si ripristina il database SSISDB su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il catalogo SSISDB non è mai stato creato, abilitare Common Language Runtime (CLR) eseguendo la stored procedure `sp_configure`. Per altre informazioni, vedere [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) e [Opzione clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md).  
  
    ```  
    use master   
           sp_configure 'clr enabled', 1  
           reconfigure  
  
    ```  
  
2.  Se si ripristina il database SSISDB a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il catalogo SSISDB non è mai stato creato, creare la chiave asimmetrica e l'accesso da quest'ultima e concedere l'autorizzazione UNSAFE all'account di accesso.  

    ```  
    Create Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey  
           FROM Executable File = 'C:\Program Files\Microsoft SQL Server\YourSQLServerDefaultCompatibilityLevel\DTS\Binn\Microsoft.SqlServer.IntegrationServices.Server.dll'  
    ```  

    È possibile trovare il valore per `YourSQLServerDefaultCompatibilityLevel` da un [elenco di livelli di compatibilità predefiniti di SQL Server](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments).
  
    [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Per le stored procedure CLR è necessario concedere le autorizzazioni UNSAFE all'account di accesso, poiché per questo account è richiesto un accesso aggiuntivo alle risorse limitate, ad esempio l'API Microsoft Win32. Per altre informazioni sull'autorizzazione codice UNSAFE, vedere [Creazione di un assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md).  

    ```  
    Create Login ##MS_SQLEnableSystemAssemblyLoadingUser## FROM Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey   
    Grant Unsafe Assembly to ##MS_SQLEnableSystemAssemblyLoadingUser##    
    ```  
  
3.  Ripristinare il database SSISDB dal backup tramite la finestra di dialogo **Ripristina database** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere gli argomenti seguenti:  
  
    -   [Ripristina database &#40;pagina Generale&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
    -   [Ripristina database &#40;pagina File&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
    -   [Ripristina database &#40;pagina Opzioni&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
4.  Eseguire gli script creati nella procedura [Per eseguire il backup del database SSIS](#backup) per ##MS_SSISServerCleanupJobLogin##, sp_ssis_startup e per il processo di manutenzione del server SSIS. Verificare che SQL Server Agent sia stato avviato.  
  
5.  Eseguire l'istruzione riportata di seguito per impostare l'esecuzione automatica della stored procedure sp_ssis_startup. Per altre informazioni, vedere [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md).  
  
    ```  
    EXEC sp_procoption N'sp_ssis_startup','startup','on'  
    ```  
  
6.  Eseguire il mapping dell'utente di SSISDB ##MS_SSISServerCleanupJobUser## (database SSISDB) a ##MS_SSISServerCleanupJobLogin## tramite la finestra di dialogo **Proprietà account di accesso** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
7.  Ripristinare la chiave master utilizzando uno dei metodi riportati di seguito. Per ulteriori informazioni sulla crittografia e sulle chiavi master, vedere [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
    -   **Metodo 1**  
  
         Usare questo metodo se si è già eseguito un backup della chiave master del database e si dispone della password usata per crittografare la chiave master.  
  
        ```  
               Restore master key from file = 'c:\temp\RCTestInstKey'  
               Decryption by password = 'LS2Setup!' -- 'Password used to encrypt the master key during SSISDB backup'  
               Encryption by password = 'LS3Setup!' -- 'New Password'  
               Force  
  
        ```  
  
        > [!NOTE]  
        >  Verificare che l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponga delle autorizzazioni per leggere il file della chiave di backup.  
  
        > [!NOTE]  
        >  Se la chiave master del database non è stata ancora crittografata dalla chiave master del servizio, si riceve il messaggio di avviso seguente visualizzato in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Ignorare il messaggio.  
        >   
        >  **Impossibile decrittografare la chiave master corrente. L'errore è stato ignorato perché è stata specificata l'opzione FORCE.**  
        >   
        >  L'argomento FORCE consente di specificare che è consigliabile che il processo di ripristino continui anche se la chiave master del database corrente non è aperta. Per il catalogo SSISDB, dal momento che la chiave master del database non è stata aperta nell'istanza in cui si esegue il ripristino del database, viene visualizzato questo messaggio.  
  
    -   **Metodo 2**  
  
         Utilizzare questo metodo se si dispone della password originale utilizzata per creare SSISDB.  
  
        ```  
        open master key decryption by password = 'LS1Setup!' --'Password used when creating SSISDB'  
               Alter Master Key Add encryption by Service Master Key  
        ```  
  
8.  Determinare se lo schema del catalogo SSISDB e i file binari di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (assembly ISServerExec e SQLCLR) sono compatibili eseguendo [catalog.check_schema_version](../../integration-services/system-stored-procedures/catalog-check-schema-version.md).  
  
9. Per verificare il corretto ripristino del database SSISDB, effettuare delle operazioni nel catalogo SSISDB, ad esempio l'esecuzione dei pacchetti distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per altre informazioni, vedere [Eseguire pacchetti di Integration Services (SSIS)](../../integration-services/packages/run-integration-services-ssis-packages.md).  
  
### <a name="to-move-the-ssis-database"></a>Per spostare il database SSIS  
  
-   Seguire le istruzioni per lo spostamento di database utente. Per altre informazioni, vedere [Spostare database utente](../../relational-databases/databases/move-user-databases.md).  
  
     Assicurarsi che venga eseguito il backup della chiave master per il database SSISDB e proteggere il file di backup. Per altre informazioni, vedere [Per eseguire il backup del database SSIS](#backup).  
  
     Assicurarsi che gli oggetti pertinenti a Integration Services (SSIS) vengano creati nella nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui non è ancora stato creato il catalogo SSISDB.  

## <a name="upgrade-the-ssis-catalog-ssisdb"></a>Aggiornare il catalogo SSIS (SSISDB)
  Eseguire Aggiornamento guidato del database SSISDB per aggiornare il database di catalogo SSIS, SSISDB, quando il database è antecedente alla versione corrente dell'istanza di SQL Server. Il database può essere antecedente quando viene soddisfatta una delle condizioni seguenti.  
  
-   Il database è stato ripristinato da una versione precedente di SQL Server.  
  
-   Il database non è stato rimosso da un gruppo di disponibilità AlwaysOn prima di aggiornare l'istanza di SQL Server. Questa condizione impedisce l'aggiornamento automatico del database. Per ulteriori informazioni, vedere [Upgrading SSISDB in an availability group](#Upgrade).  
  
 La procedura guidata può aggiornare solo il database in un'istanza del server locale.  
  
### <a name="upgrade-the-ssis-catalog-ssisdb-by-running-the-ssisdb-upgrade-wizard"></a>Aggiornare il catalogo SSIS (SSISDB) con Aggiornamento guidato del database SSISDB  
  
1.  Eseguire il backup del database del catalogo SSIS, SSISDB.  
  
2.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]espandere il server locale e **Cataloghi di Integration Services**.  
  
3.  Fare clic con il pulsante destro del mouse su **SSISDB** e quindi selezionare **Aggiornamento database** per avviare Aggiornamento guidato del database SSISDB. In alternativa, avviare l'Aggiornamento guidato del database SSISDB eseguendo `C:\Program Files\Microsoft SQL Server\140\DTS\Binn\ISDBUpgradeWizard.exe` con autorizzazioni elevate nel server locale.
  
     ![Avviare l'Aggiornamento guidato del database SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-1.png)

4.  Nella pagina **Seleziona istanza** selezionare un'istanza di SQL Server nel server locale.  
  
    > [!IMPORTANT]  
    >  La procedura guidata può aggiornare solo il database in un'istanza del server locale.  
  
     Selezionare la casella di controllo per indicare che è stato eseguito il backup del database SSISDB prima della procedura guidata.  
  
     ![Selezionare il server nell'Aggiornamento guidato SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-2.png "Selezionare il server nell'Aggiornamento guidato SSISDB")  
  
5.  Selezionare **Aggiorna** per aggiornare il database del catalogo SSIS.  
  
6.  Nella pagina **Risultato** esaminare i risultati.  
  
     ![Rivedere i risultati nell'Aggiornamento guidato SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-3.png "Rivedere i risultati nell'Aggiornamento guidato SSISDB")  

## <a name="always-on-for-ssis-catalog-ssisdb"></a>Always On per il catalogo SSIS (SSISDB)
  I gruppi di disponibilità Always On sono una soluzione di disponibilità elevata e recupero di emergenza che offre un'alternativa di livello enterprise al mirroring del database. Un gruppo di disponibilità supporta un ambiente di failover per un set discreto di database utente, noto come database di disponibilità, il cui failover avviene contemporaneamente. Per altre informazioni, vedere [Gruppi di disponibilità AlwaysOn](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 Per garantire una disponibilità elevata del catalogo SSIS (SSISDB) e del relativo contenuto (progetti, pacchetti, log di esecuzione e così via) è possibile aggiungere il database SSISDB a un gruppo di disponibilità Always On, come accade con qualsiasi altro database utente. Quando si verifica un failover, uno dei nodi secondari diventa automaticamente il nuovo nodo primario.  
 
 > [!IMPORTANT]
 > Quando si verifica un failover, i pacchetti in esecuzione non vengono riavviati o ripresi. 
 
 **Contenuto della sezione:**  
  
1.  [Prerequisiti](#prereq)  
  
2.  [Configurare il supporto SSIS per Always On](#Firsttime)  
  
3.  [Aggiornamento di SSISDB in un gruppo di disponibilità](#Upgrade)  
  
###  <a name="prerequisites"></a><a name="prereq"></a> Prerequisiti  
Eseguire questi passaggi preliminari prima di abilitare il supporto Always On per il database SSISDB.  
  
1.  Configurare un cluster di failover di Windows Vedere il post del blog relativo all’ [installazione della funzionalità e degli strumenti per il cluster di failover per Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) per le istruzioni. Installare la funzionalità e gli strumenti in tutti i nodi del cluster.  
  
2.  Installare SQL Server 2016 con Integration Services (SSIS) in ogni nodo del cluster.  
  
3.  Abilitare i gruppi di disponibilità Always On per ogni istanza di SQL server. Per altre informazioni, vedere [Abilitare e disabilitare la funzionalità Gruppi di disponibilità AlwaysOn](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md) .  
  
###  <a name="configure-ssis-support-for-always-on"></a><a name="Firsttime"></a> Configurare il supporto SSIS per Always On  
  
-   [Passaggio 1: Creare un catalogo di Integration Services](#Step1)  
  
-   [Passaggio 2: Aggiungere SSISDB a un gruppo di disponibilità Always On](#Step2)  
  
-   [Passaggio 3: Abilitare il supporto SSIS per Always On](#Step3)  
  
> [!IMPORTANT]  
> -   È necessario attenersi alla procedura seguente nel **nodo primario** del gruppo di disponibilità.
> -   Abilitare il **supporto SSIS per Always On** *dopo* aver aggiunto SSISDB a un gruppo di disponibilità Always On.  

> [!NOTE]
> Per altre informazioni, vedere la procedura dettagliata con screenshot aggiuntivi redatta da Marcos Freccia, Data Platform MVP: [Adding SSISDB to AG for SQL Server 2016](https://marcosfreccia.com/2017/04/28/adding-ssisdb-to-ag-for-sql-server-2016/) (Aggiunta di SSISDB a un gruppo di disponibilità per SQL Server 2016).

####  <a name="step-1-create-integration-services-catalog"></a><a name="Step1"></a> Passaggio 1: Creare un catalogo di Integration Services  
  
1.  Avviare **SQL Server Management Studio** e connettersi a un'istanza di SQL Server nel cluster che si vuole definire come **nodo primario** del gruppo di disponibilità Always On per SSISDB.  
  
2.  In Esplora oggetti espandere il nodo del server, fare clic con il pulsante destro del mouse sul nodo **Cataloghi di Integration Services** e quindi fare clic su **Creazione catalogo**.  
  
3.  Fare clic su **Abilitazione integrazione con CLR**. Nel catalogo vengono utilizzate stored procedure CLR.  
  
4.  Fare clic su **Abilita l’esecuzione automatica della stored procedure di Integration Services all’avvio di SQL Server** per abilitare l’esecuzione della stored procedure [catalog.startup](../system-stored-procedures/catalog-startup.md) a ogni riavvio dell’istanza del server SSIS. La stored procedure esegue la manutenzione dello stato delle operazioni per il catalogo SSISDB. Corregge lo stato di eventuali pacchetti in esecuzione in caso di arresto dell'istanza del server SSIS.  
  
5.  Immettere una **password** e quindi fare clic su **Ok**. La password consente di proteggere la chiave del database master utilizzata per crittografare i dati del catalogo. Salvare la password in un percorso sicuro. È consigliabile eseguire inoltre il backup della chiave master del database. Per altre informazioni, vedere [Backup della chiave master di un database](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
####  <a name="step-2-add-ssisdb-to-an-always-on-availability-group"></a><a name="Step2"></a> Passaggio 2: Aggiungere SSISDB a un gruppo di disponibilità Always On  
Per aggiungere il database SSISDB a un gruppo di disponibilità Always On, si procede come per l'aggiunta di qualsiasi altro database utente a un gruppo di disponibilità. Vedere [Utilizzare la Creazione guidata Gruppo di disponibilità](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
Inserire la password specificata durante la creazione del catalogo SSIS nella pagina **Seleziona database** della creazione guidata **Nuovo gruppo di disponibilità**.

![Nuovo gruppo di disponibilità](../../integration-services/service/media/ssis-newavailabilitygroup.png "Nuovo gruppo di disponibilità")  
  
####  <a name="step-3-enable-ssis-support-for-always-on"></a><a name="Step3"></a> Passaggio 3: Abilitare il supporto SSIS per Always On  
 Dopo aver creato il catalogo di Integration Services, fare clic con il pulsante destro del mouse sul nodo **Cataloghi di Integration Services** e quindi scegliere **Abilita supporto per AlwaysOn**. Verrà visualizzata la finestra di dialogo **Abilita supporto per AlwaysOn** illustrata di seguito. Se questa voce di menu è disabilitata, verificare di avere installato tutti i prerequisiti e fare clic su **Aggiorna**.  
  
 ![Abilitare il supporto per AlwaysOn](../../integration-services/service/media/ssis-enablesupportforalwayson.png)  
  
> [!WARNING]  
>  Fino a quando non si abilita il supporto SSIS per Always On, il failover automatico del database SSISDB non è supportato.  
  
 Le repliche secondarie appena aggiunte dal gruppo di disponibilità AlwaysOn sono visualizzate nella tabella. Fare clic sul pulsante **Connetti** per ogni replica dell'elenco e immettere le credenziali di autenticazione per la connessione alla replica. Per abilitare il supporto SSIS per AlwaysOn, l'account utente deve appartenere al gruppo sysadmin in ogni replica. Dopo aver eseguito la connessione a ogni replica, fare clic su **OK** per abilitare il supporto SSIS per Always On.  
 
Se l'opzione **Abilita supporto per AlwaysOn** nel menu di scelta rapida risulta disabilitata dopo aver completato gli altri prerequisiti, provare quanto segue:
1.  Aggiornare il menu di scelta rapida facendo clic sull'opzione **Aggiorna**.
2.  Verificare di essere connessi al nodo primario. È necessario abilitare il supporto AlwaysOn nel nodo primario.
3.  Verificare che la versione di SQL Server sia 13.0 o successiva. SSIS supporta AlwaysOn solo in SQL Server 2016 e versioni successive.

###  <a name="upgrading-ssisdb-in-an-availability-group"></a><a name="Upgrade"></a> Aggiornamento di SSISDB in un gruppo di disponibilità  
 Se si sta aggiornando SQL Server da una versione precedente e SSISDB è incluso in un gruppo di disponibilità Always On, l'aggiornamento può essere bloccato dalla regola "Verifica presenza di SSISDB in un gruppo di disponibilità Always On". Si ha tale blocco perché l'aggiornamento viene eseguito in modalità utente singolo, mentre un database di disponibilità deve essere un database multiutente. Di conseguenza durante l'aggiornamento o l'applicazione di patch, tutti i database di disponibilità, incluso SSISDB, sono portati offline e non sono aggiornati né corretti. Per completare l'aggiornamento, per prima cosa rimuovere SSISDB dal gruppo di disponibilità, quindi aggiornare o applicare le patch a ogni nodo e infine aggiungere di nuovo SSISDB al gruppo di disponibilità.  
  
 Se la regola "Verifica presenza di SSISDB in un gruppo di disponibilità AlwaysOn" blocca l'aggiornamento, per aggiornare SQL Server seguire questa procedura.  
  
1.  Rimuovere il database SSISDB dal gruppo di disponibilità. Per altre informazioni, vedere [Rimuovere un database secondario da un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md) e [Rimuovere un database primario da un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
2.  Fare clic su **Riesegui** nell'aggiornamento guidato. Il controllo della regola "Verifica presenza di SSISDB in un gruppo di disponibilità AlwaysOn" viene superato.  
  
3.  Fare clic su **Avanti** per continuare l’aggiornamento.  
  
4.  Dopo aver aggiornato tutti i nodi, aggiungere di nuovo il database SSISDB al gruppo di disponibilità Always On. Per altre informazioni, vedere [Aggiungere un database a un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  
 Se durante l'aggiornamento di SQL Server non si verifica alcun blocco e se SSISDB è presente in un gruppo di disponibilità Always On, aggiornare separatamente SSISDB dopo aver aggiornato il motore di database di SQL Server. Usare l'aggiornamento guidato SSIS per aggiornare il database SSISDB come descritto nella procedura seguente.  
  
1.  Spostare il database SSISDB dal gruppo di disponibilità, o eliminare quest’ultimo se SSISDB è l'unico database nel gruppo. Avviare **SQL Server Management Studio** nel **nodo primario** del gruppo di disponibilità per eseguire questa attività.  
  
2.  Rimuovere il database SSISDB da tutti i **nodi di replica**.  
  
3.  Aggiornare il database SSISDB nel **nodo primario**. In **Esplora oggetti** di SQL Server Management Studio espandere i **Cataloghi di Integration Services**, fare clic con il pulsante destro del mouse su **SSISDB** e quindi scegliere **Aggiornamento database**. Seguire le istruzioni dell’ **Aggiornamento guidato SSISDB** per aggiornare il database. Avviare l'**aggiornamento guidato di SSIDB** localmente nel **nodo primario**.  
  
4.  Seguire le istruzioni al [Passaggio 2: Aggiungere SSISDB a un gruppo di disponibilità Always On](#Step2) per aggiungere di nuovo il database SSISDB a un gruppo di disponibilità.  
  
5.  Seguire le istruzioni al [Passaggio 3: Abilitare il supporto SSIS per Always On](#Step3).  


## <a name="ssisdb-catalog-and-delegation-in-double-hop-scenarios"></a>Catalogo SSISDB e delega in scenari con doppio hop

Per impostazione predefinita, la chiamata remota dei pacchetti SSIS archiviati nel catalogo SSISDB non supporta la delega delle credenziali, detta anche doppio hop. 

Si immagini uno scenario in cui un utente accede al computer client A e avvia SQL Server Management Studio (SSMS). In SSMS l'utente si connette a un server SQL ospitato nel computer B, che contiene il catalogo SSISDB. Il pacchetto SSIS viene archiviato in questo catalogo SSISDB e a sua volta si connette a un servizio SQL Server in esecuzione nel computer C (il pacchetto potrebbe anche accedere a qualsiasi altro servizio). Quando l'utente richiama l'esecuzione del pacchetto SSIS dal computer A, SSMS passa innanzitutto le credenziali dell'utente dal computer A al computer B (dove il processo di runtime SSIS sta eseguendo il pacchetto). A questo punto è necessario il processo runtime di esecuzione SSIS (ISServerExec.exe) per delegare le credenziali dell'utente dal computer B al computer C affinché l'esecuzione venga completata correttamente. La delega delle credenziali non è tuttavia abilitata per impostazione predefinita.

Un utente può abilitare la delega delle credenziali concedendo il diritto *Utente attendibile per la delega a qualsiasi servizio (solo Kerberos)* all'account del servizio SQL Server (nel computer B), che avvia ISServerExec.exe come processo figlio. Questo processo è noto come configurazione della delega non vincolata o della delega aperta per un account del servizio SQL Server. Prima di concedere questo diritto, valutare se soddisfa i requisiti di sicurezza dell'organizzazione.

SSISDB non supporta la delega vincolata. In un ambiente a doppio hop, se l'account del servizio SQL Server che ospita il catalogo SSISDB (computer B nell'esempio) è configurato per la delega vincolata, ISServerExec.exe non sarà in grado di delegare le credenziali al terzo computer (computer C). Questa condizione è applicabile agli scenari in cui è abilitato Windows Defender Credential Guard, che richiede obbligatoriamente la configurazione della delega vincolata.

  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   Intervento nel blog su [SSIS e PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015)sul sito Web blogs.msdn.com.  
  
-   Intervento nel blog sui [suggerimenti per il controllo dell'accesso al catalogo SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-catalog-access-control-tips/ba-p/388057)sul sito Web blogs.msdn.com.  
  
-   Intervento nel blog relativo a [uno sguardo rapido del modello a oggetti gestito del catalogo SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)su blogs.msdn.com.