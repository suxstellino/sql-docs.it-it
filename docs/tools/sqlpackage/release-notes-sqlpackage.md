---
title: Note sulla versione di DacFx e SqlPackage
description: Note sulla versione per sqlpackage Microsoft.
ms.custom: tools|sos
ms.date: 02/02/2019
ms.prod: sql
ms.reviewer: alayu; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 1b93c290596cf9af23e4963912efff652e9c80ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061046"
---
# <a name="release-notes-for-sqlpackageexe"></a>Note sulla versione per SqlPackage.exe

**[Scaricare la versione più recente](sqlpackage-download.md)**

Questo articolo elenca le funzionalità e le correzioni distribuite con le versioni rilasciate di SqlPackage.exe.

## <a name="186-sqlpackage"></a>18.6 sqlpackage

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 settembre 2020|18.6|15.0.4897.1|
|macOS .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2143659)|18 settembre 2020| 18.6|15.0.4897.1|
|Linux .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2143497)|18 settembre 2020| 18.6|15.0.4897.1|
|Windows .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2143496)|18 settembre 2020| 18.6|15.0.4897.1|

### <a name="features"></a>Funzionalità
| Funzionalità | Dettagli |
| :------ | :------ |
| Piattaforma | Versione di sqlpackage per .NET Core aggiornata a .NET Core 3.1 |
| Always Encrypted | Aggiunto il supporto per l'importazione e l'esportazione di enclave sicuri per SQL Server 2019 |
| Distribuzione | Aggiunto il supporto che consente di ignorare le tabelle abilitate per Change Data Capture durante l'esportazione dal database SQL di Azure |
| Distribuzione | Aggiunto il supporto per l'opzione di indice OPTIMIZE_FOR_SEQUENTIAL_KEY nel database SQL di Azure |
| Distribuzione | Aggiunta del supporto per le colonne Identity per Azure Synapse Analytics | 
| Help | Output della versione di sqlpackage nella guida (/?) e supporto del parametro /version | 

### <a name="fixes"></a>Correzioni
| Funzionalità | Dettagli |
| :------ | :------ | 
| Distribuzione | Correzione di uno script di distribuzione non corretto generato quando si specifica la destinazione Istanza gestita di SQL di Azure come utente diverso da sysadmin  | 
| Distribuzione | Correzione del caricamento dei collaboratori alla distribuzione durante l'esecuzione di azioni script | 
| Help | Output del tempo trascorso corretto in sqlpackage quando l'operazione dura più di un giorno | 
| Distribuzione | Correzione della registrazione di dacpac durante la distribuzione per .NET Core | 
| Distribuzione | Correzione di sqlpackage nella gestione di .NET Core del parametro /accessToken (/at) | 
| Distribuzione | Consentite le istruzioni ALTER TABLE nelle stored procedure come istruzioni non di primo livello | 
| Distribuzione | Correzione in Azure Synapse Analytics della convalida delle viste materializzate in modo da non fare distinzione tra maiuscole e minuscole | 

### <a name="known-issues"></a>Problemi noti
| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | La funzionalità Gestione del carico di lavoro di Azure Synapse Analytics (gruppi di carichi di lavoro e classificatori del carico di lavoro) non è ancora supportata | 

## <a name="1851-sqlpackage"></a>18.5.1 sqlpackage

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2134206)|24 giugno 2020|18.5.1|15.0.4826.1|
|macOS .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2134312)|24 giugno 2020| 18.5.1|15.0.4826.1|
|Linux .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2134311)|24 giugno 2020| 18.5.1|15.0.4826.1|
|Windows .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2134310)|24 giugno 2020| 18.5.1|15.0.4826.1|

### <a name="fixes"></a>Correzioni
| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | Correzione di una regressione introdotta nella versione 18.5 per cui si verifica un errore di tipo "Sintassi errata vicino a 'tipo'" quando si distribuisce un dacpac o si importa un bacpac con un utente con accesso esterno all'ambiente locale | 

## <a name="185-sqlpackage"></a>18.5 sqlpackage

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2128142)|28 aprile 2020|18.5|15.0.4769.1|
|macOS .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2128145)|28 aprile 2020| 18.5|15.0.4769.1|
|Linux .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2128144)|28 aprile 2020| 18.5|15.0.4769.1|
|Windows .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2128143)|28 aprile 2020| 18.5|15.0.4769.1|

### <a name="features"></a>Funzionalità
| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | La classificazione di riservatezza dei dati è ora supportata per SQL Server 2008 e versioni successive, il database SQL di Azure e Azure Synapse Analytics |
| Distribuzione | Aggiunta del supporto di Azure Synapse Analytics per i vincoli di tabella |
| Distribuzione | Aggiunta del supporto di Azure Synapse Analytics per l'indice columnstore cluster ordinato |
| Distribuzione | Aggiunta del supporto per un'origine dati esterna (per Oracle, Teradata, MongoDB/CosmosDB, ODBC, cluster Big Data) e una tabella esterna per cluster Big Data di SQL Server 2019 |
| Distribuzione | Aggiunta di un'istanza di database SQL Edge come edizione supportata |
| Distribuzione | Supporto di nomi di server di Istanza gestita nel formato '\<server>.\<dnszone>.database.windows.net' |
| Distribuzione | Aggiunta del supporto per il comando di copia in Azure Synapse Analytics |
| Distribuzione | Aggiunta dell'opzione di distribuzione IgnoreTablePartitionOptions durante la pubblicazione per evitare che venga nuovamente creata la tabella in caso di modifica della funzione di partizione relativa alla tabella per Azure Synapse Analytics |
| .NET Core | Aggiunta del supporto di Microsoft.Data.SqlClient nella versione .NET Core di sqlpackage |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni
| Fix | Dettagli |
| :-- | :------ |
| Distribuzione | Correzione dell'analisi del percorso JSON come espressione |
| Distribuzione | Correzione della generazione di istruzioni GRANT per le autorizzazioni AlterAnyDatabaseScopedConfiguration e AlterAnySensitivityClassification |
| Distribuzione | Correzione dell'autorizzazione script esterno non riconosciuta |
| Distribuzione | Correzione per la proprietà inline - l'aggiunta implicita della proprietà non deve essere visualizzata nella differenza ma la menzione esplicita dovrebbe essere visualizzata tramite script |
| Distribuzione | Risoluzione di un problema a causa del quale la modifica di una tabella a cui fa riferimento una vista materializzata (MV) causa la generazione di istruzioni ALTER VIEW che non sono supportate per le viste materializzate per Azure Synapse Analytics |
| Distribuzione | Correzione dell'errore di pubblicazione quando si aggiunge una colonna a una tabella con dati per Azure Synapse Analytics |
| Distribuzione | Correzione dello script di aggiornamento che deve spostare i dati in una nuova tabella quando si modifica il tipo di colonna di distribuzione (scenario di perdita dei dati) per Azure Synapse Analytics |
| ScriptDom | Correzione del bug ScriptDom a causa del quale non funzionava il riconoscimento dei vincoli inline definiti dopo un indice inline |
| ScriptDom | Correzione della parentesi di chiusura mancante per ScriptDom SYSTEM_TIME in un'istruzione batch |
| Always Encrypted | Correzione per la tabella #tmpErrors che non può essere eliminata se sqlpackage si riconnette e la tabella temporanea è già stata eliminata perché la tabella temporanea scompare al termine della connessione |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti
| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione |  Nella versione 18.5 è stata introdotta una regressione per cui si verifica un errore di tipo "Sintassi errata vicino a 'tipo'" quando si distribuisce un dacpac o si importa un bacpac con un utente con accesso esterno all'ambiente locale. La soluzione alternativa consiste nell'usare sqlpackage 18.4. Il problema verrà corretto nella prossima versione di sqlpackage. | 
| .NET Core | L'importazione di bacpac con classificazione di riservatezza non riesce con il messaggio "Errore irreversibile della connessione interna" a causa di questo [problema noto](https://github.com/dotnet/SqlClient/issues/559) di Microsoft.Data.SqlClient. Questo problema verrà risolto nella prossima versione di sqlpackage. |
| &nbsp; | &nbsp; |

## <a name="1841-sqlpackage"></a>sqlpackage 18.4.1

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2113703)|13 dicembre 2019|18.4.1|15.0.4630.1|
|macOS .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2113705)|13 dicembre 2019| 18.4.1|15.0.4630.1|
|Linux .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2113331)|13 dicembre 2019| 18.4.1|15.0.4630.1|
|Windows .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2113704)|13 dicembre 2019| 18.4.1|15.0.4630.1|

### <a name="fixes"></a>Correzioni
| Fix | Dettagli |
| :-- | :------ |
| ScriptDom |  Nella versione 18.3.1 è stata introdotta una regressione di analisi ScriptDom che considera erroneamente 'RENAME' come token di primo livello, causando l'esito negativo dell'analisi.
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti 

| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione |  È stata introdotta una regressione nella versione 18.4.1, causando la presenza di un errore "Riferimento a un oggetto non impostato su un'istanza di oggetto." durante la distribuzione di un dacpac o l'importazione di un bacpac con un utente con account di accesso esterno. La soluzione alternativa consiste nell'usare sqlpackage 18.4. Il problema verrà corretto nella prossima versione di sqlpackage. | 
| &nbsp; | &nbsp; |

## <a name="184-sqlpackage"></a>sqlpackage 18.4

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2108813)|29 ottobre 2019|18.4|15.0.4573.2|
|macOS .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2108815)|29 ottobre 2019| 18.4|15.0.4573.2|
|Linux .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2108814)|29 ottobre 2019| 18.4|15.0.4573.2|
|Windows .NET Core |[.zip file](https://go.microsoft.com/fwlink/?linkid=2109019)|29 ottobre 2019| 18.4|15.0.4573.2|

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | Aggiungere il supporto per la distribuzione in Azure Synapse Analytics (disponibilità generale). | 
| Piattaforma | Versione GA di sqlpackage per .NET Core per macOS, Linux e Windows. | 
| Security | Rimuovere la firma del codice SHA1. |
| Distribuzione | Aggiungere il supporto per le nuove edizioni del database di Azure: GeneralPurpose, BusinessCritical, Hyperscale |
| Distribuzione | Aggiungere il supporto di Istanza gestita per utenti e gruppi di Azure Active Directory. |
| Distribuzione | Supportare il parametro/AccessToken per sqlpackage in .NET Core. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti 

| Funzionalità | Dettagli |
| :------ | :------ |
| ScriptDom |  Nella versione 18.3.1 è stata introdotta una regressione di analisi ScriptDom che considera erroneamente 'RENAME' come token di primo livello, causando l'esito negativo dell'analisi. Questo problema verrà risolto nella prossima versione di sqlpackage. | 
| &nbsp; | &nbsp; |

### <a name="known-issues-for-net-core"></a>Problemi noti per .NET Core

| Funzionalità | Dettagli |
| :------ | :------ |
| Importa |  Per i file con estensione bacpac con file compressi di dimensioni superiori a 4 GB, può essere necessario eseguire l'importazione con la versione di sqlpackage per .NET Core.  Questo comportamento è dovuto al modo in cui .NET Core genera le intestazioni dei file zip, che, anche se valide, non sono leggibili per la versione di sqlpackage per .NET Full Framework. | 
| Distribuzione | Il parametro /p:Storage=File non è supportato. In .NET Core è supportato solo il parametro Memory. | 
| Always Encrypted | sqlpackage per .NET Core non supporta le colonne Always Encrypted. | 
| Sicurezza | sqlpackage per .NET Core non supporta il parametro /ua per l'autenticazione a più fattori. | 
| Distribuzione | Non sono supportati i file con estensione dacpac e bacpac V2 meno recenti che usano la serializzazione dei dati JSON. |
| &nbsp; | &nbsp; |

## <a name="1831-sqlpackage"></a>sqlpackage 18.3.1

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2102893)|13 settembre 2019|18.3.1|15.0.4538.1|
|macOS .NET Core (anteprima)|[.zip file](https://go.microsoft.com/fwlink/?linkid=2102894)|13 settembre 2019| 18.3.1|15.0.4538.1|
|Linux .NET Core (anteprima)|[.zip file](https://go.microsoft.com/fwlink/?linkid=2102978)|13 settembre 2019| 18.3.1|15.0.4538.1|
|Windows .NET Core (anteprima)|[.zip file](https://go.microsoft.com/fwlink/?linkid=2102979)|13 settembre 2019| 18.13.1|15.0.4538.1|

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | Aggiungere il supporto per la distribuzione in Azure Synapse Analytics (anteprima). | 
| Distribuzione | Aggiungere il parametro /p:DatabaseLockTimeout=(INT32 '60') a sqlpackage. | 
| Distribuzione | Aggiungere il parametro /p:LongRunningCommandTimeout=(INT32) a sqlpackage. |
| Esportazione/estrazione | Aggiungere il parametro /p:TempDirectoryForTableData=(STRING) a sqlpackage. |
| Distribuzione | Consente di caricare collaboratori alla distribuzione da percorsi aggiuntivi. I collaboratori alla distribuzione verranno caricati dalla stessa directory dei file con estensione dacpac di destinazione in corso di distribuzione, dalla directory delle estensioni relativa al file binario sqlpackage.exe e dal parametro /p:AdditionalDeploymentContributorPaths=(STRING) aggiunto a sqlpackage, in cui è possibile specificare percorsi di directory aggiuntivi. |
| Distribuzione | Aggiungere il supporto per OPTIMIZE_FOR_SEQUENTIAL_KEY. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Distribuzione | Correzione che consente di ignorare gli indici automatici, in modo che non vengano rimossi durante la distribuzione. | 
| Always Encrypted | Correzione per la gestione delle colonne varchar Always Encrypted. | 
| Compilazione/Distribuzione | Correzione che risolve il problema del metodo node () per i set di colonne XML.| 
| ScriptDom | Correzione dei casi aggiuntivi in cui la stringa 'URL' veniva interpretata come un token di primo livello. | 
| Grafico | Codice TSQL generato dalla correzione per i riferimenti a pseudocolonne nei vincoli.  | 
| Esportazione | Generazione di password casuali che soddisfano i requisiti di complessità. | 
| Distribuzione | Correzione che consente di rispettare i timeout dei comandi durante il recupero di vincoli. | 
| .NET Core (anteprima) | Correzione della registrazione diagnostica in un file. | 
| .NET Core (anteprima) | Uso del flusso per l'esportazione dei dati delle tabelle per il supporto di tabelle di grandi dimensioni. | 
| &nbsp; | &nbsp; |

## <a name="182-sqlpackage"></a>sqlpackage 18.2

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|Windows|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2087429)|15 aprile 2019|18.2|15.0.4384.2|
|macOS .NET Core (anteprima)|[.zip file](https://go.microsoft.com/fwlink/?linkid=2087247)|15 aprile 2019 | 18.2 |15.0.4384.2|
|Linux .NET Core (anteprima)|[.zip file](https://go.microsoft.com/fwlink/?linkid=2087431)|15 aprile 2019 | 18.2 |15.0.4384.2|

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Grafico | Aggiunta del supporto di tabelle grafo per i vincoli di arco e le clausole dei vincoli di arco. |
| Distribuzione | Abilitazione della regola di convalida del modello per il supporto di 32 colonne per le chiavi di indice per SQL Server 2016 e versioni successive. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Distribuzione | Correzione del reverse engineering di un database di SQL Server 2016 RTM a causa dell'uso di un hint per la query non supportato. |
| Distribuzione | Correzione dell'ordine di distribuzione in modo che le istruzioni di modifica con chiusura automatica vengano eseguite prima delle istruzioni di creazione di filegroup. |
| ScriptDom | Correzione della regressione di analisi ScriptDom in cui la stringa 'URL' veniva interpretata come un token di primo livello. |
| Distribuzione | Correzione di un'eccezione per riferimento Null durante l'analisi di un'istruzione ALTER TABLE ADD INDEX. | 
| Confronto schema | Correzione del confronto dello schema per le colonne calcolate persistenti che ammettono valori Null che indica sempre differenze.|
| &nbsp; | &nbsp; |

## <a name="181-sqlpackage"></a>sqlpackage 18.1

Data di rilascio: &nbsp; 1° febbraio 2019  
Build: &nbsp; 15.0.4316.1  
Versione di anteprima.

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | Aggiunta del supporto per le regole di confronto UTF8. |
| Distribuzione | Abilitazione degli indici columnstore non cluster su una vista indicizzata. |
| Piattaforma | Passaggio a .NET Core 2.2. | 
| Confronto schema | Uso dell'archiviazione supportata dalla memoria per il confronto schema in .NET Core. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Prestazioni | Correzione delle prestazioni per usare lo strumento di stima della cardinalità legacy per le query di reverse engineering. | 
| Prestazioni | Correzione di un problema di prestazioni significativo di confronto schema durante la generazione di uno script. | 
| Confronto schema | Correzione della logica di rilevamento di deviazione dallo schema per ignorare alcune sessioni di eventi estesi (xevent). |
| Grafico | Correzione dell'ordine di importazione per le tabelle grafo. | 
| Esportazione | Correzione dell'esportazione di tabelle esterne con autorizzazioni per oggetti. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

Questa versione include le build di anteprima multipiattaforma di sqlpackage destinate a .NET Core 2.2. È supportata l'esecuzione di sqlpackage in macOS e Linux.

| Problema noto | Dettagli |
| :---------- | :------ |
| Distribuzione | Per .NET Core non sono supportati i collaboratori alla compilazione e alla distribuzione. | 
| Distribuzione | Per .NET Core non sono supportati i file con estensione dacpac e bacpac meno recenti che usano la serializzazione dei dati JSON. | 
| Distribuzione | Per .NET Core, possibili errori di risoluzione dei file dacpac (ad esempio master.dacpac) nei riferimenti a causa di problemi con i file system con distinzione tra maiuscole e minuscole. | Una soluzione alternativa consiste nel convertire in maiuscolo il nome del file di riferimento (ad esempio MASTER.BACPAC). |
| &nbsp; | &nbsp; |

## <a name="180-sqlpackage"></a>sqlpackage 18.0

Data di rilascio: &nbsp; 24 ottobre 2018  
Build: &nbsp; 15.0.4200.1

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione | Aggiunta del supporto per il livello di compatibilità del database 150. | 
| Distribuzione | Aggiunta del supporto per le istanze gestite. | 
| Prestazioni | Aggiunta del parametro della riga di comando MaxParallelism per specificare il grado di parallelismo per le operazioni di database. | 
| Sicurezza | Aggiunta del parametro della riga di comando AccessToken per specificare un token di autenticazione quando ci si connette a SQL Server. | 
| Importa | Aggiunta del supporto per lo streaming dei tipi di dati BLOB/CLOB per le importazioni. | 
| Distribuzione | Aggiunta del supporto per l'opzione 'INLINE' UDF scalare. | 
| Grafico | Aggiunta del supporto per la sintassi 'MERGE' nella tabella grafo. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Grafico | Correzione della pseudo-colonna non risolta per le tabelle grafo. |
| Distribuzione | Correzione della creazione di un database con filegroup ottimizzati per la memoria quando vengono usate tabelle ottimizzate per la memoria. |
| Distribuzione | Correzione dell'inclusione di proprietà estese per le tabelle esterne. |
| &nbsp; | &nbsp; |

## <a name="178-sqlpackage"></a>sqlpackage 17.8

Data di rilascio: &nbsp; 22 giugno 2018  
Build: &nbsp; 14.0.4079.2

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Diagnostica | Miglioramento dei messaggi di errore per gli errori di connessione, incluso il messaggio di eccezione SqlClient. |
| Distribuzione | Supporto della compressione dell'indice negli indici a partizione singola per importazione/esportazione. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Distribuzione | Risolto un problema di reverse engineering per set di colonne XML con SQL 2017 e versioni successive. | 
| Distribuzione | Risolto un problema a causa del quale le operazioni di scripting per il livello di compatibilità 140 erano ignorate per il database SQL di Azure. |
| &nbsp; | &nbsp; |

## <a name="1741-sqlpackage"></a>sqlpackage 17.4.1

Data di rilascio: &nbsp; 25 gennaio 2018  
Build: &nbsp; 14.0.3917.1

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Importazione/Esportazione | Aggiunta del parametro della riga di comando ThreadMaxStackSize per analizzare codice Transact-SQL con un numero elevato di istruzioni annidate. |
| Distribuzione | Supporto delle regole di confronto del catalogo di database. | 
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Importa | Sono stati corretti gli errori causati dal _mancato supporto delle chiavi master di database senza password in questa versione di SQL Server_. Tali errori si presentavano al momento dell'importazione di un file con estensione bacpac del database SQL di Azure in un'istanza locale. |
| Grafico | Correzione di un errore di pseudo-colonna non risolta per le tabelle grafo. |
| Confronto schema | Correzione dell'autenticazione SQL per il confronto di schemi. | 
| &nbsp; | &nbsp; |

## <a name="1740-sqlpackage"></a>sqlpackage 17.4.0

Data di rilascio: &nbsp; 12 dicembre 2017  
Build: &nbsp; 14.0.3881.1

### <a name="features"></a>Funzionalità

| Funzionalità | Dettagli |
| :------ | :------ |
| Distribuzione |  Aggiunta del supporto per i _criteri di conservazione temporali_ in SQL 2017+ e nel database SQL di Azure. | 
| Diagnostica | Aggiunta del parametro della riga di comando /DiagnosticsFile:"C:\Temp\sqlpackage.log" per specificare un percorso di file per salvare le informazioni di diagnostica. | 
| Diagnostica | Aggiunta del parametro della riga di comando /Diagnostics per registrare le informazioni di diagnostica nella console. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correzioni

| Fix | Dettagli |
| :-- | :------ |
| Distribuzione | Non viene attivato un blocco in presenza di un livello di compatibilità del database non riconosciuto. Viene invece presupposto l'uso della versione più recente del database SQL di Azure o della piattaforma locale. |
| &nbsp; | &nbsp; |
