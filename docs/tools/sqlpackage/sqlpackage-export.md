---
title: Export SqlPackage
description: Informazioni su come automatizzare le attività di sviluppo di database con Export di SqlPackage.exe. È possibile visualizzare esempi, oltre a parametri, proprietà e variabili SQLCMD disponibili.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: c0c3b1462fe165678e3826585f5ce82d5945de56
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577854"
---
# <a name="sqlpackage-export-parameters-and-properties"></a>Parametri e proprietà di Export di SqlPackage
L'azione Export di SqlPackage.exe esporta un database attivo da un database di SQL Server o SQL di Azure in un pacchetto BACPAC (file con estensione bacpac). Per impostazione predefinita, nel file bacpac verranno inclusi i dati di tutte le tabelle. Facoltativamente, è possibile specificare solo un subset di tabelle per cui esportare i dati. La convalida dell'azione Export garantisce la compatibilità del database SQL di Azure per il database di destinazione completo anche se viene specificato un subset di tabelle per l'esportazione. 

## <a name="command-line-syntax"></a>Sintassi della riga di comando

**SqlPackage.exe** avvia le azioni specificate usando i parametri, le proprietà e le variabili SQLCMD indicati nella riga di comando.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-export-action"></a>Parametri per l'azione Export

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

## <a name="properties-specific-to-the-export-action"></a>Proprietà specifiche dell'azione Esportazione

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

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [sqlpackage](sqlpackage.md)