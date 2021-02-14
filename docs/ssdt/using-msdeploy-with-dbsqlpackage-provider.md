---
title: Utilizzo di MSDeploy con il provider dbSqlPackage
description: Informazioni sul provider MSDeploy obsoleto DbSqlPackage. Visualizzare parametri, esempi e strumenti di pubblicazione alternativi per database di SQL Server e database SQL di Azure.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 213b91ab-03e9-431a-80f0-17eed8335abe
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: e9ceb447b078f7219d2fadec2dee4274a74190e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066826"
---
# <a name="using-msdeploy-with-dbsqlpackage-provider"></a>Utilizzo di MSDeploy con il provider dbSqlPackage

**DbSqlPackage** è un provider **MSDeploy** che consente di interagire con database di SQL Server o database SQL di Azure. **DbSqlPackage** supporta le azioni seguenti:  
  
-   **Extract**: crea un file snapshot del database con estensione dacpac da un database SQL Server o SQL di Azure attivo.  
  
-   **Publish**: aggiorna in modo incrementale uno schema di database affinché corrisponda allo schema di un file di origine con estensione dacpac.  
  
-   **DeployReport**: crea un report XML delle modifiche che verrebbero effettuate da un'azione Publish.  
  
-   **Script**: crea uno script Transact\-SQL equivalente allo script eseguito dall'azione di pubblicazione.  
  
Per altre informazioni su DACFx, vedere la documentazione dell'API gestita DACFx all'indirizzo [https://msdn.microsoft.com/library/microsoft.sqlserver.dac.aspx](/dotnet/api/microsoft.sqlserver.dac) oppure [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) (strumento della riga di comando di DACFx).  
  
> [!IMPORTANT]  
> La funzionalità del provider dbSqlPackage verrà rimossa a partire dalla prossima versione principale di Visual Studio. Per informazioni su come eseguire la pubblicazione del database con Distribuzione Web, vedere [dbDacFx Provider for Incremental Database publishing](https://www.iis.net/learn/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) (Provider dbDacFx per la pubblicazione di database incrementale).  
  
## <a name="command-line-syntax"></a>Sintassi della riga di comando  
Tramite **MSDeploy** con il provider **dbSqlPackage** è possibile usare una riga di comando nel formato seguente:  
  
```  
  
MSDeploy -verb: MSDeploy-verb -source:dbSqlPackage="Input"[,dbSqlPackage-source-parameters] -dest:dpSqlPackage="Input"[,dbSqlPackage-target-parameters]  
```  
  
## <a name="ms-deploy-verbs"></a>Verbi di MS-Deploy  
È possibile specificare i verbi di MS-Deploy mediante l'opzione **-verb** nella riga di comando di MS-Deploy. Il provider **dbSqlPackage** supporta i verbi **MSDeploy** seguenti:  
  
|Verbo|Descrizione|  
|--------|---------------|  
|dump|Fornisce informazioni, tra cui nome, numero di versione e descrizione, su un database di origine contenuto in un file con estensione dacpac. Specificare il database di origine utilizzando il formato seguente nella riga di comando:<br /><br />**msdeploy -verb:dump -source:dbSqlPackage="** _.dacpac-file-path_ **"**|  
|sync|Specifica le azioni di dbSqlPackage utilizzando il formato seguente nella riga di comando:<br /><br />**msdeploy -verb:sync -source:dbSqlPackage**="input" _[,DbSqlPackage-source-parameters] -_ **dest:dbSqlPackage**="input" *[,DbSqlPackage-destination-parameters]*<br /><br />Vedere le sezioni riportate di seguito per i parametri di origine e di destinazione validi per il verbo sync.|  
  
## <a name="dbsqlpackage-source"></a>Origine di dbSqlPackage  
Il provider **dbSqlPackage** accetta un input che è una stringa di connessione valida di SQL Azure o di SQL Server oppure un percorso di un file con estensione dacpac su disco.  La sintassi per specificare l'origine di input per il provider è la seguente:  
  
|Input|Predefinito|Descrizione|  
|---------|-----------|---------------|  
|**-source:dbSqlPackage=** {*input*}|**N/D**|*input* è una stringa di connessione valida di SQL Server o di SQL Azure o di SQL Server oppure un percorso di un file con estensione dacpac su disco.<br /><br />**NOTA:** le uniche proprietà della stringa di connessione supportate quando si usa una stringa di connessione come origine di input sono *InitialCatalog, DataSource, UserID, Password, IntegratedSecurity, Encrypt, TrustServerCertificate* e *ConnectionTimeout*.|  
  
Se l'origine di input è una stringa di connessione a un database di SQL Server o a un database SQL di Azure, **dbSqlPackage** estrarrà uno snapshot del database sotto forma di file con estensione dacpac da un database di SQL Server o un database SQL di Azure.  
  
I parametri **Source** sono:  
  
|Parametro|Predefinito|Descrizione|  
|-------------|-----------|---------------|  
|**Profile**:{ *stringa*}|N/D|Specifica il percorso file a un profilo di pubblicazione dell'applicazione livello dati. Il profilo consente di definire una raccolta di proprietà e variabili da utilizzare durante la generazione del pacchetto di applicazione livello dati risultante. Il profilo di pubblicazione viene passato alla destinazione e utilizzato come opzioni predefinite durante l'esecuzione di un'azione **Publish**, **Script** o **DeployReport**.|  
|**DacApplicationName**={ *stringa* }|Nome database|Definisce il nome dell'applicazione da archiviare nei metadati del pacchetto di applicazione livello dati. La stringa predefinita corrisponde al nome del database.|  
|**DacMajorVersion** ={*intero*}|**1**|Definisce il numero di versione principale da archiviare nei metadati del pacchetto di applicazione livello dati.|  
|**DacMinorVersion**={*intero*}|**0**|Definisce il numero di versione secondario da archiviare nei metadati del pacchetto di applicazione livello dati.|  
|**DacApplicationDescription**={ *stringa* }|N/D|Definisce la descrizione dell'applicazione da archiviare nei metadati del pacchetto di applicazione livello dati.|  
|**ExtractApplicationScopedObjectsOnly={True &#124; False}**|**True**|Se **True**, vengono estratti solo gli oggetti con ambito di applicazione dall'origine. Se **False**, vengono estratti sia gli oggetti con ambito di applicazione che quelli senza ambito di applicazione.|  
|**ExtractReferencedServerScopedElements={True &#124; False}**|**True**|Se **True**, estrarre gli oggetti Login, Server Audit e Credential a cui fanno riferimento gli oggetti del database di origine.|  
|**ExtractIgnorePermissions={True &#124; False}**|**False**|Se **True**, vengono ignorate le autorizzazioni di estrazione per tutti gli oggetti estratti. Se **False**, tale operazione non viene eseguita.|  
|**ExtractStorage={File&#124;Memory}**|**File**|Specifica il tipo di archivio di backup per il modello schema utilizzato durante l'estrazione.|  
|**ExtractIgnoreExtendedProperties={True&#124;False}**|**False**|Specifica se devono essere ignorate le proprietà estese.|  
|**VerifyExtraction = {True&#124;False}**|**False**|Specifica se il pacchetto di applicazione livello dati estratto deve essere verificato.|  
  
## <a name="dbsqlpackage-destination"></a>Destinazione di dbSqlPackage  
Il provider **dbSqlPackage** accetta una stringa di connessione valida di SQL Azure o di SQL Server oppure un percorso di un file con estensione dacpac su disco come input di destinazione.  La sintassi per specificare la destinazione per il provider è la seguente:  
  
|Input|Predefinito|Descrizione|  
|---------|-----------|---------------|  
|-**dest:dbSqlPackage**={*input*}|N/D|*input* è una stringa di connessione valida di SQL Server o di SQL Azure oppure un percorso completo o parziale di un file con estensione dacpac su disco. Se *input* è un percorso di file, non può essere specificato nessun altro parametro.|  
  
I parametri **Destination** seguenti sono disponibili per tutte le operazioni **dbSqlPackage**:  
  
|Proprietà|Predefinito|Descrizione|  
|------------|-----------|---------------|  
|**Action={Publish&#124;DeployReport&#124;Script}**|N/D|Parametro facoltativo che specifica l'azione da eseguire in **Destination**.|  
|**AllowDropBlockingAssemblies ={True &#124; False}**|**False**|Specifica se eliminare gli assembly di blocco durante la pubblicazione di **SqlClr** come parte del piano di distribuzione. Per impostazione predefinita, tutti gli assembly di blocco o di riferimento bloccano l'aggiornamento di un assembly se l'assembly di riferimento deve essere eliminato.|  
|**AllowIncompatiblePlatform={True &#124; False}**|**False**|Specifica se l'azione di pubblicazione deve continuare indipendentemente dalle piattaforme SQL Server potenzialmente incompatibili.|  
|**BackupDatabaseBeforeChanges={True &#124; False}**|**False**|Esegue il backup del database prima di distribuire qualsiasi modifica.|  
|**BlockOnPossibleDataLoss={ True &#124; False}**|**True**|Specifica se terminare l'operazione di pubblicazione nel caso in cui possa provocare una perdita di dati.|  
|**BlockWhenDriftDetected={ True &#124; False}**|**True**|Specifica se bloccare l'aggiornamento di un database il cui schema non corrisponde più alla relativa registrazione o di cui è stata annullata la registrazione.|  
|**CommentOutSetVarDeclarations= {True &#124; False}**|**False**|Specifica se impostare come commento le dichiarazioni della variabile **SETVAR** nello script di pubblicazione generato. È possibile scegliere questo approccio se si prevedere di usare uno strumento come **SQLCMD.EXE** per specificare i valori nella riga di comando durante la pubblicazione.|  
|**CompareUsingTargetCollation={ True &#124; False}**|**False**|Con questa impostazione è possibile stabilire come vengono gestite le regole di confronto del database durante la distribuzione. Per impostazione predefinita, le regole di confronto del database verranno aggiornate se non corrispondono alle regole di confronto specificate dall'origine.  Quando questa opzione è impostata, devono essere utilizzate le regole di confronto del server o del database di destinazione.|  
|**CreateNewDatabase={ True &#124; False}**|**False**|Specifica se deve essere aggiornato il database di destinazione o se deve essere eliminato e ricreato durante la pubblicazione in un database.|  
|**DeployDatabaseInSingleUserMode={ True &#124; False}**|**False**|Se **True**, il database viene impostato sulla modalità utente singolo prima della distribuzione.|  
|**DisableAndReenableDdlTriggers={True &#124; False}**|**True**|Specifica se disabilitare i trigger DDL (Data Definition Language) all'inizio del processo di pubblicazione e riabilitarli alla fine dell'azione di pubblicazione.|  
|**DoNotAlterChangeDataCaptureObjects={ True &#124; False}**|**True**|Se **True**, gli oggetti Change Data Capture non vengono modificati.|  
|**DoNotAlterReplicatedObjects=( True &#124; False}**|**True**|Specifica se gli oggetti replicati vengono identificati durante la verifica.|  
|**DropConstraintsNotInSource= {True &#124; False}**|**True**|Specifica se l'azione di pubblicazione deve eliminare i vincoli che non esistono nello snapshot di database con estensione dacpac dal database di destinazione durante la pubblicazione in un database.|  
|**DropDmlTriggersNotInSource= {True &#124; False}**|**True**|Specifica se l'azione di pubblicazione deve eliminare i trigger DML (Data Manipulation Language) che non esistono nello snapshot di database con estensione dacpac dal database di destinazione durante la pubblicazione in un database.|  
|**DropExtendedPropertiesNotInSource= {True &#124; False}**|**True**|Specifica se l'azione di pubblicazione deve eliminare le proprietà estese che non esistono nello snapshot di database con estensione dacpac dal database di destinazione durante la pubblicazione in un database.|  
|**DropIndexesNotInSource= {True &#124; False}**|**True**|Specifica se l'azione di pubblicazione deve eliminare gli indici che non esistono nello snapshot di database con estensione dacpac dal database di destinazione durante la pubblicazione in un database.|  
|**DropObjectsNotInSource= {True &#124; False}**|**False**|Specifica se gli oggetti che non esistono nel file (con estensione dacpac) di snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|  
|**DropPermissionsNotInSource= {True &#124; False}**|**False**|Specifica se l'azione di pubblicazione deve eliminare le autorizzazioni che non esistono nello snapshot di database con estensione dacpac dal database di destinazione durante la pubblicazione in un database.|  
|**DropRoleMembersNotInSource= {True &#124; False}**|**False**|Specifica se l'azione di pubblicazione deve eliminare i membri dei ruoli che non esistono nello snapshot di database con estensione dacpac dal database di destinazione durante la pubblicazione in un database.|  
|**GenerateSmartDefaults={True &#124; False}**|**False**|Specifica se **SqlPackage.exe** deve fornire automaticamente un valore predefinito quando viene aggiornata una tabella contenente dati con una colonna che non consente valori Null.|  
|**IgnoreAnsiNulls= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nell'impostazione **ANSI NULLS** durante la pubblicazione in un database.|  
|**IgnoreAuthorizer= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nel provider di autorizzazioni durante la pubblicazione in un database.|  
|**IgnoreColumnCollation= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle regole di confronto di una colonna durante la pubblicazione in un database.|  
|**IgnoreComments= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nell'ordine dei commenti durante la pubblicazione in un database.|  
|**IgnoreCryptographicProviderFile= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nel percorso file per un provider del servizio di crittografia durante la pubblicazione in un database.|  
|**IgnoreDdlTriggerOrder= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nell'ordine dei trigger DDL (Data Definition Language) durante la pubblicazione in un database.|  
|**IgnoreDdlTriggerState={True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nello stato abilitato o disabilitato dei trigger DDL durante la pubblicazione in un database.|  
|**IgnoreDefaultSchema={True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nello schema predefinito durante la pubblicazione in un database.|  
|**IgnoreDmlTriggerOrder= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nell'ordine dei trigger DML durante la pubblicazione in un database.|  
|**IgnoreDmlTriggerState= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nello stato abilitato o disabilitato dei trigger DML durante la pubblicazione in un database.|  
|**IgnoreExtendedProperties= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle proprietà estese durante la pubblicazione in un database.|  
|**IgnoreFileAndLogFilePath={True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nel percorsi dei file e dei file di log durante la pubblicazione in un database.|  
|**IgnoreFilegroupPlacement= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nella posizione dei **FILEGROUP** durante la pubblicazione in un database.|  
|**IgnoreFileSize= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nelle dimensioni dei file durante la pubblicazione in un database.|  
|**IgnoreFillFactor= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nei fattori di riempimento durante la pubblicazione in un database.|  
  
|Proprietà|Predefinito|Descrizione|  
|------------|-----------|---------------|  
|**IgnoreFullTextCatalogFilePath= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nel percorso dei file di indice full-text durante la pubblicazione in un database.|  
|**IgnoreIdentitySeed= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nel valore di inizializzazione per una colonna Identity durante la pubblicazione in un database.|  
|**IgnoreIncrement= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nell'incremento per una colonna Identity durante la pubblicazione in un database.|  
|**IgnoreIndexOptions ={True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle opzioni di indice durante la pubblicazione in un database.|  
|**IgnoreIndexPadding= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nella spaziatura indice durante la pubblicazione in un database.|  
|**IgnoreKeywordCasing= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nell'utilizzo di maiuscole e minuscole nelle parole chiave durante la pubblicazione in un database.|  
|**IgnoreLockHintsOnIndexes= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze negli hint di blocco negli indici durante la pubblicazione in un database.|  
|**IgnoreLoginSids= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nell'ID di sicurezza (SID) durante la pubblicazione in un database.|  
|**IgnoreNotForReplication= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare l'impostazione non per la replica durante la pubblicazione in un database.|  
|**IgnoreObjectPlacementOnPartitionScheme= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare la posizione di un oggetto in uno schema di partizione durante la pubblicazione in un database.|  
|**IgnorePartitionSchemes= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze negli schemi di partizione e nelle funzioni durante la pubblicazione in un database.|  
|**IgnorePermissions= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle autorizzazioni durante la pubblicazione in un database.|  
|**IgnoreQuotedIdentifiers= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle impostazioni degli identificatori delimitati durante la pubblicazione in un database.|  
|**IgnoreRoleMembership={True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nell'appartenenza al ruolo degli account di accesso durante la pubblicazione in un database.|  
|**IgnoreRouteLifetime= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nelle appartenenze ai ruoli degli account di accesso durante la pubblicazione in un database.|  
|**IgnoreSemicolonBetweenState= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nei punti e virgola tra le istruzioni Transact-SQL durante la pubblicazione in un database.|  
|**IgnoreTableOptions= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle opzioni tabella durante la pubblicazione in un database.|  
|**IgnoreUserSettingsObjects= {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle opzioni di impostazione utente durante la pubblicazione in un database.|  
|**IgnoreWhitespace= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nello spazio vuoto durante la pubblicazione in un database.|  
|**IgnoreWithNocheckOnCheckConstraints={True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nel valore della clausola **WITH NOCHECK** per vincoli CHECK durante la pubblicazione in un database.|  
|**IgnoreWithNocheckOnForeignKeys={True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nel valore della clausola **WITH NOCHECK** per chiavi esterne durante la pubblicazione in un database.|  
|**IncludeCompositeObjects= {True &#124; False}**|**False**|Specifica se includere tutti gli elementi composti come parte di una singola operazione di pubblicazione.|  
|**IncludeTransactionalScripts={True &#124; False}**|**False**|Specifica se utilizzare istruzioni transazionali ove possibile durante la pubblicazione in un database.|  
|**NoAlterStatementsToChangeClrTypes={True &#124; False}**|**False**|Specifica che, in caso di differenze, la pubblicazione deve sempre comportare l'eliminazione e la ricreazione di un assembly, anziché l'esecuzione di un'istruzione ALTER ASSEMBLY.|  
|**PopulateFilesOnFilegroups= {True &#124; False}**|**True**|Specifica se insieme alla creazione di un nuovo **FileGroup** nel database di destinazione viene creato anche un nuovo file.|  
|**RegisterDataTierApplication={True &#124; False}**|**False**|Specifica se lo schema è registrato con il server di database.|  
|**ScriptDatabaseCollation {True &#124; False}**|**False**|Specifica se ignorare o aggiornare le differenze nelle regole di confronto del database durante la pubblicazione in un database.|  
|**ScriptDatabaseCompatibility= {True &#124; False}**|**True**|Specifica se ignorare o aggiornare le differenze nella compatibilità del database durante la pubblicazione in un database.|  
|**ScriptDatabaseOptions= {True &#124; False}**|**True**|Specifica se impostare o aggiornare le proprietà del database di destinazione durante la pubblicazione in un database.|  
|**ScriptFileSize={True &#124; False}**|**False**|Controlla se le dimensioni sono specificate quando si aggiunge un file a un filegroup.|  
|**ScriptNewConstraintValidation= {True &#124; False}**|**True**|Specifica se verificare tutti i vincoli come appartenenti a un unico set al termine della pubblicazione, evitando errori nei dati causati da un controllo o da un vincolo della chiave esterna durante l'azione di pubblicazione. Se questa opzione è **False**, i vincoli vengono pubblicati senza il controllo dei dati corrispondenti.|  
|**ScriptDeployStateChecks={True &#124; False}**|**False**|Specifica se generare istruzioni nello script di pubblicazione per verificare che i nomi di server e di database corrispondano ai nomi specificati nel progetto di database.|  
|**ScriptRefreshModule= {True &#124; False}**|**True**|Specifica se includere istruzioni di aggiornamento alla fine dello script di pubblicazione.|  
|**Storage={File&#124;Memory}**|**Memoria**|Specifica la modalità di archiviazione degli elementi durante la compilazione del modello di database. Per motivi di prestazioni, l'impostazione predefinita è **Memory**. In caso di database di dimensioni molto estese, è necessaria l'archiviazione di file di cui è stato eseguito il backup.|  
|**TreatVerificationErrorsAsWarnings= {True &#124; False}**|**False**|Specifica se gli errori che si verificano durante la verifica della pubblicazione devono essere considerati come avvisi. Il controllo viene effettuato sul piano di distribuzione generato prima che questo venga eseguito sul database di destinazione. La verifica del piano consente di rilevare problemi quali la perdita di oggetti della sola destinazione, ad esempio gli indici, che devono essere eliminati per apportare una modifica. Con la verifica è inoltre possibile individuare le dipendenze, ad esempio una tabella o una visualizzazione, che sono presenti a causa di un riferimento a un progetto composito ma che non esistono nel database di destinazione. È possibile scegliere di considerare gli errori di verifica come avvisi per ottenere un elenco completo dei problemi anziché arrestare l'azione di pubblicazione al primo errore.|  
|**UnmodifiableObjectWarnings= {True &#124; False}**|**True**|Specifica se generare avvisi quando vengono rilevate differenze negli oggetti che non possono essere modificati, ad esempio quando risultano differenti la dimensione o i percorsi di un file.|  
|**VerifyCollationCompatibility={True &#124; False}**|**True**|Specifica se viene verificata la compatibilità delle regole di confronto.|  
|**VerifyDeployment={True &#124; False}**|**True**|Specifica se prima della pubblicazione devono essere eseguiti i controlli che arrestano l'azione di pubblicazione se vengono rilevati problemi che potrebbero bloccare l'esecuzione della pubblicazione. Ad esempio, l'azione di pubblicazione potrebbe venire arrestata se le chiavi esterne presenti nel database di destinazione non esistono nel progetto di database, situazione che genera errori durante la pubblicazione.|  
  
> [!NOTE]  
> Tutti i parametri di destinazione specificati eseguiranno l'override di quelli specificati nel profilo di pubblicazione di origine.  
  
> [!NOTE]  
> I valori e le variabili **SQLCMD** devono essere specificati nel parametro di origine del profilo di pubblicazione, dal momento che non possono essere specificati come parametri di destinazione.  
  
I parametri **Destination** seguenti sono disponibili solo per le operazioni **DeployReport** e **Script**:  
  
|Parametro|Predefinito|Descrizione|  
|-------------|-----------|---------------|  
|**OutputPath**={ *stringa* }|N/D|Parametro facoltativo che indica a **dbSqlPackage** di creare il file di output XML di DeployReport o il file di output SQL di Script nel percorso su disco specificato da *stringa*. Tale azione sovrascrive tutti gli script che si trovano attualmente nel percorso specificato dalla stringa.|  
  
> [!NOTE]  
> Se il parametro **OutputPath** non viene fornito per un'azione **DeployReport** o **Script**, l'output viene restituito come messaggio.  
  
## <a name="examples"></a>Esempi  
Di seguito è riportata la sintassi di esempio per un'operazione **Extract** tramite **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source connection string>",<source parameter> -dest:dbSqlPackage="<target dacpac file path>"  
```  
  
Di seguito è riportata la sintassi di esempio per un'operazione **Publish** tramite **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Publish,<destination parameters>  
```  
  
Di seguito è riportata la sintassi di esempio per un'operazione **DeployReport** tramite **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=DeployReport,OutputPath="<path to output XML file>",<destination parameters>  
```  
  
Di seguito è riportata la sintassi di esempio per un'operazione **Script** tramite **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Script,OutputPath="<path to output sql script>",<destination parameters>  
```  
