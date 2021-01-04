---
title: Script SqlPackage
description: Informazioni su come automatizzare le attività di sviluppo di database con Script di SqlPackage.exe. È possibile visualizzare esempi, oltre a parametri, proprietà e variabili SQLCMD disponibili.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/4/2020
ms.openlocfilehash: cffeb28f69fe93bb69cfdafd0afa98ef663e6ef8
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577842"
---
# <a name="sqlpackage-script-parameters-and-properties"></a>Parametri e proprietà di Script di SqlPackage
L'azione Script di SqlPackage.exe consente di creare uno script di aggiornamento incrementale Transact-SQL tramite cui viene aggiornato lo schema di un database di destinazione affinché corrisponda allo schema di un database di origine. 

## <a name="command-line-syntax"></a>Sintassi della riga di comando

**SqlPackage.exe** avvia le azioni specificate usando i parametri, le proprietà e le variabili SQLCMD indicati nella riga di comando.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-script-action"></a>Parametri per l'azione Script

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

## <a name="properties-specific-to-the-script-action"></a>Proprietà specifiche dell'azione Script

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

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [sqlpackage](sqlpackage.md)