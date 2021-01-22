---
description: DBCC SHOWCONTIG (Transact-SQL)
title: DBCC SHOWCONTIG (Transact-SQL)
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC_SHOWCONTIG_TSQL
- DBCC SHOWCONTIG
- SHOWCONTIG
- SHOWCONTIG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying defragmentation information
- DBCC SHOWCONTIG statement
- defragmenting indexes
- leaf level defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
ms.assetid: 1df2123a-1197-4fff-91a3-25e3d8848aaa
author: pmasl
ms.author: umajay
ms.openlocfilehash: ed1ba4d3fb6114a74dbf972ff0a04f84a463af06
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597080"
---
# <a name="dbcc-showcontig-transact-sql"></a>DBCC SHOWCONTIG (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Visualizza informazioni sulla frammentazione dei dati e degli indici per la tabella o vista specificata.
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Usare [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) in alternativa.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] alla [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md))
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DBCC SHOWCONTIG   
[ (   
    { table_name | table_id | view_name | view_id }   
    [ , index_name | index_id ]   
) ]   
    [ WITH   
        {   
         [ , [ ALL_INDEXES ] ]   
         [ , [ TABLERESULTS ] ]   
         [ , [ FAST ] ]  
         [ , [ ALL_LEVELS ] ]   
         [ NO_INFOMSGS ]  
         }  
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *table_name* \| *table_id* \| *view_name* \| *view_id*  
 Tabella o vista di cui controllare le informazioni sulla frammentazione. Se viene omesso, vengono controllate tutte le tabelle e le viste indicizzate nel database corrente. Per ottenere l'ID della tabella o vista, usare la funzione [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md).  
  
 *index_name* \| *index_id*  
 Indice di cui controllare le informazioni sulla frammentazione. Se viene omesso, l'istruzione elabora l'indice di base per la tabella o vista specificata. Per ottenere l'ID dell'indice, usare la vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 WITH  
 Specifica le opzioni per il tipo di informazioni restituite dall'istruzione DBCC.  
  
 FAST  
 Specifica se eseguire un'analisi rapida dell'indice con restituzione di informazioni di output minime. Durante un'analisi rapida non vengono lette le pagine del livello foglia o dati dell'indice.  
  
 ALL_INDEXES  
 Visualizza i risultati per tutti gli indici delle tabelle e viste specificate, anche se viene indicato un indice specifico.  
  
 TABLERESULTS  
 Visualizza i risultati come set di righe, con informazioni aggiuntive.  
  
 ALL_LEVELS  
 Supportata per compatibilità con le versioni precedenti. Anche se si specifica ALL_LEVELS, viene elaborato solo il livello foglia dell'indice oppure il livello dati della tabella.  
  
 NO_INFOMSGS  
 Evita la visualizzazione di tutti i messaggi informativi con livello di gravità compreso tra 0 e 10.  
  
## <a name="result-sets"></a>Set di risultati  
Nella tabella seguente vengono descritte le informazioni del set di risultati.
  
|Statistiche|Descrizione|  
|---|---|
|**Pagine sottoposte ad analisi**|Numero di pagine della tabella o dell'indice.|  
|**Extent sottoposti ad analisi**|Numero di extent della tabella o dell'indice.|  
|**Cambi di extent**|Numero di passaggi dell'istruzione DBCC da un extent all'altro durante l'attraversamento delle pagine della tabella o dell'indice.|  
|**Byte Pagine per extent**|Numero di pagine per extent nella catena di pagine.|  
|**Densità di analisi [conteggio ottimale: conteggio effettivo]**|Valore percentuale. Rapporto tra **Conteggio ottimale** e **Conteggio effettivo**. Questo valore è 100 se tutti gli elementi sono contigui, è minore di 100 in presenza di frammentazioni.<br /><br /> **Conteggio ottimale** rappresenta il numero ideale di cambi di extent se tutti gli elementi fossero contigui. **Conteggio effettivo** rappresenta il numero effettivo di cambi di extent.|  
|**Frammentazione analisi logica**|Percentuale di pagine non ordinate restituite dall'analisi delle pagine foglia di un indice. Questo valore non è rilevante per gli heap. Una pagina risulta non ordinata quando la pagina fisica successiva allocata all'indice è diversa da quella a cui fa riferimento il *puntatore di pagina successiva* nella pagina foglia corrente.|  
|**Frammentazione analisi extent**|Percentuale di extent non ordinati rilevati durante l'analisi delle pagine foglia di un indice. Questo valore non è rilevante per gli heap. Un extent risulta non ordinato quando l'extent contenente la pagina corrente di un indice non corrisponde fisicamente all'extent successivo a quello che contiene la pagina precedente di un indice.<br /><br /> Nota: questo numero non è significativo se l'indice si estende su più file.|  
|**Byte disponibili per pagina**|Numero medio di byte disponibili nelle pagine sottoposte ad analisi. Maggiore è il numero, minore sarà il livello di riempimento delle pagine. I numeri minori indicano una situazione migliore se nell'indice non verranno eseguiti numerosi inserimenti casuali. Anche le dimensioni delle righe influiscono su questo valore, che risulta maggiore per righe di grandi dimensioni.|  
|**Byte densità pagina (completa)**|Densità media della pagina, in percentuale. Questo valore tiene conto delle dimensioni delle righe e pertanto rappresenta un'indicazione più precisa dell'effettivo livello di riempimento delle pagine. Sono preferibili valori elevati.|  
  
Se si specificano *table_id* e l'opzione FAST, l'istruzione DBCC SHOWCONTIG restituisce un set di risultati che include solo le colonne seguenti.
-   **Pagine sottoposte ad analisi**  
-   **Cambi di extent**  
-   **Densità di analisi [conteggio ottimale:conteggio effettivo]**  
-   **Frammentazione analisi extent**  
-   **Frammentazione analisi logica**  
  
Se si specifica TABLERESULTS, DBCC SHOWCONTIG restituisce le colonne seguenti oltre alle nove colonne descritte nella tabella precedente.
  
|Statistiche|Descrizione|  
|---|---|
|**nome oggetto**|Nome della tabella o vista elaborata.|  
|**ObjectId**|ID del nome di oggetto.|  
|**IndexName**|Nome dell'indice elaborato. NULL per un heap.|  
|**IndexId**|ID dell'indice. 0 per un heap.|  
|**Level**|Livello dell'indice. Il livello 0 corrisponde al livello foglia (o dati) dell'indice.<br /><br /> Il livello è 0 per un heap.|  
|**Pagine**|Numero di pagine che compongono tale livello dell'indice o l'intero heap.|  
|**prime righe**|Numero di record di dati o dell'indice a tale livello dell'indice. Nel caso di un heap, corrisponde al numero di record di dati dell'intero heap.<br /><br /> Per un heap, il numero di record restituito da questa funzione potrebbe non corrispondere al numero di righe restituito eseguendo un SELECT COUNT (*) sull'heap. Questo perché una riga potrebbe contenere più record. Ad esempio, in alcune situazioni di aggiornamento, un'unica riga dell'heap potrebbe presentare un record di inoltro e un record inoltrato a seguito dell'operazione di aggiornamento. Inoltre, nell'archiviazione LOB_DATA la maggior parte delle righe LOB viene suddivisa in più record.|  
|**MinimumRecordSize**|Dimensioni minime dei record in tale livello dell'indice o nell'intero heap.|  
|**MaximumRecordSize**|Dimensioni massime dei record in tale livello dell'indice o nell'intero heap.|  
|**AverageRecordSize**|Dimensioni medie dei record in tale livello dell'indice o nell'intero heap.|  
|**ForwardedRecords**|Numero di record inoltrati in tale livello dell'indice o nell'intero heap.|  
|**Extents**|Numero di extent in tale livello dell'indice o nell'intero heap.|  
|**ExtentSwitches**|Numero di passaggi dell'istruzione DBCC da un extent all'altro durante l'attraversamento delle pagine della tabella o dell'indice.|  
|**AverageFreeBytes**|Numero medio di byte disponibili nelle pagine sottoposte ad analisi. Maggiore è il numero, minore sarà il livello di riempimento delle pagine. I numeri minori indicano una situazione migliore se nell'indice non verranno eseguiti numerosi inserimenti casuali. Anche le dimensioni delle righe influiscono su questo valore, che risulta maggiore per righe di grandi dimensioni.|  
|**AveragePageDensity**|Densità media della pagina, in percentuale. Questo valore tiene conto delle dimensioni delle righe e pertanto rappresenta un'indicazione più precisa dell'effettivo livello di riempimento delle pagine. Sono preferibili valori elevati.|  
|**ScanDensity**|Valore percentuale. Rapporto tra **BestCount** e **ActualCount**. Questo valore è 100 se tutti gli elementi sono contigui, è minore di 100 in presenza di frammentazioni.|  
|**BestCount**|Rappresenta il numero ideale di cambi di extent se tutti gli elementi fossero contigui.|  
|**ActualCount**|Rappresenta il numero effettivo di cambi di extent.|  
|**LogicalFragmentation**|Percentuale di pagine non ordinate restituite dall'analisi delle pagine foglia di un indice. Questo valore non è rilevante per gli heap. Una pagina risulta non ordinata quando la pagina fisica successiva allocata all'indice è diversa da quella a cui fa riferimento il *puntatore di pagina successiva* nella pagina foglia corrente.|  
|**ExtentFragmentation**|Percentuale di extent non ordinati rilevati durante l'analisi delle pagine foglia di un indice. Questo valore non è rilevante per gli heap. Un extent risulta non ordinato quando l'extent contenente la pagina corrente di un indice non corrisponde fisicamente all'extent successivo a quello che contiene la pagina precedente di un indice.<br /><br /> Nota: questo numero non è significativo se l'indice si estende su più file.|  
  
Se si specificano le opzioni WITH TABLERESULTS e FAST, il set di risultati è uguale a quello restituito specificando WITH TABLERESULTS, con l'eccezione delle colonne seguenti che avranno valori Null:

| Righe| Extents |
|---|---|
|**MinimumRecordSize**|**AverageFreeBytes**|  
|**MaximumRecordSize**|**AveragePageDensity**|  
|**AverageRecordSize**|**ExtentFragmentation**|  
|**ForwardedRecords**||  
  
## <a name="remarks"></a>Osservazioni  
Quando si specifica *index_id*, l'istruzione DBCC SHOWCONTIG attraversa la catena di pagine al livello foglia dell'indice specificato. Se si specifica solo *table_id* oppure *index_id* è 0, viene eseguita l'analisi delle pagine di dati della tabella specificata. L'operazione richiede esclusivamente un blocco a livello di tabella preventivo condiviso (IS). In questo modo è possibile eseguire tutti gli aggiornamenti e gli inserimenti, con l'eccezione delle operazioni che richiedono un blocco a livello di tabella esclusivo (X). Ciò consente di ottenere una velocità di esecuzione accettabile senza riduzione della concorrenza per il numero di statistiche restituite. Tuttavia, se il comando viene utilizzato esclusivamente per ottenere dati di misurazione della frammentazione, è consigliabile utilizzare l'opzione WITH FAST per ottenere prestazioni ottimali. Durante un'analisi rapida non vengono lette le pagine del livello foglia o dati dell'indice. L'opzione WITH FAST non si applica a un heap.
  
## <a name="restrictions"></a>Restrizioni  
DBCC SHOWCONTIG non visualizza dati di tipo **ntext**, **text** e **image**. La mancata visualizzazione è dovuta al fatto gli indici di testo che archiviano dati di tipo text e image non esistono più.
  
Inoltre, DBCC SHOWCONTIG non supporta alcune nuove caratteristiche. Ad esempio:
-   Se la tabella o l'indice specificato è partizionato, DBCC SHOWCONTIG visualizza solo la prima partizione della tabella o dell'indice specificato.  
-   DBCC SHOWCONTIG non supporta la visualizzazione di informazioni di archiviazione per i dati di overflow della riga e di altri tipi nuovi per dati all'esterno di righe, come **nvarchar(max)** , **varchar(max)** , **varbinary(max)** e **xml**.  
-   Gli indici spaziali non sono supportati da DBCC SHOWCONTIG.  
  
Tutte le nuove funzionalità sono completamente supportate dalla vista a gestione dinamica [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).
  
## <a name="table-fragmentation"></a>Frammentazione della tabella  
L'istruzione DBCC SHOWCONTIG determina se la tabella è molto frammentata. La frammentazione si verifica in seguito ai processi di modifica dei dati della tabella (istruzioni INSERT, UPDATE e DELETE). Poiché tali modifiche in genere non sono distribuite in modo equo tra le righe della tabella, il livello di riempimento di ogni pagina può variare nel corso del tempo. Nel caso di query che eseguono l'analisi di un'intera tabella o di una parte di tabella, tale frammentazione della tabella potrebbe comportare letture di pagine aggiuntive, operazione che ostacola l'analisi parallela dei dati.
  
Per ridurre il livello di frammentazione di un indice molto frammentato, è possibile eseguire una delle operazioni seguenti:
-   Eliminare e ricreare un indice cluster.  
     Quando si ricrea un indice cluster, i dati vengono riorganizzati e si ottengono pagine di dati complete. È possibile configurare il livello di riempimento tramite l'opzione FILLFACTOR dell'istruzione CREATE INDEX. Questo metodo presenta due svantaggi, ovvero l'indice rimane offline durante l'operazione di eliminazione o ricostruzione e l'operazione è atomica. Se la creazione dell'indice viene interrotta, l'indice non viene ricreato.  
-   Ridisporre in ordine logico le pagine del livello foglia dell'indice.  
     Per ridisporre in ordine logico le pagine del livello foglia dell'indice, usare ALTER INDEX...REORGANIZE. L'operazione viene eseguita online e pertanto l'indice rimane disponibile durante l'esecuzione dell'istruzione. È inoltre possibile interrompere l'operazione senza perdere il lavoro completato. Con questo metodo, tuttavia, i dati non vengono riorganizzati in modo altrettanto efficiente di quanto consentito dall'operazione di eliminazione o ricostruzione di un indice cluster.  
-   Ricompilare l'indice.  
     Per ricompilare l'indice, utilizzare ALTER INDEX con REBUILD. Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
Il numero **Media byte disponibili per pagina** e **Media densità pagina (completa)** nel set di risultati indicano il livello di riempimento delle pagine dell'indice. Il numero **Media byte disponibili per pagina** deve essere basso e il numero **Media densità pagina (completa)** deve essere elevato per un indice per cui non sono previsti numerosi inserimenti casuali. L'eliminazione e la ricostruzione di un indice con l'opzione FILLFACTOR può consentire di migliorare tali statistiche. L'istruzione ALTER INDEX con l'opzione REORGANIZE consente inoltre di compattare un indice tenendo conto del valore FILLFACTOR corrispondente, con un conseguente miglioramento delle statistiche.
  
> [!NOTE]  
>  Per un indice con numerosi inserimenti casuali e pagine molto piene si verificherà un maggior numero di divisioni di pagina e di conseguenza una maggiore frammentazione.  
  
È possibile determinare il livello di frammentazione di un indice nei modi seguenti:
-   Tramite il confronto dei valori di **Cambi di extent** ed **Extent sottoposti ad analisi**.  
     Il valore **Cambi di extent** deve essere il più possibile prossimo al valore **Extent sottoposti ad analisi**. Questo rapporto viene calcolato dal valore **Densità di analisi**. Tale valore deve essere il più alto possibile e può essere migliorato riducendo la frammentazione dell'indice.  
  
    > [!NOTE]  
    >  Questo metodo non funziona se l'indice è esteso a più file.  
  
-   Tramite l'analisi dei valori **Frammentazione analisi logica** e **Frammentazione analisi extent**.  
     Il valore **Frammentazione analisi logica** e, anche se in misura minore, il valore **Frammentazione analisi extent** sono i migliori indicatori del livello di frammentazione di una tabella. Entrambi i valori dovrebbero essere il più possibile prossimi allo zero, anche se possono essere accettabili valori compresi tra 0% e 10%.  
  
    > [!NOTE]  
    >  Il valore **Frammentazione analisi extent** sarà elevato se l'indice è esteso a più file. Per ridurre questo valore, è necessario ridurre la frammentazione dell'indice.  
  
## <a name="permissions"></a>Autorizzazioni  
L'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_owner** o **db_ddladmin**.
  
## <a name="examples"></a>Esempi  
### <a name="a-displaying-fragmentation-information-for-a-table"></a>R. Visualizzazione delle informazioni sulla frammentazione di una tabella  
Nell'esempio seguente vengono visualizzate informazioni sulla frammentazione della tabella `Employee`.
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG ('HumanResources.Employee');  
GO  
```  
  
### <a name="b-using-object_id-to-obtain-the-table-id-and-sysindexes-to-obtain-the-index-id"></a>B. Utilizzo di OBJECT_ID per ottenere l'ID della tabella e di sys.indexes per ottenere l'ID dell'indice  
Nell'esempio seguente vengono usate la funzione `OBJECT_ID` e la vista del catalogo `sys.indexes` per ottenere l'ID di tabella e l'ID di indice per l'indice `AK_Product_Name` della tabella `Production.Product` nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @id INT, @indid INT  
SET @id = OBJECT_ID('Production.Product')  
SELECT @indid = index_id   
FROM sys.indexes  
WHERE object_id = @id   
   AND name = 'AK_Product_Name'  
DBCC SHOWCONTIG (@id, @indid);  
GO  
```  
  
### <a name="c-displaying-an-abbreviated-result-set-for-a-table"></a>C. Visualizzazione di un set di risultati abbreviato per una tabella  
Nell'esempio seguente viene restituito un set di risultati abbreviato per la tabella `Product`nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG ('Production.Product', 1) WITH FAST;  
GO  
```  
  
### <a name="d-displaying-the-full-result-set-for-every-index-on-every-table-in-a-database"></a>D. Visualizzazione del set di risultati completo per ogni indice di tutte le tabelle di un database  
Nell'esempio seguente viene restituito un set completo di risultati relativi a ogni indice di tutte le tabelle del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG WITH TABLERESULTS, ALL_INDEXES;  
GO  
```  
  
### <a name="e-using-dbcc-showcontig-and-dbcc-indexdefrag-to-defragment-the-indexes-in-a-database"></a>E. Utilizzo di DBCC SHOWCONTIG e DBCC INDEXDEFRAG per deframmentare gli indici di un database  
Nell'esempio seguente viene illustrato un metodo semplice per deframmentare tutti gli indici di un database il cui livello di frammentazione è superiore alla soglia massima specificata.
  
```sql
/*Perform a 'USE <database name>' to select the database in which to run the script.*/  
-- Declare variables  
SET NOCOUNT ON;  
DECLARE @tablename VARCHAR(255);  
DECLARE @execstr   VARCHAR(400);  
DECLARE @objectid  INT;  
DECLARE @indexid   INT;  
DECLARE @frag      DECIMAL;  
DECLARE @maxfrag   DECIMAL;  
  
-- Decide on the maximum fragmentation to allow for.  
SELECT @maxfrag = 30.0;  
  
-- Declare a cursor.  
DECLARE tables CURSOR FOR  
   SELECT TABLE_SCHEMA + '.' + TABLE_NAME  
   FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_TYPE = 'BASE TABLE';  
  
-- Create the table.  
CREATE TABLE #fraglist (  
   ObjectName CHAR(255),  
   ObjectId INT,  
   IndexName CHAR(255),  
   IndexId INT,  
   Lvl INT,  
   CountPages INT,  
   CountRows INT,  
   MinRecSize INT,  
   MaxRecSize INT,  
   AvgRecSize INT,  
   ForRecCount INT,  
   Extents INT,  
   ExtentSwitches INT,  
   AvgFreeBytes INT,  
   AvgPageDensity INT,  
   ScanDensity DECIMAL,  
   BestCount INT,  
   ActualCount INT,  
   LogicalFrag DECIMAL,  
   ExtentFrag DECIMAL);  
  
-- Open the cursor.  
OPEN tables;  
  
-- Loop through all the tables in the database.  
FETCH NEXT  
   FROM tables  
   INTO @tablename;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
-- Do the showcontig of all indexes of the table  
   INSERT INTO #fraglist   
   EXEC ('DBCC SHOWCONTIG (''' + @tablename + ''')   
      WITH FAST, TABLERESULTS, ALL_INDEXES, NO_INFOMSGS');  
   FETCH NEXT  
      FROM tables  
      INTO @tablename;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE tables;  
DEALLOCATE tables;  
  
-- Declare the cursor for the list of indexes to be defragged.  
DECLARE indexes CURSOR FOR  
   SELECT ObjectName, ObjectId, IndexId, LogicalFrag  
   FROM #fraglist  
   WHERE LogicalFrag >= @maxfrag  
      AND INDEXPROPERTY (ObjectId, IndexName, 'IndexDepth') > 0;  
  
-- Open the cursor.  
OPEN indexes;  
  
-- Loop through the indexes.  
FETCH NEXT  
   FROM indexes  
   INTO @tablename, @objectid, @indexid, @frag;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   PRINT 'Executing DBCC INDEXDEFRAG (0, ' + RTRIM(@tablename) + ',  
      ' + RTRIM(@indexid) + ') - fragmentation currently '  
       + RTRIM(CONVERT(varchar(15),@frag)) + '%';  
   SELECT @execstr = 'DBCC INDEXDEFRAG (0, ' + RTRIM(@objectid) + ',  
       ' + RTRIM(@indexid) + ')';  
   EXEC (@execstr);  
  
   FETCH NEXT  
      FROM indexes  
      INTO @tablename, @objectid, @indexid, @frag;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE indexes;  
DEALLOCATE indexes;  
  
-- Delete the temporary table.  
DROP TABLE #fraglist;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)  
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
  
