---
title: Procedure consigliate per Query Store | Microsoft Docs
description: Informazioni sulle procedure consigliate per l'uso di SQL Server Query Store con il carico di lavoro, ad esempio l'uso delle versioni più recenti di SQL Server Management Studio e Informazioni dettagliate prestazioni query.
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, best practices
ms.assetid: 5b13b5ac-1e4c-45e7-bda7-ebebe2784551
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83985f8cb34b9efa8485f9827dd9692a38f91a65
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100344796"
---
# <a name="best-practices-with-query-store"></a>Procedure consigliate per Query Store

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Questo articolo descrive le procedure consigliate per l'uso di SQL Server Query Store con un carico di lavoro.

## <a name="use-the-latest-ssmanstudiofull"></a><a name="SSMS"></a> Usare la versione più recente di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ha un set di interfacce utente progettate per la configurazione di Query Store e l'utilizzo dei dati raccolti relativi al carico di lavoro. Scaricare da [qui](../../ssms/download-sql-server-management-studio-ssms.md) la versione più recente di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

Per una rapida descrizione di come usare Query Store in scenari di risoluzione dei problemi, vedere i post relativi a [Query Store nei blog di @Azure](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

## <a name="use-query-performance-insight-in-azure-sql-database"></a><a name="Insight"></a> Usare Informazioni dettagliate prestazioni query nel database SQL di Azure

Se si esegue Query Store nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], è possibile usare [Informazioni dettagliate prestazioni query](/azure/sql-database/sql-database-query-performance) per analizzare il consumo delle risorse nel tempo. Anche se è possibile usare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) per ottenere il consumo dettagliato delle risorse per tutte le query, ad esempio CPU, memoria e I/O, Informazioni dettagliate prestazioni query offre un metodo rapido ed efficace per determinare l'impatto delle query sul consumo di DTU complessivo per il database. Per altre informazioni, vedere l'articolo relativo a [Informazioni dettagliate prestazioni query del database SQL di Azure](/azure/azure-sql/database/query-performance-insight-use).

Questa sezione descrive impostazioni di configurazione predefinite ottimali progettate per garantire un funzionamento affidabile di Query Store e delle funzionalità dipendenti. La configurazione predefinita è ottimizzata per la raccolta di dati continua, ossia per un tempo minimo di OFF/READ_ONLY. Per altre informazioni su tutte le opzioni di Query Store disponibili, vedere [Opzioni ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store).

| Configurazione | Descrizione | Predefinito | Comment |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Specifica il limite per lo spazio dati che Query Store occupa all'interno del database del cliente |100 |Applicato per i nuovi database |
| INTERVAL_LENGTH_MINUTES |Definisce la dimensione dell'intervallo di tempo durante il quale le statistiche di runtime raccolte per i piani di query vengono aggregate e rese persistenti. Tutti i piani di query attivi hanno al massimo una riga per un periodo di tempo definito con questa configurazione |60 |Applicato per i nuovi database |
| STALE_QUERY_THRESHOLD_DAYS |Criterio di pulizia basato sul tempo che controlla il periodo di memorizzazione delle statistiche di runtime persistenti e delle query inattive |30 |Applicato per i nuovi database e i database con un'impostazione predefinita precedente (367) |
| SIZE_BASED_CLEANUP_MODE |Specifica se la pulizia automatica dei dati viene eseguita quando la dimensione dati dell'archivio query si avvicina al limite |AUTO |Applicato per tutti i database |
| QUERY_CAPTURE_MODE |Specifica se vengono monitorate tutte le query o solo un sottoinsieme di esse |AUTO |Applicato per tutti i database |
| FLUSH_INTERVAL_SECONDS |Specifica il periodo massimo durante il quale le statistiche di runtime acquisite vengono mantenute in memoria prima di essere scaricate su disco |900 |Applicato per i nuovi database |
| | | | |

> [!IMPORTANT]
> I valori predefiniti vengono applicati automaticamente nella fase finale dell'attivazione di Query Store in tutti i [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Dopo l'abilitazione, il [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non modificherà i valori di configurazione impostati dai clienti, a meno che non abbiano un impatto negativo sul carico di lavoro primario o sulle operazioni affidabili di Query Store.

> [!NOTE]  
> Non è possibile disabilitare Query Store in database singolo [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e in un pool elastico. L'esecuzione di `ALTER DATABASE [database] SET QUERY_STORE = OFF` restituirà l'avviso `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

Se si desidera mantenere le impostazioni personalizzate, usare [ALTER DATABASE con le opzioni dell'archivio query](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store) per riportare la configurazione allo stato precedente. Vedere [Procedure consigliate per Query Store](../../relational-databases/performance/best-practice-with-the-query-store.md) per informazioni su come scegliere i parametri di configurazione ottimali.

## <a name="use-query-store-with-elastic-pool-databases"></a>Usare Query Store con database di pool elastici

È possibile usare Archivio query in tutti i database, anche in pool molto compressi. Tutti i problemi correlati all'utilizzo eccessivo delle risorse che possono essersi verificati quando Query Store era abilitato per un numero elevato di database nei pool elastici sono stati risolti.

## <a name="keep-query-store-adjusted-to-your-workload"></a><a name="Configure"></a> Adattare Query Store al proprio carico di lavoro

Configurare l'archivio query in base al carico di lavoro e ai requisiti di risoluzione dei problemi di prestazioni.
I parametri predefiniti sono sufficienti per iniziare, ma è opportuno monitorare il comportamento di Query Store nel tempo e adattare la configurazione di conseguenza.

 ![Proprietà di Query Store](../../relational-databases/performance/media/query-store-properties.png "query-store-properties")

 Di seguito sono riportate alcune linee guida per l'impostazione dei valori dei parametri:

**Dimensioni massime (MB)** : specifica il limite per lo spazio dati che Query Store occupa all'interno del database. Si tratta dell'impostazione più importante, che influisce direttamente sulla modalità di funzionamento di Query Store.

Mentre Query Store raccoglie query, piani di esecuzione e statistiche, le sue dimensioni nel database aumentano fino a quando non viene raggiunto questo limite. A quel punto, l'archivio query passa automaticamente alla modalità operativa di sola lettura e smette di raccogliere nuovi dati. Questo si riflette negativamente sull'accuratezza dell'analisi delle prestazioni.

Il valore predefinito in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] è 100 MB. Queste dimensioni potrebbero non essere sufficienti se il carico di lavoro genera un numero elevato di query e piani diversi o se si vuole conservare la cronologia delle query per un periodo di tempo più lungo. A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il valore predefinito è 1 GB. Tenere traccia dell'utilizzo dello spazio e aumentare le **Dimensioni massime (MB)** per impedire che Query Store passi alla modalità di sola lettura.

> [!IMPORTANT]
> Il limite **Dimensioni massime (MB)** non è necessariamente applicato. Le dimensioni di archiviazione vengono controllate solo quando Query Store scrive i dati su disco. Questo intervallo viene impostato dall'opzione **Intervallo di scaricamento dati (minuti)** . Se Query Store ha violato il limite di dimensioni massime tra i controlli delle dimensioni di archiviazione, passa alla modalità di sola lettura. Se è abilitata la **Modalità di pulizia basata sulle dimensioni**, viene attivato anche il meccanismo di pulizia per applicare il limite di dimensioni massime.

Usare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o eseguire lo script seguente per ottenere informazioni aggiornate sulle dimensioni dell'archivio query:

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
 max_storage_size_mb, readonly_reason
FROM sys.database_query_store_options;
```

Lo script seguente imposta un nuovo valore per **Dimensioni massime (MB)** :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

 **Intervallo di scaricamento dati (minuti)** : definisce la frequenza per salvare in modo permanente le statistiche di runtime raccolte su disco. È espresso in minuti nell'interfaccia utente grafica (GUI), ma in [!INCLUDE[tsql](../../includes/tsql-md.md)] è espresso in secondi. Il valore predefinito è 900 secondi, ovvero 15 minuti nell'interfaccia utente grafica. Valutare la possibilità di usare un valore più elevato se il carico di lavoro non genera un numero elevato di query e piani diversi o se è possibile attendere più tempo per salvare i dati in modo permanente prima dell'arresto di un database.

> [!NOTE]
> L'uso del flag di traccia 7745 impedisce la scrittura su disco dei dati di Query Store nel caso di un comando di failover o arresto. Per altre informazioni, vedere la sezione [Usare i flag di traccia nei server cruciali](#Recovery).

Usare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] per impostare un valore diverso per **Intervallo di scaricamento dati**:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS = 900);
```

**Intervallo di raccolta statistiche**: definisce il livello di granularità delle statistiche di runtime raccolte, espresso in minuti. Il valore predefinito è 60 minuti. È possibile usare un valore inferiore se è necessaria una maggiore granularità o maggiore rapidità nel rilevare e limitare i problemi. Tenere presente che il valore influisce direttamente sulle dimensioni dei dati di Query Store. Usare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] per impostare un valore diverso per **Intervallo di raccolta statistiche**:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 60);
```

**Soglia per query non aggiornate (giorni)** : criteri di pulizia basati sul tempo, che controllano il periodo di conservazione di statistiche di runtime persistenti e query inattive, espresso in giorni. Per impostazione predefinita, Query Store è configurato in modo da conservare i dati per 30 giorni, che per alcuni scenari potrebbe essere un periodo eccessivamente lungo.

Evitare di conservare i dati cronologici che non si intende usare. Questo accorgimento riduce il ricorso allo stato di sola lettura. Le dimensioni dei dati di Query Store e il tempo necessario per rilevare e limitare il problema saranno più prevedibili. Usare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] oppure lo script seguente per configurare i criteri di pulizia basati sul tempo:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 90));
```

**Modalità di pulizia basata sulle dimensioni**: specifica se viene eseguita la pulizia automatica dei dati quando le dimensioni dei dati di Query Store si avvicinano al limite. Attivare la pulizia basata sulle dimensioni per assicurarsi che Query Store venga sempre eseguito in modalità lettura/scrittura e possa raccoglie i dati più recenti.  Si noti che non esiste alcuna garanzia in presenza di carichi di lavoro gravosi che la pulizia di Query Store manterrà costantemente le dimensioni dei dati al di sotto del limite. È possibile che la pulizia automatica dei dati superi tale limite ed attivi (temporaneamente) la modalità di sola lettura.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
```

**Modalità di acquisizione di Query Store**: specifica i criteri di acquisizione delle query per Query Store.

- **All** (Tutto): Consente di acquisire tutte le query. Si tratta dell'opzione predefinita in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].
- **Auto**: le query poco frequenti e le query con durata di compilazione ed esecuzione non significativa vengono ignorate. Le soglie per la durata del runtime, della compilazione e del conteggio esecuzioni vengono determinate internamente. A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], si tratta dell'opzione predefinita.
- **Nessuna**: Query Store smette di acquisire nuove query.
- **Custom**: consente un maggiore controllo e la capacità di ottimizzare i criteri di raccolta dati. Le nuove impostazioni personalizzate definiscono che cosa accade entro la soglia di tempo per i criteri di acquisizione interni. Si tratta di un limite di tempo durante il quale vengono valutate le condizioni configurabili e, se si verifica una di tali condizioni, la query è idonea per l'acquisizione da parte di Query Store.

> [!IMPORTANT]
> I cursori, le query all'interno delle stored procedure e le query compilate in modo nativo vengono sempre acquisiti quando la modalità di acquisizione di Query Store è impostata su **All**, **Auto** o **Custom**. Per acquisire le query compilate in modo nativo, abilitare la raccolta delle statistiche per query usando [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

 Lo script seguente imposta QUERY_CAPTURE_MODE su AUTO:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

### <a name="examples"></a>Esempi

L'esempio seguente imposta QUERY_CAPTURE_MODE su AUTO e imposta le altre opzioni consigliate in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

L'esempio seguente imposta QUERY_CAPTURE_MODE su AUTO e imposta le altre opzioni consigliate in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] in modo da includere le statistiche di attesa:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON
    );
```

L'esempio seguente imposta QUERY_CAPTURE_MODE su AUTO, definisce le altre opzioni consigliate in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e configura *facoltativamente* i criteri di acquisizione CUSTOM con i valori predefiniti, in alternativa alla nuova modalità di acquisizione AUTO predefinita:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="start-with-query-performance-troubleshooting"></a>Iniziare a risolvere i problemi di prestazioni relativi alle query

Il flusso di lavoro di risoluzione dei problemi relativi a Query Store è semplice, come illustra il diagramma seguente:

![Risoluzione dei problemi di Query Store](../../relational-databases/performance/media/query-store-troubleshooting.png "query-store-troubleshooting")

Abilitare Query Store usando [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], come descritto nella sezione precedente, o eseguire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:

```sql
ALTER DATABASE [DatabaseOne] SET QUERY_STORE = ON;
```

La raccolta di un set di dati che rappresenti in modo accurato il carico di lavoro da parte di Query Store può richiedere del tempo. In genere, un giorno è sufficiente anche per carichi di lavoro molto complessi. Tuttavia, dopo aver abilitato la funzionalità è possibile iniziare a esplorare i dati e identificare le query che richiedono attenzione immediata. Andare alla sottocartella di Query Store nel nodo database in Esplora oggetti di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] per aprire le viste di risoluzione dei problemi per scenari specifici.

Le viste dell'archivio query di[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] possono essere usate con un set di metriche di esecuzione, espresse come una delle funzioni statistiche seguenti:

|Versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Metrica di esecuzione|Funzione statistica|
|----------------------|----------------------|------------------------|
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Tempo CPU, Durata, Conteggio esecuzioni, Letture logiche, Scritture logiche, Utilizzo memoria, Letture fisiche, Tempo CLR, Grado di parallelismo e Conteggio righe|Media, Massimo, Minimo, Deviazione standard, Totale|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|Tempo CPU, Durata, Conteggio esecuzioni, Letture logiche, Scritture logiche, Utilizzo memoria, Letture fisiche, Tempo CLR, Grado di parallelismo, Conteggio righe, Memoria log, Memoria TempDB e Tempi di attesa|Media, Massimo, Minimo, Deviazione standard, Totale|

L'immagine seguente mostra come trovare le viste dell'archivio query:

![Viste di Query Store](../../relational-databases/performance/media/objectexplorerquerystore_sql17.png "Viste di Query Store")

La tabella seguente illustra quando usare ognuna delle viste dell'archivio query:

|Vista di SQL Server Management Studio|Scenario|
|---------------|--------------|
|**Query regredite**|Trovare le query per cui le metriche di esecuzione sono recentemente regredite (ad esempio, peggiorate). <br />Usare questa vista per correlare i problemi di prestazioni osservati nell'applicazione con le query effettive da correggere o migliorare.|
|**Consumo complessivo risorse**|Analizzare il consumo totale delle risorse per il database per una delle metriche di esecuzione.<br />Usare questa vista per identificare gli schemi di consumo delle risorse, ad esempio nei carichi di lavoro diurni e notturni, e ottimizzare il consumo complessivo per il database.|
|**Prime query per consumo di risorse**|Scegliere una metrica di esecuzione di interesse e trovare le query con i valori più estremi in un intervallo di tempo specificato. <br />Usare questa vista per concentrare l'attenzione sulle query più rilevanti che hanno l'impatto maggiore sul consumo delle risorse di database.|
|**Query con piani forzati**|Elenca i piani forzati in precedenza tramite Query Store. <br />Usare questa vista per accedere rapidamente a tutti i piani attualmente forzati.|
|**Query con variazione elevata**|Analizzare le query con variazione di esecuzione elevata in relazione alle dimensioni disponibili, ad esempio Durata, Tempo CPU, I/O e Utilizzo memoria, nell'intervallo di tempo desiderato.<br />Usare questa vista per identificare le query con prestazioni molto variabili che possono influire negativamente sull'esperienza utente in tutte le applicazioni.|
|**Statistiche di attesa query**|Analizzare le categorie di attesa più attive in un database e le query che contribuiscono maggiormente alla categoria di attesa selezionata.<br />Usare questa vista per analizzare le statistiche di attesa e identificare le query che possono influire negativamente sull'esperienza utente in tutte le applicazioni.<br /><br />Si applica a: a partire da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18.0 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].|
|**Query rilevate**|Tenere traccia dell'esecuzione delle query più importanti in tempo reale. In genere, questa vista viene usata in presenza di query con piani forzati per garantire la stabilità delle prestazioni delle query.|

> [!TIP]
> Per informazioni dettagliate sull'uso di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] per identificare le prime query per consumo di risorse e correggere le query regredite a causa della modifica di una scelta del piano, vedere i [post relativi a Query Store nei blog di @Azure](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

Quando si identifica una query con prestazioni non ottimali, l'azione correttiva dipende dalla natura del problema.

- Se la query è stata eseguita con più piani e l'ultimo piano è notevolmente peggiore rispetto al piano precedente, è possibile ricorrere al meccanismo di uso forzato del piano. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prova a forzare il piano in query optimizer. Se l'uso forzato del piano ha esito negativo, viene generato un XEvent e a query optimizer viene richiesto di ottimizzare in modo normale.

  ![Forzatura del piano di Query Store](../../relational-databases/performance/media/query-store-force-plan.png "query-store-force-plan")

  > [!NOTE]
  > Nella figura precedente sono illustrate forme diverse per piani di query specifici, con i significati seguenti per ogni stato possibile:<br />
  >
  > |Con forme|Significato|
  > |-------------------|-------------|
  > |Circle|Query completata, che significa che una normale esecuzione è stata completata correttamente.|
  > |Square|Annullata, che significa che l'esecuzione inizializzata sul lato client è stata interrotta.|
  > |Triangle|Non riuscita, che significa che l'esecuzione è stata interrotta da un'eccezione.|
  >
  > Le dimensioni della forma riflettono inoltre il numero di esecuzioni di query nell'intervallo di tempo specificato. Le dimensioni aumentano in base al numero di esecuzioni.

- Si può concludere che la query sia priva di un indice per l'esecuzione ottimale. Queste informazioni vengono rese disponibili all'interno del piano di esecuzione query. Creare l'indice mancante e verificare le prestazioni della query usando Query Store.

   ![Visualizzare il piano di Query Store](../../relational-databases/performance/media/query-store-show-plan.png "query-store-show-plan")

Se si esegue il carico di lavoro in [!INCLUDE[ssSDS](../../includes/sssds-md.md)], iscriversi a Index Advisor per [!INCLUDE[ssSDS](../../includes/sssds-md.md)] per ricevere automaticamente indicazioni relative agli indici.

- In alcuni casi si potrebbe applicare la ricompilazione delle statistiche, se c'è una notevole differenza tra il numero stimato di righe nel piano di esecuzione e quello effettivo.
- Riscrivere le query problematiche, ad esempio per sfruttare i vantaggi della parametrizzazione delle query o per implementare la logica ottimale.

## <a name="verify-that-query-store-collects-query-data-continuously"></a><a name="Verify"></a> Verificare che Query Store raccolga i dati delle query in modo continuativo

Query Store può cambiare automaticamente la modalità operativa. Monitorare regolarmente lo stato di Query Store per assicurarsi che sia in funzione e per prevenire errori dovuti a cause evitabili. Eseguire questa query per determinare la modalità operativa e visualizzare i parametri più importanti:

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

La differenza tra `actual_state_desc` e `desired_state_desc` indica che si è verificata una modifica automatica della modalità operativa. La modifica più comune è il passaggio automatico di Query Store alla modalità di sola lettura. Eccezionalmente, Query Store può passare a uno stato di errore a causa di errori interni.

Quando lo stato effettivo è di sola lettura, usare la colonna **readonly_reason** per determinare la causa radice. In genere, Query Store passa alla modalità di sola lettura quando viene superato il limite delle dimensioni. In questo caso **readonly_reason** è impostato su 65536. Per altre situazioni, vedere [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).

Per riportare l'archivio query in modalità lettura/scrittura e attivare la raccolta dei dati, prendere in considerazione i passaggi seguenti:

- Aumentare le dimensioni massime dello spazio di archiviazione usando l'opzione **MAX_STORAGE_SIZE_MB** di **ALTER DATABASE**.
- Eseguire la pulizia dei dati dell'archivio query usando l'istruzione seguente:

  ```sql
  ALTER DATABASE [QueryStoreDB] SET QUERY_STORE CLEAR;
  ```

È possibile applicare uno o entrambi questi passaggi eseguendo questa istruzione, che ripristina in modo esplicito la modalità lettura/scrittura:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
```

Seguire questa procedura:

- È possibile prevenire le modifiche automatiche della modalità operativa applicando le procedure consigliate. Assicurarsi che le dimensioni di Query Store siano sempre inferiori al valore massimo consentito per ridurre drasticamente il rischio di passaggio alla modalità di sola lettura. Attivare criteri basati sulle dimensioni, come descritto nella sezione [Configurare Query Store](#Configure), in modo che Query Store esegua automaticamente la pulizia dei dati quando le dimensioni si avvicinano al limite.
- Per assicurarsi che vengano conservati i dati più recenti, configurare criteri basati sul tempo per rimuovere regolarmente le informazioni non aggiornate.
- Infine, è consigliabile impostare la **modalità di acquisizione di Query Store** su **Auto** per escludere le query generalmente meno rilevanti per il carico di lavoro.

### <a name="error-state"></a>Stato di errore

Per recuperare Query Store, provare a impostare in modo esplicito la modalità lettura/scrittura e ricontrollare lo stato effettivo.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

Se il problema persiste, il danneggiamento dei dati di Query Store è persistente sul disco.

A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], è possibile recuperare Query Store eseguendo la stored procedure **sp_query_store_consistency_check** all'interno del database interessato. Prima di provare a eseguire l'operazione di ripristino, è necessario disabilitare Query Store. Per [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è necessario cancellare i dati da Query Store come illustrato.

Se il ripristino non riesce, è possibile provare a cancellare Query Store prima di impostare la modalità lettura/scrittura.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE CLEAR;
GO

ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

## <a name="set-the-optimal-query-store-capture-mode"></a>Impostare la modalità di acquisizione di Query Store ottimale

Mantenere i dati più rilevanti nell'archivio query. La tabella seguente descrive gli scenari tipici per ogni modalità di acquisizione di Query Store:

|Modalità di acquisizione dell'archivio query|Scenario|
|------------------------|--------------|
|**Tutto**|Analizzare accuratamente il carico di lavoro in termini di forme di query, frequenza di esecuzione e altre statistiche.<br /><br /> Identificare le nuove query nel carico di lavoro.<br /><br /> Stabilire se vengono usate query ad hoc per identificare le opportunità di parametrizzazione automatica o da parte dell'utente.<br /><br />Nota: si tratta della modalità di acquisizione predefinita in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].|
|**Auto**|Concentrare l'attenzione su query rilevanti e da correggere. Un esempio sono le query eseguite regolarmente o che hanno un consumo di risorse elevato.<br /><br />Nota: a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], si tratta della modalità di acquisizione predefinita.|
|**Nessuno**|Il set di query da monitorare è stato già acquisito in fase di esecuzione e si vuole eliminare qualsiasi distrazione introdotta da altre query.<br /><br /> È adatta ad ambienti di testing e di benchmarking.<br /><br /> È adatta ai fornitori di software che forniscono l'archivio query configurato per il monitoraggio del carico di lavoro della relativa applicazione.<br /><br /> Deve essere usata con cautela perché può precludere la possibilità di rilevare e ottimizzare nuove query importanti. Evitare di usare questa modalità a meno che non sia richiesta da uno scenario specifico.|
|**Impostazione personalizzata**|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce una modalità di acquisizione Custom nel comando `ALTER DATABASE SET QUERY_STORE`. Una volta abilitate, le configurazioni aggiuntive di Query Store sono disponibili in una nuova impostazione di criteri di acquisizione di Query Store per ottimizzare la raccolta dati in un server specifico.<br /><br />Le nuove impostazioni personalizzate definiscono che cosa accade entro la soglia di tempo per i criteri di acquisizione interni. Si tratta di un limite di tempo durante il quale vengono valutate le condizioni configurabili e, se si verifica una di tali condizioni, la query è idonea per l'acquisizione da parte di Query Store. Per altre informazioni, vedere [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).|

> [!NOTE]
> I cursori, le query all'interno delle stored procedure e le query compilate in modo nativo vengono sempre acquisiti quando la modalità di acquisizione di Query Store è impostata su **All**, **Auto** o **Custom**. Per acquisire le query compilate in modo nativo, abilitare la raccolta delle statistiche per query usando [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

## <a name="keep-the-most-relevant-data-in-query-store"></a>Mantenere i dati più rilevanti in Query Store

Configurare Query Store in modo che contenga solo i dati rilevanti per garantire l'esecuzione ininterrotta e la risoluzione ottimale dei problemi con un impatto minimo sul normale carico di lavoro.
La tabella seguente riporta le procedure consigliate:

|Procedura consigliata|Impostazione|
|-------------------|-------------|
|Limitare i dati cronologici conservati.|Configurare criteri basati sul tempo per attivare la pulizia automatica.|
|Escludere le query non rilevanti.|Configurare la **modalità di acquisizione di Query Store** su **Auto**.|
|Eliminare le query meno rilevanti quando vengono raggiunte le dimensioni massime.|Attivare criteri di pulizia basati sulle dimensioni.|

## <a name="avoid-using-non-parameterized-queries"></a><a name="Parameterize"></a> Evitare l'uso di query senza parametri

L'uso di query senza parametri quando non è strettamente necessario non è una procedura consigliata. Un esempio è in caso di analisi ad hoc. Non è possibile usare nuovamente i piani memorizzati nella cache e questo impone a Query Optimizer di compilare query per ogni testo della query univoco. Per altre informazioni, vedere [Linee guida per l'utilizzo della parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).

Inoltre, Query Store può superare rapidamente il limite di dimensioni a causa del numero potenzialmente elevato di testi delle query diversi e del conseguente numero elevato di piani di esecuzione diversi con forma simile. Questo influisce negativamente sulle prestazioni del carico di lavoro e Query Store potrebbe passare alla modalità di sola lettura o eliminare dati continuamente per tentare di gestire le query in ingresso.

Valutare le opzioni seguenti:

- Parametrizzare le query, se applicabile. Ad esempio, eseguire il wrapping delle query all'interno di una stored procedure o sp_executesql. Per altre informazioni, vedere [Parametri e riutilizzo del piano di esecuzione](../../relational-databases/query-processing-architecture-guide.md#PlanReuse).
- Usare l'opzione [Ottimizza per carichi di lavoro ad hoc](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) se il carico di lavoro contiene molti batch ad hoc monouso con piani di query diversi.
  - Confrontare il numero di valori query_hash distinti con il numero totale di voci in sys.query_store_query. Se il rapporto è vicino a 1, il carico di lavoro ad hoc genera query diverse.
- Applicare la [parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) per il database o per un subset di query, se il numero di piani di query diversi non è elevato.
  - Usare una [guida di piano](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md) per forzare la parametrizzazione solo per la query selezionata.
  - Configurare la parametrizzazione forzata usando il comando dell'[opzione di database Parameterization](../../relational-databases/databases/database-properties-options-page.md#miscellaneous), se nel carico di lavoro è presente un numero ridotto di piani di query diversi. Un esempio è quando il rapporto tra il numero di valori query_hash distinti e il numero totale di voci in sys.query_store_query è molto inferiore a 1.
- Impostare QUERY_CAPTURE_MODE su AUTO per filtrare automaticamente le query ad hoc con un consumo di risorse ridotto.

## <a name="avoid-a-drop-and-create-pattern-for-containing-objects"></a><a name="Drop"></a> Evitare il modello DROP e CREATE per gli oggetti contenitore

Query Store associa ogni voce di query a un oggetto contenitore, come una stored procedure, una funzione o un trigger. Quando si ricrea un oggetto contenitore, viene generata una nuova voce di query per lo stesso testo della query. Questo impedisce di monitorare le statistiche sulle prestazioni relative a tale query nel tempo e di ricorrere al meccanismo di uso forzato del piano. Per evitare questa situazione, usare il processo `ALTER <object>` per modificare la definizione dell'oggetto contenitore, quando è possibile.

## <a name="check-the-status-of-forced-plans-regularly"></a><a name="CheckForced"></a> Verificare regolarmente lo stato dei piani forzati

L'uso forzato del piano è un meccanismo efficace per risolvere i problemi di prestazioni delle query critiche e renderle più prevedibili. Come accade con gli hint di piano e le guide di piano, forzare un piano non garantisce che poi venga usato nelle esecuzioni successive. In genere, quando lo schema del database viene modificato in modo che gli oggetti a cui fa riferimento il piano di esecuzione vengano modificati o eliminati, l'uso forzato del piano ha esito negativo. In questo caso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] opta per la ricompilazione delle query, mentre il motivo effettivo dell'errore viene esposto in [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). La query seguente restituisce informazioni sui piani forzati:

```sql
USE [QueryStoreDB];
GO

SELECT p.plan_id, p.query_id, q.object_id as containing_object_id,
    force_failure_count, last_force_failure_reason_desc
FROM sys.query_store_plan AS p
JOIN sys.query_store_query AS q on p.query_id = q.query_id
WHERE is_forced_plan = 1;
```

Per un elenco completo dei motivi, vedere [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). È anche possibile usare l'oggetto XEvent **query_store_plan_forcing_failed** per monitorare e risolvere gli errori di uso forzato del piano.

## <a name="avoid-renaming-databases-for-queries-with-forced-plans"></a><a name="Renaming"></a> Evitare di rinominare i database per query con piani forzati

I piani di esecuzione fanno riferimento agli oggetti usando nomi in tre parti come `database.schema.object`.

Se si rinomina un database, l'uso forzato del piano ha esito negativo e questo provoca la ricompilazione in tutte le esecuzioni di query successive.

## <a name="using-query-store-in-mission-critical-servers"></a><a name="Recovery"></a> Uso di Query nei server mission-critical

I flag di traccia globali 7745 e 7752 possono essere usati per migliorare la disponibilità dei database tramite Query Store. Per altre informazioni, vedere [Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

- Il flag di traccia 7745 previene il comportamento predefinito quando Query Store scrive i dati su disco prima dell'arresto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo significa che i dati di Query Store che sono stati raccolti ma non sono ancora stati salvati su disco andranno persi, per l'intervallo di tempo definito con `DATA_FLUSH_INTERVAL_SECONDS`.
- Il flag di traccia 7752 abilita il caricamento asincrono di Query Store. Questo consente di portare online un database ed eseguire query prima del completamento del ripristino di Query Store. Il comportamento predefinito consiste nell'eseguire un caricamento sincrono di Query Store. Il comportamento predefinito impedisce l'esecuzione delle query prima del completamento del ripristino di Query Store, ma evita anche l'esclusione di query nella raccolta di dati.

> [!NOTE]
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], questo comportamento è controllato dal motore e il flag di traccia 7752 non ha alcun effetto.

> [!IMPORTANT]
> Se si usa Query Store per informazioni dettagliate sui carichi di lavoro just-in-time in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], prevedere l'installazione dei miglioramenti della scalabilità delle prestazioni in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU2 ([KB 4340759](https://support.microsoft.com/help/4340759)) appena possibile. Senza questi miglioramenti, quando il database è sottoposto a carichi di lavoro intensivi, può verificarsi una contesa di spinlock e le prestazioni del server possono risultare rallentate. In particolare, è possibile che si verifichi una contesa significativa sullo spinlock `QUERY_STORE_ASYNC_PERSIST` o `SPL_QUERY_STORE_STATS_COOKIE_CACHE`. Applicato questo miglioramento, Query Store non provocherà più contese di spinlock.

> [!IMPORTANT]
> Se si usa Query Store per informazioni dettagliate sui carichi di lavoro just-in-time in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), prevedere l'installazione del miglioramento della scalabilità delle prestazioni in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU15, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU22 e [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU8 appena possibile. Senza questo miglioramento, quando il database è sottoposto a carichi di lavoro ad hoc pesanti, Query Store può usare una quantità elevata di memoria e le prestazioni del server possono risultare rallentate. Applicato questo miglioramento, Query Store impone limiti interni alla quantità di memoria che può essere usata dai vari componenti e può modificare automaticamente la modalità di operazione in sola lettura finché non viene restituita memoria sufficiente a [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Si noti che i limiti di memoria interni di Query Store non sono documentati perché sono soggetti a modifiche.  


## <a name="using-query-store-in-azure-sql-database-active-geo-replication"></a><a name="geosyncreplicas"></a> Uso di Query Store nella replica geografica attiva del database SQL di Azure

Query Store in una replica geografica attiva secondaria del database SQL di Azure sarà una copia di sola lettura dell'attività nella replica primaria. 

Evitare la partecipazione alla replica geografica di livelli di database SQL di Azure non corrispondenti. Un database secondario deve avere dimensioni di calcolo uguali o molto simili al database primario ed essere incluso nello stesso livello di servizio del database primario. Cercare il tipo di attesa HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO in [sys.dm_db_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database), che indica la limitazione della frequenza del log delle transazioni nella replica primaria a causa del ritardo del database secondario.

Per altre informazioni sulla stima e la configurazione delle dimensioni del database SQL di Azure secondario della replica geografica attiva, vedere [Configurazione del database secondario](/azure/azure-sql/database/active-geo-replication-overview#configuring-secondary-database).


## <a name="see-also"></a>Vedere anche

- [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)
- [Viste del catalogo di Query Store &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [Stored procedure di Query Store &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [Uso di Query Store con OLTP in-memoria](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [Monitoraggio delle prestazioni con Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md)