---
description: Distribuire progetti e pacchetti di Integration Services (SSIS)
title: Distribuire progetti e pacchetti di Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: vanto
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.bids.converttolegacydeployment.f1
- sql13.ssis.deploymentwizard.f1
- sql13.ssis.ssms.isenvprop.permissions.f1
- sql13.ssis.ssms.isenvprop.general.f1
- sql13.ssis.ssms.iscreateenv.f1
- sql13.ssis.ssms.isenvprop.variables.f1
- sql13.ssis.migrationwizard.f1
ms.assetid: bea8ce8d-cf63-4257-840a-fc9adceade8c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5ee14ee789174ad12db2cea297202fc63ee68d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346934"
---
# <a name="deploy-integration-services-ssis-projects-and-packages"></a>Distribuire progetti e pacchetti di Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] supporta due modelli di distribuzione, ovvero il modello di distribuzione del progetto e il modello di distribuzione del pacchetto legacy. Tramite il modello di distribuzione del progetto è possibile distribuire i progetti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
Per altre informazioni sul modello di distribuzione del pacchetto legacy, vedere [Distribuzione del pacchetto legacy &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
> [!NOTE]  
>  Il modello di distribuzione del progetto è stato introdotto in [!INCLUDE[ssISversion11](../../includes/ssisversion11-md.md)]. Con questo modello di distribuzione non era possibile distribuire uno o più pacchetti senza distribuire l'intero progetto. In [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] è stata introdotta la funzionalità di distribuzione dei pacchetti incrementale che consente di distribuire uno o più pacchetti senza distribuire l'intero progetto.

> [!NOTE]
> In questo articolo viene illustrato come distribuire i pacchetti SSIS a livello generale e come distribuire i pacchetti in locale. È possibile distribuire i pacchetti SSIS anche nelle piattaforme seguenti:
> - **Cloud di Microsoft Azure**. Per altre informazioni, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
> - **Linux**. Per altre informazioni, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md).

## <a name="compare-project-deployment-model-and-legacy-package-deployment-model"></a>Confrontare il modello di distribuzione del progetto e il modello di distribuzione del pacchetto legacy

 Il tipo di modello di distribuzione scelto determina le opzioni di sviluppo e amministrazione disponibili per il progetto. Nella tabella seguente vengono illustrate le differenze e le similitudini tra l'utilizzo del modello di distribuzione del progetto e quello del pacchetto.  
  
|Utilizzo del modello di distribuzione del progetto|Uso del modello di distribuzione del pacchetto legacy|  
|---------------------------------------------|----------------------------------------------------|  
|L'unità di distribuzione è un progetto.|L'unità di distribuzione è un pacchetto.|  
|Per assegnare valori alle proprietà del pacchetto si utilizzano i parametri.|Per assegnare valori alle proprietà del pacchetto si utilizzano le configurazioni.|  
|Un progetto, contenente pacchetti e parametri, viene compilato su un file di distribuzione del progetto (estensione ispac).|I pacchetti (estensione dtsx) e le configurazioni (estensione dtsConfig) vengono salvati singolarmente nel file system.|  
|Un progetto, contenente pacchetti e parametri, viene distribuito nel catalogo SSISDB in un'istanza di SQL Server.|I pacchetti e le configurazioni vengono copiati nel file system in un altro computer. I pacchetti possono anche essere salvati nel database MSDB in un'istanza di SQL Server.|  
|L'integrazione con CLR è necessaria nel motore di database.|L'integrazione con CLR non è necessaria nel motore di database.|  
|I valori dei parametri specifici dell'ambiente vengono archiviati nelle variabili di ambiente.|I valori di configurazione specifici dell'ambiente vengono archiviati nei file di configurazione.|  
|I progetti e i pacchetti nel catalogo possono essere convalidati nel server prima dell'esecuzione. È possibile utilizzare SQL Server Management Studio, le stored procedure o il codice gestito per eseguire la convalida.|I pacchetti vengono convalidati appena prima dell'esecuzione. È anche possibile convalidare un pacchetto con dtExec o codice gestito.|  
|I pacchetti vengono eseguiti avviando un'esecuzione nel motore di database. L'identificatore di un progetto, i valori di parametri espliciti (facoltativi) e i riferimenti all'ambiente (facoltativi) vengono assegnati a un'esecuzione prima dell'avvio.<br /><br /> I pacchetti possono essere eseguiti anche usando **dtExec**.|I pacchetti vengono eseguiti usando le utilità di esecuzione **dtExec** e **DTExecUI** . Le configurazioni applicabili vengono identificate tramite argomenti del prompt dei comandi (facoltativo).|  
|Durante l'esecuzione, gli eventi generati dal pacchetto vengono acquisiti automaticamente e salvati nel catalogo. È possibile eseguire query su questi eventi con le viste Transact-SQL.|Durante l'esecuzione, gli eventi generati da un pacchetto non vengono acquisiti automaticamente. Per acquisire gli eventi, è necessario aggiungere un provider di log al pacchetto.|  
|I pacchetti vengono eseguiti in un processo di Windows separato.|I pacchetti vengono eseguiti in un processo di Windows separato.|  
|Per pianificare l'esecuzione dei pacchetti si utilizza SQL Server Agent.|Per pianificare l'esecuzione dei pacchetti si utilizza SQL Server Agent.|  


## <a name="features-of-project-deployment-model"></a>Funzionalità del modello di distribuzione del progetto  
 Nella tabella seguente sono elencate le funzionalità disponibili per i progetti sviluppati solo per il modello di distribuzione del progetto.  
  
|Feature|Descrizione|  
|-------------|-----------------|  
|Parametri|Un parametro specifica i dati che verranno utilizzati da un pacchetto. È possibile determinare l'ambito dei parametri a livello di pacchetto o di progetto, rispettivamente con i parametri di pacchetto e i parametri di progetto. I parametri possono essere utilizzati in espressioni o attività. Quando il progetto viene distribuito nel catalogo, è possibile assegnare un valore letterale a ogni parametro oppure utilizzare il valore predefinito assegnato in fase di progettazione. Al posto di un valore letterale, è anche possibile fare riferimento a una variabile di ambiente. I valori delle variabili di ambiente vengono risolti al momento dell'esecuzione del pacchetto.|  
|Ambienti|Un ambiente è un contenitore di variabili a cui i progetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] possono fare riferimento. Ogni progetto può presentare più riferimenti all'ambiente, tuttavia una singola istanza di esecuzione del pacchetto può fare riferimento solo alle variabili di un ambiente. Gli ambienti consentono di organizzare i valori assegnati a un pacchetto. È ad esempio possibile disporre di ambienti denominati "Dev", "test" e "Produzione".|  
|Variabili di ambiente|Una variabile di ambiente definisce un valore letterale che è possibile assegnare a un parametro durante l'esecuzione del pacchetto. Per utilizzare una variabile di ambiente, creare un riferimento all'ambiente (nel progetto che corrisponde all'ambiente con il parametro), assegnare un valore di parametro al nome della variabile di ambiente, quindi specificare il riferimento all'ambiente corrispondente quando si configura un'istanza di esecuzione.|  
|Catalogo SSISDB|Tutti gli oggetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vengono archiviati e gestiti in un'istanza di SQL Server in un database noto come catalogo SSISDB. Il catalogo consente di utilizzare cartelle per organizzare progetti e ambienti. Ogni istanza di SQL Server può disporre di un catalogo. Ogni catalogo può contenere zero o più cartelle. Ogni cartella può contenere zero o più progetti e zero o più ambienti. Una cartella del catalogo può anche essere utilizzata come limite per le autorizzazioni per gli oggetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|Viste e stored procedure del catalogo|È possibile utilizzare diverse stored procedure e viste per gestire gli oggetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nel catalogo. È ad esempio possibile specificare valori per parametri e variabili di ambiente, creare e avviare esecuzioni e monitorare le operazioni del catalogo. È inoltre possibile sapere esattamente quali valori verranno utilizzati da un pacchetto prima dell'avvio dell'esecuzione.|  
  
## <a name="project-deployment"></a>Distribuzione del progetto  
 Al centro del modello di distribuzione del progetto si trova il file di distribuzione con estensione ispac. Il file di distribuzione del progetto è un'unità di distribuzione autonoma che include solo le informazioni essenziali sui pacchetti e i parametri del progetto. Il file di distribuzione del progetto non acquisisce tutte le informazioni contenute nel file di progetto di Integration Services (estensione dtproj). Ad esempio, i file di testo aggiuntivi che si utilizzano per la scrittura di note non vengono archiviati nel file di distribuzione del progetto, pertanto non vengono distribuiti nel catalogo.  

## <a name="permissions-required-to-deploy-ssis-projects-and-packages"></a>Autorizzazioni necessarie per distribuire progetti e pacchetti SSIS

Se si modifica l'account del servizio SSIS in un account diverso da quello predefinito, per distribuire i pacchetti può essere necessario concedere autorizzazioni aggiuntive all'account del servizio non predefinito. Se l'account del servizio non predefinito non ha le autorizzazioni necessarie, può essere visualizzato il messaggio di errore seguente.

`A .NET Framework error occurred during execution of user-defined routine or aggregate "deploy_project_internal":
System.ComponentModel.Win32Exception: A required privilege is not held by the client.`

Questo errore è in genere dovuto alla mancanza di autorizzazioni DCOM. Per correggere l'errore, eseguire le operazioni seguenti:

1.  Aprire la console di **Servizi componenti** (o eseguire Dcomcnfg.exe).
2.  Nella console di **Servizi componenti** espandere **Servizi componenti** > **Computer** > **Risorse del computer** > **Configurazione DCOM**.
3.  Nell'elenco individuare **Microsoft SQL Server Integration Services xx.0** per la versione di SQL Server in uso. Per SQL Server 2016, ad esempio, la versione corretta è la 13.
4.  Fare clic con il pulsante destro del mouse e scegliere **Proprietà**.
5.  Nella finestra di dialogo **Proprietà Microsoft SQL Server Integration Services 13.0** selezionare la scheda **Sicurezza**.
6.  Per ognuno dei tre set di autorizzazioni (Avvio e attivazione, Accesso e Configurazione) selezionare **Personalizza** e quindi **Modifica** per aprire la finestra di dialogo **Autorizzazioni**.
7.  Nella finestra di dialogo **Autorizzazioni** aggiungere l'account del servizio non predefinito e concedere autorizzazioni **Consenti** secondo le esigenze. In genere, un account ha le autorizzazioni **Avvio locale** e **Attivazione locale**.
8.  Fare clic su **OK** due volte e quindi chiudere la console di **Servizi componenti**.

Per altre informazioni sull'errore descritto in questa sezione e sulle autorizzazioni richieste dall'account del servizio SSIS, vedere il post di blog seguente:
 
- [System.ComponentModel.Win32Exception: Privilegio obbligatorio non disponibile per il client durante la distribuzione di un progetto SSIS](/archive/blogs/dataaccesstechnologies/system-componentmodel-win32exception-a-required-privilege-is-not-held-by-the-client-while-deploying-ssis-project)

## <a name="deploy-projects-to-integration-services-server"></a>Distribuire progetti nel server Integration Services
  Nella versione corrente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]è possibile distribuire i progetti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Con il server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è possibile gestire ed eseguire pacchetti, nonché configurare i valori di runtime per i pacchetti tramite ambienti.  
  
> [!NOTE]  
>  Come nelle versioni precedenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], nella versione corrente è possibile distribuire i pacchetti in un'istanza di SQL Server e utilizzare il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per eseguire e gestire i pacchetti. Viene utilizzato il modello di distribuzione del pacchetto. Per altre informazioni, vedere [Distribuzione del pacchetto legacy &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
 Per distribuire un progetto nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , completare le attività seguenti:  
  
1.  Creare un catalogo SSISDB, se non è stato ancora creato. Per altre informazioni, vedere [Catalogo SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
2.  Convertire il progetto nel modello di distribuzione del progetto eseguendo la **Conversione guidata progetto di Integration Services**. Per altre informazioni, vedere le istruzioni seguenti: [Per convertire un progetto nel modello di distribuzione del progetto](#convert)  
  
    -   Se il progetto è stato creato in [!INCLUDE[ssISversion12](../../includes/ssisversion12-md.md)] o versione successiva, per impostazione predefinita il progetto usa il modello di distribuzione del progetto.  
  
    -   Se il progetto è stato creato in una versione precedente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], dopo aver aperto il file di progetto in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], convertirlo nel modello di distribuzione del progetto.  
  
        > [!NOTE]  
        >  Se nel progetto sono incluse una o più origini dati, queste ultime vengono rimosse al completamento della conversione del progetto. Per creare una connessione a un'origine dati che può essere condivisa dai pacchetti nel progetto, aggiungere una gestione connessione a livello del progetto. Per altre informazioni, vedere [aggiungere, eliminare o condividere una gestione connessione in un pacchetto](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130)).  
  
         Le diverse attività di conversione eseguite dalla **Conversione guidata progetto di Integration Services** variano a seconda che la procedura guidata venga eseguita da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
        -   Se la procedura guidata viene eseguita da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], i pacchetti contenuti nel progetto vengono convertiti da [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 2005, 2008 o 2008 R2 al formato utilizzato dalla versione corrente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Vengono aggiornati il progetto originale, con estensione dtproj, e i file di pacchetto, con estensione dtsx.  
  
        -   Se la procedura guidata viene eseguita da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vengono generati un file di distribuzione progetto, con estensione ispac, dai pacchetti e le configurazioni contenute nel progetto. I file di pacchetto, con estensione dtsx, originali non vengono aggiornati.  
  
             È possibile selezionare un file esistente o crearne uno nuovo nella pagina **Seleziona destinazione** della procedura guidata.  
  
             Per aggiornare i file di pacchetto durante la conversione di un progetto, eseguire la **Conversione guidata progetti di Integration Services** da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Per aggiornare i file di pacchetto separatamente rispetto alla conversione del progetto, eseguire la **Conversione guidata progetto di Integration Services** da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , quindi eseguire l' **Aggiornamento guidato pacchetti SSIS**. Se i file di pacchetto vengono aggiornati separatamente, assicurarsi di salvare le modifiche. In caso contrario, quando si converte il progetto nel modello di distribuzione del progetto, eventuali modifiche non salvate apportate al pacchetto non vengono convertite.  
  
     Per altre informazioni sull'aggiornamento di pacchetti, vedere [Aggiornare pacchetti di Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md) e [Aggiornare i pacchetti di Integration Services mediante l'Aggiornamento guidato pacchetti SSIS](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md).  
  
3.  Distribuire il progetto nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per ulteriori informazioni, vedere le istruzioni riportate di seguito: [Per distribuire un progetto nel server Integration Services](#deploy).  
  
4.  (Facoltativo) Creare un ambiente per il progetto distribuito. 
  
###  <a name="to-convert-a-project-to-the-project-deployment-model"></a><a name="convert"></a> Per convertire un progetto nel modello di distribuzione del progetto  
  
1.  Aprire il progetto in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], quindi in Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Converti nel modello di distribuzione del progetto**.  
  
     -oppure-  
  
     Da Esplora oggetti di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]fare clic con il pulsante destro del mouse sul nodo **Progetti** e selezionare **Importa pacchetto**.  
  
2.  Completare la procedura guidata.
  
###  <a name="to-deploy-a-project-to-the-integration-services-server"></a><a name="deploy"></a> Per distribuire un progetto nel server Integration Services  
  
1.  Aprire il progetto in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], quindi scegliere **Distribuisci** dal menu **Progetto** per avviare la **Distribuzione guidata Integration Services**.  
  
     o  
  
     In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] espandere il nodo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] > **SSISDB** in Esplora oggetti e individuare la cartella Progetti per il progetto da distribuire. Fare clic con il pulsante destro del mouse sulla cartella **Progetti** , quindi scegliere **Distribuzione progetto**.  
  
     o  
  
     Al prompt dei comandi eseguire **isdeploymentwizard.exe** da **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn**. Nei computer a 64 bit è presente anche una versione a 32 bit dello strumento in **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn**.  
  
2.  Nella pagina **Seleziona origine** fare clic su **File distribuzione progetto** per selezionare il file di distribuzione per il progetto.  
  
     o  
  
     Fare clic su **Catalogo di Integration Services** per selezionare un progetto che è già stato distribuito nel catalogo SSISDB.  
  
3.  Completare la procedura guidata. 

## <a name="deploy-packages-to-integration-services-server"></a>Distribuire pacchetti nel server Integration Services
  La funzionalità di distribuzione dei pacchetti incrementale introdotta in  [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] consente di distribuire uno o più pacchetti in un progetto nuovo o esistente senza distribuire l'intero progetto.  
  
###  <a name="deploy-packages-by-using-the-integration-services-deployment-wizard"></a><a name="DeployWizard"></a> Distribuire pacchetti con la Distribuzione guidata Integration Services  
  
1.  Al prompt dei comandi eseguire **isdeploymentwizard.exe** da **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn**. Nei computer a 64 bit è presente anche una versione a 32 bit dello strumento in **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn**.  
  
2.  Nella pagina **Seleziona origine** passare al **modello di distribuzione del pacchetto**. Selezionare quindi la cartella che contiene i pacchetti di origine e configurare i pacchetti.  
  
3.  Completare la procedura guidata. Eseguire i passaggi successivi descritti in [Modello di distribuzione del pacchetto](#PackageModel).  
  
###  <a name="deploy-packages-by-using-sql-server-management-studio"></a><a name="SSMS"></a> Distribuire pacchetti con SQL Server Management Studio  
  
1.  In SQL Server Management Studio espandere il nodo **Cataloghi di Integration Services** > **SSISDB** in Esplora oggetti.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Progetti** e quindi scegliere **Distribuzione progetto**.  
  
3.  Se viene visualizzata la pagina **Introduzione** , fare clic su **Avanti** per continuare.  
  
4.  Nella pagina **Seleziona origine** passare al **modello di distribuzione del pacchetto**. Selezionare quindi la cartella che contiene i pacchetti di origine e configurare i pacchetti.  
  
5.  Completare la procedura guidata. Eseguire i passaggi successivi descritti in [Modello di distribuzione del pacchetto](#PackageModel).  
  
###  <a name="deploy-packages-by-using-sql-server-data-tools-visual-studio"></a><a name="SSDT"></a> Distribuire pacchetti con SQL Server Data Tools (Visual Studio)  
  
1.  In Visual Studio, con un progetto di Integration Services aperto, selezionare i pacchetti da distribuire.  
  
2.  Fare clic con il pulsante destro del mouse e scegliere **Distribuzione pacchetto**. Viene aperta la Distribuzione guidata con i pacchetti selezionati configurati come pacchetti di origine.  
  
3.  Completare la procedura guidata. Eseguire i passaggi successivi descritti in [Modello di distribuzione del pacchetto](#PackageModel).  
  
###  <a name="deploy-packages-by-using-the-deploy_packages-stored-procedure"></a><a name="StoredProcedure"></a> Distribuire i pacchetti usando la stored procedure deploy_packages  
 Si può usare la stored procedure **[catalog].[deploy_packages]** per distribuire uno o più pacchetti SSIS nel catalogo SSIS. L'esempio di codice seguente mostra come usare questa stored procedure per distribuire pacchetti in un server SSIS. Per altre informazioni, vedere [catalog.deploy_packages](../../integration-services/system-stored-procedures/catalog-deploy-packages.md).  
  
```cs
  
private static void Main(string[] args)  
{  
    // Connection string to SSISDB  
    var connectionString = "Data Source=.;Initial Catalog=SSISDB;Integrated Security=True;MultipleActiveResultSets=false";  
  
    using (var sqlConnection = new SqlConnection(connectionString))  
    {  
        sqlConnection.Open();  
  
        var sqlCommand = new SqlCommand  
        {  
            Connection = sqlConnection,  
            CommandType = CommandType.StoredProcedure,  
            CommandText = "[catalog].[deploy_packages]"  
        };  
  
        var packageData = Encoding.UTF8.GetBytes(File.ReadAllText(@"C:\Test\Package.dtsx"));  
  
        // DataTable: name is the package name without extension and package_data is byte array of package.  
        var packageTable = new DataTable();  
        packageTable.Columns.Add("name", typeof(string));  
        packageTable.Columns.Add("package_data", typeof(byte[]));  
        packageTable.Rows.Add("Package", packageData);  
  
        // Set the destination project and folder which is named Folder and Project.  
        sqlCommand.Parameters.Add(new SqlParameter("@folder_name", SqlDbType.NVarChar, ParameterDirection.Input, "Folder", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@project_name", SqlDbType.NVarChar, ParameterDirection.Input, "Project", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@packages_table", SqlDbType.Structured, ParameterDirection.Input, packageTable, -1));  
  
        var result = sqlCommand.Parameters.Add("RetVal", SqlDbType.Int);  
        result.Direction = ParameterDirection.ReturnValue;  
  
        sqlCommand.ExecuteNonQuery();  
    }  
}  
  
```  
  
###  <a name="deploy-packages-using-the-management-object-model-api"></a><a name="MOMApi"></a> Distribuire pacchetti con l'API del modello a oggetti di gestione  
 L'esempio di codice seguente mostra come usare l'API del modello a oggetti di gestione per distribuire pacchetti in un server.  
  
```cs 
  
static void Main()  
 {  
     // Before deploying packages, make sure the destination project exists in SSISDB.  
     var connectionString = "Data Source=.;Integrated Security=True;MultipleActiveResultSets=false";  
     var catalogName = "SSISDB";  
     var folderName = "Folder";  
     var projectName = "Project";  
  
     // Get the folder instance.  
     var sqlConnection = new SqlConnection(connectionString);  
     var store = new Microsoft.SqlServer.Management.IntegrationServices.IntegrationServices(sqlConnection);  
     var folder = store.Catalogs[catalogName].Folders[folderName];  
  
     // Key is package name without extension and value is package binaries.  
     var packageDict = new Dictionary<string, string>();  
  
     var packageData = File.ReadAllText(@"C:\Folder\Package.dtsx");  
     packageDict.Add("Package", packageData);  
  
     // Deploy package to the destination project.  
     folder.DeployPackages(projectName, packageDict);  
 }  
  
```

## <a name="convert-to-package-deployment-model-dialog-box"></a>Finestra di dialogo Converti in modello di distribuzione di pacchetti
  Con il comando **Converti nel modello di distribuzione del pacchetto** è possibile convertire un pacchetto nel modello di distribuzione del pacchetto dopo aver verificato la compatibilità del progetto e di ogni relativo pacchetto con questo modello. Se in un pacchetto vengono utilizzate funzionalità univoche per il modello di distribuzione del progetto, ad esempio parametri, il pacchetto non può essere convertito.  

 Per la conversione di un pacchetto nel modello di distribuzione del pacchetto sono necessari due passaggi.  
  
1.  Quando si seleziona il comando **Converti nel modello di distribuzione del pacchetto** dal menu **Progetto** viene verificata la compatibilità del progetto e di ogni relativo pacchetto con questo modello. I risultati vengono visualizzati nella tabella **Risultati** .  
  
     Se la verifica di compatibilità del progetto o di un pacchetto non viene completata correttamente, per ottenere ulteriori informazioni fare clic su **Non riuscito** nella colonna **Risultato** . Fare clic su **Salva report** per salvare una copia di queste informazioni in un file di testo.  
  
2.  Se il progetto e tutti i pacchetti superano la verifica di compatibilità, fare clic su **OK** per convertire il pacchetto.  
  
> [!NOTE]
> Per convertire un progetto nel modello di distribuzione del progetto usare la **Conversione guidata progetto di Integration Services**. Per altre informazioni, vedere [Integration Services Project Conversion Wizard](deploy-integration-services-ssis-projects-and-packages.md).  

## <a name="integration-services-deployment-wizard"></a>Distribuzione guidata Integration Services
  La **Distribuzione guidata Integration Services** supporta due modelli di distribuzione:
   - Modello di distribuzione del progetto
   - Modello di distribuzione del pacchetto 
   
 Il **modello di distribuzione del progetto** consente di distribuire un progetto di SQL Server Integration Services (SSIS) come una singola unità nel catalogo SSIS.
 
 Il **modello di distribuzione del pacchetto** consente di distribuire i pacchetti che sono stati aggiornati nel catalogo SSIS senza dover distribuire l'intero progetto. 
 
 > [!NOTE]
 > il modello di distribuzione del progetto è il modello predefinito della procedura guidata.  
  
### <a name="launch-the-wizard"></a>Avviare la procedura guidata
Avviare la procedura guidata in uno dei due modi seguenti:

 - Digitare **"Distribuzione guidata di SQL Server"** in Windows Search 

 o

 - Cercare il file eseguibile **ISDeploymentWizard.exe** nella cartella di installazione di SQL Server, ad esempio: "C:\Programmi (x86)\Microsoft SQL Server\130\DTS\Binn". 
 
 > **NOTA:** se viene visualizzata la pagina **Introduzione** , fare clic su **Avanti** per passare alla pagina **Seleziona origine** . 
 
 Le impostazioni in questa pagina sono diverse per ogni modello di distribuzione. Seguire la procedura nella sezione [Project Deployment Model](#ProjectModel) o nella sezione [Package Deployment Model](#PackageModel) in base al modello selezionato in questa pagina.  
  
###  <a name="project-deployment-model"></a><a name="ProjectModel"></a> Project Deployment Model  
  
#### <a name="select-source"></a>Selezionare l'origine

 Per distribuire un file di distribuzione del progetto creato, selezionare **File distribuzione progetto** e immettere il percorso al file con estensione ispac. Per distribuire un progetto che si trova nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , selezionare **Catalogo di Integration Services**, quindi immettere il nome del server e il percorso del progetto nel catalogo. Fare clic su **Avanti** per visualizzare la pagina **Seleziona destinazione** .  
  
#### <a name="select-destination"></a>Seleziona destinazione

 Per selezionare la cartella di destinazione per il progetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , immettere l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o fare clic su **Sfoglia** per effettuare una selezione da un elenco di server. Immettere il percorso del progetto in SSISDB o fare clic su **Sfoglia** per selezionarlo. Fare clic su **Avanti** per visualizzare la pagina **Verifica** .  
  
#### <a name="review-and-deploy"></a>Verifica (e distribuisci)

 La pagina consente di controllare le impostazioni selezionate. È possibile modificare le selezioni facendo clic su **Indietro** o selezionando un qualsiasi passaggio nel riquadro sinistro. Fare clic su **Distribuisci** per avviare il processo di distribuzione.  
  
#### <a name="results"></a>Risultati

 Al termine del processo di distribuzione, verrà visualizzata la pagina **Risultati** . Questa pagina consente di visualizzare l'esito positivo o negativo di ogni azione. Se l'azione non viene completata correttamente, fare clic su **Non riuscito** nella colonna **Risultato** per visualizzare una spiegazione dell'errore. Fare clic su **Salva report...** per salvare i risultati in un file XML oppure su **Chiudi** per uscire dalla procedura guidata.
  
###  <a name="package-deployment-model"></a><a name="PackageModel"></a> Package Deployment Model  
  
#### <a name="select-source"></a>Selezionare l'origine

 La pagina **Seleziona origine** in **Distribuzione guidata Integration Services** mostra le impostazioni specifiche per il modello di distribuzione del pacchetto al momento della selezione dell'opzione **Distribuzione di pacchetti** per il **modello di distribuzione**.  
  
 Per selezionare i pacchetti di origine, fare clic sul pulsante **Sfoglia** per selezionare la **cartella** contenente i pacchetti o digitarne il percorso nella casella di testo **Percorso cartella pacchetti** e fare clic sul pulsante **Aggiorna** nella parte inferiore della pagina. A questo punto, tutti i pacchetti dovrebbero essere visualizzati nella cartella specificata nella casella di riepilogo. Per impostazione predefinita, tutti i pacchetti sono selezionati. Fare clic sulla **casella di controllo** nella prima colonna per scegliere i pacchetti da distribuire nel server.  
  
 Fare riferimento alle colonne **Stato** e **Messaggio** per verificare lo stato del pacchetto. Se lo stato è impostato su **Pronto** o **Avviso**, la distribuzione guidata non blocca il processo di distribuzione. Se lo stato è impostato su **Errore**, la procedura guidata interrompe la distribuzione dei pacchetti selezionati. Per visualizzare il dettaglio dei messaggi di avviso o errore, fare clic sul collegamento nella colonna **Messaggio**.  
  
 Se i dati sensibili o i dati del pacchetto sono crittografati con una password, digitarla nella colonna **Password** e fare clic sul pulsante **Aggiorna** per verificare che venga accettata. Se la password è corretta, lo stato verrà modificato in **Pronto** e il messaggio di avviso non verrà più visualizzato. Se sono presenti più pacchetti con la stessa password, selezionare quelli con la stessa password di crittografia, digitarla nella casella di testo **Password** e selezionare il pulsante **Applica**. La password viene applicata ai pacchetti selezionati.  
  
 Se lo stato di tutti i pacchetti selezionati non è impostato su **Errore**, verrà abilitato il pulsante **Avanti** per proseguire con il processo di distribuzione dei pacchetti.  
  
#### <a name="select-destination"></a>Seleziona destinazione

 Dopo aver selezionato le origini dei pacchetti, fare clic sul pulsante **Avanti** per passare alla pagina **Seleziona destinazione**. I pacchetti devono essere distribuiti in un progetto nel catalogo SSIS (SSISDB). Prima di distribuirli, verificare che il progetto di destinazione esista già nel catalogo SSIS. In caso contrario, creare un progetto vuoto. Nella pagina **Seleziona destinazione** digitare il nome del server nella casella di testo **Nome server** oppure fare clic sul pulsante **Sfoglia** per selezionare un'istanza del server. Fare quindi clic sul pulsante **Sfoglia** accanto alla casella di testo **Percorso** per specificare il progetto di destinazione. Se il progetto non esiste, fare clic sul pulsante **Nuovo progetto** per creare un progetto vuoto come progetto di destinazione. Il progetto deve essere creato in una cartella.  
  
#### <a name="review-and-deploy"></a>Verifica e distribuisci

 Fare clic su **Avanti** nella pagina **Seleziona destinazione** per passare alla pagina **Verifica** nella **Distribuzione guidata Integration Services**. Nella pagina della verifica rivedere il report di riepilogo sull'azione di distribuzione. Dopo la verifica, fare clic sul pulsante **Distribuisci** per eseguire l'azione di distribuzione.  
  
#### <a name="results"></a>Risultati

 Al termine del processo di distribuzione, verrà visualizzata la pagina **Risultati** . Nella pagina **Risultati** esaminare i risultati di ogni passaggio nel processo di distribuzione. Fare clic su **Salva report** per salvare il report di distribuzione o su **Chiudi** per chiudere la procedura guidata.  

## <a name="create-and-map-a-server-environment"></a>Creare ed eseguire il mapping di un ambiente server

  Creare un ambiente server per specificare i valori di runtime per i pacchetti contenuti in un progetto distribuito nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. È quindi possibile eseguire il mapping delle variabili di ambiente ai parametri, per un pacchetto specifico, per i pacchetti del punto di ingresso o per tutti i pacchetti di un progetto specificato. Un pacchetto del punto di ingresso è in genere un pacchetto padre che esegue un pacchetto figlio.  
  
> [!IMPORTANT]  
>  Per un'esecuzione specifica, un pacchetto può essere eseguito solo con i valori contenuti in un ambiente server singolo.  
  
 È possibile eseguire query sulle viste per un elenco di ambienti server, riferimenti all'ambiente e variabili di ambiente. È anche possibile chiamare stored procedure per aggiungere, eliminare e modificare gli ambienti, i riferimenti all'ambiente e le variabili di ambiente. Per altre informazioni, vedere la sezione **Ambienti server, variabili del server e riferimenti all'ambiente del server** in [Catalogo SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
### <a name="to-create-and-use-a-server-environment"></a>Per creare e utilizzare un ambiente server  
  
1.  In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] espandere il nodo Cataloghi di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]**SSISDB** in Esplora oggetti e individuare la cartella **Ambienti** del progetto per il quale si vuole creare un ambiente.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Ambienti** e quindi scegliere **Creazione ambiente**.  
  
3.  Digitare un nome per l'ambiente e facoltativamente aggiungere una descrizione. Fare clic su **OK**.  
  
4.  Fare clic con il pulsante destro del mouse sul nuovo ambiente e scegliere **Proprietà**.  
  
5.  Nella pagina **Variabili** effettuare le operazioni seguenti per aggiungere una variabile.  
  
    1.  Selezionare il **Tipo** per la variabile. Il nome della variabile non deve corrispondere al nome del parametro del progetto di cui verrà eseguito il mapping alla variabile.  
  
    2.  Immettere una **Descrizione** facoltativa per la variabile.  
  
    3.  Immettere il **Valore** per la variabile di ambiente.  
  
         Per informazioni sulle regole per i nomi delle variabili di ambiente, vedere la sezione **Variabile di ambiente** in [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
    4.  Indicare se nella variabile è contenuto il valore sensibile, selezionando o deselezionando la casella di controllo **Sensibile** .  
  
         Se si seleziona **Sensibile**, il valore della variabile non viene visualizzato nel campo **Valore** .  
  
         I valori sensibili vengono crittografati nel catalogo SSISDB. Per ulteriori informazioni sulla crittografia, vedere [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
6.  Nella pagina **Autorizzazioni** concedere o negare le autorizzazioni per gli utenti e i ruoli selezionati effettuando le operazioni riportate di seguito.  
  
    1.  Fare clic su **Sfoglia**, quindi selezionare uno o più utenti e ruoli nella finestra di dialogo **Sfoglia tutte le entità** .  
  
    2.  Nell'area **Account di accesso o ruoli** selezionare l'utente o il ruolo per cui si desidera concedere o negare le autorizzazioni.  
  
    3.  Nell'area **Esplicita** selezionare **Concedi** o **Nega** accanto a ogni autorizzazione.  
  
7.  Per creare lo script dell'ambiente, fare clic su **Script**. Per impostazione predefinita, lo script viene visualizzato in una nuova finestra dell'editor di query.  
  
    > [!TIP]  
    >  È necessario fare clic su **Script** dopo aver apportato una o più modifiche alle proprietà dell'ambiente, ad esempio se si aggiunge una variabile, e prima di fare clic su **OK** nella finestra di dialogo **Proprietà ambiente**. In caso contrario, non viene creato alcuno script.  
  
8.  Fare clic su **OK** per salvare le modifiche apportate alle proprietà di ambiente.  
  
9. Nel nodo **SSISDB** in Esplora oggetti espandere la cartella **Progetti** , fare clic con il pulsante destro del mouse sul progetto e quindi fare clic su **Configura**.  
  
10. Nella pagina **Riferimenti** fare clic su **Aggiungi** per aggiungere un ambiente, quindi scegliere **OK** per salvare il riferimento all'ambiente.  
  
11. Fare nuovamente clic con il pulsante destro del mouse sul progetto e quindi scegliere **Configura**.  
  
12. Per eseguire il mapping della variabile di ambiente a un parametro aggiunto al pacchetto in fase di progettazione o a un parametro che è stato generato durante la conversione del progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al modello di distribuzione del progetto, eseguire le operazioni seguenti:
  
    1.  Nella scheda **Parametri** della pagina **Parametri** fare clic sul pulsante Sfoglia accanto al campo **Valore** .  
  
    2.  Fare clic su **Usa variabile di ambiente**, quindi selezionare la variabile di ambiente creata.  
  
13. Per eseguire il mapping della variabile di ambiente a una proprietà di gestione connessione, effettuare le operazioni seguenti. I parametri vengono automaticamente generati nel server SSIS per le proprietà di gestione connessione.  
  
    1.  Nella scheda **Gestioni connessioni** della pagina **Parametri** fare clic sul pulsante **Sfoglia** accanto al campo **Valore**.  
  
    2.  Fare clic su **Usa variabile di ambiente**, quindi selezionare la variabile di ambiente creata.  
  
14. Fare due volte clic su **OK** per salvare le modifiche.  

## <a name="deploy-and-execute-ssis-packages-using-stored-procedures"></a>Distribuire ed eseguire pacchetti SSIS utilizzando le stored procedure

  Quando un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene configurato in modo da utilizzare il relativo modello di distribuzione, è possibile utilizzare le stored procedure nel catalogo di [!INCLUDE[ssIS](../../includes/ssis-md.md)] per distribuire il progetto ed eseguire i pacchetti. Per informazioni sui modelli di distribuzione di progetti, vedere [Distribuzione di progetti e pacchetti](deploy-integration-services-ssis-projects-and-packages.md#compare-project-deployment-model-and-legacy-package-deployment-model).  
  
 Per distribuire il progetto ed eseguire i pacchetti, è inoltre possibile utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Per altre informazioni, vedere gli argomenti nella sezione **Vedere anche** .  
  
> [!TIP]
>  Ad eccezione di catalog.deploy_project, è possibile generare facilmente le istruzioni Transact-SQL per le stored procedure elencate nella procedura descritta di seguito effettuando le operazioni seguenti:
> 
>  1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]espandere il nodo **Cataloghi di Integration Services** in Esplora oggetti e selezionare il pacchetto che si desidera eseguire.  
> 2.  Fare clic con il pulsante destro del mouse sul pacchetto, quindi scegliere **Esegui**.  
> 3.  In base alle esigenze, impostare i valori dei parametri, le proprietà di gestione connessione e le opzioni nella scheda **Avanzate** , ad esempio il livello di registrazione.  
> 
>      Per altre informazioni sui livelli di registrazione, vedere [Abilitare la registrazione per l'esecuzione di pacchetti nel server SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
> 4.  Prima di fare clic su **OK** per eseguire il pacchetto, scegliere **Script**. Transact-SQL viene visualizzato in una finestra dell'editor di query in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="to-deploy-and-execute-a-package-using-stored-procedures"></a>Per distribuire ed eseguire un pacchetto utilizzando le stored procedure  
  
1.  Chiamare [catalog.deploy_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md) per distribuire il progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto sul server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
     Per recuperare il contenuto binario del file di distribuzione del progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], per il parametro _\@project_stream_ usare un'istruzione SELECT con la funzione OPENROWSET e il provider BULK per set di righe. Questo provider consente di leggere i dati da un file. Tramite l'argomento SINGLE_BLOB per il provider BULK per set di righe, il contenuto del file di dati viene restituito come un set di righe a riga e colonna singole di tipo varbinary(max). Per altre informazioni, vedere [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
     Nell'esempio seguente, il progetto SSISPackages_ProjectDeployment viene distribuito nella cartella di pacchetti SSIS nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . I dati binari vengono letti dal file di progetto (SSISPackage_ProjectDeployment.ispac) e archiviati nel parametro _\@ProjectBinary di tipo varbinary(max). Il valore del parametro _\@ProjectBinary_ viene assegnato al parametro _\@project_stream_.  
  
    ```sql
    DECLARE @ProjectBinary as varbinary(max)  
    DECLARE @operation_id as bigint  
    Set @ProjectBinary = (SELECT * FROM OPENROWSET(BULK 'C:\MyProjects\ SSISPackage_ProjectDeployment.ispac', SINGLE_BLOB) as BinaryData)  
  
    Exec catalog.deploy_project @folder_name = 'SSIS Packages', @project_name = 'DeployViaStoredProc_SSIS', @Project_Stream = @ProjectBinary, @operation_id = @operation_id out  
  
    ```  
  
2.  Chiamare [catalog.create_execution &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) per creare un'istanza di esecuzione del pacchetto e chiamare facoltativamente [catalog.set_execution_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) per impostare i valori del parametro di runtime.  
  
     Nell'esempio seguente, tramite catalog.create_execution viene creata un'istanza di esecuzione per package.dtsx che è contenuto nel progetto SSISPackage_ProjectDeployment. Il progetto si trova nella cartella di pacchetti SSIS. Il valore di execution_id restituito dalla stored procedure viene utilizzato per la chiamata a catalog.set_execution_parameter_value. Tramite questa seconda stored procedure il parametro LOGGING_LEVEL viene impostato su 3 (registrazione dettagliata) e un parametro del pacchetto denominato Parameter1 viene impostato su un valore 1.  
  
     Per i parametri come LOGGING_LEVEL, il valore di object_type è 50. Per i parametri del pacchetto, il valore di object_type è 30.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    GO  
  
    ```  
  
3.  Chiamare [catalog.start_execution &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) per eseguire il pacchetto.  
  
     Nell'esempio seguente, una chiamata a catalog.start_execution viene aggiunta a Transact-SQL per avviare l'esecuzione del pacchetto. Viene utilizzato il valore di execution_id restituito dalla stored procedure catalog.create_execution.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    EXEC [SSISDB].[catalog].[start_execution] @execution_id  
    GO  
  
    ```  
  
### <a name="to-deploy-a-project-from-server-to-server-using-stored-procedures"></a>Per distribuire un progetto da un server in un altro mediante stored procedure

 È possibile distribuire un progetto da server a server tramite le stored procedure [catalog.get_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md) e [catalog.deploy_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md).  
  
 Prima di eseguire le stored procedure, è necessario effettuare le operazioni riportate di seguito.  
  
-   Creare un oggetto server collegato. Per altre informazioni, vedere [Creare server collegati &#40;Motore di database di SQL Server&#41;](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md).  
  
     Nella pagina **Opzioni server** della finestra di dialogo **Proprietà server collegato** impostare **RPC** e **RPC Out** su **True**. Impostare anche **Abilita innalzamento di livello delle transazioni distribuite per RPC** su **False**.  
  
-   Abilitare i parametri dinamici per il provider selezionato per il server collegato espandendo il nodo **Provider** in **Server collegati** in Esplora oggetti, facendo clic con il pulsante destro del mouse sul provider e selezionando **Proprietà**. Selezionare **Abilita** accanto a **Parametro dinamico**.  
  
-   Verificare che Distributed Transaction Coordinator (DTC) venga avviato in entrambi i server.  
  
 Chiamare catalog.get_project per restituire i dati binari per il progetto, quindi chiamare catalog.deploy_project. Il valore restituito da catalog.get_project viene inserito in una variabile di tabella di tipo varbinary(max). Tramite il server collegato non possono essere restituiti risultati di tipo varbinary(max).  
  
 Nell'esempio seguente, tramite catalog.get_project viene restituito un file binario per il progetto SSISPackages nel server collegato. Tramite catalog.deploy_project il progetto viene distribuito nella cartella denominata DestFolder nel server locale.  
  
```sql
declare @resultsTableVar table (  
project_binary varbinary(max)  
)  
  
INSERT @resultsTableVar (project_binary)  
EXECUTE [MyLinkedServer].[SSISDB].[catalog].[get_project] 'Packages', 'SSISPackages'  
  
declare @project_binary varbinary(max)  
select @project_binary = project_binary from @resultsTableVar  
  
exec [SSISDB].[CATALOG].[deploy_project] 'DestFolder', 'SSISPackages', @project_binary  
  
```  

## <a name="integration-services-project-conversion-wizard"></a>Conversione guidata progetto di Integration Services
  Con la **Conversione guidata progetto di Integration Services** è possibile convertire un progetto nel modello di distribuzione del progetto.  
  
> [!NOTE]  
>  Se nel progetto sono incluse una o più origini dati, queste ultime vengono rimosse al completamento della conversione del progetto. Per creare una connessione a un'origine dati che può essere condivisa dai pacchetti nel progetto, aggiungere una gestione connessione al livello del progetto. Per altre informazioni, vedere [aggiungere, eliminare o condividere una gestione connessione in un pacchetto](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130)).  
  
 **Per saperne di più**  
  
-   [Aprire la Conversione guidata progetti di Integration Services](#open_dialog)  
  
-   [Impostare le opzioni nella pagina Individua pacchetti](#locate)  
  
-   [Impostare le opzioni nella pagina Seleziona pacchetti](#selectPackages)  
  
-   [Impostare le opzioni nella pagina Seleziona destinazione](#destination)  
  
-   [Impostare le opzioni nella pagina Specificare le proprietà del progetto](#projectProperties)  
  
-   [Impostare le opzioni nella pagina Aggiorna attività Esegui pacchetto](#executePackage)  
  
-   [Impostare le opzioni nella pagina Seleziona configurazioni](#configurations)  
  
-   [Impostare le opzioni nella pagina Crea parametri](#createParameters)  
  
-   [Impostare le opzioni nella pagina Configura parametri](#configureParameters)  
  
-   [Impostare le opzioni nella pagina Verifica](#review)  
  
-   [Impostare le opzioni nella pagina Esegui conversione](#conversion)  
  
###  <a name="open-the-integration-services-project-conversion-wizard"></a><a name="open_dialog"></a> Aprire la Conversione guidata progetti di Integration Services  
 Eseguire una delle operazioni seguenti per aprire la **Conversione guidata progetto di Integration Services** .  
  
-   Aprire il progetto in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], quindi in Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Converti nel modello di distribuzione del progetto**.  
  
-   Da Esplora oggetti di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fare clic con il pulsante destro del mouse sul nodo **Progetti** nel **Catalogo di Integration Services** e selezionare **Importa pacchetti**.  
  
 Le diverse attività di conversione eseguite dalla **Conversione guidata progetto di Integration Services** variano a seconda che la procedura guidata venga eseguita da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].   
  
###  <a name="set-options-on-the-locate-packages-page"></a><a name="locate"></a> Impostare le opzioni nella pagina Individua pacchetti  
  
> [!NOTE]  
>  La pagina **Individua pacchetti** è disponibile solo quando si esegue la procedura guidata da [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 L'opzione seguente viene visualizzata nella pagina quando si seleziona **File system** nell'elenco a discesa **Origine** . Selezionare questa opzione se il pacchetto si trova nel file system.  
  
 **Cartella**  
 Digitare il percorso del pacchetto o passare al pacchetto facendo clic su **Sfoglia**.  
  
 Le opzioni seguenti vengono visualizzate nella pagina quando si seleziona **Archivio pacchetti SSIS** nell'elenco a discesa **Origine**. Per altre informazioni sull'archiviazione dei pacchetti, vedere [Gestione dei pacchetti &#40;servizio SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 **Server**  
 Digitare il nome del server o selezionare il server.  
  
 **Cartella**  
 Digitare il percorso del pacchetto o passare al pacchetto facendo clic su **Sfoglia**.  
  
 Le opzioni seguenti vengono visualizzate nella pagina quando si seleziona **Microsoft SQL Server** nell'elenco a discesa **Origine** . Selezionare questa opzione se il pacchetto si trova in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Server**  
 Digitare il nome del server o selezionare il server.  
  
 **Usa autenticazione di Windows**  
 Tale modalità consente di connettersi tramite un account utente di Windows. Se si utilizza l'autenticazione di Windows non è necessario specificare un nome utente o una password.  
  
 **Usa autenticazione di SQL Server**  
 Quando un utente si connette da una connessione non trusted con un nome di account di accesso e una password specificati, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autentica la connessione controllando se è stato impostato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e se la password specificata corrisponde a quella registrata in precedenza. Se non è stato impostato alcun account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'autenticazione non viene completata e viene segnalato un errore all'utente.  
  
 **Nome utente**  
 Specificare un nome utente quando si utilizza l'autenticazione di SQL Server.  
  
 **Password**  
 Specificare una password quando si utilizza l'autenticazione di SQL Server.  
  
 **Cartella**  
 Digitare il percorso del pacchetto o passare al pacchetto facendo clic su **Sfoglia**.  
  
###  <a name="set-options-on-the-select-packages-page"></a><a name="selectPackages"></a> Impostare le opzioni nella pagina Seleziona pacchetti  
 **Nome pacchetto**  
 Viene elencato il file del pacchetto.  
  
 **Status**  
 Viene indicato se un pacchetto è pronto per la conversione nel modello di distribuzione del progetto.  
  
 **Message**  
 Viene visualizzato un messaggio associato al pacchetto.  
  
 **Password**  
 Viene visualizzata una password associata al pacchetto. Il testo della password è nascosto.  
  
 **Applica a selezione**  
 Fare clic per applicare la password nella casella di testo **Password** ai pacchetti selezionati.  
  
 **Aggiorna**  
 Viene aggiornato l'elenco dei pacchetti.  
  
###  <a name="set-options-on-the-select-destination-page"></a><a name="destination"></a> Impostare le opzioni nella pagina Seleziona destinazione  
 In questa pagina specificare il nome e il percorso di un nuovo file di distribuzione progetto (con estensione ispac) o selezionare un file esistente.  
  
> [!NOTE]  
>  La pagina **Seleziona destinazione** è disponibile solo quando si esegue la procedura guidata da [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **Percorso di output**  
 Digitare il percorso del file di distribuzione o passare al file facendo clic su **Sfoglia**.  
  
 **Nome progetto**  
 Digitare il nome del progetto.  
  
 **Livello di protezione**  
 Selezionare il livello di protezione. Per altre informazioni, vedere [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Descrizione progetto**  
 Digitare una descrizione facoltativa per il progetto.  
  
###  <a name="set-options-on-the-specify-project-properties-page"></a><a name="projectProperties"></a> Impostare le opzioni nella pagina Specificare le proprietà del progetto  
  
> [!NOTE]  
>  La pagina **Specifica proprietà del progetto** è disponibile solo quando si esegue la procedura guidata da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
 **Nome progetto**  
 Viene elencato il nome del progetto.  
  
 **Livello di protezione**  
 Selezionare un livello di protezione per i pacchetti contenuti nel progetto. Per altre informazioni sui livelli di protezione, vedere [Controllo dell'accesso per dati sensibili nei pacchetti](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Descrizione progetto**  
 Digitare una descrizione facoltativa del progetto.  
  
###  <a name="set-options-on-the-update-execute-package-task-page"></a><a name="executePackage"></a> Impostare le opzioni nella pagina Aggiorna attività Esegui pacchetto  
 Aggiornare le attività Esegui pacchetto contenute nei pacchetti al fine di utilizzare un riferimento basato sul progetto. Per altre informazioni, vedere [Editor attività Esegui pacchetto](../control-flow/execute-package-task.md).  
  
 **Pacchetto padre**  
 Viene elencato il nome del pacchetto tramite cui viene eseguito il pacchetto figlio utilizzando l'attività Esegui pacchetto.  
  
 **Nome attività**  
 Viene elencato il nome dell'attività Esegui pacchetto.  
  
 **Riferimento originale**  
 Viene elencato il percorso corrente del pacchetto figlio.  
  
 **Assegna riferimento**  
 Selezionare un pacchetto figlio archiviato nel progetto.  
  
###  <a name="set-options-on-the-select-configurations-page"></a><a name="configurations"></a> Impostare le opzioni nella pagina Seleziona configurazioni  
 Selezionare le configurazioni del pacchetto che si desidera sostituire con i parametri.  
  
 **Pacchetto**  
 Viene elencato il file del pacchetto.  
  
 **Tipo**  
 Viene elencato il tipo di configurazione, ad esempio un file di configurazione XML.  
  
 **Stringa di configurazione**  
 Viene elencato il percorso del file di configurazione.  
  
 **Status**  
 Viene visualizzato un messaggio di stato per la configurazione. Fare clic sul messaggio per visualizzare il relativo testo completo.  
  
 **Aggiungi configurazioni**  
 Aggiungere le configurazioni del pacchetto contenute in altri progetti all'elenco delle configurazioni disponibili che si desidera sostituire con i parametri. È possibile selezionare le configurazioni archiviate in un file system o in SQL Server.  
  
 **Aggiorna**  
 Fare clic su questa opzione per aggiornare l'elenco delle configurazioni.  
  
 **Rimuovi configurazioni da tutti i pacchetti dopo la conversione**  
 È consigliabile rimuovere tutte le configurazioni dal progetto selezionando questa opzione.  
  
 Se non si seleziona questa opzione, vengono rimosse solo le configurazioni selezionate per la sostituzione con i parametri.  
  
###  <a name="set-options-on-the-create-parameters-page"></a><a name="createParameters"></a> Impostare le opzioni nella pagina Crea parametri  
 Selezionare il nome e l'ambito del parametro per ogni proprietà di configurazione.  
  
 **Pacchetto**  
 Viene elencato il file del pacchetto.  
  
 **Nome parametro**  
 Viene elencato il nome del parametro.  
  
 **Ambito**  
 Selezionare l'ambito del parametro, vale a dire pacchetto o progetto.  
  
###  <a name="set-options-on-the-configure-parameters-page"></a><a name="configureParameters"></a> Impostare le opzioni nella pagina Configura parametri  
 **Nome**  
 Viene elencato il nome del parametro.  
  
 **Ambito**  
 Viene elencato l'ambito del parametro.  
  
 **Valore**  
 Viene elencato il valore del parametro.  
  
 Fare clic sul pulsante con i puntini di sospensione accanto al campo del valore per configurare le proprietà del parametro.  
  
 Nella finestra di dialogo **Imposta dettagli parametri** è possibile modificare il valore del parametro. È anche possibile specificare se il valore del parametro deve essere fornito quando si esegue il pacchetto.  
  
 È possibile modificare il valore nella pagina **Parametri** della finestra di dialogo **Configura** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]facendo clic sul pulsante Sfoglia accanto al parametro. Viene visualizzata la finestra di dialogo **Imposta valore parametro** .  
  
 Nella finestra di dialogo **Imposta dettagli parametri** sono inoltre elencati il tipo di dati del valore del parametro e l'origine del parametro.  
  
###  <a name="set-the-options-on-the-review-page"></a><a name="review"></a> Impostare le opzioni nella pagina Verifica  
 Usare la pagina **Verifica** per confermare le opzioni selezionate per la conversione del progetto.  
  
 **Indietro**  
 Fare clic su questo pulsante per modificare un'opzione.  
  
 **Converti**  
 Fare clic su questa opzione per convertire il progetto nel modello di distribuzione del progetto.  
  
###  <a name="set-the-options-on-the-perform-conversion"></a><a name="conversion"></a> Impostare le opzioni nella pagina Esegui conversione  
 Nella pagina Esegui conversione viene mostrato lo stato della conversione del progetto.  
  
 **Azione**  
 Viene elencato un passaggio specifico della conversione.  
  
 **Risultato**  
 Viene elencato lo stato di ogni passaggio della conversione. Fare clic sul messaggio di stato per ulteriori informazioni.  
  
 La conversione del progetto non viene salvata fino al salvataggio del progetto in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
 **Salvare il report**  
 Fare clic su questa opzione per salvare un riepilogo della conversione del progetto in un file con estensione xml.