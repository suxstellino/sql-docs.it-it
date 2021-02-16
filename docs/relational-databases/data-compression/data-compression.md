---
title: Compressione dei dati | Microsoft Docs
description: Applicare la compressione dei dati delle righe e delle pagine o la compressione dell'archivio columnstore e columnstore usando SQL Server e il database SQL di Azure.
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- page compression [Database Engine]
- indexes [SQL Server], compressed
- compressed indexes [SQL Server]
- storage compression [Database Engine]
- tables [SQL Server], compressed
- storage [SQL Server], compressed
- compression [SQL Server]
- row compression [Database Engine]
- compression [SQL Server], about compressed tables and indexes
- data compression [Database Engine]
- compressed tables [SQL Server]
ms.assetid: 5f33e686-e115-4687-bd39-a00c48646513
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8f28c1d2aaa3e1e1e431feaf265d7b1bc22f9d9
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525194"
---
# <a name="data-compression"></a>Compressione dei dati

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE [sssdsmifull-md](../../includes/sssdsmifull-md.md)] supportano la compressione di righe e pagine per le tabelle e gli indici rowstore e supportano la compressione degli archivi columnstore e columnstore per le tabelle e gli indici columnstore.  
  
 Per le tabelle e gli indici rowstore, utilizzare la funzionalità di compressione dei dati per ridurre le dimensioni del database. Oltre a risparmiare spazio, la compressione dei dati migliora le prestazioni dei carichi di lavoro di I/O a utilizzo elevato di memoria perché i dati vengono archiviati in un numero inferiore di pagine e le query devono leggere un numero inferiore di pagine dal disco. Sono tuttavia necessarie risorse della CPU aggiuntive nel server di database per comprimere e decomprimere i dati, mentre i dati vengono scambiati con l'applicazione. È possibile configurare la compressione di righe e pagine sugli oggetti di database seguenti:   
-   Un'intera tabella archiviata come heap.  
-   Un'intera tabella archiviata come indice cluster.  
-   Un intero indice non cluster.  
-   Un'intera vista indicizzata.  
-   Per gli indici e le tabelle partizionati, è possibile configurare l'opzione di compressione per ogni partizione. Non è inoltre necessario che per le varie partizioni di un oggetto sia configurata la stessa impostazione di compressione.  
  
Per gli indici e le tabelle columnstore, tutti gli indici e le tabelle columnstore utilizzano sempre la compressione columnstore che non è configurabile dall'utente. Utilizzare la compressione dell'archivio columnstore per ridurre ulteriormente le dimensioni dei dati in situazioni in cui è possibile concedere altro tempo e altre risorse della CPU per archiviare e recuperare i dati.  È possibile configurare la compressione dell'archivio columnstore sugli oggetti di database seguenti:  
-   Un'intera tabella columnstore o un intero indice columnstore cluster.  Poiché una tabella columnstore viene archiviata come indice columnstore cluster, entrambi gli approcci producono gli stessi risultati.  
-   Un intero indice columnstore non cluster.  
-   Per gli indici columnstore e le tabelle columnstore partizionati, è possibile configurare l'opzione di compressione dell'archivio per ogni partizione. Non è inoltre necessario che per le varie partizioni sia configurata la stessa impostazione di compressione dell'archivio.  
  
> [!NOTE]  
> È anche possibile comprimere i dati usando il formato di algoritmo GZIP. Si tratta di un passaggio aggiuntivo ed è particolarmente adatto per comprimere porzioni di dati durante l'archiviazione di vecchi dati per l'archiviazione a lungo termine. I dati compressi usando la funzione `COMPRESS` non possono essere indicizzati. Per altre informazioni, vedere [COMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/compress-transact-sql.md).  
  
## <a name="row-and-page-compression"></a>Compressione di riga e di pagina

Quando si utilizza la compressione di riga e di pagina, tenere presente le considerazioni seguenti:  
-   I dettagli relativi alla compressione dei dati sono soggetti a modifiche senza preavviso nei Service Pack o nelle versioni successive.
-   La compressione è disponibile in [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]  
-   La compressione non è disponibile in ogni edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
-   La compressione non è disponibile per le tabelle di sistema.  
-   La compressione consente di archiviare più righe in una pagina, ma non di modificare le dimensioni massime delle righe di una tabella o di un indice.  
-   Una tabella non può essere abilitata per la compressione quando la somma delle dimensioni massime delle righe e dell'overhead relativo alla compressione supera le dimensioni massime di 8.060 byte delle righe. Ad esempio, una tabella in cui sono presenti le colonne `c1 CHAR(8000)` e `c2 CHAR(53)` non può essere compressa a causa dell'overhead aggiuntivo relativo alla compressione. Se si utilizza il formato di archiviazione vardecimal, il controllo delle dimensioni delle righe viene eseguito al momento dell'abilitazione del formato stesso. Per la compressione di riga e di pagina, il controllo delle dimensioni delle righe viene eseguito quando l'oggetto viene compresso inizialmente e successivamente in occasione dell'inserimento o della modifica di ogni riga. Quando viene utilizzata la compressione, vengono applicate le due regole seguenti:  
    -   Un aggiornamento a un tipo a lunghezza fissa deve sempre avere esito positivo.  
    -   La disabilitazione della compressione dei dati deve sempre avere esito positivo. Anche se la riga compressa rientra nella pagina, ovvero se le dimensioni della riga sono minori di 8.060 byte, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono impediti gli aggiornamenti che non sarebbe possibile includere nella riga non compressa.  
-   Quando viene specificato un elenco di partizioni, il tipo di compressione può essere impostato su ROW, PAGE o NONE per ogni singola partizione. Se l'elenco di partizioni non è specificato, tutte le partizioni vengono impostate in base alla proprietà di compressione dei dati specificata nell'istruzione. Quando viene creato un indice o una tabella, la compressione dei dati viene impostata su NONE se non specificato diversamente. Quando una tabella viene modificata, viene mantenuta la compressione esistente se non specificato diversamente.  
-   Se si specifica un elenco di partizioni o una partizione non compresa nell'intervallo, viene generato un errore.  
-   Gli indici non cluster non ereditano la proprietà di compressione della tabella. Per comprimere gli indici, è necessario impostarne in modo esplicito la proprietà di compressione. Per impostazione predefinita, al momento della creazione dell'indice la compressione è impostata su NONE.  
-   Quando un indice cluster viene creato in un heap, tale indice eredita lo stato di compressione dell'heap, a meno che non venga specificato uno stato di compressione alternativo.  
-   Quando un heap è configurato per la compressione a livello di pagina, le pagine vengono compresse solo nelle modalità seguenti:  
    -   Viene eseguita l'importazione bulk dei dati con l'ottimizzazione delle operazioni bulk abilitata.  
    -   I dati vengono inseriti usando la sintassi `INSERT INTO ... WITH (TABLOCK)` e la tabella non contiene un indice non cluster.  
    -   Una tabella viene ricompilata eseguendo l'istruzione `ALTER TABLE ... REBUILD` con l'opzione di compressione PAGE.  
-   Le nuove pagine allocate in un heap nel corso di operazioni DML non usano la compressione PAGE finché l'heap non viene ricompilato. Ricompilare l'heap rimuovendo e riapplicando la compressione oppure creando e rimuovendo un indice cluster.  
-   Per modificare l'impostazione di compressione di un heap, è necessario ricompilare tutti gli indici non cluster della tabella in modo che dispongano di puntatori ai nuovi percorsi delle righe nell'heap.  
-   È possibile abilitare o disabilitare l'impostazione di compressione ROW o PAGE online oppure offline. L'abilitazione della compressione in un heap è un'operazione a thread singolo se eseguita online.  
-   I requisiti di spazio su disco per l'abilitazione o la disabilitazione della compressione di riga o di pagina sono gli stessi necessari per la creazione o la ricompilazione di un indice. Per i dati partizionati, è possibile ridurre lo spazio richiesto abilitando o disabilitando la compressione per una partizione alla volta.  
-   Per determinare lo stato della compressione delle partizioni in una tabella partizionata, eseguire una query sulla colonna data_compression della vista del catalogo sys.partitions.  
-   Durante la compressione di indici, alle pagine a livello foglia è possibile applicare sia la compressione di riga che quella di pagina, mentre alle pagine a livello non foglia non è possibile applicare la compressione di pagina.  
-   A causa della dimensione, i tipi di dati per valori di grandi dimensioni vengono a volte archiviati separatamente rispetto ai dati delle righe normali in pagine specifiche. La compressione dei dati non è disponibile per i dati archiviati separatamente.  
-   Le tabelle in cui era implementato il formato di archiviazione vardecimal in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] mantengono questa impostazione anche dopo l'aggiornamento. È possibile applicare la compressione di riga a una tabella che utilizza il formato di archiviazione vardecimal. Tuttavia, poiché la compressione di riga è un superset del formato di archiviazione vardecimal, non è necessario mantenere quest'ultimo. Quando si utilizza il formato di archiviazione vardecimal con la compressione di riga, per i valori decimali non si ottiene alcun miglioramento in termini di compressione. Sebbene sia possibile inoltre applicare la compressione di pagina a una tabella per cui è implementato il formato di archiviazione vardecimal, per le colonne che utilizzano tale formato probabilmente non è possibile ottenere compressione aggiuntiva.  
  
    > [!NOTE]  
    > Tutte le versioni supportate di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supportano il formato di archiviazione vardecimal. Tuttavia, poiché la compressione dei dati raggiunge gli stessi obiettivi, il formato di archiviazione vardecimal è deprecato. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="columnstore-and-columnstore-archive-compression"></a>Compressione dell'archivio columnstore e columnstore  

Gli indici e le tabelle columnstore vengono sempre archiviati con la compressione columnstore. È possibile ridurre ulteriormente le dimensioni dei dati columnstore configurando una compressione aggiuntiva denominata compressione dell'archivio.  Per eseguire la compressione dell'archivio, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito l'algoritmo di compressione Microsoft XPRESS sui dati. Aggiungere o rimuovere la compressione dell'archivio utilizzando i tipi di compressione dati seguenti:  
-   Usare la compressione dati **COLUMNSTORE_ARCHIVE** per comprimere i dati columnstore con la compressione dell'archivio.  
-   Utilizzare la compressione dati **COLUMNSTORE** per decomprimere la compressione dell'archivio. I dati risultanti verranno compressi con la compressione columnstore.  
  
Per aggiungere la compressione dell'archivio, usare [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) o [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) con l'opzione REBUILD e DATA COMPRESSION = COLUMNSTORE_ARCHIVE.  
  
Ad esempio:

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE ON PARTITIONS (2,4)) ;  
```  
  
Per rimuovere la compressione dell'archivio e ripristinare i dati nella compressione columnstoe, usare [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) o [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) con l'opzione REBUILD e DATA COMPRESSION = COLUMNSTORE.  

Ad esempio:

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (2,4) ) ;  
```  
  
Nell'esempio seguente la compressione dei dati viene impostata su columnstore in alcune partizioni e su archivio columnstore in altre.  
  
```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (  
    DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (4,5),  
    DATA COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (1,2,3)  
) ;  
```  
  
### <a name="performance"></a>Prestazioni

 La compressione degli indici columnstore con la compressione dell'archivio comporta un calo delle prestazioni dell'indice rispetto agli indici columnstore compressi con un altro tipo di compressione. Utilizzare la compressione dell'archivio solo quando è possibile concedere altro tempo e altre risorse della CPU per comprimere e recuperare i dati.  
  
 Il vantaggio della compressione dell'archivio è uno spazio di archiviazione ridotto, utile per i dati a cui non si accede di frequente. Se ad esempio si dispone di una partizione per ogni mese di dati e la maggior parte dell'attività è relativa ai mesi più recenti, è possibile archiviare i mesi precedenti per ridurre i requisiti di archiviazione.  
  
### <a name="metadata"></a>Metadati

Nelle viste di sistema seguenti sono contenute informazioni sulla compressione dei dati per gli indici cluster:  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md): le colonne **type** e **type_desc** includono CLUSTERED COLUMNSTORE e NONCLUSTERED COLUMNSTORE.  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md): le colonne **data_compression** e **data_compression_desc** includono COLUMNSTORE e COLUMNSTORE_ARCHIVE.  
  
La stored procedure [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) si applica anche agli indici columnstore.  
  
## <a name="impact-on-partitioned-tables-and-indexes"></a>Effetti su tabelle e indici partizionati

 Quando si utilizza la compressione dei dati con tabelle e indici partizionati, tenere presente le considerazioni seguenti:  
-   Quando le partizioni vengono suddivise usando l'istruzione `ALTER PARTITION`, entrambe le partizioni ereditano l'attributo di compressione dei dati della partizione originale.  
-   Quando due partizioni vengono unite, la partizione risultante eredita l'attributo di compressione dei dati della partizione di destinazione.  
-   Per cambiare una partizione, è necessario che la relativa proprietà di compressione dei dati corrisponda a quella della tabella.  
-   Per modificare la compressione di una tabella o di un indice partizionato, è possibile utilizzare due sintassi diverse:  
    -   La sintassi seguente consente di ricompilare solo la partizione cui si fa riferimento:  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  <option>)  
        ```  
    
    -   La sintassi seguente consente di ricompilare l'intera tabella utilizzando l'impostazione di compressione esistente per qualsiasi partizione cui non si fa riferimento:  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = ALL   
        WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(<range>),  
        ... )  
        ```  
  
     Per gli indici partizionati viene seguito lo stesso principio usando l'istruzione `ALTER INDEX`.  
  
-   Quando un indice cluster viene eliminato, le partizioni di heap corrispondenti mantengono l'impostazione di compressione dei dati, a meno che lo schema di partizione non venga modificato. Se lo schema di partizionamento viene modificato, tutte le partizioni vengono ricompilate in un stato non compresso. Per eliminare un indice cluster e modificare lo schema di partizionamento, è necessario effettuare le operazioni descritte nei passaggi seguenti:  
     1. Eliminare l'indice cluster.  
     2. Modificare la tabella usando l'opzione `ALTER TABLE ... REBUILD` che specifica l'opzione di compressione.  
  
     L'eliminazione OFFLINE di un indice cluster è un'operazione di rapida esecuzione, poiché vengono rimossi solo i livelli superiori degli indici cluster. Quando un indice cluster viene eliminato ONLINE, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è necessario ricompilare l'heap due volte, una volta per l'operazione descritta nel passaggio 1 e una volta per quella descritta nel passaggio 2.  
  
## <a name="how-compression-affects-replication"></a>Impatto della compressione sulla replica

Quando si utilizza la compressione dei dati con la replica, tenere presente le considerazioni seguenti:  
-   Quando l'agente snapshot genera lo script dello schema iniziale, il nuovo schema usa le stesse impostazioni di compressione sia per la tabella che per i relativi indici. Non è possibile abilitare la compressione solo sulla tabella e non sull'indice.  
-   Per la replica transazionale, l'opzione dello schema dell'articolo determina gli oggetti e le proprietà dipendenti da inserire nello script. Per altre informazioni, vedere [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
     Quando applica gli script, l'agente di distribuzione non verifica la presenza di Sottoscrittori legacy. Se la replica della compressione è selezionata, la creazione della tabella in Sottoscrittori legacy non riesce. Nel caso di una topologia mista, non abilitare la replica della compressione.  
-   Per la replica di tipo merge, il livello di compatibilità della pubblicazione prevale sulle opzioni dello schema e determina gli oggetti dello schema per i quali viene generato uno script.  
     Nel caso di una topologia mista, se non è necessario per supportare le nuove opzioni di compressione, il livello di compatibilità della pubblicazione deve essere impostato sulla versione del Sottoscrittore legacy. Se necessario, comprimere le tabelle nel Sottoscrittore dopo che sono state create.  
  
Nella tabella seguente vengono illustrate le impostazioni di replica che controllano la compressione durante la replica stessa.  
  
|Operazione che l'utente intende eseguire|Replica dello schema di partizione per una tabella o un indice|Replica delle impostazioni di compressione|Comportamento a livello di script|  
|-----------------|-----------------------------------------------------|------------------------------------|------------------------|  
|Replicare lo schema di partizione e abilitare la compressione sulla partizione nel Sottoscrittore.|True|True|Inserisce nello script lo schema di partizione e le impostazioni di compressione.|  
|Replicare lo schema di partizione senza comprimere i dati nel Sottoscrittore.|True|Falso|Inserimento nello script dello schema di partizione, ma non delle impostazioni di connessione per la partizione.|  
|Non replicare lo schema di partizione né comprimere i dati nel Sottoscrittore.|Falso|False|Non inserisce nello script né la partizione né le impostazioni di compressione.|  
|Comprimere la tabella nel Sottoscrittore se tutte le partizioni sono compresse nel server di pubblicazione, senza replicare lo schema di partizione.|False|True|Controlla se tutte le partizioni sono abilitate per la compressione.<br /><br /> Inserisce nello script la compressione a livello di tabella.|  
  
## <a name="impact-on-other-sql-server-components"></a>Effetti sugli altri componenti di SQL Server

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   
 La compressione viene eseguita nel motore di archiviazione e i dati vengono presentati alla maggior parte degli altri componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in uno stato non compresso, limitando gli effetti della compressione negli altri componenti in relazione agli aspetti seguenti:  
-   Operazioni di importazione ed esportazione bulk  
     Quando i dati vengono esportati, anche in formato nativo, il relativo output è in formato di riga non compresso. In questo modo le dimensioni del file di dati esportato potrebbero risultare molto più elevate rispetto ai dati di origine.  
     Quando i dati vengono importati, se la tabella di destinazione è stata abilitata per la compressione i dati vengono convertiti dal motore di archiviazione in formato di riga compresso. In questo modo l'utilizzo della CPU potrebbe aumentare rispetto a una situazione in cui i dati vengono importati in una tabella non compressa.  
     Quando i dati vengono importati in blocco in un heap con compressione di pagina, al momento dell'inserimento dei dati l'operazione di importazione in blocco tenta di comprimerli mediante la compressione di pagina.  
-   La compressione non influisce sulle operazioni di backup e ripristino.  
-   La compressione non influisce sul log shipping.  
-   La compressione dei dati è incompatibile con le colonne di tipo sparse. Non è pertanto possibile comprimere le tavole contenenti colonne di tipo sparse, né aggiungere le colonne di questo tipo a una tabella compressa.  
-   L'abilitazione della compressione può provocare la modifica dei piani di query, in quanto i dati vengono archiviati tramite un numero diverso di pagine e un numero diverso di righe per pagina.  
  
## <a name="see-also"></a>Vedi anche  
 [Implementazione della compressione di riga](../../relational-databases/data-compression/row-compression-implementation.md)   
 [Implementazione della compressione di pagina](../../relational-databases/data-compression/page-compression-implementation.md)   
 [Implementazione della compressione Unicode](../../relational-databases/data-compression/unicode-compression-implementation.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
  

