---
title: Import SqlPackage
description: Informazioni su come automatizzare le attività di sviluppo di database con Import di SqlPackage.exe. È possibile visualizzare esempi, oltre a parametri, proprietà e variabili SQLCMD disponibili.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: cecc6de89a9e8f82a64942acb08af7ddee48c5be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081462"
---
# <a name="sqlpackage-import-parameters-and-properties"></a>Parametri e proprietà di Import di SqlPackage
L'azione Import di SqlPackage.exe importa i dati di tabella e schema da un pacchetto BACPAC (file con estensione bacpac) in un database nuovo o vuoto in SQL Server o nel database SQL di Azure. Al momento dell'operazione di importazione in un database esistente, il database di destinazione non può contenere oggetti dello schema definiti dall'utente.  

## <a name="command-line-syntax"></a>Sintassi della riga di comando

**SqlPackage.exe** avvia le azioni specificate usando i parametri, le proprietà e le variabili SQLCMD indicati nella riga di comando.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-import-action"></a>Parametri per l'azione Import

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

## <a name="properties-specific-to-the-import-action"></a>Proprietà specifiche dell'azione Import

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
  

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [sqlpackage](sqlpackage.md)