---
description: sys.dm_db_index_physical_stats (Transact-SQL)
title: sys.dm_db_index_physical_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_index_physical_stats
- sys.dm_db_index_physical_stats_TSQL
- sys.dm_db_index_physical_stats
- dm_db_index_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_physical_stats dynamic management function
- fragmentation [SQL Server]
ms.assetid: d294dd8e-82d5-4628-aa2d-e57702230613
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 603ad99e126f1175cce21a48933362e4ad6d7aaa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094175"
---
# <a name="sysdm_db_index_physical_stats-transact-sql"></a>sys.dm_db_index_physical_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce le informazioni sulle dimensioni e sulla frammentazione per i dati e gli indici della tabella o della vista specificata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un indice, viene restituita una riga per ogni livello dell'albero B in ogni partizione. Per un heap, viene restituita una riga per l'unità di allocazione IN_ROW_DATA di ogni partizione. Per i dati LOB (Large Object), viene restituita una riga per l'unità di allocazione LOB_DATA di ogni partizione. Se nella tabella esistono dati di overflow della riga, viene restituita una riga per l'unità di allocazione ROW_OVERFLOW_DATA in ogni partizione. Non restituisce informazioni sugli indici columnstore con ottimizzazione per la memoria xVelocity.  
  
> [!IMPORTANT]
> Se si esegue una query **sys.dm_db_index_physical_stats** su un'istanza del server che ospita una [replica secondaria leggibile](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)always on, è possibile che si verifichi un problema di blocco del ripristino. Questa condizione si verifica perché la DMV acquisisce un blocco IS nella vista o nella tabella utente specificata che può bloccare le richieste di una fase di rollforward per un blocco X presente in tale vista o tabella utente.  
  
 **sys.dm_db_index_physical_stats** non restituisce informazioni sugli indici ottimizzati per la memoria. Per informazioni sull'utilizzo dell'indice ottimizzato per la memoria, vedere [sys.dm_db_xtp_index_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_db_index_physical_stats (   
    { database_id | NULL | 0 | DEFAULT }  
  , { object_id | NULL | 0 | DEFAULT }  
  , { index_id | NULL | 0 | -1 | DEFAULT }  
  , { partition_number | NULL | 0 | DEFAULT }  
  , { mode | NULL | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Argomenti  
 *database_id* \| \| \| Valore predefinito 0 null  
 ID del database. *database_id* è **smallint**. Gli input validi sono il numero di ID di un database, NULL, 0 o DEFAULT. Il valore predefinito è 0. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto.  
  
 Specificare NULL per restituire informazioni per tutti i database presenti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si specifica NULL per *database_id*, è necessario specificare null anche per *object_id*, *index_id* e *partition_number*.  
  
 È possibile specificare la funzione predefinita [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Quando si utilizza DB_ID senza specificare un nome di database, il livello di compatibilità del database corrente deve essere 90 o un valore superiore.  
  
 *object_id* \| \| \| Valore predefinito 0 null  
 ID oggetto della tabella o vista in cui si trova l'indice. *object_id* è di **tipo int**.  
  
 Gli input validi sono il numero di ID di una tabella o vista, NULL, 0 o DEFAULT. Il valore predefinito è 0. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto. A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] , gli input validi includono anche il nome della coda di Service Broker o il nome della tabella interna della coda. Quando vengono applicati parametri predefiniti, ovvero tutti gli oggetti, tutti gli indici e così via, le informazioni sulla frammentazione per tutte le code sono incluse nel set di risultati.  
  
 Specificare NULL per restituire le informazioni per tutte le tabelle e le viste nel database specificato. Se si specifica NULL per *object_id*, è necessario specificare null anche per *index_id* e *partition_number*.  
  
 *index_id* \| 0 \| null \| -1 \| valore predefinito  
 ID dell'indice. *index_id* è di **tipo int**. Gli input validi sono il numero di ID di un indice, 0 se *object_id* è un heap, null,-1 o default. Il valore predefinito è -1. NULL,-1 e DEFAULT sono valori equivalenti in questo contesto.  
  
 Specificare NULL per restituire le informazioni per tutti gli indici per una vista o tabella di base. Se si specifica NULL per *index_id*, è necessario specificare null anche per *partition_number*.  
  
 *partition_number* \| \| \| Valore predefinito 0 null  
 Numero di partizione nell'oggetto. *partition_number* è di **tipo int**. Gli input validi sono la *partion_number* di un indice o di un heap, null, 0 o default. Il valore predefinito è 0. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto.  
  
 Specificare NULL per restituire le informazioni per tutte le partizioni dell'oggetto.  
  
 *partition_number* è in base 1. Un indice o heap non partizionato ha *partition_number* impostato su 1.  
  
 *modalità* \| \| valore predefinito null  
 Nome della modalità. *mode* specifica il livello di analisi utilizzato per ottenere le statistiche. *mode* è di **tipo sysname**. Gli input validi sono DEFAULT, NULL, LIMITED, SAMPLED o DETAILED. Il valore predefinito (NULL) è LIMITED.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|ID database della tabella o della vista.|  
|object_id|**int**|ID oggetto della tabella o della vista in cui è contenuto l'indice.|  
|index_id|**int**|ID di un indice.<br /><br /> 0 = Heap.|  
|partition_number|**int**|Numero di partizione in base 1 all'interno dell'oggetto proprietario, una tabella, una vista o un indice.<br /><br /> 1 = Indice o heap non partizionato.|  
|index_type_desc|**nvarchar(60)**|Descrizione del tipo di indice:<br /><br /> HEAP<br /><br /> CLUSTERED INDEX<br /><br /> NONCLUSTERED INDEX<br /><br /> PRIMARY XML INDEX<br /><br /> EXTENDED INDEX<br /><br /> XML INDEX<br /><br /> Indice di MAPPING COLUMNStore (interno)<br /><br /> Indice DELETEBUFFER COLUMNStore (interno)<br /><br /> Indice DELETEBITMAP COLUMNStore (interno)|  
|hobt_id|**bigint**|ID dell'heap o dell'albero B dell'indice o della partizione.<br /><br /> Oltre a restituire la hobt_id di indici definiti dall'utente, viene restituito anche il hobt_id degli indici columnstore interni.|  
|alloc_unit_type_desc|**nvarchar(60)**|Descrizione del tipo dell'unità di allocazione:<br /><br /> IN_ROW_DATA<br /><br /> LOB_DATA<br /><br /> ROW_OVERFLOW_DATA<br /><br /> L'unità di allocazione LOB_DATA contiene i dati archiviati nelle colonne di tipo **Text**, **ntext**, **Image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)** e **XML**. Per altre informazioni, vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).<br /><br /> L'unità di allocazione ROW_OVERFLOW_DATA contiene i dati archiviati nelle colonne di tipo **varchar (n)**, **nvarchar (n)**, **varbinary (n)** e **sql_variant** spostati all'esterno di righe.|  
|index_depth|**tinyint**|Numero di livelli dell'indice.<br /><br /> 1 = Heap o unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA.|  
|index_level|**tinyint**|Livello corrente dell'indice.<br /><br /> 0 per i livelli foglia di indice, gli heap e le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA.<br /><br /> Maggiore di 0 per i livelli dell'indice non foglia. *index_level* sarà il più elevato al livello di radice di un indice.<br /><br /> I livelli non foglia degli indici vengono elaborati solo quando *mode* = detailed.|  
|avg_fragmentation_in_percent|**float**|Frammentazione logica per gli indici o frammentazione extent per gli heap nell'unità di allocazione IN_ROW_DATA.<br /><br /> Il valore viene misurato come percentuale e a tal fine vengono presi in considerazione più file. Per le definizioni di frammentazione logica ed extent, vedere la sezione Osservazioni.<br /><br /> 0 per le unità di allocazione LOB_DATA e ROW_OVERFLOW_DATA.<br /><br /> NULL per gli heap quando *mode* = sampled.|  
|fragment_count|**bigint**|Numero di frammenti nel livello foglia di un'unità di allocazione IN_ROW_DATA. Per ulteriori informazioni sui frammenti, vedere la sezione Osservazioni.<br /><br /> NULL per i livelli non foglia di un indice e per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA.<br /><br /> NULL per gli heap quando *mode* = sampled.|  
|avg_fragment_size_in_pages|**float**|Numero medio di pagine in un frammento nel livello foglia di un'unità di allocazione IN_ROW_DATA.<br /><br /> NULL per i livelli non foglia di un indice e per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA.<br /><br /> NULL per gli heap quando *mode* = sampled.|  
|page_count|**bigint**|Numero totale di pagine di dati o di indice.<br /><br /> Per un indice, il numero totale di pagine di indice nel livello corrente dell'albero B nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per un heap, il numero totale di pagine di dati nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA, il numero totale di pagine nell'unità di allocazione.|  
|avg_page_space_used_in_percent|**float**|Percentuale media dello spazio di archiviazione dei dati utilizzato in tutte le pagine.<br /><br /> Per un indice, la media si applica al livello corrente dell'albero B nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per un heap, indica la media di tutte le pagine di dati nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per le unità di allocazione LOB_DATA o ROW_OVERFLOW DATA, indica la media di tutte le pagine nell'unità di allocazione.<br /><br /> NULL quando *mode* = Limited.|  
|record_count|**bigint**|Numero totale di record.<br /><br /> Per un indice, il numero totale di record si applica al livello corrente dell'albero B nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per un heap, indica il numero totale di pagine di dati nell'unità di allocazione IN_ROW_DATA.<br /><br /> **Nota:** Per un heap, il numero di record restituiti da questa funzione potrebbe non corrispondere al numero di righe restituite eseguendo un SELECT COUNT () sull' \* heap. Questo perché una riga potrebbe contenere più record. Ad esempio, in alcune situazioni di aggiornamento, un'unica riga dell'heap potrebbe presentare un record di inoltro e un record inoltrato a seguito dell'operazione di aggiornamento. Inoltre, nell'archiviazione LOB_DATA la maggior parte delle righe LOB viene suddivisa in più record.<br /><br /> Per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA, indica il numero totale di record nell'unità di allocazione completa.<br /><br /> NULL quando *mode* = Limited.|  
|ghost_record_count|**bigint**|Numero di record fantasma pronti per la rimozione tramite l'attività di pulizia dei record fantasma nell'unità di allocazione.<br /><br /> 0 per i livelli non foglia di un indice nell'unità di allocazione IN_ROW_DATA.<br /><br /> NULL quando *mode* = Limited.|  
|version_ghost_record_count|**bigint**|Numero di record fantasma mantenuti da una transazione di isolamento dello snapshot in attesa in un'unità di allocazione.<br /><br /> 0 per i livelli non foglia di un indice nell'unità di allocazione IN_ROW_DATA.<br /><br /> NULL quando *mode* = Limited.|  
|min_record_size_in_bytes|**int**|Dimensioni minime dei record in byte.<br /><br /> Per un indice, le dimensioni minime dei record si applicano al livello corrente dell'albero B nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per un heap, indica le dimensioni minime dei record nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA, indica le dimensioni minime dei record nell'unità di allocazione completa.<br /><br /> NULL quando *mode* = Limited.|  
|max_record_size_in_bytes|**int**|Dimensioni massime dei record in byte.<br /><br /> Per un indice, le dimensioni massime dei record si applicano al livello corrente dell'albero B nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per un heap, indica le dimensioni massime dei record nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA, indica le dimensioni massime dei record nell'unità di allocazione completa.<br /><br /> NULL quando *mode* = Limited.|  
|avg_record_size_in_bytes|**float**|Dimensioni medie dei record in byte.<br /><br /> Per un indice, le dimensioni medie dei record si applicano al livello corrente dell'albero B nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per un heap, indica le dimensioni medie dei record nell'unità di allocazione IN_ROW_DATA.<br /><br /> Per le unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA, indica le dimensioni medie dei record nell'unità di allocazione completa.<br /><br /> NULL quando *mode* = Limited.|  
|forwarded_record_count|**bigint**|Numero di record in un heap che hanno inoltrato puntatori a un altro percorso dei dati. Questo stato si verifica durante un aggiornamento, nel caso in cui non vi sia spazio sufficiente per archiviare la riga nel percorso originale.<br /><br /> NULL per tutte le unità di allocazione escluse le unità di allocazione IN_ROW_DATA per un heap.<br /><br /> NULL per gli heap quando *mode* = Limited.|  
|compressed_page_count|**bigint**|Numero di pagine compresse.<br /><br /> Per gli heap, alle pagine appena allocate non viene applicata la compressione di tipo PAGE. A un heap viene applicata la compressione di tipo PAGE in due condizioni speciali, ovvero quando i dati vengono importati mediante un'operazione bulk o quando un heap viene ricompilato. Alle operazioni DML tipiche che determinano le allocazioni delle pagine non verrà applicata la compressione di tipo PAGE. Ricompilare un heap quando il valore compressed_page_count aumenta oltre la soglia desiderata.<br /><br /> Per tabelle in cui è presente un indice cluster, il valore compressed_page_count indica l'efficacia della compressione di tipo PAGE.|  
|hobt_id|bigint|Solo per gli indici columnstore, questo è l'ID di un set di righe che tiene traccia dei dati columnstore interni per una partizione. I set di righe vengono archiviati come heap di dati o alberi binari. Hanno lo stesso ID di indice dell'indice columnstore padre. Per ulteriori informazioni, vedere [sys.internal_partitions &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md).<br /><br /> NULL se <br /><br /> **Si applica a**: SQL Server 2016 e versioni successive, database SQL di Azure, azure SQL istanza gestita|  
|column_store_delete_buffer_state|TINYINT| 0 = NOT_APPLICABLE<br /><br /> 1 = OPEN<br /><br /> 2 = SVUOTAMENTO<br /><br /> 3 = SCARICAMENTO<br /><br /> 4 = RITIRO<br /><br /> 5 = PRONTO<br /><br />**Si applica a**: SQL Server 2016 e versioni successive, database SQL di Azure, azure SQL istanza gestita|  
|column_store_delete_buff_state_desc|| NON valido: l'indice padre non è un indice columnstore.<br /><br /> Gli scanner e gli eliminatori utilizzano questa operazione.<br /><br /> SVUOTAMENTO: gli eliminatori si svuotano, ma gli scanner lo usano ancora.<br /><br /> Il buffer di SCARICAmento viene chiuso e le righe nel buffer vengono scritte nella bitmap Delete.<br /><br /> Il ritiro delle righe nel buffer di eliminazione chiuso è stato scritto nella bitmap di eliminazione, ma il buffer non è stato troncato perché gli scanner lo usano ancora. I nuovi scanner non devono usare il buffer di ritiro perché il buffer aperto è sufficiente.<br /><br /> PRONTO: il buffer di eliminazione è pronto per l'uso. <br /><br /> **Si applica a**: SQL Server 2016 e versioni successive, database SQL di Azure, azure SQL istanza gestita|  
|version_record_count|**bigint**|Conteggio dei record della versione di riga mantenuti in questo indice.  Queste versioni di riga vengono gestite dalla funzionalità di [recupero accelerato del database](../../relational-databases/accelerated-database-recovery-concepts.md) . <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|inrow_version_record_count|**bigint**|Numero di record di versione ADR conservati nella riga di dati per il recupero rapido. <br /><br />  [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e|  
|inrow_diff_version_record_count|**bigint**| Numero di record di versione ADR mantenuti sotto forma di differenze rispetto alla versione di base. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|total_inrow_version_payload_size_in_bytes|**bigint**|Dimensioni totali in byte dei record della versione delle per questo indice. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|offrow_regular_version_record_count|**bigint**|Conteggio dei record versione conservati all'esterno della riga di dati originale. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|offrow_long_term_version_record_count|**bigint**|Conteggio dei record versione considerati a lungo termine. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  

## <a name="remarks"></a>Osservazioni  
 La funzione a gestione dinamica sys.dm_db_index_physical_stats sostituisce l'istruzione DBCC SHOWCONTIG.  
  
## <a name="scanning-modes"></a>Modalità di analisi  
 La modalità di esecuzione della funzione determina il livello di analisi eseguito per ottenere i dati statistici utilizzati dalla funzione. la *modalità* è specificata come Limited, SAMPLED o detailed. La funzione consente di attraversare le catene di pagine per le unità di allocazione che costituiscono le partizioni specificate della tabella o dell'indice. sys.dm_db_index_physical_stats richiede solo un blocco di tabella Intent-Shared (IS), indipendentemente dalla modalità in cui viene eseguito.  
  
 La modalità LIMITED è la più veloce ed esegue l'analisi del minor numero di pagine. Per un indice, viene eseguita l'analisi delle sole pagine di livello padre dell'albero B (ovvero le pagine sopra il livello foglia). Per un heap, vengono esaminate solo le pagine associate PFS e IAM, mentre le pagine di dati vengono analizzate in modalità LIMITED.  
  
 Con la modalità LIMITED, compressed_page_count è NULL perché [!INCLUDE[ssDE](../../includes/ssde-md.md)] analizza solo le pagine non foglia dell'albero B e le pagine IAM e PFS dell'heap. Utilizzare la modalità SAMPLEd per ottenere un valore stimato per compressed_page_count e utilizzare la modalità dettagliata per ottenere il valore effettivo per compressed_page_count. La modalità SAMPLED restituisce statistiche in base a un campione pari all'1% di tutte le pagine nell'indice o nell'heap. I risultati ottenuti in modalità SAMPLED devono essere considerati approssimativi. Se l'indice o l'heap contiene meno di 10.000 pagine, viene utilizzata la modalità DETAILED invece della modalità SAMPLED.  
  
 La modalità DETAILED esegue l'analisi di tutte le pagine e restituisce tutte le statistiche.  
  
 La velocità della modalità è decrescente da LIMITED a DETAILED, a causa dell'aumento della quantità di lavoro. Per misurare rapidamente le dimensioni o il livello di frammentazione di una tabella o di un indice, utilizzare la modalità LIMITED. Si tratta della modalità più veloce e non restituisce una riga per ogni livello non foglia nell'unità di allocazione IN_ROW_DATA dell'indice.  
  
## <a name="using-system-functions-to-specify-parameter-values"></a>Utilizzo di funzioni di sistema per specificare i valori dei parametri  
 È possibile utilizzare le [!INCLUDE[tsql](../../includes/tsql-md.md)] funzioni [DB_ID](../../t-sql/functions/db-id-transact-sql.md) e [object_id](../../t-sql/functions/object-id-transact-sql.md) per specificare un valore per i parametri *database_id* e *object_id* . Se si passano valori non validi a queste funzioni, tuttavia, si potrebbero provocare risultati imprevisti. Se, ad esempio, non è possibile trovare il nome del database o dell'oggetto poiché non esiste o non è stato digitato correttamente, entrambe le funzioni restituiranno NULL. La funzione sys.dm_db_index_physical_stats interpreta NULL come un valore di carattere jolly che specifica tutti i database o tutti gli oggetti.  
  
 Inoltre, la funzione OBJECT_ID viene elaborata prima della chiamata della funzione sys.dm_db_index_physical_stats e pertanto viene valutata nel contesto del database corrente, non nel database specificato in *database_id*. Questo comportamento può causare la restituzione di un valore NULL da parte della funzione OBJECT_ID. In alternativa, se il nome dell'oggetto esiste sia nel contesto del database corrente, sia nel database specificato, può essere restituito un messaggio di errore. Negli esempi seguenti vengono illustrati questi risultati imprevisti.  
  
```  
USE master;  
GO  
-- In this example, OBJECT_ID is evaluated in the context of the master database.   
-- Because Person.Address does not exist in master, the function returns NULL.  
-- When NULL is specified as an object_id, all objects in the database are returned.  
-- The same results are returned when an object that is not valid is specified.  
SELECT * FROM sys.dm_db_index_physical_stats  
    (DB_ID(N'AdventureWorks'), OBJECT_ID(N'Person.Address'), NULL, NULL , 'DETAILED');  
GO  
-- This example demonstrates the results of specifying a valid object name  
-- that exists in both the current database context and  
-- in the database specified in the database_id parameter of the   
-- sys.dm_db_index_physical_stats function.  
-- An error is returned because the ID value returned by OBJECT_ID does not  
-- match the ID value of the object in the specified database.  
CREATE DATABASE Test;  
GO  
USE Test;  
GO  
CREATE SCHEMA Person;  
GO  
CREATE Table Person.Address(c1 int);  
GO  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_index_physical_stats  
    (DB_ID(N'Test'), OBJECT_ID(N'Person.Address'), NULL, NULL , 'DETAILED');  
GO  
-- Clean up temporary database.  
DROP DATABASE Test;  
GO  
```  
  
### <a name="best-practice"></a>Procedura consigliata  
 Quando si usa DB_ID o OBJECT_ID, verificare sempre che venga restituito un ID valido. Ad esempio, quando si utilizza OBJECT_ID, specificare un nome in tre parti, ad esempio `OBJECT_ID(N'AdventureWorks2012.Person.Address')` , o testare il valore restituito dalle funzioni prima di utilizzarle nella funzione sys.dm_db_index_physical_stats. Negli esempi A e B seguenti viene illustrato come specificare ID database e oggetto in modo sicuro.  
  
## <a name="detecting-fragmentation"></a>Rilevamento della frammentazione  
 La frammentazione si verifica nel corso delle modifiche dei dati (istruzioni INSERT, UPDATE e DELETE) apportate alla tabella e, di conseguenza, agli indici in essa definiti. Poiché tali modifiche in genere non sono distribuite in modo equo tra le righe della tabella e gli indici, il livello di riempimento di ogni pagina può variare nel tempo. Nel caso di query che eseguono l'analisi di una parte o di tutti gli indici di una tabella, questo tipo di frammentazione potrebbe comportare letture di pagine aggiuntive, operazione che ostacola l'analisi parallela dei dati.  
  
 Il livello di frammentazione di un indice o di un heap viene illustrato nella colonna avg_fragmentation_in_percent. Per gli heap, il valore rappresenta la frammentazione extent dell'heap. Per gli indici, il valore rappresenta la frammentazione logica dell'indice. Diversamente da DBCC SHOWCONTIG, gli algoritmi di calcolo della frammentazione considerano in entrambi i casi l'archivio che si estende su più file e sono pertanto accurati.  
  
 **Frammentazione logica**  
  
 Percentuale delle pagine non ordinate nelle pagine foglia di un indice. Una pagina risulta non ordinata quando la pagina fisica successiva allocata all'indice è diversa da quella a cui fa riferimento il *puntatore di pagina successiva* nella pagina foglia corrente.  
  
 **Frammentazione extent**  
  
 Percentuale di extent non ordinati nelle pagine foglia di un heap. Un extent è considerato non ordinato quando l'extent che contiene la pagina corrente per un heap non è fisicamente l'extent successivo a quello che contiene la pagina precedente.  
  
 Il valore per avg_fragmentation_in_percent deve essere il più possibile prossimo a zero per ottenere le massime prestazioni. Tuttavia, i valori compresi tra 0% e 10% possono essere accettabili. Per ridurre questi valori, è possibile utilizzare tutti i metodi per ridurre la frammentazione, ad esempio ricompilazione, riorganizzazione o ricreazione. Per ulteriori informazioni su come analizzare il livello di frammentazione in un indice, vedere [riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
## <a name="reducing-fragmentation-in-an-index"></a>Riduzione della frammentazione in un indice  
 Quando la frammentazione di un indice avviene in modo tale da influire sulle prestazioni delle query, è possibile effettuare una delle tre operazioni seguenti:  
  
-   Rimuovere e ricreare l'indice cluster.  
  
     Ricreando un indice cluster i dati vengono ridistribuiti e si ottengono pagine di dati complete. È possibile configurare il livello di riempimento tramite l'opzione FILLFACTOR dell'istruzione CREATE INDEX. Gli svantaggi di questo metodo consistono nel fatto che l'indice è offline durante il ciclo di rimozione e ricreazione e che l'operazione è atomica. Se la creazione dell'indice viene interrotta, l'indice non viene ricreato. Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
-   Utilizzare ALTER INDEX REORGANIZE, l'alternativa a DBCC INDEXDEFRAG, per riordinare le pagine di livello foglia dell'indice in un ordine logico. Poiché si tratta di un'operazione online, l'indice è disponibile durante l'esecuzione dell'istruzione. L'operazione può essere interrotta senza perdere il lavoro già completato. Lo svantaggio di questo metodo è che il processo di riorganizzazione dei dati non è tanto efficiente quanto un'operazione di ricompilazione dell'indice e non aggiorna le statistiche.  
  
-   Utilizzare ALTER INDEX REBUILD, l'alternativa a DBCC DBREINDEX, per ricompilare l'indice online o offline. Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
 La sola frammentazione non è un motivo sufficiente per riorganizzare o ricompilare un indice. L'effetto principale della frammentazione è che rallenta la velocità read-ahead effettiva delle pagine durante le analisi di indici, provocando tempi di risposta più lenti. Se il carico di lavoro della query in una tabella o in un indice frammentato non comporta analisi, poiché il carico di lavoro è rappresentato principalmente da ricerche singleton, la rimozione della frammentazione potrebbe non avere alcun effetto.
  
> [!NOTE]  
>  L'esecuzione di DBCC SHRINKFILE o DBCC SHRINKDATABASE potrebbe provocare la frammentazione se un indice viene spostato completamente o in parte durante l'operazione di compattazione. Pertanto, se è necessario eseguire un'operazione di compattazione, è consigliabile eseguirla prima della rimozione della frammentazione.  
  
## <a name="reducing-fragmentation-in-a-heap"></a>Riduzione della frammentazione in un heap  
 Per ridurre la frammentazione extent di un heap, creare un indice cluster nella tabella e quindi rimuovere l'indice. I dati vengono quindi ridistribuiti in maniera ottimale durante la creazione dell'indice cluster, tenendo in considerazione la distribuzione dello spazio disponibile nel database. Quando l'indice cluster viene successivamente rimosso per ricreare l'heap, i dati non vengono spostati e rimangono nella posizione ottimale. Per informazioni sull'esecuzione di queste operazioni, vedere [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) e [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md).  
  
> [!CAUTION]  
>  La creazione e l'eliminazione di un indice cluster in una tabella ricompilano tutti gli indici non cluster di tale tabella due volte.  
  
## <a name="compacting-large-object-data"></a>Compattazione dei dati LOB (Large Object)  
 Per impostazione predefinita, l'istruzione ALTER INDEX REORGANIZE compatta le pagine contenenti dati LOB. Poiché le pagine LOB non vengono deallocate quando sono vuote, la compattazione di questi dati può migliorare l'utilizzo dello spazio su disco se sono stati cancellati molti dati LOB o se viene eliminata una colonna LOB.  
  
 La riorganizzazione di un indice cluster specificato compatta tutte le colonne LOB contenute nell'indice cluster. La riorganizzazione di un indice non cluster compatta tutte le colonne LOB che sono colonne non chiave (incluse) nell'indice. Quando viene specificato ALL nell'istruzione, tutti gli indici associati alla tabella o vista vengono riorganizzati. Inoltre, tutte le colonne LOB associate all'indice cluster, alla tabella sottostante o all'indice non cluster con colonne vengono compattati.  
  
## <a name="evaluating-disk-space-use"></a>Valutazione dell'utilizzo dello spazio su disco  
 La colonna avg_page_space_used_in_percent indica il livello di riempimento pagina. Per ottenere un utilizzo ottimale dello spazio su disco, è necessario che questo valore sia prossimo al 100% per un indice che non avrà molti inserimenti casuali. Tuttavia, un indice con molti inserimenti casuali e pagine con un alto livello di riempimento avrà un maggior numero di divisioni di pagina e di conseguenza una maggiore frammentazione. Pertanto, per ridurre il numero di divisioni di pagina, è necessario che il valore sia inferiore a 100%. La ricompilazione di un indice con l'opzione FILLFACTOR specificata consente la modifica del livello di riempimento pagina in modo che corrisponda al modello di query nell'indice. Per ulteriori informazioni sul fattore di riempimento, vedere [specificare un fattore di riempimento per un indice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md). ALTER INDEX REORGANIZE compatterà inoltre un indice cercando di riempire le pagine fino al fattore di riempimento FILLFACTOR specificato per ultimo, facendo aumentare il valore in avg_space_used_in_percent. Si noti che ALTER INDEX REORGANIZE non può ridurre il livello di riempimento pagina. È necessario invece eseguire una ricompilazione dell'indice.  
  
## <a name="evaluating-index-fragments"></a>Valutazione dei frammenti di indice  
 Un frammento è composto da pagine foglia fisicamente consecutive nello stesso file per un'unità di allocazione. Ogni indice ha almeno un frammento. Il numero massimo di frammenti che un indice può avere è uguale al numero di pagine nel livello foglia dell'indice. La presenza di frammenti più grandi indica che è necessaria una quantità minore di I/O su disco per leggere lo stesso numero di pagine. Pertanto, maggiore sarà il valore di avg_fragment_size_in_pages e migliori saranno le prestazioni di analisi dell'intervallo. I valori avg_fragment_size_in_pages e avg_fragmentation_in_percent sono inversamente proporzionali. La ricompilazione o la riorganizzazione di un indice dovrebbe quindi ridurre la quantità di frammentazione e far aumentare le dimensioni del frammento.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non restituisce dati per gli indici columnstore cluster.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste le autorizzazioni seguenti:  
  
-   Autorizzazione CONTROL per l'oggetto specificato all'interno del database.  
  
-   Autorizzazione VIEW DATABASE STATE per la restituzione di informazioni su tutti gli oggetti all'interno del database specificato, tramite il carattere jolly di oggetto @*object_id*= null.  
  
-   Autorizzazione VIEW SERVER STATE per la restituzione di informazioni su tutti i database, tramite il carattere jolly di database @*database_id* = null.  
  
 La concessione di VIEW DATABASE STATE consente la restituzione di tutti gli oggetti nel database, indipendentemente dalle eventuali autorizzazioni CONTROL negate per oggetti specifici.  
  
 La negazione di VIEW DATABASE STATE non consente la restituzione di tutti gli oggetti nel database, indipendentemente dalle eventuali autorizzazioni CONTROL concesse per oggetti specifici. Inoltre, quando viene specificato il carattere jolly di database @*database_id*= null, il database viene omesso.  
  
 Per altre informazioni, vedere [viste a gestione dinamica e funzioni &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-information-about-a-specified-table"></a>R. Restituzione delle informazioni su una tabella specificata  
 Nell'esempio seguente vengono restituite le statistiche su dimensioni e frammentazione per tutti gli indici e le partizioni della tabella `Person.Address`. La modalità di analisi viene impostata su `'LIMITED'` per ottimizzare le prestazioni e limitare le statistiche restituite. L'esecuzione di questa query richiede almeno l'autorizzazione CONTROL per la tabella `Person.Address`.  
  
```  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
  
IF @db_id IS NULL  
BEGIN;  
    PRINT N'Invalid database';  
END;  
ELSE IF @object_id IS NULL  
BEGIN;  
    PRINT N'Invalid object';  
END;  
ELSE  
BEGIN;  
    SELECT * FROM sys.dm_db_index_physical_stats(@db_id, @object_id, NULL, NULL , 'LIMITED');  
END;  
GO  
  
```  
  
### <a name="b-returning-information-about-a-heap"></a>B. Restituzione delle informazioni su un heap  
 Nell'esempio seguente vengono restituite le statistiche per l'heap `dbo.DatabaseLog` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Poiché la tabella contiene dati LOB, viene restituita una riga per l'unità di allocazione `LOB_DATA` in aggiunta alla riga restituita per `IN_ROW_ALLOCATION_UNIT` che archivia le pagine di dati dell'heap. L'esecuzione di questa query richiede almeno l'autorizzazione CONTROL per la tabella `dbo.DatabaseLog`.  
  
```  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.dbo.DatabaseLog');  
IF @object_id IS NULL   
BEGIN;  
    PRINT N'Invalid object';  
END;  
ELSE  
BEGIN;  
    SELECT * FROM sys.dm_db_index_physical_stats(@db_id, @object_id, 0, NULL , 'DETAILED');  
END;  
GO  
  
```  
  
### <a name="c-returning-information-for-all-databases"></a>C. Restituzione delle informazioni su tutti i database  
 Nell'esempio seguente vengono restituite tutte le statistiche per tutte le tabelle e gli indici presenti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificando il carattere jolly `NULL` per tutti i parametri. L'esecuzione di questa query richiede l'autorizzazione VIEW SERVER STATE.  
  
```  
SELECT * FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, NULL);  
GO  
  
```  
  
### <a name="d-using-sysdm_db_index_physical_stats-in-a-script-to-rebuild-or-reorganize-indexes"></a>D. Utilizzo di sys.dm_db_index_physical_stats in uno script per ricompilare o riorganizzare gli indici  
 Nell'esempio seguente vengono riorganizzate o ricompilate automaticamente tutte le partizioni di un database che hanno una frammentazione media superiore al 10%. Per l'esecuzione di questa query, è necessario disporre dell'autorizzazione VIEW DATABASE STATE. Nell'esempio viene specificato `DB_ID` come primo parametro senza includere un nome di database. Se il database corrente ha un livello di compatibilità pari a 80 o inferiore, viene generato un errore. Per risolvere l'errore, sostituire `DB_ID()` con un nome di database valido. Per ulteriori informazioni sui livelli di compatibilità del database, vedere [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
```  
-- Ensure a USE <databasename> statement has been executed first.  
SET NOCOUNT ON;  
DECLARE @objectid int;  
DECLARE @indexid int;  
DECLARE @partitioncount bigint;  
DECLARE @schemaname nvarchar(130);   
DECLARE @objectname nvarchar(130);   
DECLARE @indexname nvarchar(130);   
DECLARE @partitionnum bigint;  
DECLARE @partitions bigint;  
DECLARE @frag float;  
DECLARE @command nvarchar(4000);   
-- Conditionally select tables and indexes from the sys.dm_db_index_physical_stats function   
-- and convert object and index IDs to names.  
SELECT  
    object_id AS objectid,  
    index_id AS indexid,  
    partition_number AS partitionnum,  
    avg_fragmentation_in_percent AS frag  
INTO #work_to_do  
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL , NULL, 'LIMITED')  
WHERE avg_fragmentation_in_percent > 10.0 AND index_id > 0;  
  
-- Declare the cursor for the list of partitions to be processed.  
DECLARE partitions CURSOR FOR SELECT * FROM #work_to_do;  
  
-- Open the cursor.  
OPEN partitions;  
  
-- Loop through the partitions.  
WHILE (1=1)  
    BEGIN;  
        FETCH NEXT  
           FROM partitions  
           INTO @objectid, @indexid, @partitionnum, @frag;  
        IF @@FETCH_STATUS < 0 BREAK;  
        SELECT @objectname = QUOTENAME(o.name), @schemaname = QUOTENAME(s.name)  
        FROM sys.objects AS o  
        JOIN sys.schemas as s ON s.schema_id = o.schema_id  
        WHERE o.object_id = @objectid;  
        SELECT @indexname = QUOTENAME(name)  
        FROM sys.indexes  
        WHERE  object_id = @objectid AND index_id = @indexid;  
        SELECT @partitioncount = count (*)  
        FROM sys.partitions  
        WHERE object_id = @objectid AND index_id = @indexid;  
  
-- 30 is an arbitrary decision point at which to switch between reorganizing and rebuilding.  
        IF @frag < 30.0  
            SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REORGANIZE';  
        IF @frag >= 30.0  
            SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REBUILD';  
        IF @partitioncount > 1  
            SET @command = @command + N' PARTITION=' + CAST(@partitionnum AS nvarchar(10));  
        EXEC (@command);  
        PRINT N'Executed: ' + @command;  
    END;  
  
-- Close and deallocate the cursor.  
CLOSE partitions;  
DEALLOCATE partitions;  
  
-- Drop the temporary table.  
DROP TABLE #work_to_do;  
GO  
  
```  
  
### <a name="e-using-sysdm_db_index_physical_stats-to-show-the-number-of-page-compressed-pages"></a>E. Utilizzo di sys.dm_db_index_physical_stats per visualizzare il numero di pagine a cui è stata applicata la compressione di pagina  
 Nell'esempio seguente viene illustrato come visualizzare e confrontare il numero complessivo di pagine rispetto a quelle a cui è stata applicata una compressione di riga e di pagina. Tali informazioni possono essere utilizzate per determinare i vantaggi offerti dalla compressione per un indice o per una tabella.  
  
```  
SELECT o.name,  
    ips.partition_number,  
    ips.index_type_desc,  
    ips.record_count, ips.avg_record_size_in_bytes,  
    ips.min_record_size_in_bytes,  
    ips.max_record_size_in_bytes,  
    ips.page_count, ips.compressed_page_count  
FROM sys.dm_db_index_physical_stats ( DB_ID(), NULL, NULL, NULL, 'DETAILED') ips  
JOIN sys.objects o on o.object_id = ips.object_id  
ORDER BY record_count DESC;  
```  
  
### <a name="f-using-sysdm_db_index_physical_stats-in-sampled-mode"></a>F. Utilizzo di sys.dm_db_index_physical_stats in modalità SAMPLED  
 Nell'esempio seguente viene illustrato come la modalità SAMPLED restituisce un valore approssimato diverso che dai risultati della modalità DETAILED.  
  
```  
CREATE TABLE t3 (col1 int PRIMARY KEY, col2 varchar(500)) WITH(DATA_COMPRESSION = PAGE);  
GO  
BEGIN TRAN  
DECLARE @idx int = 0;  
WHILE @idx < 1000000  
BEGIN  
    INSERT INTO t3 (col1, col2)   
    VALUES (@idx,   
    REPLICATE ('a', 100) + CAST (@idx as varchar(10)) + REPLICATE ('a', 380))  
    SET @idx = @idx + 1  
END  
COMMIT;  
GO  
SELECT page_count, compressed_page_count, forwarded_record_count, *   
FROM sys.dm_db_index_physical_stats (db_id(),   
    object_id ('t3'), null, null, 'SAMPLED');  
SELECT page_count, compressed_page_count, forwarded_record_count, *   
FROM sys.dm_db_index_physical_stats (db_id(),   
    object_id ('t3'), null, null, 'DETAILED');  
```  
  
### <a name="g-querying-service-broker-queues-for-index-fragmentation"></a>G. Esecuzione di query sulle code di Service Broker per la frammentazione dell'indice  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 Negli esempi seguenti viene illustrato come eseguire una query sulle code di Service Broker per la frammentazione.  
  
```  
--Using queue internal table name   
select * from sys.dm_db_index_physical_stats (db_id(), object_id ('sys.queue_messages_549576996'), default, default, default)   
  
--Using queue name directly  
select * from sys.dm_db_index_physical_stats (db_id(), object_id ('ExpenseQueue'), default, default, default)  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni a gestione dinamica e DMV correlate all'indice &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [Viste di sistema &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)  
  
