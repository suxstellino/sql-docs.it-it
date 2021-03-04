---
title: Always Encrypted con enclave sicuri
description: Informazioni su Always Encrypted con enclave sicure per SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: cbec7eb15ed8acad746e3fcb2013ee8100bcdd26
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837701"
---
# <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted con enclave sicuri

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Always Encrypted con enclave sicure espande le funzionalità di confidential computing di [Always Encrypted](always-encrypted-database-engine.md) abilitando la crittografia sul posto e query riservate più avanzate. Always Encrypted con enclave sicure è disponibile in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] e in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] (in anteprima).

Introdotta in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] nel 2015 e in [!INCLUDE[sssql16](../../../includes/sssql16-md.md)], la funzionalità Always Encrypted protegge la riservatezza dei dati sensibili da malware e utenti con privilegi elevati *non autorizzati*: amministratori di database, amministratori di computer, amministratori di cloud o altri utenti che possono accedere in modo legittimo a istanze del server, hardware e così via, ma che non dovrebbero avere accesso ad alcuni o a tutti i dati effettivi.  

Senza i miglioramenti illustrati in questo articolo, Always Encrypted protegge i dati crittografandoli sul lato client e non consentendo *mai* ai dati o alle chiavi di crittografia corrispondenti di essere visualizzate come testo non crittografato all'interno del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. Di conseguenza, le funzionalità per le colonne crittografate all'interno del database sono soggette a notevoli restrizioni. Le uniche operazioni che [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] può eseguire sui dati crittografati sono i confronti di uguaglianza (disponibili solo con la [crittografia deterministica](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)). Tutte le altre operazioni, tra cui le operazioni di crittografia (crittografia iniziale dei dati o rotazione delle chiavi) e le query più avanzate (ad esempio, criteri di ricerca), non sono supportate all'interno del database. Gli utenti devono spostare i dati all'esterno del database per eseguire queste operazioni sul lato client.

Always Encrypted *con enclave sicure* supera queste limitazioni, consentendo l'esecuzione di alcuni calcoli su dati di testo non crittografato all'interno di un'enclave sicura sul lato server. Un'enclave sicura è un'area protetta della memoria all'interno del processo del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. L'enclave sicura viene visualizzata come black box per il resto di [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] e altri processi nel computer host. Non vi è alcun modo per visualizzare dati o codice all'interno dell'enclave dall'esterno, anche con un debugger. Queste proprietà rendono l'enclave sicura un *ambiente di esecuzione attendibile* che può accedere in modo sicuro alle chiavi crittografiche e ai dati sensibili in testo non crittografato, senza compromettere la riservatezza dei dati.

Always Encrypted usa gli enclave sicuri come illustrato nel diagramma seguente:

![flusso di dati](./media/always-encrypted-enclaves/ae-data-flow.png)

Durante l'analisi di un'istruzione Transact-SQL inviata da un'applicazione, il [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] determina se l'istruzione contiene operazioni su dati crittografati che richiedono l'uso dell'enclave sicura. Per tali istruzioni:

- Il driver client invia le chiavi di crittografia della colonna necessarie per le operazioni all'enclave sicura (tramite un canale sicuro) e invia l'istruzione per l'esecuzione.

- Durante l'elaborazione dell'istruzione, il [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] delega le operazioni di crittografia o i calcoli sulle colonne crittografate all'enclave sicura. Se necessario, l'enclave decrittografa i dati ed esegue i calcoli in testo non crittografato.

Durante l'elaborazione dell'istruzione, sia i dati che le chiavi di crittografia della colonna non vengono esposte come testo non crittografato nel motore [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] al di fuori dell'enclave sicura.

## <a name="supported-enclave-technologies"></a>Tecnologie di enclave supportate

In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] Always Encrypted con enclave sicuri usa gli enclave di memoria sicuri di tipo [sicurezza basata su virtualizzazione](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/) in Windows, noti anche come enclave in modalità protetta virtuale o VSM.

In [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], Always Encrypted con enclave sicure usa le enclave [Intel Software Guard Extensions (Intel SGX)](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/). Intel SGX è una tecnologia ambientale di esecuzione attendibile basata su hardware supportata nei database che usano la configurazione hardware della [serie DC](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series).

## <a name="secure-enclave-attestation"></a>Attestazione delle enclave sicure

L'enclave sicura all'interno del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] può accedere ai dati sensibili e alle chiavi di crittografia della colonna in testo non crittografato. Pertanto, prima di inviare al [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] un'istruzione che comporta calcoli dell'enclave, il driver client all'interno dell'applicazione deve verificare che l'enclave sicura sia una vera enclave con sicurezza basata su virtualizzazione o SGX e che il codice in esecuzione nell'enclave sicura sia la libreria di Always Encrypted originale che implementa algoritmi di crittografia di Always Encrypted per la crittografia sul posto e le operazioni supportate nelle query riservate.

Il processo di verifica dell'enclave è chiamato **attestazione dell'enclave** e implica che un driver client all'interno dell'applicazione e [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] contattino un servizio di attestazione esterno. I dettagli specifici del processo di attestazione variano a seconda del tipo dell'enclave (sicurezza basata su virtualizzazione o SGX) e del servizio di attestazione.

Il processo di attestazione per le enclave sicure con sicurezza basata su virtualizzazione in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] è l'[attestazione di runtime di System Guard di Windows Defender](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/), che richiede il servizio Sorveglianza host come servizio di attestazione. 

L'attestazione delle enclave di Intel SGX nel [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] richiede l'[attestazione di Microsoft Azure](/azure/attestation/overview).

> [!NOTE]
> [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] non supporta l'attestazione di Microsoft Azure. Il servizio Sorveglianza host è l'unica soluzione di attestazione supportata per le enclave con sicurezza basata su virtualizzazione in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

## <a name="supported-client-drivers"></a>Driver client supportati

Per usare Always Encrypted con enclave sicuri, un'applicazione deve usare un driver client che supporta la funzionalità. Configurare l'applicazione e il driver client per abilitare i calcoli dell'enclave e l'attestazione dell'enclave. Per informazioni dettagliate, incluso l'elenco dei driver client supportati, vedere [Sviluppare applicazioni con Always Encrypted](always-encrypted-client-development.md).

## <a name="terminology"></a>Terminologia

### <a name="enclave-enabled-keys"></a>Chiavi abilitate per l'enclave

Always Encrypted con enclave sicuri introduce il concetto di chiavi abilitate per l'enclave:

- **Chiave master di colonna abilitata per l'enclave** - Chiave master della colonna con la proprietà `ENCLAVE_COMPUTATIONS` specificata nell'oggetto metadati chiave master della colonna all'interno del database. L'oggetto metadati chiave master della colonna deve anche contenere una firma valida delle proprietà dei metadati. Per altre informazioni, vedere [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md).
- **Chiave di crittografia di colonna abilitata per l'enclave** - Chiave di crittografia di colonna crittografata con una chiave master della colonna abilitata per l'enclave. Solo le chiavi di crittografia di colonna abilitata per l'enclave possono essere usate per i calcoli all'interno dell'enclave sicura. 

Per altre informazioni, vedere [Gestire le chiavi per Always Encrypted con enclave sicuri](always-encrypted-enclaves-manage-keys.md).

### <a name="enclave-enabled-columns"></a>Colonne abilitate per l'enclave

Una colonna abilitata per l'enclave è una colonna del database crittografata con una chiave di crittografia di colonna abilitata per l'enclave.

## <a name="confidential-computing-capabilities-for-enclave-enabled-columns"></a>Funzionalità di confidential computing per le colonne abilitate per l'enclave

I due principali vantaggi di Always Encrypted con le enclave sicure sono la crittografia sul posto e le query riservate avanzate.

### <a name="in-place-encryption"></a>Crittografia sul posto

La crittografia sul posto consente di eseguire operazioni crittografiche sulle colonne del database all'interno dell'enclave sicura, senza spostare i dati all'esterno del database. La crittografia sul posto migliora le prestazioni e l'affidabilità della crittografia. È possibile eseguire la crittografia sul posto usando l'istruzione [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md). 

Le operazioni crittografiche supportate sul posto sono:

- Crittografia di una colonna di testo non crittografato con una chiave di crittografia della colonna abilitata per l'enclave.
- Riapplicazione della crittografia a una colonna crittografata abilitata per l'enclave per:
  - Ruotare una chiave di crittografia della colonna: crittografare nuovamente la colonna con una nuova chiave di crittografia della colonna abilitata per l'enclave.
  - Cambiare il tipo di crittografia di una colonna abilitata per l'enclave, ad esempio da deterministica a casuale.
- Decrittografia dei dati archiviati in una colonna abilitata per l'enclave (conversione della colonna in colonna di testo non crittografato).

La crittografia sul posto è consentita sia con la crittografia deterministica che con quella casuale, purché le chiavi di crittografia della colonna usate in un'operazione di crittografia siano abilitate per l'enclave.

### <a name="confidential-queries"></a>Query riservate

Le query riservate sono [query DML](../../../t-sql/queries/queries.md) che comportano operazioni sulle colonne abilitate per l'enclave, eseguite all'interno dell'enclave sicura.

Le operazioni supportate all'interno delle enclave sicure sono:

| Operazione| [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] | [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] |
|:---|:---|:---|
| [Operatori di confronto](../../../mdx/comparison-operators.md) | Supportato | Supportato |
| [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md) | Supportato | Supportato |
| [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md) | Supportato | Supportato |
| [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md) | Supportato | Supportato |
| [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select) | Supportato | Supportato |
| [Join](../../performance/joins.md) | Sono supportati solo i join annidati di cicli | Supportato |
| [Clausola SELECT - ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md) | Non supportato | Supportato |
| [SELECT - GROUP BY- Transact-SQL](../../../t-sql/queries/select-group-by-transact-sql.md) | Non supportato | Supportato |

> [!NOTE]
> Le operazioni precedenti sono supportate nelle enclave sicure solo sulle colonne abilitate per le enclave che usano la crittografia **casuale** e non la crittografia deterministica. L'operatore per il confronto di uguaglianza rimane l'unico operatore di calcolo supportato sulle colonne che usano la crittografia deterministica e viene eseguito tramite il confronto del testo crittografato al di fuori dell'enclave, indipendentemente dal fatto che la colonna sia abilitata o meno per l'enclave. La crittografia deterministica supporta le operazioni seguenti che usano gli operatori per il confronto di uguaglianza: 
> - [= (uguale a)](../../../t-sql/language-elements/equals-transact-sql.md) nella ricerca di punti, nelle ricerche e nei join
> - [IN](../../../t-sql/language-elements/in-transact-sql.md)
> - [SELECT - GROUP BY](../../../t-sql/queries/select-group-by-transact-sql.md)
> - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
>
> In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], le query riservate che usano le enclave su una colonna di stringhe di caratteri (`char`, `nchar`) richiedono che la colonna usi regole di confronto binary2 (BIN2) per l'ordinamento. In [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], le query riservate sulle stringhe di caratteri richiedono regole di confronto BIN2 o UTF-8. 

### <a name="indexes-on-enclave-enabled-columns"></a>Indici sulle colonne abilitate per l'enclave

È possibile creare indici non cluster sulle colonne abilitate per l'enclave tramite la crittografia casuale per velocizzare l'esecuzione delle query DML riservate che usano l'enclave sicura.

Per garantire che un indice su una colonna crittografata tramite la crittografia casuale non determini una perdita di dati sensibili, i valori di chiave nella struttura dei dati di indice (albero B) sono crittografati e ordinati in base ai relativi valori di testo non crittografato. L'ordinamento in base al valore di testo non crittografato è utile anche per l'elaborazione di query all'interno dell'enclave. Quando l'utilità di esecuzione query nel [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] usa un indice su una colonna crittografata per i calcoli all'interno dell'enclave, esegue una ricerca nell'indice dei valori specifici archiviati nella colonna. Ogni ricerca può implicare più confronti. L'utilità di esecuzione query delega ogni confronto all'enclave, che decrittografa un valore archiviato nella colonna e il valore della chiave di indice crittografata da confrontare, esegue il confronto in testo non crittografato e restituisce il risultato del confronto all'utilità di esecuzione.

La creazione di indici su colonne che usano la crittografia casuale e non sono abilitate per l'enclave non è supportata.

Un indice su una colonna con crittografia deterministica è ordinato in base al testo crittografato (non al testo non crittografato), indipendentemente dal fatto che la colonna sia abilitata per l'enclave o meno.

Per altre informazioni, vedere [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md). Per informazioni generali sul funzionamento dell'indicizzazione nel [!INCLUDE[ssde-md](../../../includes/ssde-md.md)], vedere l'articolo [Descrizione di indici cluster e non cluster](../../indexes/clustered-and-nonclustered-indexes-described.md).

### <a name="database-recovery"></a>Ripristino del database

Se si verifica un errore in un'istanza di SQL Server, i relativi database possono rimanere in uno stato in cui i file di dati possono contenere alcune modifiche da transazioni incomplete. Quando l'istanza viene avviata, viene eseguito un processo denominato [ripristino del database](../../logs/the-transaction-log-sql-server.md#recovery-of-all-incomplete-transactions-when--is-started), che implica il rollback di tutte le transazioni incomplete rilevate nel log delle transazioni per salvaguardare l'integrità del database. Se una transazione incompleta ha apportato modifiche a un indice, anche tali modifiche devono essere annullate. Ad esempio, potrebbe essere necessario rimuovere o reinserire alcuni valori di chiave nell'indice.

> [!IMPORTANT]
> Microsoft consiglia di abilitare il [ripristino accelerato del database](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) nel database **prima** di creare il primo indice su una colonna abilitata per l'enclave crittografata con la crittografia casuale. Il ripristino accelerato del database è abilitato per impostazione predefinita nel [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], ma non in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

Con il [tradizionale processo di ripristino del database](/azure/sql-database/sql-database-accelerated-database-recovery#the-current-database-recovery-process) (che segue il modello di ripristino [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf)), per annullare una modifica a un indice, SQL Server deve attendere che un'applicazione fornisca all'enclave la chiave di crittografia della colonna, operazione che può richiedere molto tempo. Il ripristino accelerato del database riduce notevolmente il numero di operazioni di annullamento che devono essere rinviate perché una chiave di crittografia della colonna non è disponibile nella cache all'interno dell'enclave. Di conseguenza, incrementa notevolmente la disponibilità del database, riducendo al minimo la possibilità che una nuova transazione resti bloccata. Con il ripristino accelerato del database abilitato, SQL Server potrebbe comunque richiedere una chiave di crittografia della colonna per completare la pulizia delle versioni precedenti dei dati, ma esegue tale operazione come un'attività in background che non influisce sulla disponibilità delle transazioni del database o degli utenti. Potrebbero tuttavia essere visualizzati messaggi di errore nel log degli errori, che indicano operazioni di pulizia non riuscite a causa di una chiave di crittografia della colonna mancante.

## <a name="security-considerations"></a>Considerazioni relative alla sicurezza

Le considerazioni sulla sicurezza seguenti si applicano ad Always Encrypted con enclave sicuri.

- La sicurezza dei dati all'interno dell'enclave dipende da un protocollo di attestazione e un servizio di attestazione. È pertanto necessario assicurarsi che il servizio di attestazione e i criteri di attestazione applicati da tale servizio vengano gestiti da un amministratore attendibile. Inoltre, i servizi di attestazione in genere supportano diversi criteri e protocolli di attestazione, alcuni dei quali eseguono una verifica minima dell'enclave e del relativo ambiente e sono progettati per il test e lo sviluppo. Seguire attentamente le linee guida specifiche per il servizio di attestazione in uso, in modo da assicurarsi di usare le configurazioni e i criteri consigliati per le distribuzioni di produzione. 
- La crittografia di una colonna tramite la crittografia casuale con una chiave di crittografia della colonna abilitata per l'enclave può comportare la divulgazione dell'ordine dei dati archiviati nella colonna, in quanto tali colonne supportano i confronti degli intervalli. Ad esempio, se una colonna crittografata che contiene gli stipendi dei dipendenti dispone di un indice, un amministratore di database malintenzionato potrebbe analizzare l'indice per trovare il valore crittografato dello stipendio massimo e identificare la persona con lo stipendio massimo (presupponendo che il nome della persona non sia crittografato). 
- Se si usa Always Encrypted per proteggere i dati sensibili dall'accesso non autorizzato da parte degli amministratori di database, non condividere le chiavi master della colonna o le chiavi di crittografia della colonna con gli amministratori di database. Un amministratore di database può gestire gli indici nelle colonne crittografate senza avere accesso diretto alle chiavi, sfruttando la cache delle chiavi di crittografia di colonna all'interno dell'enclave.

## <a name="considerations-for-business-continuity-disaster-recovery-and-data-migration"></a><a name="anchorname-1-considerations-availability-groups-db-migration"></a> Considerazioni su continuità aziendale, ripristino di emergenza e migrazione dei dati

Quando si configura una soluzione di disponibilità elevata o di ripristino di emergenza per un database che usa Always Encrypted con enclave sicure, verificare che tutte le repliche del database possano usare un'enclave sicura. Se è disponibile un'enclave per la replica primaria, ma non per la replica secondaria, eventuali istruzioni che provano a usare la funzionalità di Always Encrypted con enclave sicure avranno esito negativo dopo il failover.

Quando si esegue la copia o la migrazione di un database che usa Always Encrypted con enclave sicure, verificare che l'ambiente di destinazione supporti sempre le enclave. In caso contrario, le istruzioni che usano le enclave non funzioneranno sulla copia o sul database di cui è stata eseguita la migrazione.

Occorre tenere presenti queste considerazioni specifiche:

- **SQL Server**
  - Quando si configura un [gruppo di disponibilità Always On](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md), assicurarsi che ogni istanza di SQL Server che ospita un database nel gruppo di disponibilità supporti Always Encrypted con enclave sicure e che siano state configurate un'enclave e un'attestazione.
  - Quando si ripristina un file di backup di un database che usa la funzionalità Always Encrypted con enclave sicure in un'istanza di SQL Server in cui non è configurata l'enclave, l'operazione di ripristino avrà esito positivo e tutte le funzionalità che non usano l'enclave saranno disponibili. Tuttavia, tutte le istruzioni successive che usano la funzionalità dell'enclave avranno esito negativo e gli indici sulle colonne abilitate per l'enclave che usano la crittografia casuale non saranno più validi. Lo stesso vale quando si collega un database con Always Encrypted con enclave sicure nell'istanza in cui non è configurata l'enclave.
  - Se il database include indici sulle colonne abilitate per l'enclave con crittografia casuale, assicurarsi di abilitare il [ripristino accelerato del database](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) nel database prima di creare un backup del database. Il ripristino accelerato del database garantirà che il database, inclusi gli indici, sarà immediatamente disponibile dopo il ripristino del database. Per altre informazioni, vedere [Ripristino del database](#database-recovery).
  
- **Database SQL di Azure**
  - Quando si configura la [replica geografica attiva](/azure/azure-sql/database/active-geo-replication-overview), verificare che un database secondario supporti le enclave sicure, se il database primario le supporta.

Sia in SQL Server che nel database SQL di Azure, quando si esegue la migrazione del database tramite un file con estensione bacpac, è necessario accertarsi di eliminare tutti gli indici per le colonne abilitate per l'enclave con crittografia casuale prima di creare il file con estensione bacpac.

## <a name="known-limitations"></a>Limitazioni note

Always Encrypted con enclave sicure risolve alcune limitazioni di Always Encrypted supportando la crittografia sul posto e query riservate più avanzate con indici, come illustrato in [Funzionalità di confidential computing per le colonne abilitate per l'enclave](#confidential-computing-capabilities-for-enclave-enabled-columns).

Tutte le altre limitazioni per Always Encrypted elencate in [Informazioni sulle funzionalità](always-encrypted-database-engine.md#feature-details) si applicano anche ad Always Encrypted con enclave sicure.

Le limitazioni seguenti sono specifiche per Always Encrypted con enclave sicuri:

- Non è possibile creare indici cluster sulle colonne abilitate per l'enclave che usano la crittografia casuale.
- Le colonne abilitate per l'enclave che usano la crittografia casuale non possono essere colonne chiave primaria e non è possibile farvi riferimento da vincoli di chiave esterna o vincoli di chiave univoca.
- In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] (questa limitazione non si applica al [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]) solo i join a cicli annidati (tramite indici, se disponibili) sono supportati nelle colonne abilitate per l'enclave che usano la crittografia casuale. Per informazioni su altre differenze tra i diversi prodotti, vedere [Query riservate](#confidential-queries).
- Non è possibile combinare le operazioni di crittografia sul posto con qualsiasi altra modifica dei metadati di colonna, ad eccezione della modifica delle regole di confronto all'interno della stessa tabella codici e del supporto dei valori Null. Ad esempio, non è possibile crittografare, crittografare nuovamente o decrittografare una colonna E modificare un tipo di dati della colonna in un'unica istruzione Transact-SQL `ALTER TABLE`/`ALTER COLUMN`. Usare due istruzioni separate.
- L'uso di chiavi abilitate per l'enclave per le colonne in tabelle in memoria non è supportato.
- Le espressioni che definiscono le colonne calcolate non possono eseguire alcun calcolo sulle colonne abilitate per le enclave usando la crittografia casuale (anche se i calcoli sono tra le operazioni supportate elencate in [Query riservate](#confidential-queries)).
- I caratteri di escape non sono supportati nei parametri dell'operatore LIKE nelle colonne abilitate per l'enclave che usano la crittografia casuale.
- Le query con l'operatore LIKE o un operatore di confronto con un parametro di query che usa uno dei seguenti tipi di dati (che dopo la crittografia, diventano oggetti di grandi dimensioni) ignorano gli indici ed eseguono scansioni di tabella.
  - `nchar[n]` e `nvarchar[n]`, se n è maggiore di 3967.
  - `char[n]`, `varchar[n]`, `binary[n]`, `varbinary[n]`, se n è maggiore di 7935.
- Limitazioni degli strumenti:
  - Gli unici archivi di chiavi supportati per l'archiviazione di chiavi master della colonna abilitate per l'enclave sono Archivio certificati Windows e Azure Key Vault.
  - L'importazione e l'esportazione di database che contengono chiavi abilitate per l'enclave non sono supportate.
  - Per attivare un'operazione di crittografia sul posto tramite `ALTER TABLE`/`ALTER COLUMN`, è necessario eseguire l'istruzione in una finestra di query in SSMS oppure è possibile scrivere un programma personalizzato che esegue l'istruzione. Il cmdlet Set-SqlColumnEncryption nel modulo SqlServer di PowerShell e la procedura guidata Always Encrypted in SQL Server Management Studio attualmente non supportano la crittografia sul posto, ma spostano i dati fuori dal database per le operazioni di crittografia, anche se le chiavi di crittografia di colonna usate per le operazioni sono abilitate per l'enclave.

## <a name="next-steps"></a>Passaggi successivi

- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Configurare e usare Always Encrypted con enclave sicuri](configure-always-encrypted-enclaves.md)

## <a name="see-also"></a>Vedere anche

- [Gestire le chiavi per Always Encrypted con enclave sicuri](always-encrypted-enclaves-manage-keys.md)