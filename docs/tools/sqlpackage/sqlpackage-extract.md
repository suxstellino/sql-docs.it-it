---
title: Extract SqlPackage
description: Informazioni su come automatizzare le attività di sviluppo di database con Extract di SqlPackage.exe. È possibile visualizzare esempi, oltre a parametri, proprietà e variabili SQLCMD disponibili.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 3/10/2021
ms.openlocfilehash: 84405eb4d5ac63f287f2696dcef0a1e3ddc09cc4
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622659"
---
# <a name="sqlpackage-extract-parameters-and-properties"></a>Parametri e proprietà di Extract di SqlPackage
L'azione Estrai SqlPackage.exe crea uno schema di un database connesso in un file DACPAC (con estensione dacpac). Per impostazione predefinita, i dati non sono inclusi nel file con estensione dacpac. Per includere i dati, usare l' [Azione Esporta](sqlpackage-export.md) o usare Estrai proprietà *ExtractAllTableData* / *TableData*. 

## <a name="command-line-syntax"></a>Sintassi della riga di comando

**SqlPackage.exe** avvia le azioni specificate usando i parametri, le proprietà e le variabili SQLCMD indicati nella riga di comando.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> Quando viene estratto un database con credenziali con password (ad esempio, un utente di autenticazione SQL), la password viene sostituita con una password diversa con una complessità adeguata. Gli utenti di SqlPackage o DacFx devono modificare la password dopo la pubblicazione del pacchetto dacpac.

## <a name="parameters-for-the-extract-action"></a>Parametri per l'azione Extract

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

## <a name="properties-specific-to-the-extract-action"></a>Proprietà specifiche dell'azione Estrai

|Proprietà|Valore|Descrizione|
|---|---|---|
|**/p:**|AzureStorageBlobEndpoint = (stringa)|Endpoint di archiviazione BLOB di Azure, vedere [SqlPackage for Azure sinapsi Analytics](sqlpackage-for-azure-synapse-analytics.md#extract).|
|**/p:**|AzureStorageContainer = (stringa)|Contenitore di archiviazione BLOB di Azure, vedere [SqlPackage for Azure sinapsi Analytics](sqlpackage-for-azure-synapse-analytics.md#extract).|
|**/p:**|AzureStorageKey = (stringa)|Chiave dell'account di archiviazione di Azure, vedere [SqlPackage for Azure sinapsi Analytics](sqlpackage-for-azure-synapse-analytics.md#extract).|
|**/p:**|AzureStorageRootPath = (stringa)|Percorso radice di archiviazione all'interno del contenitore. Senza questa proprietà, il percorso predefinito è `servername/databasename/timestamp/` . Vedere [SqlPackage for Azure sinapsi Analytics](sqlpackage-for-azure-synapse-analytics.md#extract).|
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

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [sqlpackage](sqlpackage.md)