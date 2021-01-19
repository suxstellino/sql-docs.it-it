---
description: Guida sull'architettura di pagina ed extent
title: Guida sull'architettura di pagina ed extent | Microsoft Docs
ms.custom: ''
ms.date: 03/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- page and extent architecture guide
- guide, page and extent architecture
ms.assetid: 83a4aa90-1c10-4de6-956b-7c3cd464c2d2
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22e1a4832e3ef02d2b596ecd0dd4af3a08a7ec6e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171873"
---
# <a name="pages-and-extents-architecture-guide"></a>Guida sull'architettura di pagina ed extent
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

La pagina è l'unità di base per l'archiviazione dei dati in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Un extent è una raccolta di otto pagine fisicamente contigue. Gli extent aiutano a gestire le pagine in modo efficace. Questa guida descrive le strutture dei dati usate per gestire pagine ed extent in tutte le versioni di SQL Server. La comprensione dell'architettura delle pagine e degli extent è importante per la progettazione e lo sviluppo di database efficienti.

## <a name="pages-and-extents"></a>Pagine ed extent

L'unità di base per l'archiviazione dei dati in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è la pagina. Lo spazio su disco allocato in un file di dati (con estensione mdf o ndf) di un database viene suddiviso logicamente in pagine numerate in modo sequenziale da 0 a n. Le operazioni di I/O su disco vengono eseguite al livello della pagina. Questo significa che SQL Server legge o scrive pagine di dati intere.

Gli extent sono un gruppo di otto pagine fisicamente contigue e vengono utilizzati per gestire in maniera efficiente le pagine. Tutte le pagine sono organizzate in extent.

### <a name="pages"></a>Pagine

In un libro normale tutto il contenuto è scritto nelle pagine. Analogamente a un libro, in SQL Server tutte le righe di dati vengono scritte in pagine. In un libro, tutte le pagine hanno le stesse dimensioni fisiche. Analogamente, in SQL Server tutte le pagine di dati hanno dimensioni uguali a 8 kilobyte. In un libro la maggior parte delle pagine contiene dati, ovvero il contenuto principale del libro, e alcune pagine contengono i metadati relativi al contenuto, ad esempio sommario e indice. Anche in questo caso, SQL Server non è diverso. La maggior parte delle pagine contiene righe di dati effettive archiviate dagli utenti, denominate pagine di dati, e pagine di testo/immagini (per casi speciali). Le pagine di indice contengono riferimenti agli indici relativi alla posizione dei dati e infine vi sono le pagine di sistema in cui sono archiviati diversi metadati relativi all'organizzazione dei dati (pagine PFS, GAM, SGAM, IAM, DCM, BCM). Vedere la tabella seguente per i tipi di pagina e la relativa descrizione.

Come indicato, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] la dimensione della pagina è di 8 KB. I database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] includono pertanto 128 pagine per megabyte. Ogni pagina inizia con un'intestazione di 96 byte utilizzata per archiviare informazioni di sistema relative alla pagina. Queste informazioni includono il numero della pagina, il tipo di pagina, la quantità di spazio disponibile nella pagina e l'ID dell'unità di allocazione dell'oggetto proprietario della pagina.

Nella tabella seguente vengono elencati i tipi di pagina utilizzati nei file di dati di un database [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

|Tipo di pagina | Sommario |
|-------|-------|
|Data |Righe di dati con tutti i dati, ad eccezione di dati text, ntext, image, nvarchar (max), varchar (max), varbinary (max) e xml, quando il testo nella riga è impostato su ON. |
|Indice |Voci di indice. |
|Text/Image |Tipi di dati Large Object: (text, ntext, image, nvarchar(max), varchar(max), varbinary(max) e dati xml) <br> Colonne a lunghezza variabile quando la riga di dati supera 8 KB: (varchar, nvarchar, varbinary e sql_variant) |
|Mappa di allocazione globale (GAM, Global Allocation Map), Mappa di allocazione globale condivisa (SGAM, Shared Global Allocation Map) |Informazioni che indicano se gli extent sono allocati. |
|Spazio libero nella pagina (PFS, Page Free Space) |Informazioni sull'allocazione delle pagine e sullo spazio disponibile nelle pagine. |
|Index Allocation Map |Informazioni sugli extent utilizzati da una tabella o da un indice per unità di allocazione. |
|Mappa delle modifiche di copia bulk (BCM, Bulk Changed Map) |Informazioni sugli extent modificati da operazioni bulk eseguite dopo l'ultima istruzione BACKUP LOG per unità di allocazione. |
|Mappa differenziale delle modifiche (DCM, Differential Changed Map) |Informazioni sugli extent modificati dopo l'esecuzione dell'ultima istruzione BACKUP DATABASE. |

> [!NOTE]
> I file di log non includono pagine, ma solo una serie di record di log.

Le righe di dati vengono inserite in sequenza nella pagina iniziando immediatamente dopo l'intestazione. Una tabella dell'offset delle righe inizia alla fine della pagina. Ogni tabella dell'offset delle righe include una voce per ogni riga della pagina e in ogni voce di offset riga viene registrata la distanza del primo byte della riga dall'inizio della pagina. La funzione della tabella dell'offset delle righe è quindi quella di aiutare SQL Server a trovare rapidamente le righe in una pagina. Le voci della tabella dell'offset delle righe sono in sequenza inversa rispetto a quella delle righe della pagina.

![page_architecture](../relational-databases/media/page-architecture.gif)

#### <a name="large-row-support"></a>Supporto per righe di grandi dimensioni  

Le righe non possono estendersi su più pagine. È tuttavia possibile che parti di una riga vengano spostate all'esterno della pagina della riga in modo che questa possa di fatto essere di grandi dimensioni. La quantità massima di dati e overhead contenuti in una singola riga di una pagina è 8.060 byte (8 KB). Questa limitazione non include tuttavia i dati archiviati nel tipo di pagina Text/Image. 

Questa restrizione è assoluta per tabelle contenenti colonne di tipo varchar, nvarchar, varbinary o sql_variant. Quando la dimensione totale delle righe di tutte le colonne a lunghezza fissa e variabile di una tabella supera il limite di 8.060 byte, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sposta dinamicamente una o più colonne a lunghezza variabile all'interno di pagine nell'unità di allocazione ROW_OVERFLOW_DATA, iniziando dalla colonna con la larghezza maggiore. 

Questa operazione viene eseguita ogni volta che un aggiornamento o un inserimento aumenta la dimensione totale della riga fino a superare il limite di 8.060 byte. Quando una colonna viene spostata in una pagina nell'unità di allocazione ROW_OVERFLOW_DATA, viene mantenuto un puntatore di 24 byte sulla pagina originale nell'unità di allocazione IN_ROW_DATA. Se un'operazione successiva riduce la dimensione della riga, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sposta nuovamente le colonne nella pagina di dati originale in maniera dinamica. 

##### <a name="row-overflow-considerations"></a>Considerazioni sull'overflow della riga 

Come indicato in precedenza, una riga non può risiedere su più pagine e può eseguire l'overflow se la dimensione combinata dei campi di tipo di dati a lunghezza variabile supera il limite di 8060 byte. Ad esempio è possibile creare una tabella con due colonne: una varchar(7000) e un'altra varchar(2000). Se considerata individualmente, nessuna delle due colonne supera il limite di 8060 byte, ma questo può accadere se le colonne vengono combinate e viene riempita l'intera larghezza di ogni colonna. SQL Server potrebbe spostare in modo dinamico la colonna di lunghezza variabile varchar(7000) a pagine dell'unità di allocazione ROW_OVERFLOW_DATA. Quando si combinano colonne di tipo varchar, nvarchar, varbinary, sql_variant o CLR definito dall'utente che superano gli 8.060 byte per riga, tenere presente quanto segue:
-  Lo spostamento di record di grandi dimensioni in un'altra pagina avviene dinamicamente in quanto la lunghezza dei record dipende dalle operazioni di aggiornamento. In seguito a operazioni di aggiornamento che comportano una diminuzione della lunghezza dei record, i record possono venire spostati di nuovo nella pagina originale nell'unità di allocazione IN_ROW_DATA. L'esecuzione di query e di altre operazioni di selezione, ad esempio ordinamenti o join in record di grandi dimensioni contenenti dati di overflow della riga, rallenta i tempi di esecuzione perché questi record vengono elaborati in modo sincrono anziché asincrono.   
   Quando si progetta una tabella con più colonne di tipo varchar, nvarchar, varbinary, sql_variant o CLR definito dall'utente valutare quindi la percentuale di righe in cui probabilmente si verificherà un overflow e la frequenza con cui potrebbero venire eseguite query su questi dati di overflow. Se è probabile che verranno eseguite di frequente query su molte righe di dati di overflow della riga, valutare la possibilità di normalizzare la tabella in modo che alcune colonne vengano spostate in un'altra tabella. Sarà quindi possibile eseguire query in un'operazione JOIN asincrona. 
-  La lunghezza delle singole colonne deve comunque rientrare nel limite di 8.000 byte per le colonne di tipo varchar, nvarchar, varbinary, sql_variant e CLR definito dall'utente. Solo le lunghezze combinate possono superare il limite di 8.060 byte per riga.
-  La somma delle colonne con altri tipi di dati, inclusi i dati char e nchar, deve rientrare nel limite di 8.060 byte per riga. Questo limite non vale inoltre per i dati di oggetti di grandi dimensioni. 
-  La chiave di indice di un indice cluster non può contenere colonne di tipo varchar con dati esistenti nell'unità di allocazione ROW_OVERFLOW_DATA. Se viene creato un indice cluster in una colonna varchar e i dati esistenti si trovano nell'unità di allocazione IN_ROW_DATA, le azioni di inserimento o aggiornamento successive eseguite nella colonna che comporterebbero lo spostamento dei dati all'esterno delle righe avranno esito negativo. Per altre informazioni sulle unità di allocazione, vedere Organizzazione di tabelle e indici.
-  È possibile includere colonne contenenti dati di overflow della riga come colonne chiave o non chiave di un indice non cluster.
-  Il limite per le dimensioni dei record per le tabelle che utilizzano colonne di tipo sparse è 8.018 byte. Quando i dati convertiti più i dati dei record esistenti superano gli 8.018 byte, viene restituito l'[ERRORE 576 MSSQLSERVER](../relational-databases/errors-events/database-engine-events-and-errors.md). Quando le colonne vengono convertite dal tipo sparse al tipo nonsparse e viceversa, il motore di database mantiene una copia dei dati dei record correnti. In questo modo, lo spazio di archiviazione richiesto per il record viene temporaneamente raddoppiato.
-  Per ottenere informazioni su tabelle o indici che potrebbero contenere dati di overflow della riga, usare la funzione a gestione dinamica [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).

### <a name="extents"></a>Extents 

L'extent è l'unità di base in cui viene gestito lo spazio. Un extent è costituito da otto pagine fisicamente contigue, ovvero da 64 KB. I database di SQL Server includono pertanto 16 extent per megabyte.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] include due tipi di extent: 

* Gli extent **uniformi** sono di proprietà di un unico oggetto, pertanto le otto pagine dell'extent possono essere usate solo da tale oggetto.
* Gli extent **misti** possono essere condivisi da un massimo di otto oggetti. Ognuna delle otto pagine dell'extent può essere di proprietà di un oggetto differente.

![Extent uniformi e misti](../relational-databases/media/extents.gif)

Fino a [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] incluso, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non alloca interi extent in tabelle che includono quantità di dati ridotte. In una nuova tabella o in un nuovo indice vengono generalmente allocate pagine da extent misti. Se la tabella o l'indice aumenta di dimensioni fino a includere otto pagine, per le allocazioni successive a esso dirette vengono utilizzati extent uniformi. Se si crea un indice per una tabella esistente che contiene un numero di righe sufficiente per generare otto pagine nell'indice, tutte le allocazioni per l'indice appartengono a extent uniformi. 

A partire da [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], il valore predefinito per la maggior parte delle allocazioni in un database utente e in tempdb consiste nell'usare extent uniformi, ad eccezione delle allocazioni appartenenti alle prime otto pagine di una [catena IAM](#IAM). Le allocazioni per i database master, msdb e modello continuano a mantenere il comportamento precedente. 

> [!NOTE]
> Fino a [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] incluso, è possibile usare il flag di traccia 1118 per modificare l'allocazione predefinita in modo da usare sempre extent uniformi. Per altre informazioni su questo flag di traccia, vedere [DBCC TRACEON - Flag di traccia](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).   
>   
> A partire da [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], la funzionalità fornita dal flag di traccia 1118 è abilitata automaticamente per tempdb e tutti i database utente. Per i database utente, questo comportamento è controllato dall'opzione `SET MIXED_PAGE_ALLOCATION` di `ALTER DATABASE`, con il valore predefinito OFF, e il flag di traccia 1118 non ha alcun effetto. Per altre informazioni, vedere [Opzioni ALTER DATABASE SET (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-set-options.md).

A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], la funzione di sistema `sys.dm_db_database_page_allocations` può segnalare le informazioni sull'allocazione delle pagine per un database, una tabella, un indice e una partizione.

> [!IMPORTANT]
> La funzione di sistema `sys.dm_db_database_page_allocations` non è documentata ed è soggetta a modifiche. Non è garantita la compatibilità. 

A partire da [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], la funzione di sistema [sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) è disponibile e restituisce informazioni su una pagina in un database. La funzione restituisce una riga che contiene le informazioni di intestazione della pagina, tra cui object_id, index_id e partition_id. Questa funzione sostituisce l'uso di `DBCC PAGE` nella maggior parte dei casi.

## <a name="managing-extent-allocations-and-free-space"></a>Gestione delle allocazioni di extent e dello spazio libero 

Le strutture di dati di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che consentono di gestire le allocazioni degli extent e di tenere traccia dello spazio su disco sono organizzate in modo relativamente semplice. Ciò offre i vantaggi seguenti: 

* Le informazioni relative allo spazio libero sono compresse al massimo e occupano pertanto un numero ridotto di pagine.   
  Questa caratteristica comporta un aumento della velocità a causa della riduzione della quantità di letture su disco necessarie per recuperare le informazioni sull'allocazione. In questo modo, aumenta inoltre la probabilità che le pagine di allocazione vengano mantenute nella memoria e non richiedano ulteriori letture. 

* La maggior parte delle informazioni relative all'allocazione non è concatenata. Questo aspetto semplifica la gestione delle informazioni sull'allocazione.    
  Ogni allocazione o deallocazione di pagina può essere eseguita in modo rapido. Questa caratteristica riduce la contesa tra attività simultanee di allocazione o deallocazione di pagine. 

### <a name="managing-extent-allocations"></a>Gestione delle allocazioni di extent

In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vengono usati due tipi di mappe per la registrazione delle allocazioni di extent: 

- **Mappa di allocazione globale (GAM, Global Allocation Map)**    
  Nelle pagine GAM vengono registrati gli extent allocati. In ogni pagina GAM possono essere registrati riferimenti a 64.000 extent, ovvero a circa 4 gigabyte (GB) di dati. La pagina GAM include 1 bit per ogni extent dell'intervallo che la riguarda. Se il bit è 1, l'extent è disponibile, mentre se è 0, l'extent è allocato. 

- **Mappa di allocazione globale condivisa (SGAM, Shared Global Allocation Map)**    
  Nelle pagine SGAM vengono registrate informazioni sugli extent misti con almeno una pagina inutilizzata. In ogni pagina SGAM possono essere registrati riferimenti a 64.000 extent, ovvero a circa 4 GB di dati. La pagina SGAM include 1 bit per ogni extent dell'intervallo che la riguarda. Se il bit è 1, l'extent viene utilizzato come extent misto e include una pagina disponibile. Se il bit è 0, l'extent non viene utilizzato come extent misto o rappresenta un extent misto di cui sono in uso tutte le pagine. 

Nelle pagine GAM e SGAM per ogni extent sono impostati gli schemi di bit indicati di seguito, in base all'utilizzo corrente dell'extent. 

|Utilizzo corrente dell'extent | Impostazione del bit nella pagina GAM | Impostazione del bit nella pagina SGAM |
|---------|----------|------| 
|Disponibile, non utilizzato |1 |0 |
|Extent uniforme oppure extent misto senza spazio libero |0 |0 |
|Extent misto con pagine disponibili |0 |1 |
 
Ciò consente di utilizzare semplici algoritmi di gestione degli extent. 
-   Per allocare un extent uniforme, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] cerca un bit 1 nella pagina GAM e lo imposta su 0. 
-   Per cercare un extent misto con pagine libere, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] cerca un bit 1 nella pagina SGAM. 
-   Per allocare un extent misto, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] cerca un bit 1 nella pagina GAM, lo imposta su 0 e quindi imposta su 1 anche il bit corrispondente nella pagina SGAM. 
-   Per deallocare un extent, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verifica che il bit GAM sia impostato su 1 e il bit SGAM su 0. Gli algoritmi effettivamente usati internamente da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sono più sofisticati rispetto a quanto descritto in questo articolo, poiché [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] distribuisce dati in un database in modo uniforme. Anche gli algoritmi reali, tuttavia, risultano semplificati, in quanto non devono gestire catene di informazioni sull'allocazione degli extent.

### <a name="tracking-free-space"></a>Rilevamento dello spazio libero

Le pagine **PFS (Page Free Space, Spazio libero nella pagina)** consentono di rilevare lo stato di allocazione di ogni pagina, se una singola pagina è stata allocata e la quantità di spazio libero in ogni pagina. PFS include 1 byte per ogni pagina, indicando se la pagina è allocata e, in tal caso, se si tratta di una pagina vuota, in uso dall'1% al 50%, dal 51% all'80%, dall'81% al 95% o dal 96% al 100%.

Dopo che un extent è stato allocato a un oggetto, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilizza le pagine PFS per registrare le pagine dell'extent allocate e quelle disponibili. Queste informazioni vengono utilizzate quando [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] deve allocare una nuova pagina. La quantità di spazio libero in una pagina viene mantenuta solo per le pagine heap e text/image. Questo spazio viene utilizzato quando [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] deve trovare una pagina con spazio libero disponibile per includere una nuova riga inserita. Per gli indici non è necessario tenere traccia dello spazio libero nella pagina, in quanto il punto di inserimento di una nuova riga viene impostato dai valori delle chiavi di indice.

Viene aggiunta una nuova pagina PFS, GAM o SGAM al file di dati per ogni intervallo aggiuntivo di cui tiene traccia. Pertanto è presente una nuova pagina PFS 8.088 pagine dopo la prima pagina PFS e altre pagine PFS a intervalli successivi di 8.088 pagine. Ad esempio l'ID pagina 1 è una pagina PFS, l'ID pagina 8088 è una pagina PFS, l'ID pagina 16176 è una pagina PFS e così via. Una nuova pagina GAM è presente 64.000 extent dopo la prima pagina GAM e tiene traccia di 64.000 extent successivi; la sequenza continua a intervalli di 64.000 extent. In modo analogo, è presente una nuova pagina SGAM 64.000 extent dopo la prima pagina SGAM e sono presenti altre pagine SGAM a intervalli di 64.000 extent successivi. Nella figura seguente viene illustrata la sequenza di pagine utilizzata da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per allocare e gestire gli extent.

![manage_extents](../relational-databases/media/manage-extents.gif)

## <a name="managing-space-used-by-objects"></a><a name="IAM"></a> Gestione dello spazio usato dagli oggetti 

Una pagina **IAM (Index Allocation Map)** esegue il mapping degli extent in una parte da 4 GB di un file di database usato da un'unità di allocazione. Le unità di allocazione possono essere di tre tipi:

- IN_ROW_DATA   
    Contiene una partizione di un heap o di un indice.

- LOB_DATA   
   Contiene tipi di dati Large Object (LOB), ad esempio XML, VARBINARY (max) e VARCHAR(max).

- ROW_OVERFLOW_DATA   
   Contiene dati a lunghezza variabile archiviati in colonne VARCHAR, NVARCHAR, VARBINARY o SQL_VARIANT che superano il limite della lunghezza di riga di 8.060 byte. 

Ogni partizione di un heap o di un indice contiene almeno un'unità di allocazione IN_ROW_DATA. Può inoltre contenere un'unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA, a seconda dello schema dell'heap o dell'indice.

Una pagina IAM include informazioni relative a un intervallo di 4 GB di un file, che corrisponde a quello di una pagina GAM o SGAM. Se l'unità di allocazione contiene extent derivati da più file o più intervalli di 4 GB di un file, saranno presenti più pagine IAM concatenate in una catena IAM. A ogni unità di allocazione corrisponde pertanto almeno una pagina IAM per ogni file in cui sono inclusi extent per l'unità di allocazione. Le pagine IAM per un file possono inoltre essere più di una se il numero di extent del file allocati all'unità di allocazione è maggiore del numero di extent che una pagina IAM è in grado di registrare. 

![iam_pages](../relational-databases/media/iam-pages.gif)

Le pagine IAM vengono allocate per ogni unità di allocazione in base alle necessità e vengono posizionate in modo casuale nel file. La vista di sistema `sys.system_internals_allocation_units` punta alla prima pagina IAM relativa a un'unità di allocazione. Tutte le pagine IAM di tale unità di allocazione sono concatenate in una catena IAM.

> [!IMPORTANT]
> La vista di sistema `sys.system_internals_allocation_units` è solo per uso interno ed è soggetta a modifiche. Non è garantita la compatibilità. Questa vista non è disponibile nel [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 

![iam_chain](../relational-databases/media/iam-chain.gif)
 
Pagine IAM collegate in catena per unità di allocazione Ogni pagina IAM include un'intestazione che indica l'extent iniziale dell'intervallo di extent sul quale viene eseguito il mapping dalla pagina IAM. La pagina IAM include inoltre una mappa di bit di grandi dimensioni in cui ogni bit rappresenta un extent. Il primo bit della mappa rappresenta il primo extent dell'intervallo, il secondo bit rappresenta il secondo extent e così via. Se un bit è 0, significa che l'extent che rappresenta non è allocato all'unità di allocazione proprietaria della pagina IAM. Se il bit è 1, significa che l'extent che rappresenta è allocato all'unità di allocazione proprietaria della pagina IAM.

Se in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è necessario inserire una nuova riga ma lo spazio libero nella pagina non è sufficiente, vengono utilizzate le pagine IAM e PFS per trovare una pagina per l'allocazione o, nel caso di un heap o di una pagina di tipo text/image, una pagina in cui sia disponibile spazio sufficiente per la riga da inserire. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilizza le pagine IAM per trovare gli extent allocati all'unità di allocazione. Per ogni extent, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] cerca le pagine PFS per verificare se esiste una pagina da utilizzare. Poiché ogni pagina IAM e PFS include i dati di un numero di pagine elevato, il numero di pagine IAM e PFS di un database è ridotto. Ciò significa che le pagine IAM e PFS in genere risiedono nella memoria del pool di buffer di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e che pertanto è possibile eseguire rapidamente ricerche al loro interno. Per gli indici, il punto di inserimento di una nuova riga viene impostato dalla chiave indice, ma quando è necessaria una nuova pagina si verifica il processo descritto in precedenza.

In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] viene allocato un nuovo extent a un'unità di allocazione solo se non viene trovata rapidamente una pagina di un extent esistente in cui sia disponibile spazio sufficiente per la riga da inserire. 

<a name="ProportionalFill"></a> In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] gli extent da allocare vengono selezionati tra quelli disponibili nel filegroup usando un **algoritmo di allocazione riempimento proporzionale**. Se un filegroup include due file e in uno lo spazio libero è doppio rispetto all'altro, vengono allocate due pagine del file con lo spazio libero maggiore per ogni pagina allocata dell'altro file. Ciò significa che la percentuale di spazio utilizzato deve essere analoga in tutti i file di un filegroup. 

## <a name="tracking-modified-extents"></a>Rilevamento degli extent modificati 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa due strutture di dati interne per rilevare gli extent modificati mediante operazioni di copia bulk e quelli modificati dopo il backup completo più recente. Queste strutture di dati consentono di accelerare in misura significativa le operazioni di backup differenziale. Esse accelerano inoltre la registrazione delle operazioni di copia bulk con database per i quali viene utilizzato il modello di recupero con registrazione minima delle operazioni bulk. Come le pagine mappa di allocazione globale (GAM, Global Allocation Map) e mappa di allocazione globale condivisa (SGAM, Shared Global Allocation Map), queste strutture sono mappe di bit in cui ogni bit rappresenta un singolo extent. 

- **Mappa differenziale delle modifiche (DCM, Differential Changed Map)**    
   Rileva gli extent modificati dopo l'esecuzione dell'ultima istruzione `BACKUP DATABASE`. Se il bit di un extent è 1, l'extent è stato modificato dopo l'esecuzione dell'ultima istruzione `BACKUP DATABASE`. Se invece è 0, l'extent non è stato modificato. Durante il backup differenziale vengono lette solo le pagine DCM per identificare gli extent modificati. In questo modo, il numero di pagine di cui è necessario eseguire l'analisi durante il backup differenziale risulta notevolmente ridotto. La durata dell'esecuzione di un backup differenziale è proporzionale al numero di extent modificati dopo l'esecuzione dell'ultima istruzione BACKUP DATABASE e non alle dimensioni complessive del database. 

- **Mappa delle modifiche di copia bulk (BCM, Bulk Changed Map)**    
   Rileva gli extent modificati mediante operazioni con registrazione minima delle operazioni bulk dall'esecuzione dell'ultima istruzione `BACKUP LOG`. Se il bit di un extent è 1, l'extent è stato modificato mediante un'operazione con registrazione minima delle operazioni bulk dall'esecuzione dell'ultima istruzione `BACKUP LOG`. Se invece è 0, l'extent non è stato modificato mediante operazioni con registrazione minima delle operazioni bulk. Sebbene le pagine BCM siano presenti in tutti i database, esse risultano utili solo se il database utilizza il modello di recupero con registrazione minima delle operazioni bulk. In questo modello di recupero, quando viene eseguita un'istruzione `BACKUP LOG` il processo di backup esegue l'analisi delle pagine BCM per identificare gli extent che sono stati modificati, quindi li include nel backup del log. In questo modo, se il database viene ripristinato da un backup di database e da una sequenza di backup del log delle transazioni, viene eseguito il recupero delle operazioni con registrazione minima delle operazioni bulk. Le pagine BCM non risultano utili in database che utilizzano il modello di recupero con registrazione minima in quanto non prevede la registrazione delle operazioni con registrazione minima delle operazioni bulk, né in database che utilizzano il modello di recupero con registrazione completa in quanto, con questo modello, le operazioni con registrazione minima delle operazioni bulk vengono considerate come operazioni con registrazione completa delle operazioni bulk. 

L'intervallo tra le pagine DCM e BCM corrisponde all'intervallo tra le pagine GAM e SGAM, ovvero 64.000 extent. All'interno di un file fisico le pagine DCM e BCM sono seguite dalle pagine GAM e SGAM:

![special_page_order](../relational-databases/media/special-page-order.gif)

## <a name="see-also"></a>Vedere anche
[sys.allocation_units &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
[Heap &#40;tabelle senza indici cluster&#41;](../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures)       
[sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)     
[Lettura di pagine](../relational-databases/reading-pages.md)   
[Scrittura di pagine](../relational-databases/writing-pages.md)   
