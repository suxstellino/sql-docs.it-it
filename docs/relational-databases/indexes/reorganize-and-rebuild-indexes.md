---
title: Rilevamento e risoluzione di indici frammentati
description: Questo articolo descrive come si verifica la frammentazione degli indici, come rilevare il livello di frammentazione esistente e come determinare l'opzione migliore per risolvere la frammentazione di un indice usando T-SQL e SQL Server Management Studio.
ms.custom: ''
ms.date: 04/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.index.rebuild.f1
- sql13.swb.indexproperties.fragmentation.f1
- sql13.swb.index.reorg.f1
helpviewer_keywords:
- large object defragmenting
- indexes [SQL Server], reorganizing
- index reorganization [SQL Server]
- reorganizing indexes
- defragmenting large object data types
- index fragmentation [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- indexes [SQL Server], rebuilding
- defragmenting indexes
- nonclustered indexes [SQL Server], defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
- LOB data [SQL Server], defragmenting
- clustered indexes, defragmenting
author: pmasl
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae12478e495589f2c87ac4d3a2ebb924f173f7dd
ms.sourcegitcommit: 3bb5ea67dc0d369b921f1bee4ffd4317aba2253c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107720486"
---
# <a name="resolve-index-fragmentation-by-reorganizing-or-rebuilding-indexes"></a>Risolvere la frammentazione degli indici tramite riorganizzazione o ricompilazione di questi

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-pdw.md)]

Questo articolo descrive come si verifica la deframmentazione degli indici e ne illustra l'impatto sulle prestazioni delle query. Dopo aver determinato il [livello di frammentazione esistente per un indice](#detecting-the-amount-of-fragmentation), è possibile deframmentare un indice [riorganizzando un indice](#reorganize-an-index) o [ricompilando un indice](#rebuild-an-index) eseguendo i comandi di Transact-SQL nello strumento preferito o usando SQL Server Management Studio.

> [!Note]
> Le informazioni contenute in questo articolo non si applicano a un pool SQL dedicato in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] . Per informazioni sulla manutenzione degli indici per un pool SQL dedicato in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] , vedere Indicizzazione di tabelle di pool SQL dedicate [in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] ](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-index).

## <a name="index-fragmentation-overview"></a>Panoramica della frammentazione dell'indice

Che cos'è la frammentazione dell'indice e perché è opportuno occuparsene:

- La frammentazione si verifica quando negli indici sono presenti pagine in cui l'ordinamento logico all'interno dell'indice, basato sul valore chiave dell'indice, non corrisponde all'ordinamento fisico all'interno delle pagine dell'indice.
- Tramite il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] la modifica degli indici viene eseguita automaticamente dopo ogni operazione di modifica, inserimento o eliminazione dei dati sottostanti. Ad esempio, l'aggiunta di righe in una tabella può comportare la suddivisione delle pagine esistenti negli indici rowstore per fare spazio per l'inserimento di nuovi valori di chiave. Nel tempo, queste modifiche possono provocare la frammentazione dell'indice nel database. La frammentazione si verifica quando negli indici sono presenti pagine in cui l'ordinamento logico, basato sul valore chiave, non corrisponde all'ordinamento fisico all'interno del file di dati.
- Gli indici molto frammentati possono compromettere le prestazioni delle query perché sono necessarie operazioni di I/O aggiuntive per individuare i dati a cui puntano gli indici. Un maggior numero di operazioni di I/O causa un rallentamento nella risposta dell'applicazione, soprattutto quando sono implicate azioni di analisi.

## <a name="detecting-the-amount-of-fragmentation"></a>Rilevamento del livello di frammentazione

Il primo passaggio per decidere il metodo di deframmentazione dell'indice da usare consiste nell'eseguire un'analisi dell'indice per determinare il grado di frammentazione. La frammentazione viene rilevata in modo diverso per gli indici rowstore e columnstore.

> [!NOTE]
> È particolarmente importante verificare la frammentazione dell'indice o dell'heap dopo l'eliminazione di grandi quantità di dati. Per gli heap, se sono presenti aggiornamenti frequenti, potrebbe essere necessario anche esaminare la frammentazione per evitare la proliferazione di record di inoltro. Per altre informazioni sugli heap, vedere [Heap (tabelle senza indici cluster)](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures). 

### <a name="detecting-fragmentation-of-rowstore-indexes"></a>Rilevamento della frammentazione degli indici rowstore

È possibile usare [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) per rilevare la frammentazione in un indice specifico, in tutti gli indici di una tabella o vista indicizzata, in tutti gli indici di un database o in tutti gli indici di tutti i database. Per gli indici partizionati, **sys.dm_db_index_physical_stats** fornisce anche informazioni sulla frammentazione per ogni partizione.

Il set di risultati restituito da **sys.dm_db_index_physical_stats** include le colonne seguenti:

|Colonna|Descrizione|
|------------|-----------------|
|**avg_fragmentation_in_percent**|Percentuale di frammentazione logica (pagine non ordinate nell'indice).|
|**fragment_count**|Numero di frammenti (pagine foglia fisicamente consecutive) nell'indice.|
|**avg_fragment_size_in_pages**|Numero medio di pagine in un frammento di un indice.|

Una volta noto il grado di frammentazione, usare la tabella seguente per determinare il metodo migliore per la rimozione della frammentazione: [INDEX REORGANIZE](#reorganize-an-index) o [INDEX](#rebuild-an-index).

|Valore di **avg_fragmentation_in_percent**|Istruzione correttiva|
|-----------------------------------------------|--------------------------|
|> 5% e < = 30% <sup>1</sup>|ALTER INDEX REORGANIZE|
|> 30% <sup>1</sup>|ALTER INDEX REBUILD WITH (ONLINE = ON) <sup>2</sup>|

<sup>1</sup> Questi valori costituiscono un'indicazione approssimativa per determinare il punto in cui passare da `ALTER INDEX REORGANIZE` a `ALTER INDEX REBUILD`. I valori effettivi, in realtà, variano da caso a caso. È importante riuscire a determinare la soglia migliore per l'ambiente in uso.      

> [!TIP] 
> Se ad esempio un determinato indice viene usato principalmente per le operazioni di analisi, la rimozione della frammentazione può migliorare le prestazioni di tali operazioni. Il vantaggio in termini di prestazioni potrebbe essere meno evidente per gli indici usati principalmente per le operazioni di ricerca.    
Analogamente, la rimozione della frammentazione in un heap (una tabella senza indici cluster) è particolarmente utile per le operazioni di analisi degli indici non cluster, ma ha un effetto ridotto nelle operazioni di ricerca.

<sup>2</sup> È possibile eseguire la ricompilazione di un indice online oppure offline. La riorganizzazione di un indice viene sempre eseguita online. Per ottenere una disponibilità simile a quella offerta dall'opzione di riorganizzazione è necessario ricompilare gli indici in modalità online. Per altre informazioni, vedere [INDEX](#rebuild-an-index) ed [Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md).

Non è necessario deframmentare gli indici con frammentazione pari o inferiore al 5% perché i vantaggi offerti dalla rimozione di una frammentazione così limitata sono quasi sempre annullati dal costo a livello di CPU della riorganizzazione o della ricompilazione dell'indice. Inoltre la ricompilazione o la riorganizzazione di indici rowstore di dimensioni ridotte in genere non riduce effettivamente la frammentazione. Fino a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], incluso, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] alloca spazio usando extent misti. Per questo motivo, le pagine di indici di dimensioni ridotte vengono talvolta archiviate in extent misti. Poiché gli extent misti possono essere condivisi al massimo da otto oggetti, la frammentazione in un indice di dimensioni ridotte potrebbe non ridursi dopo la riorganizzazione o la ricompilazione dell'indice. Vedere anche [Considerazioni specifiche per la ricompilazione degli indici rowstore](#considerations-specific-to-rebuilding-rowstore-indexes). Per altre informazioni sugli extent, vedere la [Guida sull'architettura di pagina ed extent](../../relational-databases/pages-and-extents-architecture-guide.md#extents).

### <a name="detecting-fragmentation-of-columnstore-indexes"></a>Rilevamento della frammentazione degli indici columnstore

Usando [sys.dm_db_column_store_row_group_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md) è possibile determinare la percentuale di righe eliminate in un indice, ovvero una misura ragionevole per la frammentazione in un rowgroup di un indice columnstore. Usare queste informazioni per calcolare la frammentazione in un indice specifico, in tutti gli indici di una tabella, in tutti gli indici di un database o in tutti gli indici di tutti i database.

Il set di risultati restituito da **sys.dm_db_column_store_row_group_physical_stats** include le colonne seguenti:

|Colonna|Descrizione|
|------------|-----------------|
|**total_rows**|Numero di righe archiviate fisicamente nel gruppo di righe. Per i gruppi di righe compressi, sono incluse le righe contrassegnate come eliminate.|
|**deleted_rows**|Numero di righe archiviate fisicamente in un gruppo di righe compresso contrassegnate per l'eliminazione. 0 per i gruppi di righe presenti nell'archivio differenziale.|

Usare queste informazioni restituite per calcolare la frammentazione dell'indice usando la formula seguente:

```
100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0)
```

Una volta noto il grado di frammentazione dell'indice, usare la tabella seguente per determinare il metodo migliore per la rimozione della frammentazione: [INDEX REORGANIZE](#reorganize-an-index) o [INDEX](#rebuild-an-index).

|Valore di **computed fragmentation in percent**|Si applica alla versione|Istruzione correttiva|
|-----------------------------------------------|--------------------------|--------------------------|
|> = 20%|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|ALTER INDEX REBUILD|
|> = 20%|A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|ALTER INDEX REORGANIZE|

### <a name="to-check-the-fragmentation-of-a-rowstore-index-using-tsql"></a>Per controllare la frammentazione di un indice rowstore usando [!INCLUDE[tsql](../../includes/tsql-md.md)]

L'esempio seguente consente di trovare la percentuale di frammentazione media di tutti gli indici nella tabella `HumanResources.Employee` del database `AdventureWorks2016`.

```sql
SELECT a.object_id, object_name(a.object_id) AS TableName,
    a.index_id, name AS IndedxName, avg_fragmentation_in_percent
FROM sys.dm_db_index_physical_stats
    (DB_ID (N'AdventureWorks2016_EXT')
        , OBJECT_ID(N'HumanResources.Employee')
        , NULL
        , NULL
        , NULL) AS a
INNER JOIN sys.indexes AS b
    ON a.object_id = b.object_id
    AND a.index_id = b.index_id;
GO
```

L'istruzione precedente restituisce un set di risultati simile al seguente.

```
object_id   TableName    index_id    IndexName                                             avg_fragmentation_in_percent
----------- ------------ ----------- ----------------------------------------------------- ------------------------------
1557580587  Employee     1           PK_Employee_BusinessEntityID                          0
1557580587  Employee     2           IX_Employee_OrganizationalNode                        0
1557580587  Employee     3           IX_Employee_OrganizationalLevel_OrganizationalNode    0
1557580587  Employee     5           AK_Employee_LoginID                                   66.6666666666667
1557580587  Employee     6           AK_Employee_NationalIDNumber                          50
1557580587  Employee     7           AK_Employee_rowguid                                   0

(6 row(s) affected)
```

Per altre informazioni, vedere [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).

### <a name="to-check-the-fragmentation-of-a-columnstore-index-using-transact-sql"></a>Per controllare la frammentazione di un indice columnstore usando Transact-SQL

L'esempio seguente consente di trovare la percentuale di frammentazione media di tutti gli indici nella tabella `dbo.FactResellerSalesXL_CCI` del database `AdventureWorksDW2016`.

```sql
SELECT i.object_id,
    object_name(i.object_id) AS TableName,
    i.index_id,
    i.name AS IndexName,
    100*(ISNULL(SUM(CSRowGroups.deleted_rows),0))/NULLIF(SUM(CSRowGroups.total_rows),0) AS 'Fragmentation'
FROM sys.indexes AS i  
INNER JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups
    ON i.object_id = CSRowGroups.object_id
    AND i.index_id = CSRowGroups.index_id
WHERE object_name(i.object_id) = 'FactResellerSalesXL_CCI'
GROUP BY i.object_id, i.index_id, i.name
ORDER BY object_name(i.object_id), i.name;
```

L'istruzione precedente restituisce un set di risultati simile al seguente.

```
object_id   TableName                   index_id    IndexName                       Fragmentation
----------- --------------------------- ----------- ------------------------------- ---------------
114099447   FactResellerSalesXL_CCI     1           IndFactResellerSalesXL_CCI      0

(1 row(s) affected)
```

### <a name="check-index-fragmentation-using-sql-server-management-studio"></a>Controllare la frammentazione di un indice usando SQL Server Management Studio

> [!NOTE]
> [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] non può essere usato per calcolare la frammentazione degli indici columnstore in SQL Server e non può essere usato per calcolare la frammentazione di nessun indice nel database SQL di Azure. Usare l'[esempio](#to-check-the-fragmentation-of-a-columnstore-index-using-transact-sql) di [!INCLUDE[tsql](../../includes/tsql-md.md)] riportato in precedenza.

1. In Esplora oggetti espandere il database contenente la tabella in cui si desidera controllare la frammentazione di un indice.
2. Espandere la cartella **Tabelle** .
3. Espandere la tabella in cui si desidera controllare la frammentazione di un indice.
4. Espandere la cartella **Indici** .
5. Fare clic con il pulsante destro del mouse sull'indice di cui si vuole controllare la frammentazione e scegliere **Proprietà**.
6. In **Selezione pagina** selezionare **Frammentazione**.

Le informazioni seguenti sono disponibili nella pagina **Frammentazione** :

|valore|Descrizione|
|---|---|
|**Livello di riempimento pagina**|Indica il livello medio di riempimento delle pagine di indice, espresso come percentuale. Il valore 100% indica che le pagine di indice sono completamente piene. Il valore 50% indica che ogni pagina di indice è piena all'incirca per metà.|
|**Frammentazione totale**|Percentuale di frammentazione logica. Indica il numero di pagine di un indice che non sono archiviate in ordine.|
|**Dimensioni medie delle righe**|Dimensioni medie di una riga al livello foglia.|
|**Livello nidificazione**|Numero di livelli dell'indice, compreso il livello foglia.|
|**Record inoltrati**|Numero di record in un heap che hanno inoltrato puntatori a un altro percorso dei dati. Questo stato si verifica durante un aggiornamento, nel caso in cui non vi sia spazio sufficiente per archiviare la riga nel percorso originale.|
|**Righe fantasma**|Numero di righe contrassegnate come eliminate ma non ancora rimosse. Queste righe verranno rimosse da un thread di pulitura nel momento in cui il server non è occupato. Questo valore non comprende le righe mantenute a causa di una transazione di isolamento dello snapshot in attesa.|
|**Tipo di indice**|Tipo di indice. I valori possibili sono **Indice cluster**, **Indice non cluster** e **XML primario**. È inoltre possibile archiviare le tabelle come heap (senza indici), ma in questo caso non sarà possibile aprire la pagina Proprietà indice.|
|**Righe al livello foglia**|Numero di righe al livello foglia.|
|**Dimensioni massime righe**|Dimensioni massime delle righe al livello foglia.|
|**Dimensioni minime righe**|Dimensioni minime delle righe al livello foglia.|
|**Pagine**|Numero totale di pagine di dati.|
|**ID partizione**|ID partizione dell'albero B contenente l'indice.|
|**Righe fantasma versione**|Numero di record fantasma mantenuti a causa di una transazione di isolamento dello snapshot in attesa.|

## <a name="defragmenting-indexes-by-rebuilding-or-reorganizing-the-index"></a>Deframmentazione di indici tramite ricompilazione o riorganizzazione dell'indice

Per deframmentare un indice frammentato, usare uno dei metodi seguenti:

- Riorganizzazione dell'indice
- Ricompilazione dell'indice

> [!NOTE]
> Per gli indici partizionati compilati in base a uno schema di partizione, è possibile usare uno dei metodi seguenti su un intero indice o su una singola partizione.

### <a name="reorganize-an-index"></a>Riorganizzare un indice

La riorganizzazione di un indice richiede una quantità minima di risorse di sistema ed è un'operazione online. Ciò significa che i blocchi di tabella a lungo termine non vengono mantenuti attivi e le query o gli aggiornamenti inerenti la tabella sottostante possono continuare durante la transazione `ALTER INDEX REORGANIZE`.

- Per gli [indici rowstore](clustered-and-nonclustered-indexes-described.md), il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] deframmenta il livello foglia di indici cluster e non cluster di tabelle e viste tramite il riordinamento fisico delle pagine al livello foglia in base all'ordine logico dei nodi foglia (da sinistra verso destra). La riorganizzazione compatta anche le pagine di indice in base al valore del fattore di riempimento dell'indice. Per visualizzare l'impostazione del fattore di riempimento, usare [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md). Per esempi di sintassi, vedere [Esempi: Indici rowstore](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

- Quando si usano gli [indici columnstore](columnstore-indexes-overview.md), l'archivio differenziale potrebbe ritrovarsi con più rowgroup di piccole dimensioni dopo l'inserimento, l'aggiornamento e l'eliminazione dei dati nel tempo. La riorganizzazione di un indice columnstore consiste nel forzare l'inserimento di tutti i rowgroup nel columnstore per poi combinarli generando un numero minore di rowgroup con più righe. L'operazione di riorganizzazione rimuove anche le righe che sono state eliminate dal columnstore. La riorganizzazione inizialmente richiede risorse di CPU aggiuntive per comprimere i dati, il che potrebbe globalmente ridurre le prestazioni del sistema. Tuttavia, non appena i dati sono compressi, le prestazioni delle query migliorano. Per esempi di sintassi, vedere [Esempi: Indici Columnstore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes).

### <a name="rebuild-an-index"></a>Ricompilare un indice

La ricompilazione di un indice consiste nell'eliminazione e nella ricreazione dell'indice. A seconda del tipo di indice e della versione del [!INCLUDE[ssde_md](../../includes/ssde_md.md)], è possibile eseguire un'operazione di ricompilazione online o offline. Per la sintassi T-SQL, vedere [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes)

- Per gli [indici rowstore](clustered-and-nonclustered-indexes-described.md), la ricompilazione ha l'effetto di rimuovere la frammentazione, recuperare spazio su disco grazie alla compattazione delle pagine in base all'impostazione del fattore di riempimento esistente o specificata e riordinare le righe dell'indice in pagine contigue. Quando viene specificata la parola chiave `ALL`, tutti gli indici della tabella vengono eliminati e ricompilati in una singola transazione. Non è necessario eliminare in anticipo i vincoli di chiave esterna. Quando vengono ricompilati indici con un numero di extent pari o superiore a 128, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] posticipa le effettive deallocazioni delle pagine e i blocchi associati fino al termine del commit della transazione. Per esempi di sintassi, vedere [Esempi: Indici Rowstore](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

- Per gli [indici columnstore](columnstore-indexes-overview.md), la ricompilazione ha l'effetto di rimuovere la frammentazione, spostare tutte le righe nel columnstore e recuperare spazio su disco eliminando fisicamente le righe che sono state eliminate logicamente dalla tabella. 
  
  > [!TIP]
  > A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la ricompilazione dell'indice columnstore in genere non è necessaria perché `REORGANIZE` esegue gran parte della ricompilazione in background come operazione online. 
  
  Per esempi di sintassi, vedere [Esempi: Indici Columnstore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes).

### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

È richiesta l'autorizzazione `ALTER` per la tabella o la vista. L'utente deve essere un membro di almeno uno dei ruoli seguenti:

- Ruolo del database **db_ddladmin**<sup>1</sup>
- Ruolo del database **db_owner**
- Ruolo del server **sysadmin**

<sup>1</sup>Il ruolo del database **db_ddladmin** è quello con i [minori privilegi](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models).

### <a name="remove-fragmentation-using-ssmanstudiofull"></a><a name="SSMSProcedureReorg"></a> Rimuovere la frammentazione tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

#### <a name="to-reorganize-or-rebuild-an-index"></a>Per riorganizzare o ricompilare un indice

1. In Esplora oggetti espandere il database che contiene la tabella in cui si desidera riorganizzare un indice.
2. Espandere la cartella **Tabelle** .
3. Espandere la tabella in cui si desidera riorganizzare un indice.
4. Espandere la cartella **Indici** .
5. Fare clic con il pulsante destro del mouse sull'indice che si vuole riorganizzare e scegliere **Riorganizza**.
6. Nella finestra di dialogo **Riorganizza indici** verificare che nella griglia **Indici da riorganizzare** sia presente l'indice corretto, quindi scegliere **OK**.
7. Selezionare la casella di controllo **Compatta dati di colonne LOB** per specificare che tutte le pagine che contengono dati LOB vengano compattate.
8. Scegliere **OK.**

#### <a name="to-reorganize-all-indexes-in-a-table"></a>Per riorganizzare tutti gli indici in una tabella

1. In Esplora oggetti espandere il database che contiene la tabella in cui si desidera riorganizzare gli indici.
2. Espandere la cartella **Tabelle** .
3. Espandere la tabella in cui si desidera riorganizzare gli indici.
4. Fare clic con il pulsante destro del mouse sulla cartella **Indici** e scegliere **Riorganizza tutto**.
5. Nella finestra di dialogo **Riorganizza indici** verificare che nella griglia **Indici da riorganizzare** siano presenti gli indici corretti. Per rimuovere un indice dalla griglia **Indici da riorganizzare** , selezionare l'indice desiderato e premere CANC.
6. Selezionare la casella di controllo **Compatta dati di colonne LOB** per specificare che tutte le pagine che contengono dati LOB vengano compattate.
7. Scegliere **OK.**

#### <a name="to-rebuild-an-index"></a>Per ricompilare un indice

1. In Esplora oggetti espandere il database che contiene la tabella in cui si desidera riorganizzare un indice.
2. Espandere la cartella **Tabelle** .
3. Espandere la tabella in cui si desidera riorganizzare un indice.
4. Espandere la cartella **Indici** .
5. Fare clic con il pulsante destro del mouse sull'indice che si vuole riorganizzare e scegliere **Ricompila**.
6. Nella finestra di dialogo **Ricompila indici** verificare che nella griglia **Indici da ricompilare** sia presente l'indice corretto, quindi scegliere **OK**.
7. Selezionare la casella di controllo **Compatta dati di colonne LOB** per specificare che tutte le pagine che contengono dati LOB vengano compattate.
8. Scegliere **OK.**

### <a name="remove-fragmentation-using-tsql"></a><a name="TsqlProcedureReorg"></a> Rimuovere la frammentazione tramite [!INCLUDE[tsql](../../includes/tsql-md.md)]

> [!NOTE]
> Per altri esempi sull'uso di [!INCLUDE[tsql](../../includes/tsql-md.md)] per la ricompilazione o la riorganizzazione degli indici, vedere [Esempi di ALTER INDEX: Indici columnstore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes) ed [Esempi di ALTER INDEX: Indici rowstore](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

#### <a name="to-reorganize-a-fragmented-index"></a>Per riorganizzare un indice frammentato

L'esempio seguente riorganizza l'indice `IX_Employee_OrganizationalLevel_OrganizationalNode` nella tabella `HumanResources.Employee` del database `AdventureWorks2016`.

```sql
ALTER INDEX IX_Employee_OrganizationalLevel_OrganizationalNode
    ON HumanResources.Employee
    REORGANIZE;
```

L'esempio seguente riorganizza l'indice columnstore `IndFactResellerSalesXL_CCI` nella tabella `dbo.FactResellerSalesXL_CCI` del database `AdventureWorksDW2016`.

```sql
-- This command will force all CLOSED and OPEN rowgroups into the columnstore.
ALTER INDEX IndFactResellerSalesXL_CCI
    ON FactResellerSalesXL_CCI
    REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);
```

#### <a name="to-reorganize-all-indexes-in-a-table"></a>Per riorganizzare tutti gli indici in una tabella

L'esempio seguente riorganizza tutti gli indici della tabella `HumanResources.Employee` del database `AdventureWorks2016`.

```sql
ALTER INDEX ALL ON HumanResources.Employee
   REORGANIZE;
```

#### <a name="to-rebuild-a-fragmented-index"></a>Per ricompilare un indice frammentato

Nell'esempio seguente viene ricompilato un singolo indice della tabella `Employee` nel database `AdventureWorks2016`.

[!code-sql[IndexDDL#AlterIndex1](../../relational-databases/indexes/codesnippet/tsql/reorganize-and-rebuild-i_1.sql)]

#### <a name="to-rebuild-all-indexes-in-a-table"></a>Per ricompilare tutti gli indici in una tabella

L'esempio seguente ricompila tutti gli indici associati alla tabella nel database `AdventureWorks2016` tramite la parola chiave `ALL`. Vengono inoltre specificate tre opzioni.

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/reorganize-and-rebuild-i_2.sql)]

Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).

#### <a name="automatic-index-and-statistics-management"></a>Gestione automatica dell'indice e delle statistiche

Sfruttare le soluzioni, ad esempio la [deframmentazione dell'indice adattativo](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), per gestire automaticamente la deframmentazione dell'indice e gli aggiornamenti delle statistiche per uno o più database. Questa procedura sceglie automaticamente se ricompilare o riorganizzare un indice in base al relativo livello di frammentazione, tra gli altri parametri, e aggiornare le statistiche con una soglia lineare.

## <a name="considerations-specific-to-rebuilding-rowstore-indexes"></a>Considerazioni specifiche per la ricompilazione degli indici rowstore

La ricompilazione di un indice cluster ricrea automaticamente tutti gli indici non cluster che fanno riferimento alla chiave di clustering, se è necessario modificare gli identificatori fisici o logici contenuti nei record degli indici non cluster.

Gli scenari seguenti forzano la ricompilazione automatica di tutti gli indici rowstore non cluster in una tabella:

- Creazione di un indice cluster in una tabella
- Rimozione di un indice cluster, che causa l'archiviazione della tabella come heap
- Modifica della chiave di clustering per includere o escludere colonne

Gli scenari seguenti non richiedono la ricompilazione automatica di tutti gli indici rowstore non cluster in una tabella:

- Ricompilazione di un indice cluster univoco
- Ricompilazione di un indice cluster non univoco
- Modifica dello schema dell'indice, ad esempio tramite l'applicazione di uno schema di partizionamento a un indice cluster o lo spostamento dell'indice cluster in un filegroup diverso

> [!IMPORTANT]
> Non è possibile riorganizzare o ricompilare indici contenuti in un filegroup offline o di sola lettura. Quando viene specificata la parola chiave ALL e uno o più indici si trovano in un filegroup offline o di sola lettura, l'istruzione ha esito negativo.
>
> Quando viene eseguita la ricompilazione di un indice, il supporto fisico deve disporre di spazio sufficiente per archiviare due copie dell'indice. Al termine della ricompilazione, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] elimina l'indice originale.

Quando viene specificato `ALL` con l'istruzione `ALTER INDEX`, vengono riorganizzati gli indici relazionali, sia cluster sia non cluster, e gli indici XML della tabella.

## <a name="considerations-specific-to-rebuilding-a-columnstore-index"></a>Considerazioni specifiche per la ricompilazione di un indice columnstore

Durante la ricompilazione di un indice columnstore, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] legge tutti i dati dell'indice columnstore originale, incluso l'archivio differenziale. Combina i dati in nuovi rowgroup e comprime i rowgroup nel columnstore. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] deframmenta il columnstore eliminando fisicamente le righe che sono state eliminate logicamente dalla tabella. I byte eliminati vengono recuperati nel disco.

> [!NOTE]
> La riorganizzazione di un indice columnstore con [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] combina i rowgroup COMPRESSED, ma non forza la compressione di tutti i rowgroup nel columnstore. Il rowgroup CLOSED verranno compressi, mentre i rowgroup OPEN non verranno compressi nel columnstore. Per comprimere tutti i rowgroup, usare l'esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] [di seguito](#TsqlProcedureReorg).

> [!NOTE]
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il motore di tuple viene aiutato da un'attività di unione in background, che comprime automaticamente i rowgroup delta aperti più piccoli che sono esistiti per un dato periodo di tempo (come determinato da una soglia interna) oppure unisce i rowgroup compressi da cui è stato eliminato un numero elevato di righe. In questo modo viene migliorata la qualità dell'indice columnstore nel tempo.    
> Per altre informazioni sui termini e sui concetti dei columnstore, vedere [Indici columnstore - Panoramica](../../relational-databases/indexes/columnstore-indexes-overview.md).

### <a name="rebuild-a-partition-instead-of-the-entire-table"></a>Ricompilare una partizione in alternativa all'intera tabella

- La ricompilazione di un'intera tabella richiede molto tempo se l'indice è esteso ed è necessario sufficiente spazio su disco per archiviare una copia aggiuntiva dell'indice durante la ricompilazione. In genere è necessario solo ricompilare la partizione utilizzata più di recente.
- Per le tabelle partizionate, non è necessario ricompilare l'intero indice columnstore perché la frammentazione probabilmente avviene solo nelle partizioni modificate di recente. Le tabelle dei fatti e le tabelle delle dimensioni grandi vengono in genere partizionate per eseguire operazioni di backup e di gestione dei blocchi della tabella.

### <a name="rebuild-a-partition-after-heavy-dml-operations"></a>Ricompilare una partizione dopo onerose operazioni DML

La ricompilazione di una partizione deframmenta la partizione e riduce lo spazio su disco. La ricompilazione elimina dal columnstore tutte le righe contrassegnate per l'eliminazione e sposta tutti i rowgroup dall'archivio differenziale al columnstore. Nell'archivio differenziale possono esistere più rowgroup e ognuno include meno di un milione di righe.

### <a name="rebuild-a-partition-after-loading-data"></a>Ricompilare una partizione dopo il caricamento dei dati

Ricompilare una partizione dopo il caricamento dei dati assicura che tutti i dati vengano archiviati nel columnstore. Quando i processi simultanei caricano ognuno meno di 100.000 righe contemporaneamente nella stessa partizione, nella partizione possono essere creati più archivi differenziali. La ricompilazione sposta tutte le righe dell'archivio differenziale nel columnstore.

## <a name="considerations-specific-to-reorganizing-a-columnstore-index"></a>Considerazioni specifiche per la riorganizzazione di un indice columnstore

Durante la riorganizzazione di un indice columnstore, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] comprime tutti i rowgroup differenziali CLOSED nel columnstore come rowgroup compresso. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], il comando `REORGANIZE` esegue online le ottimizzazioni di deframmentazione aggiuntive seguenti:

- Rimuove fisicamente le righe da un rowgroup quando più del 10% delle righe è stato eliminato in modo logico. I byte eliminati vengono recuperati sui supporti fisici. Ad esempio, se in un rowgroup compresso di 1 milione di righe 100.000 righe sono state eliminate, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimuoverà le righe eliminate e ricomprimerà il rowgroup con 900.000 righe. Esegue il salvataggio nell'archiviazione rimuovendo le righe eliminate.

- Combina uno o più rowgroup compressi per aumentare le righe in ogni rowgroup, fino a un massimo di 1.048.576 righe. Ad esempio, se si importano globalmente 5 batch di 102.400 righe si otterranno 5 rowgroup compressi. Se si esegue l'operazione REORGANIZE, questi rowgroup verranno uniti in un rowgroup compresso di 512.000 righe. Si presuppone che non vi siano limiti di memoria o di dimensioni del dizionario.

- Per i rowgroup in cui almeno il 10% delle righe è stato eliminato in modo logico, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] tenta di combinare questo rowgroup con uno o più rowgroup. Ad esempio, il rowgroup 1 viene compresso con 500.000 righe e il rowgroup 21 viene compresso con il numero massimo di 1.048.576 righe. Nel rowgroup 21 il 60% delle righe è stato eliminato in modo da lasciare 409.830 righe. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] consente di combinare questi due rowgroup per comprimere un nuovo rowgroup con 909.830 righe.

Dopo l'esecuzione di carichi di dati, è possibile che nell'archivio differenziale rimangano più rowgroup di piccole dimensioni. È possibile usare `ALTER INDEX REORGANIZE` per forzare l'inserimento di tutti i rowgroup in columnstore e poi combinare i rowgroup in un numero inferiore di rowgroup con più righe. L'operazione di riorganizzazione rimuoverà anche le righe che sono state eliminate dal columnstore.

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni

Gli indici rowstore con più di 128 extent vengono ricompilati in due fasi separate, logica e fisica. Nella fase logica, le unità di allocazione esistenti usate dall'indice vengono contrassegnate per la deallocazione, le righe di dati vengono copiate e ordinate, quindi spostate nelle nuove unità di allocazione create per archiviare l'indice ricompilato. Nella fase fisica, le unità di allocazione precedentemente contrassegnate per la deallocazione vengono fisicamente eliminate nelle transazioni brevi eseguite in background e non richiedono molti blocchi. Per altre informazioni sugli extent, vedere la [Guida all'architettura di pagine ed extent](../../relational-databases/pages-and-extents-architecture-guide.md).

L'istruzione `ALTER INDEX REORGANIZE` richiede che nel file di dati che include l'indice sia disponibile spazio, perché l'operazione può allocare solo pagine di lavoro temporanee nello stesso file, non in un altro file nel filegroup. Anche se nel filegroup possono essere disponibili pagine libere, è tuttavia possibile che l'utente incontri l'errore 1105: `Could not allocate space for object '###' in database '###' because the '###' filegroup is full. Create disk space by deleting unneeded files, dropping objects in the filegroup, adding additional files to the filegroup, or setting autogrowth on for existing files in the filegroup.`

> [!WARNING]
> La creazione e la ricompilazione di indici non allineati per una tabella con oltre 1.000 partizioni sono possibili, ma non supportate. Questo tipo di operazioni può causare riduzioni delle prestazioni e un eccessivo consumo della memoria. Quando il numero di partizioni supera 1000, Microsoft consiglia di usare solo [indici allineati](../partitions/partitioned-tables-and-indexes.md#aligned-index).

Non è possibile riorganizzare o ricompilare indici contenuti in un filegroup **offline** o **di sola lettura**. Quando viene specificata la parola chiave `ALL` e uno o più indici si trovano in un filegroup offline o di sola lettura, l'istruzione ha esito negativo.

Statistiche:

- Quando un indice viene **creato** o **ricompilato**, le statistiche vengono create o aggiornate analizzando tutte le righe nella tabella. Tuttavia a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] le statistiche non vengono create o aggiornate analizzando tutte le righe nella tabella quando viene creato o ricompilato un indice partizionato. Query Optimizer usa invece l'algoritmo di campionamento predefinito per generare queste statistiche. Per ottenere statistiche sugli indici partizionati analizzando tutte le righe nella tabella, usare [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) o [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) con la clausola `FULLSCAN`.

- Quando un indice viene **riorganizzato**, le statistiche non vengono aggiornate.

Quando l'opzione `ALLOW_PAGE_LOCKS` è impostata su OFF, non è possibile eseguire operazioni di riorganizzazione degli indici.

Fino a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la ricompilazione di un indice columnstore cluster è un'operazione offline. Il motore di database deve acquisire un blocco esclusivo sulla tabella o sulla partizione durante la ricompilazione. I dati sono offline e non sono disponibili durante la ricompilazione, anche quando si usa `NOLOCK`, l'isolamento dello snapshot Read Committed (RCSI) o l'isolamento dello snapshot.
A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], un indice columnstore cluster può essere ricompilato tramite l'opzione `ONLINE = ON`.

## <a name="using-index-rebuild-to-recover-from-hardware-failures"></a>Uso di INDEX REBUILD per il ripristino da errori hardware

Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è talvolta possibile ricompilare un indice non cluster rowstore per risolvere le incoerenze causate da errori hardware.
A partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)], è ancora possibile correggere tali incoerenze tra l'indice e l'indice cluster tramite la ricompilazione di un indice non cluster offline. Non è possibile, tuttavia, correggere le incoerenze di indici non cluster tramite la ricompilazione dell'indice online, in quanto il meccanismo di ricompilazione online usa l'indice non cluster esistente come base per la ricompilazione e di conseguenza l'incoerenza persiste. La ricompilazione dell'indice offline può talvolta forzare l'analisi dell'indice cluster (o dell'heap) rimuovendo pertanto l'incoerenza. Per garantire una ricompilazione dall'indice cluster, eliminare e ricreare l'indice non cluster. Come nelle versioni precedenti, il metodo consigliato per il recupero in seguito all'individuazione di incoerenze consiste nel ripristino da backup dei dati interessati. È tuttavia possibile correggere le incoerenze dell'indice tramite la ricompilazione offline dell'indice non cluster. Per altre informazioni, vedere [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).

## <a name="see-also"></a>Vedere anche

- [Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md)
- [Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md)
- [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)
- [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) (Deframmentazione dell'indice adattativo)
- [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)
- [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)
- [Prestazioni delle query per gli indici columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)
- [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)
- [Indici columnstore per il data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)
- [Indici columnstore e criteri di unione per rowgroup](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)