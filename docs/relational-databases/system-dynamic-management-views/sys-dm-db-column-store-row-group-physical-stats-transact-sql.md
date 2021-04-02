---
description: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6345892c246c0ffc693a28e67833068c65243672
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054708"
---
# <a name="sysdm_db_column_store_row_group_physical_stats-transact-sql"></a>sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Fornisce informazioni aggiornate a livello di rowgroup su tutti gli indici columnstore nel database corrente.  

Viene estesa la vista del catalogo [sys.column_store_row_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md).  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID della tabella sottostante.|  
|**index_id**|**int**|ID dell'indice columnstore in *object_id* tabella.|  
|**partition_number**|**int**|ID della partizione della tabella che include *row_group_id*. È possibile utilizzare il partition_number per creare un join di questa DMV a sys.partitions.|  
|**row_group_id**|**int**|ID di questo gruppo di righe. Per le tabelle partizionate, il valore è univoco all'interno della partizione.<br /><br /> -1 per una coda in memoria.|  
|**delta_store_hobt_id**|**bigint**|Hobt_id per un gruppo di righe nell'archivio Delta.<br /><br /> NULL se il gruppo di righe non è presente nell'archivio Delta.<br /><br /> NULL per la parte finale di una tabella in memoria.|  
|**state**|**tinyint**|Numero ID associato *state_description*.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = RIMOZIONE DEFINITIVA<br /><br /> COMPRESSO è l'unico stato che si applica alle tabelle in memoria.|  
|**state_desc**|**nvarchar(60)**|Descrizione dello stato del gruppo di righe:<br /><br /> 0-invisibile: gruppo di righe in fase di compilazione. Ad esempio: <br />Un gruppo di righe nel columnstore è invisibile mentre i dati vengono compressi. Al termine della compressione, un'opzione di metadati modifica lo stato del gruppo di righe columnstore da invisibile a compresso e lo stato del gruppo di righe deltastore da chiuso a contrassegnato per la rimozione definitiva.<br /><br /> 1: apre un gruppo di righe deltastore che accetta nuove righe. Un gruppo di righe aperto presenta ancora il formato rowstore e non è stato compresso nel formato columnstore.<br /><br /> 2-CLOSED: un gruppo di righe nell'archivio Delta che contiene il numero massimo di righe ed è in attesa del processo del motore di Tuple per comprimerlo nel columnstore.<br /><br /> 3-compresso: un gruppo di righe compresso con la compressione columnstore e archiviato nel columnstore.<br /><br /> 4-TOMBSTONE: gruppo di righe precedentemente presente in deltastore e non più utilizzato.|  
|**total_rows**|**bigint**|Numero di righe archiviate fisicamente nel gruppo di righe. Per gruppi di righe compressi. Include le righe contrassegnate come eliminate.|  
|**deleted_rows**|**bigint**|Numero di righe archiviate fisicamente in un gruppo di righe compresso contrassegnate per l'eliminazione.<br /><br /> 0 per i gruppi di righe presenti nell'archivio differenziale.|  
|**size_in_bytes**|**bigint**|Dimensioni combinate, in byte, di tutte le pagine di questo gruppo di righe. Questa dimensione non include le dimensioni necessarie per archiviare i metadati o i dizionari condivisi.|  
|**trim_reason**|**tinyint**|Motivo per cui il gruppo di righe compresso ha attivato un numero di righe inferiore al massimo consentito.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1-NO_TRIM<br /><br /> 2-BULKLOAD<br /><br /> 3-REORG<br /><br /> 4-DICTIONARY_SIZE<br /><br /> 5-MEMORY_LIMITATION<br /><br /> 6-RESIDUAL_ROW_GROUP<br /><br /> 7-STATS_MISMATCH<br /><br /> 8-RIPERCUSSIONI<br /><br /> 9-AUTO_MERGE|  
|**trim_reason_desc**|**nvarchar(60)**|Descrizione della *trim_reason*.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION: si è verificato durante l'aggiornamento dalla versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> 1-NO_TRIM: il gruppo di righe non è stato tagliato. Il gruppo di righe è stato compresso con il numero massimo di 1.048.576 righe.  Il numero di righe potrebbe essere inferiore se un subset di righe è stato eliminato dopo la chiusura di Delta rowgroup<br /><br /> 2-BULKLOAD: le dimensioni del batch per il caricamento bulk hanno limitato il numero di righe.<br /><br /> 3-REORG: compressione forzata come parte del comando REORG.<br /><br /> 4-DICTIONARY_SIZE: dimensioni del dizionario troppo grandi per comprimere tutte le righe.<br /><br /> 5-MEMORY_LIMITATION: memoria disponibile insufficiente per comprimere tutte le righe.<br /><br /> 6-RESIDUAL_ROW_GROUP: chiuso come parte dell'ultimo gruppo di righe con righe < 1 milione durante l'operazione di compilazione dell'indice<br /><br /> 7-STATS_MISMATCH: solo per columnstore nella tabella in memoria. Se le statistiche indicano erroneamente >= 1 milione righe qualificate nella coda, ma ne è stato rilevato un numero inferiore, il rowgroup compresso avrà < 1 milione righe<br /><br /> 8-ripercussioni: solo per columnstore nella tabella in memoria. Se Tail include > righe qualificate 1 milione, le righe rimanenti dell'ultimo batch vengono compresse se il conteggio è compreso tra 100.000 e 1 milione<br /><br /> 9-AUTO_MERGE: un'operazione di Unione del motore di tuple in esecuzione in background ha consolidato uno o più RowGroups in questo rowgroup.|  
|**transition_to_compressed_state**|TINYINT|Mostra come questo rowgroup è stato spostato da deltastore a uno stato compresso nel columnstore.<br /><br /> 1-NOT_APPLICABLE<br /><br /> 2-INDEX_BUILD<br /><br /> 3-TUPLE_MOVER<br /><br /> 4-REORG_NORMAL<br /><br /> 5-REORG_FORCED<br /><br /> 6-BULKLOAD<br /><br /> 7-UNIONE|  
|**transition_to_compressed_state_desc**|nvarchar(60)| 1-NOT_APPLICABLE-l'operazione non è valida per deltastore. In alternativa, il rowgroup è stato compresso prima dell'aggiornamento a, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] nel qual caso la cronologia non viene mantenuta.<br /><br /> 2-INDEX_BUILD-la creazione di un indice o la ricompilazione dell'indice ha compresso rowgroup.<br /><br /> 3-TUPLE_MOVER: il motore di tuple in esecuzione in background ha compresso rowgroup. Il motore di tuple si verifica dopo la modifica dello stato di rowgroup da aperto a chiuso.<br /><br /> 4-REORG_NORMAL-operazione di riorganizzazione, ALTER INDEX... REORG, spostato il rowgroup chiuso da deltastore nel columnstore. Questo errore si è verificato prima che il motore di Tuple avesse tempo per spostare il rowgroup.<br /><br /> 5-REORG_FORCED-questo rowgroup è stato aperto in deltastore ed è stato forzato nel columnstore prima di avere un numero intero di righe.<br /><br /> 6-BULKLOAD: un'operazione di caricamento bulk ha compresso direttamente il rowgroup senza usare deltastore.<br /><br /> 7-MERGE: un'operazione di Unione ha consolidato uno o più RowGroups in questo rowgroup e quindi ha eseguito la compressione columnstore.|  
|**has_vertipaq_optimization**|bit|L'ottimizzazione VertiPaq migliora la compressione columnstore ridisponendo l'ordine delle righe nel rowgroup per ottenere una compressione più elevata. Questa ottimizzazione viene eseguita automaticamente nella maggior parte dei casi. Esistono due casi in cui l'ottimizzazione VertiPaq non viene utilizzata:<br/>  a. Quando un rowgroup Delta si sposta nel columnstore e sono presenti uno o più indici non cluster nell'indice columnstore, in questo caso l'ottimizzazione VertiPaq viene ignorata per ridurre al minimo le modifiche all'indice di mapping.<br/> b. per gli indici columnstore nelle tabelle ottimizzate per la memoria. <br /><br /> 0 = No<br /><br /> 1 = Sì|  
|**generazione**|bigint|Generazione di gruppi di righe associata a questo gruppo di righe.|  
|**created_time**|datetime2|Ora di clock del momento in cui è stato creato il rowgroup.<br /><br /> NULL: per un indice columnstore in una tabella in memoria.| 
|**closed_time**|datetime2|Ora di clock del momento in cui il rowgroup è stato chiuso.<br /><br /> NULL: per un indice columnstore in una tabella in memoria.| 
| &nbsp; | &nbsp; | &nbsp; |

## <a name="results"></a>Risultati  
 Restituisce una riga per ogni rowgroup nel database corrente.  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta `CONTROL` l'autorizzazione per la tabella e l' `VIEW DATABASE STATE` autorizzazione per il database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-calculate-fragmentation-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>R. Calcolare la frammentazione per decidere quando riorganizzare o ricompilare un indice columnstore.  
 Per gli indici columnstore, la percentuale di righe eliminate è una misura corretta per la frammentazione in un rowgroup. Quando la frammentazione è pari al 20% o superiore, rimuovere le righe eliminate. Per altri esempi, vedere [riorganizzare e ricompilare gli indici](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 In questo esempio viene unito **sys.dm_db_column_store_row_group_physical_stats** ad altre tabelle di sistema, quindi la colonna viene calcolata `Fragmentation` come stima dell'efficienza di ogni gruppo di righe nel database corrente. Per trovare informazioni su una singola tabella, rimuovere i trattini dei commenti davanti alla clausola **where** e specificare un nome di tabella.  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0) AS 'Fragmentation'
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)      
 [Architettura degli indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)         
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
 [sys.column_store_dictionaries &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
