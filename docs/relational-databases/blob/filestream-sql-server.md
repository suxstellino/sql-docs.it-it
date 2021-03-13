---
title: FILESTREAM (SQL Server) | Microsoft Docs
description: Informazioni su FILESTREAM, una funzionalità di SQL Server che archivia i dati nel file system. Verrà descritto come questa funzionalità archivia, protegge e offre l'accesso ai dati.
ms.custom: ''
ms.date: 01/11/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server]
- FILESTREAM [SQL Server], about
- FILESTREAM [SQL Server], overview
ms.assetid: 9a5a8166-bcbe-4680-916c-26276253eafa
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 515010fc6727a64607402316b4c8911fd7f22fb0
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231509"
---
# <a name="filestream-sql-server"></a>FILESTREAM (SQL Server)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only.md)]

FILESTREAM consente l'archiviazione nel file system di dati non strutturati, ad esempio documenti e immagini, da parte delle applicazioni basate su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le applicazioni possono sfruttare le numerose API di flusso e le prestazioni del file system e contemporaneamente mantenere la coerenza transazionale tra i dati non strutturati e i corrispondenti dati strutturati.  
  
FILESTREAM integra il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] con un file system NTFS o ReFS archiviando dati BLOB (Binary Large Object) binari **varbinary(max)** come file nel file system. [!INCLUDE[tsql](../../includes/tsql-md.md)] offre istruzioni che consentono di inserire, aggiornare ed eseguire query, ricerche e back up dei dati FILESTREAM. Le interfacce del file system Win32 forniscono accesso ai dati tramite flusso.  
  
FILESTREAM utilizza la cache di sistema NT per memorizzare nella cache i dati del file. Ciò consente di ridurre qualsiasi effetto che i dati FILESTREAM potrebbero avere sulle prestazioni del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Il pool di buffer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è utilizzato; pertanto questa memoria è disponibile per l'elaborazione di query.  
  
Poiché FILESTREAM non viene abilitato automaticamente al momento dell'installazione o dell'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario abilitarlo usando Gestione configurazione SQL Server e [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per utilizzare FILESTREAM, è necessario creare o modificare un database in modo che contenga un tipo speciale di filegroup e quindi creare o modificare una tabella in modo che contenga una colonna **varbinary(max)** con l'attributo FILESTREAM. Dopo avere completato queste attività, è possibile usare [!INCLUDE[tsql](../../includes/tsql-md.md)] e Win32 per gestire i dati FILESTREAM.  

## <a name="when-to-use-filestream"></a>Quando utilizzare FILESTREAM

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i BLOB possono essere dati standard **varbinary(max)** che archiviano i dati nelle tabelle oppure oggetti **varbinary(max)** di FILESTREAM che archiviano i dati nel file system. La dimensione e l'utilizzo dei dati determinano se è necessario utilizzare l'archiviazione nel database o l'archiviazione nel file system. Se le condizioni indicate di seguito vengono soddisfatte, è necessario utilizzare FILESTREAM:  

- Gli oggetti che si stanno archiviando sono, in media, più grandi di 1 MB.  
- La rapidità dell'accesso in lettura è importante.
- Si stanno sviluppando applicazioni che utilizzano un livello intermedio per la logica dell'applicazione.  

Per oggetti più piccoli, l'archiviazione di BLOB **varbinary(max)** nel database offre spesso migliori prestazioni di flusso.  

## <a name="filestream-storage"></a>Archiviazione FILESTREAM

L'archiviazione FILESTREAM viene implementata come una colonna **varbinary(max)** nella quale i dati sono archiviati come BLOB nel file system. Le dimensioni dei BLOB sono limitate solo dalle dimensioni del volume del file system. Il limite **varbinary(max)** standard di 2 GB delle dimensioni del file non si applica ai BLOB archiviati nel file system.  
  
Per indicare che una colonna deve archiviare dati nel file system, specificare l'attributo FILESTREAM in una colonna **varbinary(max)** . In questo modo [!INCLUDE[ssDE](../../includes/ssde-md.md)] archivia tutti i dati per quella colonna nel file system, ma non nel file di database.  
  
I dati FILESTREAM devono essere archiviati nei filegroup FILESTREAM. Un filegroup FILESTREAM è un filegroup speciale che contiene directory del file system anziché dei file stessi. Queste directory del file system vengono chiamate *contenitori di dati*. I contenitori di dati rappresentano l'interfaccia tra archiviazione nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] e archiviazione nel file system. 

Se si utilizza l'archiviazione FILESTREAM, considerare gli aspetti indicati di seguito:  

- Se una tabella contiene una colonna FILESTREAM, ogni riga deve avere un ID riga univoco con valore diverso da Null.  
- È possibile aggiungere più contenitori di dati a un filegroup FILESTREAM.  
- I contenitori di dati FILESTREAM non possono essere nidificati.  
- Se si utilizza il clustering di failover, i filegroup FILESTREAM devono trovarsi su risorse disco condivise.  
- I filegroup FILESTREAM possono essere su volumi compressi.

### <a name="integrated-management"></a>Gestione integrata

Poiché FILESTREAM viene implementato come colonna **varbinary(max)** e integrato direttamente nel [!INCLUDE[ssDE](../../includes/ssde-md.md)], la maggior parte degli strumenti e delle funzioni di gestione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funziona senza alcuna modifica dei dati FILESTREAM. Ad esempio, è possibile utilizzare tutti i modelli di backup e di recupero con i dati FILESTREAM e per questo viene eseguito il backup con i dati strutturati nel database. Se non si desidera eseguire il backup dei dati FILESTREAM con i dati relazionali, è possibile utilizzare un backup parziale per escludere i filegroup FILESTREAM.  

### <a name="integrated-security"></a>Sicurezza integrata

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i dati FILESTREAM sono protetti così come avviene per gli altri dati, ossia tramite la concessione di autorizzazioni ai livelli di tabella o colonna. Se un utente dispone delle autorizzazioni per la colonna FILESTREAM in una tabella, può aprire i file associati.  

> [!NOTE]
> La crittografia non è supportata sui dati FILESTREAM.  

Solo all'account con il quale viene eseguito l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono concesse autorizzazioni al contenitore FILESTREAM. È consigliabile non concedere altre autorizzazioni per il contenitore di dati ad altri account.

> [!NOTE]
> Gli account di accesso SQL non funzioneranno con contenitori FILESTREAM. Solo l'autenticazione NTFS o ReFS funziona con contenitori FILESTREAM.

## <a name="accessing-blob-data-with-transact-sql-and-file-system-streaming-access"></a><a name="dual"></a> Accesso a dati BLOB con Transact-SQL e accesso tramite flusso al file system

Dopo avere archiviato dati in una colonna FILESTREAM, è possibile accedere ai file usando le transazioni [!INCLUDE[tsql](../../includes/tsql-md.md)] oppure le API Win32.  
  
### <a name="transact-sql-access"></a>Accesso Transact-SQL

Utilizzando [!INCLUDE[tsql](../../includes/tsql-md.md)], è possibile inserire, aggiornare ed eliminare i dati FILESTREAM:  

- È possibile utilizzare un'operazione di inserimento per il popolamento preliminare di un campo FILESTREAM con un valore null, un valore vuoto, o dati inline relativamente brevi. Tuttavia, una grande quantità di dati viene trasmessa in modo più efficace in un file che utilizza interfacce Win32.  
- Quando si aggiorna un campo FILESTREAM, si modificano i dati BLOB sottostanti nel file system. Quando un campo FILESTREAM viene impostato su NULL, i dati BLOB associati al campo vengono eliminati. Non è possibile usare un aggiornamento Chunked di [!INCLUDE[tsql](../../includes/tsql-md.md)] , implementato come UPDATE **.** Write() per eseguire aggiornamenti parziali dei dati. 
- Quando si elimina una riga oppure si elimina o si tronca una tabella che contiene i dati FILESTREAM, si eliminano i dati BLOB sottostanti nel file system.

### <a name="file-system-streaming-access"></a>Accesso di flusso al file system

Il supporto di flusso di Win32 funziona nel contesto di una transazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . All'interno di una transazione, è possibile utilizzare funzioni FILESTREAM per ottenere un percorso di file system UNC logico di un file. Si usa quindi l'API OpenSqlFilestream per ottenere un handle di file. Questo handle può essere usato quindi dalle interfacce di flusso dei file Win32, ad esempio ReadFile() e WriteFile(), per accedere e aggiornare il file con il file system.  

Dato che le operazioni con i file sono transazionali, non è possibile eliminare o rinominare i file FILESTREAM tramite il file system.  

**Modello istruzione**

L'accesso al file system di FILESTREAM consente di modellare un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizzando le operazioni di apertura e chiusura dei file. L'istruzione si avvia quando un handle di file è aperto e termina quando l'handle è chiuso. Ad esempio, se un handle di scrittura è chiuso, qualsiasi possibile trigger AFTER registrato nella tabella si attiva come se un'istruzione UPDATE fosse completata.

**Spazio dei nomi dell'archiviazione**

In FILESTREAM, [!INCLUDE[ssDE](../../includes/ssde-md.md)] consente di controllare lo spazio dei nomi del file system fisico di BLOB. Una nuova funzione intrinseca [PathName](../../relational-databases/system-functions/pathname-transact-sql.md)fornisce il percorso UNC logico di BLOB che corrisponde a ogni cella di FILESTREAM nella tabella. L'applicazione usa questo percorso logico per ottenere l'handle Win32 e operare sui dati BLOB con le normali interfacce del file system Win32. La funzione restituisce NULL se il valore della colonna FILESTREAM è NULL.  

**Accesso al file system transazionale**

Una nuova funzione intrinseca [GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)fornisce il token che rappresenta la transazione corrente alla quale è associata la sessione. È necessario che la transazione sia stata avviata e non sia stata ancora interrotta o non ne sia stato ancora eseguito il commit. Ottenendo un token, l'applicazione associa le operazioni di flusso del file system di FILESTREAM con una transazione avviata. La funzione restituisce NULL se non esistono transazioni esplicitamente avviate.  

Tutti gli handle di file devono essere chiusi prima che la transazione venga interrotta o ne venga eseguito il commit. Se un handle viene lasciato aperto oltre l'ambito della transazione, le letture aggiuntive sull'handle causeranno un errore, mentre le scritture aggiuntive sull'handle riusciranno, ma in realtà i dati non saranno scritti su disco. Allo stesso modo, se il database o l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] si chiude, tutti gli handle aperti non sono più validi.  

**Durabilità delle transazioni**

Con FILESTREAM, su commit della transazione, viene assicurata nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] la durevolezza delle transazioni per i dati BLOB di FILESTREAM modificati dall'accesso di flusso al file system.  

**Semantica dell'isolamento**

La semantica dell'isolamento è gestita dai livelli di isolamento delle transazioni del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Il livello di isolamento Read Committed è supportato per [!INCLUDE[tsql](../../includes/tsql-md.md)] e per l'accesso al file system. Sono supportati le operazioni di lettura ripetibili e gli isolamenti serializzabili e degli snapshot. La lettura dirty non è supportata.  

Le operazioni di apertura dell'accesso al file system non attendono alcun blocco, ma non riescono immediatamente se non possono accedere ai dati a causa dell'isolamento della transazione. Le chiamate API di flusso non riescono con ERROR_SHARING_VIOLATION se l'operazione di apertura non può continuare a causa della violazione dell'isolamento.  

Per consentire l'esecuzione di aggiornamenti parziali, l'applicazione può eseguire un controllo FS del dispositivo (FSCTL_SQL_FILESTREAM_FETCH_OLD_CONTENT) per recuperare il contenuto obsoleto nel file a cui fa riferimento l'handle aperto. In questo modo verrà attivata una copia del contenuto obsoleto sul lato server. Per ottenere migliori prestazioni dell'applicazione ed evitare di causare timeout potenziali quando si utilizzano file molto grandi, si consiglia di utilizzare I/O asincroni.  

Se FSCTL viene eseguito dopo la scrittura sull'handle, l'ultima operazione di scrittura verrà resa persistente e le scritture precedenti eseguite sull'handle andranno perdute.

**API del file system e livelli di isolamento supportati**

Quando una API del file system non può aprire un file a causa di una violazione dell'isolamento, viene restituita un'eccezione ERROR_SHARING_VIOLATION. Si verifica questa violazione dell'isolamento quando due transazioni tentano di accedere allo stesso file. Il risultato dell'operazione di accesso dipende dalla modalità in cui è stato aperto il file e dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene eseguita la transazione. Nella tabella seguente vengono illustrati i possibili risultati se due transazioni accedono allo stesso file.

|Transazione 1|Transazione 2|Risultato in SQL Server 2008|Risultato in SQL Server 2008 R2 e versioni successive|  
|-------------------|-------------------|--------------------------------|------------------------------------------------------|  
|Aperta per la lettura.|Aperta per la lettura.|Riescono entrambe.|Riescono entrambe.|  
|Aperta per la lettura.|Aperta per la scrittura.|Riescono entrambe. Le operazioni di scrittura nella transazione 2 non influiscono sulle operazioni di lettura eseguite nella transazione 1.|Riescono entrambe. Le operazioni di scrittura nella transazione 2 non influiscono sulle operazioni di lettura eseguite nella transazione 1.|  
|Aperta per la scrittura.|Aperta per la lettura.|L'apertura per la transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|Riescono entrambe.|  
|Aperta per la scrittura.|Aperta per la scrittura.|L'apertura per la transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|L'apertura per la transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|  
|Aperta per la lettura.|Aperta per SELECT.|Riescono entrambe.|Riescono entrambe.|  
|Aperta per la lettura.|Aperta per UPDATE o DELETE.|Riescono entrambe. Le operazioni di scrittura nella transazione 2 non influiscono sulle operazioni di lettura eseguite nella transazione 1.|Riescono entrambe. Le operazioni di scrittura nella transazione 2 non influiscono sulle operazioni di lettura eseguite nella transazione 1.|  
|Aperta per la scrittura.|Aperta per SELECT.|La transazione 2 viene bloccata fino al commit o alla fine della transazione 1 o fino a quando non si verifica il timeout del blocco della transazione.|Riescono entrambe.|  
|Aperta per la scrittura.|Aperta per UPDATE o DELETE.|La transazione 2 viene bloccata fino al commit o alla fine della transazione 1 o fino a quando non si verifica il timeout del blocco della transazione.|La transazione 2 viene bloccata fino al commit o alla fine della transazione 1 o fino a quando non si verifica il timeout del blocco della transazione.|  
|Aperta per SELECT.|Aperta per la lettura.|Riescono entrambe.|Riescono entrambe.|  
|Aperta per SELECT.|Aperta per la scrittura.|Riescono entrambe. Le operazioni di scrittura nella transazione 2 non interessano la transazione 1.|Riescono entrambe. Le operazioni di scrittura nella transazione 2 non interessano la transazione 1.|  
|Aperta per UPDATE o DELETE.|Aperta per la lettura.|L'operazione di apertura nella transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|Riescono entrambe.|  
|Aperta per UPDATE o DELETE.|Aperta per la scrittura.|L'operazione di apertura nella transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|L'operazione di apertura nella transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|  
|Aperta per SELECT con lettura ripetibile.|Aperta per la lettura.|Riescono entrambe.|Riescono entrambe.|  
|Aperta per SELECT con lettura ripetibile.|Aperta per la scrittura.|L'operazione di apertura nella transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|L'operazione di apertura nella transazione 2 non riesce con eccezione ERROR_SHARING_VIOLATION.|

**Write-through da client remoti**

L'accesso remoto al file system per i dati FILESTREAM è abilitato tramite il protocollo SMB (Server Message Block). Se il client è remoto, nella cache non vengono memorizzate operazioni di scrittura lato client. Le operazioni di scrittura saranno inviate sempre al server. I dati possono essere memorizzati nella cache sul lato server. Si consiglia di far consolidare alle applicazioni eseguite sui client remoti piccole operazioni di scrittura per effettuarne un numero minore con dati di dimensioni superiori.  

La creazione di viste con mapping alla memoria (I/O con mapping alla memoria) utilizzando handle FILESTREAM non è supportata. Se il mapping di memoria viene usato per i dati FILESTREAM, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] non può garantire coerenza e durabilità dei dati o l'integrità del database.  

## <a name="recommendations-and-guidelines-for-improving-filestream-performance"></a>Indicazioni e linee guida per migliorare le prestazioni di FILESTREAM

La SQL Server funzionalità FILESTREAM consente di archiviare dati di oggetti binari di grandi dimensioni varbinary (max) come file nel file system. Quando si dispone di un numero elevato di righe nei contenitori FILESTREAM, ovvero l'archiviazione sottostante sia per le colonne FILESTREAM sia per le tabelle FileTable, è possibile ottenere un volume file system contenente un numero elevato di file. Per ottenere prestazioni ottimali durante l'elaborazione dei dati integrati dal database e dal file system, è importante assicurarsi che la file system sia ottimizzata in modo ottimale. Di seguito sono riportate alcune opzioni di ottimizzazione disponibili in una file system prospettiva:

- Verifica dell'altitudine per il driver del filtro FILESTREAM SQL Server [ad esempio rsfx0100.sys]. Valutare tutti i driver di filtro caricati per lo stack di archiviazione associato a un volume in cui la funzionalità FILESTREAM archivia i file e verificare che il driver RsFx si trovi nella parte inferiore dello stack. È possibile utilizzare il programma di controllo FLTMC.EXE per enumerare i driver di filtro per un volume specifico. Di seguito è riportato un esempio di output dell'utilità FLTMC: `C:\Windows\System32>fltMC.exe` filtri

    |Nome filtro|Numero di istanze|Altitudine|Frame|
    |---|---|---|---|
    |Sftredir|1|406000|0|
    |MpFilter|9|328000|0|
    |luafv|1|135000|0|
    |FileInfo|9|45000|0|
    |RsFx0103|1|41001,03|0|
    ||||

- Verificare che per il server sia stata disabilitata la proprietà "ora ultimo accesso" per i file. Questo attributo file system viene mantenuto nel registro di sistema:  
Nome chiave: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem`  
Nome: NtfsDisableLastAccessUpdate  
Tipo: REG_DWORD  
Valore: 1

- Verificare che il server abbia la denominazione 8,3 disabilitata. Questo attributo file system viene mantenuto nel registro di sistema:  
Nome chiave: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem`  
Nome: NtfsDisable8dot3NameCreation  
Tipo: REG_DWORD  
Valore: 1

- Verificare che i contenitori di directory FILESTREAM non dispongano della crittografia file system o della compressione file system abilitata, in quanto questi possono introdurre un livello di overhead durante l'accesso a tali file.

- Da un prompt dei comandi con privilegi elevati eseguire le istanze di fltmc e assicurarsi che nessun driver di filtro sia collegato al volume in cui si tenta di eseguire il ripristino.

- Verificare che i contenitori di directory FILESTREAM non contengano più di 300.000 file. È possibile utilizzare le informazioni disponibili `sys.database_files` nella vista del catalogo per individuare le directory presenti nei file di archivio file System `FILESTREAM-related` . Questa operazione può essere impedita dalla presenza di più contenitori. Per ulteriori informazioni, vedere l'elemento Bullet successivo.

- Con un solo filegroup FILESTREAM, tutti i file di dati vengono creati nella stessa cartella. La creazione di file di un numero molto elevato di file può essere influenzata da indici NTFS di grandi dimensioni, che possono anche diventare frammentati.

  - La presenza di più filegroup in genere dovrebbe risultare utile (l'applicazione utilizza il partizionamento o dispone di più tabelle, ognuna delle quali passa al proprio filegroup).

  - Con SQL Server 2012 e versioni successive, è possibile disporre di più contenitori o file in un filegroup FILESTREAM e viene applicato uno schema di allocazione round robin. Il numero di file NTFS per directory sarà pertanto minore.

- Il backup e il ripristino possono diventare più veloci con più contenitori FILESTREAM, se vengono usati più volumi che archiviano i contenitori.

    SQL Server 2012 supporta più contenitori per filegroup e può semplificare notevolmente le operazioni. Non è possibile che siano necessari schemi di partizionamento complessi per gestire un numero maggiore di file.

- Il MFT NTFS potrebbe diventare frammentato e ciò può causare problemi di prestazioni. Le dimensioni riservate MFT dipendono dalle dimensioni del volume, quindi è possibile che si verifichi questo problema.

  - È possibile controllare la frammentazione di MFT con `defrag /A /V C:` (modificare C: con il nome effettivo del volume).

  - È possibile riservare più spazio MFT usando fsutil behavior set mftzone 2.

  - I file di dati FILESTREAM devono essere esclusi dall'analisi del software antivirus.

    > [!NOTE]
    > Windows Server 2016 Abilita automaticamente Windows Defender. Assicurarsi che Windows Defender sia configurato in modo da escludere i file FILESTREAM. In caso contrario, è possibile che si verifichi una riduzione delle prestazioni per le operazioni di backup e ripristino.
  
    Per altre informazioni, vedere [configurare e convalidare le esclusioni per le analisi di Windows Defender Antivirus](/windows/security/threat-protection/microsoft-defender-antivirus/configure-exclusions-microsoft-defender-antivirus).

## <a name="related-tasks"></a>Attività correlate

[Abilitare e configurare FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)  
[Creazione di un database abilitato per FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md)  
[Creazione di una tabella per archiviare dati FILESTREAM](../../relational-databases/blob/create-a-table-for-storing-filestream-data.md)  
[Accedere a dati FILESTREAM con Transact-SQL](../../relational-databases/blob/access-filestream-data-with-transact-sql.md) [Creare applicazioni client per dati FILESTREAM](../../relational-databases/blob/create-client-applications-for-filestream-data.md)  
[Accesso ai dati FILESTREAM con OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
[Esecuzione di aggiornamenti parziali di dati FILESTREAM](../../relational-databases/blob/make-partial-updates-to-filestream-data.md)  
[Evitare conflitti con le operazioni del database nelle applicazioni di FILESTREAM](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)  
[Spostamento di un database abilitato per FILESTREAM](../../relational-databases/blob/move-a-filestream-enabled-database.md)  
[Configurazione di FILESTREAM in un cluster di failover](../../relational-databases/blob/set-up-filestream-on-a-failover-cluster.md)  
[Configurare un firewall per l'accesso FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)

## <a name="related-content"></a>Contenuto correlato

[Compatibilità FILESTREAM con altre funzionalità di SQL Server](../../relational-databases/blob/filestream-compatibility-with-other-sql-server-features.md)
<br>[DMV per FILESTREAM e tabelle FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Viste del catalogo Filestream e FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Stored procedure di sistema per Filestream e tabelle FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)

