---
title: SqlPackage.exe
description: Informazioni su come automatizzare le attività di sviluppo di database con SqlPackage.exe. È possibile visualizzare esempi, oltre a parametri, proprietà e variabili SQLCMD disponibili.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: ee78b145965c17ff0a496611c6506d23df1a31a3
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384500"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** è un'utilità della riga di comando che automatizza le attività di sviluppo di database seguenti:  
  
- [Versione](#version): restituisce il numero di build dell'applicazione SqlPackage.  Aggiunto nella versione 18.6.

- [Extract](#extract-parameters-and-properties): crea un file snapshot del database con estensione dacpac da un database SQL Server o SQL di Azure attivo.  
  
- [Publish](#publish-parameters-properties-and-sqlcmd-variables): aggiorna in modo incrementale uno schema di database affinché corrisponda allo schema di un file di origine con estensione dacpac. Se il database non esiste nel server, viene creato durante l'operazione di pubblicazione. In caso contrario, verrà aggiornato un database esistente.  
  
- [Export](#export-parameters-and-properties): esporta un database attivo, inclusi lo schema del database e i dati utente, da un database SQL Server o SQL di Azure in un pacchetto BACPAC (file con estensione bacpac).  
  
- [Import](#import-parameters-and-properties): importa i dati di tabella e dello schema da un pacchetto BACPAC in un nuovo database utente in un'istanza di database SQL Server o SQL di Azure.  
  
- [DeployReport](#deployreport-parameters-and-properties): crea un report XML delle modifiche che verrebbero effettuate da un'azione Publish.  
  
- [DriftReport](#driftreport-parameters): crea un report XML delle modifiche apportate a un database registrato dall'ultima registrazione.  
  
- [Script](#script-parameters-and-properties): crea uno script di aggiornamento incrementale Transact-SQL che aggiorna lo schema di una destinazione affinché corrisponda allo schema di un'origine.  
  
La riga di comando di **SqlPackage.exe** consente di specificare queste azioni insieme alle proprietà e ai parametri specifici di ogni azione.  

**[Scaricare la versione più recente](sqlpackage-download.md)** . Per informazioni dettagliate sulla versione più recente, vedere le [note sulla versione](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Sintassi della riga di comando

**SqlPackage.exe** avvia le azioni specificate usando i parametri, le proprietà e le variabili SQLCMD indicati nella riga di comando.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Esempi di utilizzo

**Generare un confronto tra database usando i file con estensione dacpac con l'output di uno script SQL**

Per iniziare, creare un file con estensione dacpac delle modifiche più recenti al database:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Creare un file con estensione dacpac della destinazione del database (senza modifiche):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Creare uno script SQL che genera le differenze dei due file dacpac:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Versione

Visualizza la versione di sqlpackage come numero di build.  Può essere usato nei prompt interattivi, nonché nelle [pipeline automatizzate](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="extract-parameters-and-properties"></a>Parametri e proprietà di estrazione
Un'azione di estrazione di SqlPackage.exe crea uno schema di un database attivo da un database SQL Server o SQL di Azure in un pacchetto DACPAC (file con estensione dacpac). Per impostazione predefinita, i dati non sono inclusi nel file con estensione dacpac. Per includere i dati, usare l'[azione di esportazione](#export-parameters-and-properties). 

### <a name="help-for-the-extract-action"></a>Guida per l'azione Estrai

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|Extract|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Specifica se i file esistenti dovranno essere sovrascritti tramite sqlpackage.exe. Se si specifica False e viene rilevato un file esistente, sqlpackage.exe interrompe l'azione. Il valore predefinito è True. |
|**/Properties:**|**/p**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una proprietà specifica dell'azione: {NomeProprietà}={Valore}. Fare riferimento alla Guida di un'azione specifica per visualizzarne i nomi delle proprietà. Esempio: sqlpackage.exe /Action:Extract /?. |
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False. |
|**/SourceConnectionString:**|**/scs**|{string}|Specifica una stringa di connessione valida di SQL Server o SQL Azure al database di origine. Se questo parametro viene specificato, deve essere usato esclusivamente per altri parametri di origine. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definisce il nome del database di origine. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Specifica se deve essere utilizzata la crittografia SQL per la connessione al database di origine. |
|**/SourcePassword:**|**/sp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di origine. |
|**/SourceServerName:**|**/ssn**|{string}|Definisce il nome del server che ospita il database di origine. |
|**/SourceTimeout:**|**/st**|{int}|Specifica il timeout in secondi per una connessione al database di origine. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di origine e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/SourceUser:**|**/su**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di origine. |
|**/TargetFile:**|**/tf**|{string}| Specifica un file di destinazione, ovvero un file DACPAC, da usare come destinazione dell'azione anziché di un database. Se si usa questo parametro, non deve essere valido nessun altro parametro di destinazione. Questo parametro non sarà valido per le azioni che supportano solo le destinazioni del database.| 
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

### <a name="properties-specific-to-the-extract-action"></a>Proprietà specifiche dell'azione Estrai

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Specifica il timeout del comando in secondi quando si eseguono query in SQL Server.|
|**/p:**|DacApplicationDescription=(STRING)|Definisce la descrizione dell'applicazione da archiviare nei metadati DACPAC.|
|**/p:**|DacApplicationName=(STRING)|Definisce il nome dell'applicazione da archiviare nei metadati del pacchetto di applicazione livello dati. Il valore predefinito corrisponde al database del database.|
|**/p:**|DacMajorVersion=(INT32 '1')|Definisce il numero di versione principale da archiviare nei metadati del pacchetto di applicazione livello dati.|
|**/p:**|DacMinorVersion=(INT32 '0')|Definisce il numero di versione secondario da archiviare nei metadati del pacchetto di applicazione livello dati.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Specifica il timeout del blocco a livello di database in secondi quando si eseguono query su SQL Server. Usare -1 per l'attesa indefinita.|
|**/p:**|ExtractAllTableData=(BOOLEAN)|Indica se vengono estratti i dati di tutte le tabelle utente. Se True, vengono estratti i dati di tutte le tabelle utente e non è possibile specificare tabelle utente singole per l'estrazione dei dati. Se False, specificare una o più tabelle utente da cui estrarre i dati.|
|**/p:**|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')|Se true, vengono estratti solo gli oggetti con ambito di applicazione per l'origine specificata. Se false, vengono estratti tutti gli oggetti per l'origine specificata.|
|**/p:**|ExtractReferencedServerScopedElements=(BOOLEAN 'True')|Se true, estrarre gli oggetti Login, Server Audit e Credential a cui fanno riferimento gli oggetti del database di origine.|
|**/p:**|ExtractUsageProperties=(BOOLEAN)|Specifica se vengono estratte dal database le proprietà di utilizzo quali, ad esempio, il numero di righe della tabella e le dimensioni dell'indice.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Specifica se devono essere ignorate le proprietà estese.|
|**/p:**|IgnorePermissions=(BOOLEAN 'True')|Specifica se devono essere ignorate le autorizzazioni.|
|**/p:**|IgnoreUserLoginMappings=(BOOLEAN)|Specifica se le relazioni tra utenti e account di accesso vengono ignorate.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Specifica il timeout del comando a esecuzione prolungata in secondi quando si eseguono query su SQL Server. Usare 0 per l'attesa indefinita.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Specifica il tipo di archivio di backup per il modello schema utilizzato durante l'estrazione.|
|**/p:**|TableData=(STRING)|Indica la tabella da cui verranno estratti i dati. Specificare il nome della tabella con o senza parentesi che racchiudono le parti che compongono il nome nel formato seguente: nome_schema.identificatore_tabella. Questa opzione può essere specificata più volte.|
|**/p:**| TempDirectoryForTableData=(STRING)|Specifica la directory temporanea usata per memorizzare nel buffer i dati della tabella prima di essere scritti nel file del pacchetto.|
|**/p:**|VerifyExtraction=(BOOLEAN)|Specifica se il pacchetto di applicazione livello dati estratto deve essere verificato.|

## <a name="publish-parameters-properties-and-sqlcmd-variables"></a>Pubblicare parametri, proprietà e variabili SQLCMD

Un'operazione di pubblicazione tramite SqlPackage.exe consente di aggiornare in modo incrementale lo schema di un database di destinazione affinché corrisponda alla struttura di un database di origine. La pubblicazione di un pacchetto di distribuzione contenente dati utente per tutte le tabelle o solo per un subset di tabelle comporta l'aggiornamento dei dati di tabella oltre allo schema. La distribuzione dei dati sovrascrive lo schema e i dati nelle tabelle esistenti del database di destinazione. La distribuzione dei dati non modifica lo schema e i dati esistenti nel database di destinazione per le tabelle non incluse nel pacchetto di distribuzione.  

### <a name="help-for-publish-action"></a>Guida per l'azione Pubblica

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|Pubblica|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/AzureKeyVaultAuthMethod:**|**/akv**|{Interactive&#124;ClientIdSecret}|Specifica il metodo di autenticazione usato per accedere ad Azure Key Vault se un'operazione di pubblicazione include modifiche a una tabella o colonna crittografata. |
|**/ClientId:**|**/cid**|{string}|Specifica l'ID client da usare nell'autenticazione con Azure Key Vault, quando necessario |
|**/DeployScriptPath:**|**/dsp**|{string}|Specifica un percorso file facoltativo in cui restituire come output lo script di distribuzione. Per una distribuzione di Azure, se sono disponibili comandi TSQL per creare o modificare il database master, verrà scritto uno script nello stesso percorso, ma con "Filename_Master.sql" come nome del file di output. |
|**/DeployReportPath:**|**/drp**|{string}|Specifica un percorso file facoltativo in cui restituire come output il file XML del report di distribuzione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Specifica se i file esistenti dovranno essere sovrascritti tramite sqlpackage.exe. Se si specifica False e viene rilevato un file esistente, sqlpackage.exe interrompe l'azione. Il valore predefinito è True. |
|**/Profile:**|**/pr**|{string}|Specifica il percorso file a un profilo di pubblicazione dell'applicazione livello dati. Il profilo consente di definire una raccolta di proprietà e variabili da utilizzare durante la generazione di output.|
|**/Properties:**|**/p**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una proprietà specifica dell'azione: {NomeProprietà}={Valore}. Fare riferimento alla Guida di un'azione specifica per visualizzarne i nomi delle proprietà. Esempio: SqlPackage.exe/Action:Publish/?.|
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False.|
|**/Secret:**|**/secr**|{string}|Specifica il segreto client da usare nell'autenticazione con Azure Key Vault, quando necessario |
|**/SourceConnectionString:**|**/scs**|{string}|Specifica una stringa di connessione valida di SQL Server o SQL Azure al database di origine. Se questo parametro viene specificato, deve essere usato esclusivamente per altri parametri di origine. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definisce il nome del database di origine. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Specifica se deve essere utilizzata la crittografia SQL per la connessione al database di origine. |
|**/SourceFile:**|**/sf**|{string}|Specifica il file di origine da usare come origine dell'azione anziché un database. Se si utilizza questo parametro, non deve essere valido nessun altro parametro di origine. |
|**/SourcePassword:**|**/sp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di origine. |
|**/SourceServerName:**|**/ssn**|{string}|Definisce il nome del server che ospita il database di origine. |
|**/SourceTimeout:**|**/st**|{int}|Specifica il timeout in secondi per una connessione al database di origine. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di origine e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/SourceUser:**|**/su**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di origine. |
|**/TargetConnectionString:**|**/tcs**|{string}|Specifica una stringa di connessione valida di SQL Server o Azure al database di destinazione. Se questo parametro viene specificato, deve essere usato esclusivamente per tutti gli altri parametri di destinazione. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Specifica un override per il nome del database di destinazione del parametro Action di sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Specifica se usare la crittografia SQL per la connessione al database di destinazione. |
|**/TargetPassword:**|**/tp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di destinazione. |
|**/TargetServerName:**|**/tsn**|{string}|Definisce il nome del server che ospita il database di destinazione. |
|**/TargetTimeout:**|**/tt**|{int}|Specifica il timeout in secondi per stabilire una connessione al database di destinazione. Per Azure AD è consigliabile che questo valore sia maggiore o uguale a 30 secondi.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di destinazione e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/TargetUser:**|**/tu**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di destinazione. |
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una variabile specifica dell'azione: {NomeVariabile}={Valore}. Nel file DACPAC è incluso l'elenco di variabili SQLCMD valide. Viene generato un errore se non viene fornito un valore per ogni variabile. |

### <a name="properties-specific-to-the-publish-action"></a>Proprietà specifiche dell'azione Pubblica

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Specifica altri argomenti per i collaboratori alla distribuzione in un elenco di valori separati da punti e virgola.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Specifica altri collaboratori alla distribuzione da eseguire durante la distribuzione del dacpac in un elenco di ID o nomi di collaboratori di compilazione completi separati da punti e virgola.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Specifica i percorsi per caricare collaboratori alla distribuzione aggiuntivi. in un elenco di valori separati da punti e virgola. | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|Questa proprietà viene utilizzata dalla distribuzione SqlClr per determinare l'eliminazione di tutti gli assembly di blocco come parte del piano di distribuzione. Per impostazione predefinita, tutti gli assembly di blocco o di riferimento bloccheranno l'aggiornamento di un assembly se l'assembly di riferimento deve essere eliminato.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Specifica se tentare l'azione indipendentemente dalle piattaforme SQL Server incompatibili.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Non bloccare il movimento di dati in una tabella con sicurezza a livello di riga se questa proprietà è impostata su true. L'impostazione predefinita è false.|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Esegue il backup del database prima di distribuire qualsiasi modifica.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')|Specifica che se esiste una possibilità di perdita di dati derivante dall'operazione di pubblicazione, l'episodio di pubblicazione deve essere interrotto.|
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Specifica se bloccare l'aggiornamento di un database il cui schema non corrisponde più alla relativa registrazione o di cui è stata annullata la registrazione.|
|**/p:**|CommandTimeout=(INT32 '60')|Specifica il timeout del comando in secondi quando si eseguono query in SQL Server.|
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Specifica se devono essere impostate come commento le dichiarazioni delle variabili SETVAR nello script di pubblicazione generato. È possibile scegliere questo approccio quando si desidera specificare i valori nella riga di comando durante la pubblicazione mediante uno strumento quale SQLCMD.EXE.|
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Con questa impostazione è possibile stabilire come vengono gestite le regole di confronto del database durante la distribuzione. Per impostazione predefinita, le regole di confronto del database verranno aggiornate se non corrispondono alle regole di confronto specificate dall'origine. Quando questa opzione è impostata, devono essere utilizzate le regole di confronto del server o del database di destinazione.|
|**/p:**|CreateNewDatabase=(BOOLEAN)|Specifica se deve essere aggiornato il database di destinazione o se deve essere eliminato e ricreato durante la pubblicazione in un database.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definisce l'edizione di un database SQL di Azure.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')|Specifica il timeout del blocco a livello di database in secondi quando si eseguono query su SQL Server. Usare -1 per l'attesa indefinita.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definisce le dimensioni massime in GB di un database SQL di Azure.|
|**/p:**|DatabaseServiceObjective=(STRING)|Definisce il livello delle prestazioni di un database SQL di Azure, ad esempio "P0" o "S1".|
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Se true, il database è impostato sulla modalità utente singolo prima della distribuzione.|
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')|Specifica se disabilitare i trigger DDL (Data Definition Language) all'inizio del processo di pubblicazione e riabilitarli alla fine dell'azione di pubblicazione.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Se true, gli oggetti Change Data Capture non sono modificati.|
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Specifica se gli oggetti replicati vengono identificati durante la verifica.|
|**/p:**|DoNotDropObjectType=(STRING)|Tipo di oggetto che non deve essere rimosso quando DropObjectsNotInSource è True. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DoNotDropObjectTypes=(STRING)|Elenco di valori delimitati da punti e virgola relativo ai tipi di oggetto che non devono essere rimossi quando DropObjectsNotInSource è true. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Specifica se i vincoli che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Specifica se i trigger DML che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Specifica se le proprietà estese che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminate dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Specifica se gli indici che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Specifica se gli oggetti che non esistono nel file (con estensione dacpac) di snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database. Questo valore ha la precedenza rispetto a DropExtendedProperties.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Specifica se le autorizzazioni che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminate dal database di destinazione durante la pubblicazione di aggiornamenti in un database.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Specifica se i membri del ruolo che non sono definiti nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione di aggiornamenti in un database.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Specifica se le statistiche che non esistono nel file snapshot del database (con estensione dacpac) vengono eliminati dal database di destinazione quando si esegue la pubblicazione in un database.|
|**/p:**|ExcludeObjectType=(STRING)|Tipo di oggetto che deve essere ignorato durante la distribuzione. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|ExcludeObjectTypes=(STRING)|Elenco di tipi di oggetto separati da punti e virgola che devono essere ignorati durante la distribuzione. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Fornisce automaticamente un valore predefinito durante l'aggiornamento di una tabella contenente dati con una colonna che non consente valori Null.|
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Specifica se le differenze nell'impostazione ANSI NULLS devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Specifica se le differenze nel provider di autorizzazioni devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Specifica se le differenze nelle regole di confronto colonna devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Specifica se le differenze nell'ordine colonne della tabella devono essere ignorate o aggiornate durante la pubblicazione di un database.|
|**/p:**|IgnoreComments=(BOOLEAN)|Specifica se le differenze nei commenti devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso file del provider del servizio di crittografia devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Specifica se le differenze nell'ordine dei trigger Data Definition Language (DDL) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database o in un server.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Specifica se le differenze nello stato abilitato o disabilitato dei trigger Data Definition Language (DDL) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Specifica se le differenze nello schema predefinito devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Specifica se le differenze nell'ordine dei trigger Data Manipulation Language (DML) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Specifica se le differenze nello stato abilitato o disabilitato dei trigger DML devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Specifica se le differenze nelle proprietà estese devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso dei file e dei file di log devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Specifica se le differenze nella posizione degli oggetti nei FILEGROUP devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Specifica se le differenze nelle dimensioni dei file devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Specifica se le differenze nel fattore di riempimento per l'archiviazione degli indici devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso file per il catalogo full-text devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Specifica se le differenze nel valore di inizializzazione per una colonna Identity devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreIncrement=(BOOLEAN)|Specifica se le differenze nell'incremento per una colonna Identity devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni di indice devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Specifica se le differenze nel riempimento indice devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Specifica se le differenze nelle maiuscole e nelle minuscole delle parole chiave devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Specifica se devono essere ignorate o aggiornate le differenze negli hint di blocco negli indici durante la pubblicazione in un database.|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')|Specifica se le differenze nell'ID di sicurezza (SID) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Specifica se le impostazioni non per la replica devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Specifica se la posizione di un oggetto in uno schema di partizione deve essere ignorata o aggiornata quando si esegue la pubblicazione in un database.|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Specifica se le differenze negli schemi di partizione e nelle funzioni devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Specifica se le differenze nelle autorizzazioni devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Specifica se le differenze nell'impostazione degli identificatori delimitati devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Specifica se ignorare o aggiornare le differenze nell'appartenenza al ruolo degli account di accesso durante la pubblicazione in un database.|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Specifica se le differenze nel tempo di conservazione in SQL Server della route nella tabella di routing devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Specifica se le differenze nei punti e virgola tra le istruzioni T-SQL verranno ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni di tabella devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni della partizione di tabella devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.  Questa opzione si applica solo ai database (data warehouse) di pool SQL Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Specifica se le differenze negli oggetti impostazioni utente devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Specifica se le differenze nello spazio devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Specifica se le differenze nel valore della clausola WITH NOCHECK per vincoli CHECK devono essere ignorate o aggiornate quando si esegue la pubblicazione.|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Specifica se le differenze nel valore della clausola WITH NOCHECK per chiavi esterne devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Include tutti gli elementi compositi come parte di una singola operazione di pubblicazione.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Specifica se le istruzioni transazionali devono essere usate ove possibile quando si esegue la pubblicazione in un database.|
|**/p:**|LongRunningCommandTimeout=(INT32)|Specifica il timeout del comando a esecuzione prolungata in secondi quando si eseguono query su SQL Server. Usare 0 per l'attesa indefinita.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Specifica che, in caso di differenze, la pubblicazione deve sempre comportare l'eliminazione e la ricreazione di un assembly, anziché l'esecuzione di un'istruzione ALTER ASSEMBLY.|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Specifica se, quando si crea un nuovo FileGroup nel database di destinazione, viene anche creato un nuovo file.|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Specifica se lo schema è registrato con il server di database.|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Specifica se i collaboratori DeploymentPlanExecutor devono essere eseguiti quando vengono eseguite altre operazioni.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Specifica se le differenze nelle regole di confronto del database devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Specifica se le differenze nella compatibilità del database devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Specifica se le proprietà del database di destinazione devono essere impostate o aggiornate come parte dell'azione di pubblicazione.|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Specifica se nello script di pubblicazione vengono generate istruzioni per verificare che il nome del database e il nome del server corrispondano ai nomi specificati nel progetto del database.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Controlla se le dimensioni sono specificate quando si aggiunge un file a un filegroup.|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Al termine della pubblicazione tutti i vincoli verranno verificati come un unico set, evitando errori nei dati causati da un controllo o da un vincolo di chiave esterna durante la pubblicazione. Se impostata su False, i vincoli verranno pubblicati senza il controllo dei dati corrispondenti.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Include istruzioni di aggiornamento alla fine dello script di pubblicazione.|
|**/p:**|Storage=({File&#124;Memory})|Specifica la modalità di archiviazione degli elementi durante la compilazione del modello di database. Per motivi di prestazioni, l'impostazione predefinita è InMemory. In caso di database di dimensioni estese, è necessaria l'archiviazione di file di cui è stato eseguito il backup.|
|**/p:**|TreatVerificationErrorsAsWarnings=(BOOLEAN)|Specifica se gli errori rilevati durante la verifica della pubblicazione devono essere considerati come avvisi. Il controllo viene effettuato a fronte del piano di distribuzione generato prima che il piano venga eseguito nel database di destinazione. La verifica del piano consente di rilevare problemi quali la perdita di oggetti della sola destinazione, ad esempio gli indici, che devono essere eliminati per apportare una modifica. Con la verifica è anche possibile individuare le dipendenze, ad esempio una tabella o una visualizzazione, presenti a causa di un riferimento a un progetto composito ma che non esistono nel database di destinazione. È possibile scegliere di eseguire questa operazione per ottenere un elenco completo di tutti i problemi, anziché dover interrompere l'azione di pubblicazione al primo errore.
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Specifica se devono essere generati avvisi quando vengono rilevate differenze negli oggetti che non possono essere modificati, ad esempio quando risultano differenti la dimensione o i percorsi di un file.|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Specifica se viene verificata la compatibilità delle regole di confronto.|
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Specifica se prima della pubblicazione devono essere eseguiti i controlli che arresteranno l'azione di pubblicazione qualora vengono rilevati problemi che potrebbero bloccare l'esecuzione della pubblicazione. Ad esempio, l'azione di pubblicazione potrebbe venire arrestata se si hanno chiavi esterne nel database di destinazione che non esistono nel progetto di database e tale situazione comporta la generazione di errori durante la pubblicazione.|
|

### <a name="sqlcmd-variables"></a>Variabili SQLCMD

Nella tabella seguente viene illustrato il formato dell'opzione da usare per eseguire l'override del valore della variabile di un comando SQL (**sqlcmd**) usata durante l'azione di pubblicazione. I valori della variabile specificati nella riga di comando eseguono l'override dei valori assegnati alla variabile, ad esempio in un profilo di pubblicazione.  
  
|Parametro|Predefinito|Descrizione|  
|-------------|-----------|---------------|  
|**/Variables:{PropertyName}={Value}**||Specifica una coppia nome/valore per una variabile specifica dell'azione: {NomeVariabile}={Valore}. Nel file DACPAC è incluso l'elenco di variabili SQLCMD valide. Viene generato un errore se non viene fornito un valore per ogni variabile.|  
  
## <a name="export-parameters-and-properties"></a>Parametri e proprietà di Export

Un'azione Esportazione di SqlPackage.exe esporta un database attivo da un database SQL Server o SQL di Azure in un pacchetto BACPAC (file BACPAC). Per impostazione predefinita, nel file bacpac verranno inclusi i dati di tutte le tabelle. Facoltativamente, è possibile specificare solo un subset di tabelle per cui esportare i dati. La convalida dell'azione Export garantisce la compatibilità del database SQL di Azure per il database di destinazione completo anche se viene specificato un subset di tabelle per l'esportazione.  
  
### <a name="help-for-export-action"></a>Guida per l'azione Esportazione

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|Esportazione|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Specifica se i file esistenti dovranno essere sovrascritti tramite sqlpackage.exe. Se si specifica False e viene rilevato un file esistente, sqlpackage.exe interrompe l'azione. Il valore predefinito è True. |
|**/Properties:**|**/p**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una proprietà specifica dell'azione: {NomeProprietà}={Valore}. Fare riferimento alla Guida di un'azione specifica per visualizzarne i nomi delle proprietà. Esempio: sqlpackage.exe /Action:Export /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False.|
|**/SourceConnectionString:**|**/scs**|{string}|Specifica una stringa di connessione valida di SQL Server o SQL Azure al database di origine. Se questo parametro viene specificato, deve essere usato esclusivamente per altri parametri di origine. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definisce il nome del database di origine. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Specifica se deve essere utilizzata la crittografia SQL per la connessione al database di origine. |
|**/SourcePassword:**|**/sp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di origine. |
|**/SourceServerName:**|**/ssn**|{string}|Definisce il nome del server che ospita il database di origine. |
|**/SourceTimeout:**|**/st**|{int}|Specifica il timeout in secondi per una connessione al database di origine. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di origine e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/SourceUser:**|**/su**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di origine. |
|**/TargetFile:**|**/tf**|{string}| Specifica un file di destinazione, ovvero un file DACPAC, da usare come destinazione dell'azione anziché di un database. Se si usa questo parametro, non deve essere valido nessun altro parametro di destinazione. Questo parametro non sarà valido per le azioni che supportano solo le destinazioni del database.|
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

### <a name="properties-specific-to-the-export-action"></a>Proprietà specifiche dell'azione Esportazione

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Specifica il timeout del comando in secondi quando si eseguono query in SQL Server.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Specifica il timeout del blocco a livello di database in secondi quando si eseguono query su SQL Server. Usare -1 per l'attesa indefinita.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Specifica il timeout del comando a esecuzione prolungata in secondi quando si eseguono query su SQL Server. Usare 0 per l'attesa indefinita.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Specifica il tipo di archivio di backup per il modello schema utilizzato durante l'estrazione.|
|**/p:**|TableData=(STRING)|Indica la tabella da cui verranno estratti i dati. Specificare il nome della tabella con o senza parentesi che racchiudono le parti che compongono il nome nel formato seguente: nome_schema.identificatore_tabella. Questa opzione può essere specificata più volte.|
|**/p:**|TempDirectoryForTableData=(STRING)|Specifica la directory temporanea usata per memorizzare nel buffer i dati della tabella prima di essere scritti nel file del pacchetto.|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|Specifica la versione del motore di destinazione necessaria. A seconda dell'impostazione di questo valore, nel bacpac generato saranno consentiti gli oggetti supportati da server di database SQL di Azure con funzionalità della versione 12, ad esempio le tabelle ottimizzate per la memoria.|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|Specifica se i tipi di documento full-text supportati per il database SQL di Microsoft Azure versione 12 devono essere verificati.|
  
## <a name="import-parameters-and-properties"></a>Parametri e proprietà di Import

L'azione Importazione di SqlPackage.exe importa i dati di tabella e schema da un pacchetto BACPAC (file BACPAC) in un database nuovo o vuoto in SQL Server o nel database SQL di Azure. Al momento dell'operazione di importazione in un database esistente, il database di destinazione non può contenere oggetti dello schema definiti dall'utente.  
  
### <a name="help-for-command-actions"></a>Guida per le azioni dei comandi

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|Importa|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/Properties:**|**/p**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una proprietà specifica dell'azione: {NomeProprietà}={Valore}. Fare riferimento alla Guida di un'azione specifica per visualizzarne i nomi delle proprietà. Esempio: sqlpackage.exe /Action:Import /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False.|
|**/SourceFile:**|**/sf**|{string}|Specifica un file di origine da usare come origine dell'azione. Se si utilizza questo parametro, non deve essere valido nessun altro parametro di origine. |
|**/TargetConnectionString:**|**/tcs**|{string}|Specifica una stringa di connessione valida di SQL Server o Azure al database di destinazione. Se questo parametro viene specificato, deve essere usato esclusivamente per tutti gli altri parametri di destinazione. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Specifica un override per il nome del database di destinazione del parametro Action di sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Specifica se usare la crittografia SQL per la connessione al database di destinazione. |
|**/TargetPassword:**|**/tp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di destinazione. |
|**/TargetServerName:**|**/tsn**|{string}|Definisce il nome del server che ospita il database di destinazione. |
|**/TargetTimeout:**|**/tt**|{int}|Specifica il timeout in secondi per stabilire una connessione al database di destinazione. Per Azure AD è consigliabile che questo valore sia maggiore o uguale a 30 secondi.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di destinazione e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/TargetUser:**|**/tu**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di destinazione. |
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

Proprietà specifiche dell'azione Importazione:

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Specifica il timeout del comando in secondi quando si eseguono query in SQL Server.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definisce l'edizione di un database SQL di Azure.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Specifica il timeout del blocco a livello di database in secondi quando si eseguono query su SQL Server. Usare -1 per l'attesa indefinita.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definisce le dimensioni massime in GB di un database SQL di Azure.|
|**/p:**|DatabaseServiceObjective=(STRING)|Definisce il livello delle prestazioni di un database SQL di Azure, ad esempio "P0" o "S1".|
|**/p:**|ImportContributorArguments=(STRING)|Specifica gli argomenti per i collaboratori alla distribuzione. in un elenco di valori separati da punti e virgola.|
|**/p:**|ImportContributors=(STRING)|Specifica i collaboratori alla distribuzione da eseguire durante l'importazione del bacpac. in un elenco di ID o nomi di collaboratori di compilazione completi separati da punti e virgola.|
|**/p:**|ImportContributorPaths=(STRING)|Specifica i percorsi per caricare collaboratori alla distribuzione aggiuntivi. in un elenco di valori separati da punti e virgola. |
|**/p:**|LongRunningCommandTimeout=(INT32)| Specifica il timeout del comando a esecuzione prolungata in secondi quando si eseguono query su SQL Server. Usare 0 per l'attesa indefinita.|
|**/p:**|Storage=({File&#124;Memory})|Specifica la modalità di archiviazione degli elementi durante la compilazione del modello di database. Per motivi di prestazioni, l'impostazione predefinita è InMemory. In caso di database di dimensioni estese, è necessaria l'archiviazione di file di cui è stato eseguito il backup.|
  
## <a name="deployreport-parameters-and-properties"></a>Parametri e proprietà di DeployReport

Un'azione report di **SqlPackage.exe** crea un report XML delle modifiche che verrebbero apportate da un'azione di pubblicazione.  
  
### <a name="help-for-deployreport-action"></a>Guida per l'azione DeployReport

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|DeployReport|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/OutputPath:**|**/op**|{string}|Specifica il percorso file in cui vengono generati i file di output. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Specifica se i file esistenti dovranno essere sovrascritti tramite sqlpackage.exe. Se si specifica False e viene rilevato un file esistente, sqlpackage.exe interrompe l'azione. Il valore predefinito è True. |
|**/Profile:**|**/pr**|{string}|Specifica il percorso file a un profilo di pubblicazione dell'applicazione livello dati. Il profilo consente di definire una raccolta di proprietà e variabili da utilizzare durante la generazione di output. |
|**/Properties:**|**/p**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una proprietà specifica dell'azione: {NomeProprietà}={Valore}. Fare riferimento alla Guida di un'azione specifica per visualizzarne i nomi delle proprietà. Esempio: sqlpackage.exe /Action:DeployReport /?. |
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False. |
|**/SourceConnectionString:**|**/scs**|{string}|Specifica una stringa di connessione valida di SQL Server o SQL Azure al database di origine. Se questo parametro viene specificato, deve essere usato esclusivamente per altri parametri di origine. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definisce il nome del database di origine. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Specifica se deve essere utilizzata la crittografia SQL per la connessione al database di origine. |
|**/SourceFile:**|**/sf**|{string}|Specifica il file di origine da usare come origine dell'azione anziché un database. Se si utilizza questo parametro, non deve essere valido nessun altro parametro di origine. |
|**/SourcePassword:**|**/sp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di origine. |
|**/SourceServerName:**|**/ssn**|{string}|Definisce il nome del server che ospita il database di origine. |
|**/SourceTimeout:**|**/st**|{int}|Specifica il timeout in secondi per una connessione al database di origine. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di origine e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/SourceUser:**|**/su**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di origine. |
|**/TargetConnectionString:**|**/tcs**|{string}|Specifica una stringa di connessione valida di SQL Server o Azure al database di destinazione. Se questo parametro viene specificato, deve essere usato esclusivamente per tutti gli altri parametri di destinazione. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Specifica un override per il nome del database di destinazione del parametro Action di sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Specifica se usare la crittografia SQL per la connessione al database di destinazione. |
|**/TargetFile:**|**/tf**|{string}|Specifica un file di destinazione, ovvero un file DACPAC, da usare come destinazione dell'azione anziché di un database. Se si usa questo parametro, non deve essere valido nessun altro parametro di destinazione. Questo parametro non sarà valido per le azioni che supportano solo le destinazioni del database.|
|**/TargetPassword:**|**/tp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di destinazione. |
|**/TargetServerName:**|**/tsn**|{string}|Definisce il nome del server che ospita il database di destinazione. |
|**/TargetTimeout:**|**/tt**|{int}|Specifica il timeout in secondi per stabilire una connessione al database di destinazione. Per Azure AD è consigliabile che questo valore sia maggiore o uguale a 30 secondi.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di destinazione e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/TargetUser:**|**/tu**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di destinazione. |
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una variabile specifica dell'azione: {NomeVariabile}={Valore}. Nel file DACPAC è incluso l'elenco di variabili SQLCMD valide. Viene generato un errore se non viene fornito un valore per ogni variabile. |

## <a name="properties-specific-to-the-deployreport-action"></a>Proprietà specifiche dell'azione DeployReport

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Specifica altri argomenti per i collaboratori alla distribuzione in un elenco di valori separati da punti e virgola.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Specifica altri collaboratori alla distribuzione da eseguire durante la distribuzione del dacpac in un elenco di ID o nomi di collaboratori di compilazione completi separati da punti e virgola.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Specifica i percorsi per caricare collaboratori alla distribuzione aggiuntivi. in un elenco di valori separati da punti e virgola. | 
|**/p:**|AllowDropBlocking Assemblies=(BOOLEAN)|Questa proprietà viene utilizzata dalla distribuzione SqlClr per determinare l'eliminazione di tutti gli assembly di blocco come parte del piano di distribuzione. Per impostazione predefinita, tutti gli assembly di blocco o di riferimento bloccheranno l'aggiornamento di un assembly se l'assembly di riferimento deve essere eliminato.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Specifica se tentare l'azione indipendentemente dalle piattaforme SQL Server incompatibili.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Non bloccare il movimento di dati in una tabella con sicurezza a livello di riga se questa proprietà è impostata su true. L'impostazione predefinita è false.|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Esegue il backup del database prima di distribuire qualsiasi modifica.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')|Specifica che se esiste una possibilità di perdita di dati derivante dall'operazione di pubblicazione, l'episodio di pubblicazione deve essere interrotto.|
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Specifica se bloccare l'aggiornamento di un database il cui schema non corrisponde più alla relativa registrazione o di cui è stata annullata la registrazione. |
|**/p:**|CommandTimeout=(INT32 '60')|Specifica il timeout del comando in secondi quando si eseguono query in SQL Server. |
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Specifica se devono essere impostate come commento le dichiarazioni delle variabili SETVAR nello script di pubblicazione generato. È possibile scegliere questo approccio quando si desidera specificare i valori nella riga di comando durante la pubblicazione mediante uno strumento quale SQLCMD.EXE. |
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Con questa impostazione è possibile stabilire come vengono gestite le regole di confronto del database durante la distribuzione. Per impostazione predefinita, le regole di confronto del database verranno aggiornate se non corrispondono alle regole di confronto specificate dall'origine. Quando questa opzione è impostata, devono essere utilizzate le regole di confronto del server o del database di destinazione. |
|**/p:**|CreateNewDatabase=(BOOLEAN)|Specifica se deve essere aggiornato il database di destinazione o se deve essere eliminato e ricreato durante la pubblicazione in un database. |
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definisce l'edizione di un database SQL di Azure.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Specifica il timeout del blocco a livello di database in secondi quando si eseguono query su SQL Server. Usare -1 per l'attesa indefinita.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definisce le dimensioni massime in GB di un database SQL di Azure.|
|**/p:**|DatabaseServiceObjective=(STRING)|Definisce il livello delle prestazioni di un database SQL di Azure, ad esempio "P0" o "S1". |
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Se true, il database è impostato sulla modalità utente singolo prima della distribuzione. |
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| Specifica se disabilitare i trigger DDL (Data Definition Language) all'inizio del processo di pubblicazione e riabilitarli alla fine dell'azione di pubblicazione.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Se true, gli oggetti Change Data Capture non sono modificati.|
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Specifica se gli oggetti replicati vengono identificati durante la verifica.|
|**/p:**|DoNotDropObjectType=(STRING)|Tipo di oggetto che non deve essere rimosso quando DropObjectsNotInSource è True. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers. |
|**/p:**|DoNotDropObjectTypes=(STRING)|Elenco di valori delimitati da punti e virgola relativo ai tipi di oggetto che non devono essere rimossi quando DropObjectsNotInSource è true. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Specifica se i vincoli che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Specifica se i trigger DML che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Specifica se le proprietà estese che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminate dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Specifica se gli indici che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Specifica se gli oggetti che non esistono nel file (con estensione dacpac) di snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database. Questo valore ha la precedenza rispetto a DropExtendedProperties.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Specifica se le autorizzazioni che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminate dal database di destinazione durante la pubblicazione di aggiornamenti in un database.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Specifica se i membri del ruolo che non sono definiti nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione di aggiornamenti in un database.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Specifica se le statistiche che non esistono nel file snapshot del database (con estensione dacpac) vengono eliminati dal database di destinazione quando si esegue la pubblicazione in un database.|
|**/p:**|ExcludeObjectType=(STRING)|Tipo di oggetto che deve essere ignorato durante la distribuzione. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|ExcludeObjectTypes=(STRING)|Elenco di tipi di oggetto separati da punti e virgola che devono essere ignorati durante la distribuzione. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Fornisce automaticamente un valore predefinito durante l'aggiornamento di una tabella contenente dati con una colonna che non consente valori Null.|
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Specifica se le differenze nell'impostazione ANSI NULLS devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Specifica se le differenze nel provider di autorizzazioni devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Specifica se le differenze nelle regole di confronto colonna devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Specifica se le differenze nell'ordine colonne della tabella devono essere ignorate o aggiornate durante la pubblicazione di un database.|
|**/p:**|IgnoreComments=(BOOLEAN)|Specifica se le differenze nei commenti devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso file del provider del servizio di crittografia devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Specifica se le differenze nell'ordine dei trigger Data Definition Language (DDL) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database o in un server.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Specifica se le differenze nello stato abilitato o disabilitato dei trigger Data Definition Language (DDL) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Specifica se le differenze nello schema predefinito devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Specifica se le differenze nell'ordine dei trigger Data Manipulation Language (DML) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Specifica se le differenze nello stato abilitato o disabilitato dei trigger DML devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Specifica se le differenze nelle proprietà estese devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso dei file e dei file di log devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Specifica se le differenze nella posizione degli oggetti nei FILEGROUP devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Specifica se le differenze nelle dimensioni dei file devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Specifica se le differenze nel fattore di riempimento per l'archiviazione degli indici devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso file per il catalogo full-text devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Specifica se le differenze nel valore di inizializzazione per una colonna Identity devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database. |
|**/p:**|IgnoreIncrement=(BOOLEAN)|Specifica se le differenze nell'incremento per una colonna Identity devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni di indice devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Specifica se le differenze nel riempimento indice devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Specifica se le differenze nelle maiuscole e nelle minuscole delle parole chiave devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Specifica se devono essere ignorate o aggiornate le differenze negli hint di blocco negli indici durante la pubblicazione in un database. |
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')| Specifica se le differenze nell'ID di sicurezza (SID) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Specifica se le impostazioni non per la replica devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Specifica se la posizione di un oggetto in uno schema di partizione deve essere ignorata o aggiornata quando si esegue la pubblicazione in un database.|
 |**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Specifica se le differenze negli schemi di partizione e nelle funzioni devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Specifica se le differenze nelle autorizzazioni devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Specifica se le differenze nell'impostazione degli identificatori delimitati devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Specifica se ignorare o aggiornare le differenze nell'appartenenza al ruolo degli account di accesso durante la pubblicazione in un database. |
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Specifica se le differenze nel tempo di conservazione in SQL Server della route nella tabella di routing devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Specifica se le differenze nei punti e virgola tra le istruzioni T-SQL verranno ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni di tabella devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni della partizione di tabella devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.  Questa opzione si applica solo ai database di data warehouse Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Specifica se le differenze negli oggetti impostazioni utente devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Specifica se le differenze nello spazio devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Specifica se le differenze nel valore della clausola WITH NOCHECK per vincoli CHECK devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Specifica se le differenze nel valore della clausola WITH NOCHECK per chiavi esterne devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.| 
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Include tutti gli elementi compositi come parte di una singola operazione di pubblicazione.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Specifica se le istruzioni transazionali devono essere usate ove possibile quando si esegue la pubblicazione in un database.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Specifica il timeout del comando a esecuzione prolungata in secondi quando si eseguono query su SQL Server. Usare 0 per l'attesa indefinita.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Specifica che, in caso di differenze, la pubblicazione deve sempre comportare l'eliminazione e la ricreazione di un assembly, anziché l'esecuzione di un'istruzione ALTER ASSEMBLY. |
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Specifica se, quando si crea un nuovo FileGroup nel database di destinazione, viene anche creato un nuovo file. |
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Specifica se lo schema è registrato con il server di database. 
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Specifica se i collaboratori DeploymentPlanExecutor devono essere eseguiti quando vengono eseguite altre operazioni.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Specifica se le differenze nelle regole di confronto del database devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Specifica se le differenze nella compatibilità del database devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database. |
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Specifica se le proprietà del database di destinazione devono essere impostate o aggiornate come parte dell'azione di pubblicazione. |
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Specifica se nello script di pubblicazione vengono generate istruzioni per verificare che il nome del database e il nome del server corrispondano ai nomi specificati nel progetto del database.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Controlla se le dimensioni sono specificate quando si aggiunge un file a un filegroup. |
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Al termine della pubblicazione tutti i vincoli verranno verificati come un unico set, evitando errori nei dati causati da un controllo o da un vincolo di chiave esterna durante la pubblicazione. Se impostata su False, i vincoli verranno pubblicati senza il controllo dei dati corrispondenti.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Include istruzioni di aggiornamento alla fine dello script di pubblicazione.|
|**/p:**|Storage=({File&#124;Memory})|Specifica la modalità di archiviazione degli elementi durante la compilazione del modello di database. Per motivi di prestazioni, l'impostazione predefinita è InMemory. In caso di database di dimensioni estese, è necessaria l'archiviazione di file di cui è stato eseguito il backup.|
|**/p:**|TreatVerificationErrorsAsWarnings=(BOOLEAN)|Specifica se gli errori rilevati durante la verifica della pubblicazione devono essere considerati come avvisi. Il controllo viene effettuato a fronte del piano di distribuzione generato prima che il piano venga eseguito nel database di destinazione. La verifica del piano consente di rilevare problemi quali la perdita di oggetti della sola destinazione, ad esempio gli indici, che devono essere eliminati per apportare una modifica. Con la verifica è anche possibile individuare le dipendenze, ad esempio una tabella o una visualizzazione, presenti a causa di un riferimento a un progetto composito ma che non esistono nel database di destinazione. È possibile scegliere di eseguire questa operazione per ottenere un elenco completo di tutti i problemi, anziché dover interrompere l'azione di pubblicazione al primo errore. |
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Specifica se devono essere generati avvisi quando vengono rilevate differenze negli oggetti che non possono essere modificati, ad esempio quando risultano differenti la dimensione o i percorsi di un file.| 
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Specifica se viene verificata la compatibilità delle regole di confronto.| 
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Specifica se prima della pubblicazione devono essere eseguiti i controlli che arresteranno l'azione di pubblicazione qualora vengono rilevati problemi che potrebbero bloccare l'esecuzione della pubblicazione. Ad esempio, l'azione di pubblicazione potrebbe venire arrestata se si hanno chiavi esterne nel database di destinazione che non esistono nel progetto di database e tale situazione comporta la generazione di errori durante la pubblicazione. |
  
## <a name="driftreport-parameters"></a>Parametri DriftReport

Un'azione report di **SqlPackage.exe** consente di creare un report XML delle modifiche apportate al database registrato dall'ultima registrazione.  
  
### <a name="help-for-driftreport-action"></a>Guida per l'azione DriftReport

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|DriftReport|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/OutputPath:**|**/op**|{string}|Specifica il percorso file in cui vengono generati i file di output. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Specifica se i file esistenti dovranno essere sovrascritti tramite sqlpackage.exe. Se si specifica False e viene rilevato un file esistente, sqlpackage.exe interrompe l'azione. Il valore predefinito è True. |
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False.|
|**/TargetConnectionString:**|**/tcs**|{string}|Specifica una stringa di connessione valida di SQL Server o Azure al database di destinazione. Se questo parametro viene specificato, deve essere usato esclusivamente per tutti gli altri parametri di destinazione. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Specifica un override per il nome del database di destinazione del parametro Action di sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Specifica se usare la crittografia SQL per la connessione al database di destinazione. |
|**/TargetPassword:**|**/tp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di destinazione. |
|**/TargetServerName:**|**/tsn**|{string}|Definisce il nome del server che ospita il database di destinazione. |
|**/TargetTimeout:**|**/tt**|{int}|Specifica il timeout in secondi per stabilire una connessione al database di destinazione. Per Azure AD è consigliabile che questo valore sia maggiore o uguale a 30 secondi.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di destinazione e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/TargetUser:**|**/tu**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di destinazione. |
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

## <a name="script-parameters-and-properties"></a>Parametri e proprietà di script

L'azione Script di **SqlPackage.exe** consente di creare uno script di aggiornamento incrementale Transact-SQL tramite cui viene aggiornato lo schema di un database di destinazione affinché corrisponda allo schema di un database di origine.  
  
### <a name="help-for-the-script-action"></a>Guida per l'azione Script

|Parametro|Forma breve|valore|Descrizione|
|---|---|---|---|
|**/Action:**|**/a**|Script|Specifica l'azione da eseguire. |
|**/AccessToken:**|**/at**|{string}| Specifica il token di accesso per l'autenticazione basata su token da usare per la connessione al database di destinazione. |
|**/DeployScriptPath:**|**/dsp**|{string}|Specifica un percorso file facoltativo in cui restituire come output lo script di distribuzione. Per una distribuzione di Azure, se sono disponibili comandi TSQL per creare o modificare il database master, verrà scritto uno script nello stesso percorso, ma con "Filename_Master.sql" come nome del file di output. |
|**/DeployReportPath:**|**/drp**|{string}|Specifica un percorso file facoltativo in cui restituire come output il file XML del report di distribuzione. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Specifica se la registrazione diagnostica viene restituita come output nella console. Il valore predefinito è False. |
|**/DiagnosticsFile:**|**/df**|{string}|Specifica un file per archiviare i log di diagnostica. |
|**/MaxParallelism:**|**/mp**|{int}| Specifica il grado di parallelismo per le operazioni simultanee in esecuzione su un database. Il valore predefinito è 8. |
|**/OutputPath:**|**/op**|{string}|Specifica il percorso file in cui vengono generati i file di output. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Specifica se i file esistenti dovranno essere sovrascritti tramite sqlpackage.exe. Se si specifica False e viene rilevato un file esistente, sqlpackage.exe interrompe l'azione. Il valore predefinito è True. |
|**/Profile:**|**/pr**|{string}|Specifica il percorso file a un profilo di pubblicazione dell'applicazione livello dati. Il profilo consente di definire una raccolta di proprietà e variabili da utilizzare durante la generazione di output.|
|**/Properties:**|**/p**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una proprietà specifica dell'azione: {NomeProprietà}={Valore}. Fare riferimento alla Guida di un'azione specifica per visualizzarne i nomi delle proprietà. Esempio: sqlpackage.exe /Action:Script /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Specifica se la notifica dettagliata dell'interfaccia utente viene eliminata. Il valore predefinito è False.|
|**/SourceConnectionString:**|**/scs**|{string}|Specifica una stringa di connessione valida di SQL Server o SQL Azure al database di origine. Se questo parametro viene specificato, deve essere usato esclusivamente per altri parametri di origine. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definisce il nome del database di origine. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Specifica se deve essere utilizzata la crittografia SQL per la connessione al database di origine. |
|**/SourceFile:**|**/sf**|{string}|Specifica un file di origine da usare come origine dell'azione. Se si utilizza questo parametro, non deve essere valido nessun altro parametro di origine. |
|**/SourcePassword:**|**/sp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di origine. |
|**/SourceServerName:**|**/ssn**|{string}|Definisce il nome del server che ospita il database di origine. |
|**/SourceTimeout:**|**/st**|{int}|Specifica il timeout in secondi per una connessione al database di origine. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di origine e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/SourceUser:**|**/su**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di origine. |
|**/TargetConnectionString:**|**/tcs**|{string}|Specifica una stringa di connessione valida di SQL Server o Azure al database di destinazione. Se questo parametro viene specificato, deve essere usato esclusivamente per tutti gli altri parametri di destinazione. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Specifica un override per il nome del database di destinazione del parametro Action di sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Specifica se usare la crittografia SQL per la connessione al database di destinazione. |
|**/TargetFile:**|**/tf**|{string}| Specifica un file di destinazione, ovvero un file DACPAC, da usare come destinazione dell'azione anziché di un database. Se si usa questo parametro, non deve essere valido nessun altro parametro di destinazione. Questo parametro non sarà valido per le azioni che supportano solo le destinazioni del database.|
|**/TargetPassword:**|**/tp**|{string}|Per gli scenari di autenticazione di SQL Server, definisce la password da usare per accedere al database di destinazione. |
|**/TargetServerName:**|**/tsn**|{string}|Definisce il nome del server che ospita il database di destinazione. |
|**/TargetTimeout:**|**/tt**|{int}|Specifica il timeout in secondi per stabilire una connessione al database di destinazione. Per Azure AD è consigliabile che questo valore sia maggiore o uguale a 30 secondi.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Specifica se usare TLS per crittografare la connessione al database di destinazione e ignorare l'analisi della catena di certificati per convalidare l'attendibilità. |
|**/TargetUser:**|**/tu**|{string}|Per gli scenari di autenticazione di SQL Server, definisce l'utente SQL Server da usare per accedere al database di destinazione. |
|**/TenantId:**|**/tid**|{string}|Rappresenta l'ID tenant o il nome di dominio di Azure AD. Questa opzione è necessaria per il supporto degli utenti guest o importati di Azure AD e per gli account Microsoft, ad esempio outlook.com, hotmail.com o live.com. Se questo parametro viene omesso, verrà usato l'ID tenant predefinito per Azure AD, supponendo che l'utente autenticato sia un utente nativo per questa istanza di AD. In questo caso, tuttavia, gli utenti guest o importati e/o gli account Microsoft ospitati in questa istanza di Azure AD non sono supportati e l'operazione avrà esito negativo. <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Specifica se deve essere usata l'autenticazione universale. Se impostato su True, viene attivato il protocollo di autenticazione interattivo per supportare l'autenticazione a più fattori. Questa opzione può essere usata anche per l'autenticazione Azure AD senza autenticazione a più fattori, usando un protocollo interattivo che richiede all'utente di immettere il nome utente e la password o l'autenticazione integrata (credenziali di Windows). Se /UniversalAuthentication è impostato su True, non è possibile specificare l'autenticazione Azure AD in SourceConnectionString (/scs). Se /UniversalAuthentication è impostato su False, l'autenticazione Azure AD deve essere specificata in SourceConnectionString (/scs). <br/> Per altre informazioni sull'autenticazione universale di Active Directory, vedere [Autenticazione universale con database SQL e Azure Synapse Analytics (supporto SSMS per MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{NomeProprietà}={Valore}|Specifica una coppia nome/valore per una variabile specifica dell'azione: {NomeVariabile}={Valore}. Nel file DACPAC è incluso l'elenco di variabili SQLCMD valide. Viene generato un errore se non viene fornito un valore per ogni variabile. |

### <a name="properties-specific-to-the-script-action"></a>Proprietà specifiche dell'azione Script

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Specifica altri argomenti per i collaboratori alla distribuzione in un elenco di valori separati da punti e virgola.
|**/p:**|AdditionalDeploymentContributors=(STRING)|Specifica altri collaboratori alla distribuzione da eseguire durante la distribuzione del dacpac in un elenco di ID o nomi di collaboratori di compilazione completi separati da punti e virgola.
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Specifica i percorsi per caricare collaboratori alla distribuzione aggiuntivi. in un elenco di valori separati da punti e virgola. | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|Questa proprietà viene utilizzata dalla distribuzione SqlClr per determinare l'eliminazione di tutti gli assembly di blocco come parte del piano di distribuzione. Per impostazione predefinita, tutti gli assembly di blocco o di riferimento bloccheranno l'aggiornamento di un assembly se l'assembly di riferimento deve essere eliminato.
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Specifica se tentare l'azione indipendentemente dalle piattaforme SQL Server incompatibili.
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Non bloccare il movimento di dati in una tabella con sicurezza a livello di riga se questa proprietà è impostata su true. L'impostazione predefinita è false.
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Esegue il backup del database prima di distribuire qualsiasi modifica.
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')|Specifica che se esiste una possibilità di perdita di dati derivante dall'operazione di pubblicazione, l'episodio di pubblicazione deve essere interrotto.
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Specifica se bloccare l'aggiornamento di un database il cui schema non corrisponde più alla relativa registrazione o di cui è stata annullata la registrazione.
|**/p:**|CommandTimeout=(INT32 '60')|Specifica il timeout del comando in secondi quando si eseguono query in SQL Server.
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Specifica se devono essere impostate come commento le dichiarazioni delle variabili SETVAR nello script di pubblicazione generato. È possibile scegliere questo approccio quando si desidera specificare i valori nella riga di comando durante la pubblicazione mediante uno strumento quale SQLCMD.EXE.
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Con questa impostazione è possibile stabilire come vengono gestite le regole di confronto del database durante la distribuzione. Per impostazione predefinita, le regole di confronto del database verranno aggiornate se non corrispondono alle regole di confronto specificate dall'origine. Quando questa opzione è impostata, devono essere utilizzate le regole di confronto del server o del database di destinazione.|
|**/p:**|CreateNewDatabase=(BOOLEAN)|Specifica se deve essere aggiornato il database di destinazione o se deve essere eliminato e ricreato durante la pubblicazione in un database.
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definisce l'edizione di un database SQL di Azure.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Specifica il timeout del blocco a livello di database in secondi quando si eseguono query su SQL Server. Usare -1 per l'attesa indefinita.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definisce le dimensioni massime in GB di un database SQL di Azure.
|**/p:**|DatabaseServiceObjective=(STRING)|Definisce il livello delle prestazioni di un database SQL di Azure, ad esempio "P0" o "S1".
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Se true, il database è impostato sulla modalità utente singolo prima della distribuzione.
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| Specifica se disabilitare i trigger DDL (Data Definition Language) all'inizio del processo di pubblicazione e riabilitarli alla fine dell'azione di pubblicazione.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Se true, gli oggetti Change Data Capture non sono modificati.
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Specifica se gli oggetti replicati vengono identificati durante la verifica.
|**/p:**|DoNotDropObjectType=(STRING)|Tipo di oggetto che non deve essere rimosso quando DropObjectsNotInSource è True. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.
|**/p:**|DoNotDropObjectTypes=(STRING)|Elenco di valori delimitati da punti e virgola relativo ai tipi di oggetto che non devono essere rimossi quando DropObjectsNotInSource è true. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Specifica se i vincoli che non esistono nel file, con estensione dacpac, dello snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Specifica se i trigger DML che non esistono nel file, con estensione dacpac, dello snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Specifica se le proprietà estese che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminate dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Specifica se gli indici che non esistono nel file, con estensione dacpac, dello snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Specifica se gli oggetti che non esistono nel file, con estensione dacpac, dello snapshot del database verranno eliminati dal database di destinazione durante la pubblicazione in un database. Questo valore ha la precedenza rispetto a DropExtendedProperties.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Specifica se le autorizzazioni che non esistono nel file, con estensione dacpac, dello snapshot di database verranno eliminate dal database di destinazione durante la pubblicazione di aggiornamenti in un database.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Specifica se i membri del ruolo che non sono definiti nel file, con estensione dacpac, dello snapshot di database verranno eliminati dal database di destinazione durante la pubblicazione di aggiornamenti in un database.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Specifica se le statistiche che non esistono nel file snapshot del database (con estensione dacpac) vengono eliminati dal database di destinazione quando si esegue la pubblicazione in un database.|
|**/p:**|ExcludeObjectType=(STRING)|Tipo di oggetto che deve essere ignorato durante la distribuzione. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.
|**/p:**|ExcludeObjectTypes=(STRING)|Elenco di tipi di oggetto separati da punti e virgola che devono essere ignorati durante la distribuzione. Nomi di oggetto validi sono: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables,FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions,UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users,Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders,DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Fornisce automaticamente un valore predefinito durante l'aggiornamento di una tabella contenente dati con una colonna che non consente valori Null.
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Specifica se le differenze nell'impostazione ANSI NULLS devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Specifica se le differenze nel provider di autorizzazioni devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Specifica se le differenze nelle regole di confronto colonna devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Specifica se le differenze nell'ordine colonne della tabella devono essere ignorate o aggiornate durante la pubblicazione di un database.|
|**/p:**|IgnoreComments=(BOOLEAN)|Specifica se le differenze nei commenti devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso file del provider del servizio di crittografia devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Specifica se le differenze nell'ordine dei trigger Data Definition Language (DDL) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database o in un server.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Specifica se le differenze nello stato abilitato o disabilitato dei trigger Data Definition Language (DDL) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Specifica se le differenze nello schema predefinito devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Specifica se le differenze nell'ordine dei trigger Data Manipulation Language (DML) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Specifica se le differenze nello stato abilitato o disabilitato dei trigger DML devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Specifica se le differenze nelle proprietà estese devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso dei file e dei file di log devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Specifica se le differenze nella posizione degli oggetti nei FILEGROUP devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Specifica se le differenze nelle dimensioni dei file devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Specifica se le differenze nel fattore di riempimento per l'archiviazione degli indici devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Specifica se le differenze nel percorso file per il catalogo full-text devono essere ignorate o se viene inviato un avviso quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Specifica se le differenze nel valore di inizializzazione per una colonna Identity devono essere ignorate o aggiornate quando si pubblicano aggiornamenti in un database.|
|**/p:**|IgnoreIncrement=(BOOLEAN)|Specifica se le differenze nell'incremento per una colonna Identity devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni di indice devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Specifica se le differenze nel riempimento indice devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Specifica se le differenze nelle maiuscole e nelle minuscole delle parole chiave devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Specifica se devono essere ignorate o aggiornate le differenze negli hint di blocco negli indici durante la pubblicazione in un database.|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')| Specifica se le differenze nell'ID di sicurezza (SID) devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Specifica se le impostazioni non per la replica devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Specifica se la posizione di un oggetto in uno schema di partizione deve essere ignorata o aggiornata quando si esegue la pubblicazione in un database.|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Specifica se le differenze negli schemi di partizione e nelle funzioni devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Specifica se le differenze nelle autorizzazioni devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Specifica se le differenze nell'impostazione degli identificatori delimitati devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Specifica se ignorare o aggiornare le differenze nell'appartenenza al ruolo degli account di accesso durante la pubblicazione in un database.|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Specifica se le differenze nel tempo di conservazione in SQL Server della route nella tabella di routing devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Specifica se le differenze nei punti e virgola tra le istruzioni T-SQL verranno ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni di tabella devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Specifica se le differenze nelle opzioni della partizione di tabella devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.  Questa opzione si applica solo ai database di data warehouse Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Specifica se le differenze negli oggetti impostazioni utente devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Specifica se le differenze nello spazio devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Specifica se le differenze nel valore della clausola WITH NOCHECK per vincoli CHECK devono essere ignorate o aggiornate quando si esegue la pubblicazione.|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Specifica se le differenze nel valore della clausola WITH NOCHECK per chiavi esterne devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Include tutti gli elementi compositi come parte di una singola operazione di pubblicazione.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Specifica se le istruzioni transazionali devono essere usate ove possibile quando si esegue la pubblicazione in un database.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Specifica il timeout del comando a esecuzione prolungata in secondi quando si eseguono query su SQL Server. Usare 0 per l'attesa indefinita.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Specifica che, in caso di differenze, la pubblicazione deve sempre comportare l'eliminazione e la ricreazione di un assembly, anziché l'esecuzione di un'istruzione ALTER ASSEMBLY.|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Specifica se, quando si crea un nuovo FileGroup nel database di destinazione, viene anche creato un nuovo file.|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Specifica se lo schema è registrato con il server di database.|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Specifica se i collaboratori DeploymentPlanExecutor devono essere eseguiti quando vengono eseguite altre operazioni.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Specifica se le differenze nelle regole di confronto del database devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Specifica se le differenze nella compatibilità del database devono essere ignorate o aggiornate quando si esegue la pubblicazione in un database.|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Specifica se le proprietà del database di destinazione devono essere impostate o aggiornate come parte dell'azione di pubblicazione.|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Specifica se nello script di pubblicazione vengono generate istruzioni per verificare che il nome del database e il nome del server corrispondano ai nomi specificati nel progetto del database.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Controlla se le dimensioni sono specificate quando si aggiunge un file a un filegroup.|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Al termine della pubblicazione tutti i vincoli verranno verificati come un unico set, evitando errori nei dati causati da un controllo o da un vincolo di chiave esterna durante la pubblicazione. Se impostata su False, i vincoli verranno pubblicati senza il controllo dei dati corrispondenti.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Include istruzioni di aggiornamento alla fine dello script di pubblicazione.|
|**/p:**|Storage=({File&#124;Memory})|Specifica la modalità di archiviazione degli elementi durante la compilazione del modello di database. Per motivi di prestazioni, l'impostazione predefinita è InMemory. In caso di database di dimensioni estese, è necessaria l'archiviazione di file di cui è stato eseguito il backup.|
|**/p:**|TreatVerificationErrorsAsWarnings=(BOOLEAN)|Specifica se gli errori rilevati durante la verifica della pubblicazione devono essere considerati come avvisi. Il controllo viene effettuato a fronte del piano di distribuzione generato prima che il piano venga eseguito nel database di destinazione. La verifica del piano consente di rilevare problemi quali la perdita di oggetti della sola destinazione, ad esempio gli indici, che devono essere eliminati per apportare una modifica. Con la verifica è anche possibile individuare le dipendenze, ad esempio una tabella o una visualizzazione, presenti a causa di un riferimento a un progetto composito ma che non esistono nel database di destinazione. È possibile scegliere di eseguire questa operazione per ottenere un elenco completo di tutti i problemi, anziché dover interrompere l'azione di pubblicazione al primo errore.|
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Specifica se devono essere generati avvisi quando vengono rilevate differenze negli oggetti che non possono essere modificati, ad esempio quando risultano differenti la dimensione o i percorsi di un file.|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Specifica se viene verificata la compatibilità delle regole di confronto.
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Specifica se prima della pubblicazione devono essere eseguiti i controlli che arresteranno l'azione di pubblicazione qualora vengono rilevati problemi che potrebbero bloccare l'esecuzione della pubblicazione. Ad esempio, l'azione di pubblicazione potrebbe venire arrestata se si hanno chiavi esterne nel database di destinazione che non esistono nel progetto di database e tale situazione comporta la generazione di errori durante la pubblicazione.|

## <a name="exit-codes"></a>Codici di uscita

Comandi che restituiscono i codici di uscita seguenti:

- 0 = esito positivo
- diverso da zero = esito negativo
