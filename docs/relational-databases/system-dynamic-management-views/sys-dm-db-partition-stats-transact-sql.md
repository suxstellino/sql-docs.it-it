---
description: sys.dm_db_partition_stats (Transact-SQL)
title: sys.dm_db_partition_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_partition_stats
- dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_partition_stats dynamic management view
ms.assetid: 9db9d184-b3a2-421e-a804-b18ebcb099b7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0838f88916f2fb1b7847ab1ee27e7f9f13a594ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179326"
---
# <a name="sysdm_db_partition_stats-transact-sql"></a>sys.dm_db_partition_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni relative al conteggio di pagine e righe per ogni partizione nel database corrente.  
  
> [!NOTE]  
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_db_partition_stats**. Il partition_id in sys.dm_pdw_nodes_db_partition_stats è diverso da quello della partition_id nella vista del catalogo sys. partitions per Azure sinapsi Analytics.
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|ID della partizione. Valore univoco all'interno di un database. Si tratta dello stesso valore del **partition_id** nella vista del catalogo **sys.** Partitions ad eccezione di Azure sinapsi Analytics.|  
|**object_id**|**int**|ID oggetto della tabella o della vista indicizzata a cui appartiene la partizione.|  
|**index_id**|**int**|ID dell'heap o dell'indice a cui appartiene la partizione.<br /><br /> 0 = heap<br /><br /> 1 = Indice cluster<br /><br /> > 1 = Indice non cluster|  
|**partition_number**|**int**|Numero di partizione in base 1 all'interno dell'indice o heap.|  
|**in_row_data_page_count**|**bigint**|Numero di pagine utilizzate per l'archiviazione di dati all'interno di righe nella partizione specifica. Se la partizione è inclusa in un heap, il valore corrisponde al numero di pagine di dati nell'heap. Se la partizione è inclusa in un indice, il valore corrisponde al numero di pagine nel livello foglia. (Le pagine non foglia nell'albero B non sono incluse nel conteggio). Le pagine IAM (Index Allocation Map) non sono incluse in entrambi i casi. Sempre 0 per un indice columnstore con ottimizzazione per la memoria xVelocity.|  
|**in_row_used_page_count**|**bigint**|Numero totale di pagine utilizzate per archiviare e gestire i dati all'interno di righe nella partizione corrente. Questo conteggio include pagine non foglia dell'albero B, pagine IAM e tutte le pagine incluse nella colonna **in_row_data_page_count**. Sempre 0 per un indice columnstore.|  
|**in_row_reserved_page_count**|**bigint**|Numero totale di pagine riservate per l'archiviazione e la gestione dei dati all'interno di righe nella partizione corrente, indipendentemente dal fatto che le pagine siano utilizzate o meno. Sempre 0 per un indice columnstore.|  
|**lob_used_page_count**|**bigint**|Numero di pagine utilizzate per l'archiviazione e la gestione delle colonne all'esterno di righe di tipo **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** e **xml** nella partizione. Le pagine IAM sono incluse.<br /><br /> Numero totale di oggetti LOB utilizzati per archiviare e gestire un indice columnstore nella partizione.|  
|**lob_reserved_page_count**|**bigint**|Numero totale di pagine riservate per l'archiviazione e la gestione delle colonne all'esterno di righe di tipo **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** e **xml** nella partizione, indipendentemente dal fatto che le pagine vengano utilizzate o meno. Le pagine IAM sono incluse.<br /><br /> Numero totale di oggetti LOB riservati per l'archiviazione e la gestione di un indice columnstore nella partizione.|  
|**row_overflow_used_page_count**|**bigint**|Numero di pagine utilizzate per l'archiviazione e la gestione delle colonne di overflow della riga di tipo **varchar**, **nvarchar**, **varbinary** e **sql_variant** nella partizione. Le pagine IAM sono incluse.<br /><br /> Sempre 0 per un indice columnstore.|  
|**row_overflow_reserved_page_count**|**bigint**|Numero totale di pagine riservate per l'archiviazione e la gestione delle colonne di overflow della riga di tipo **varchar**, **nvarchar**, **varbinary** e **sql_variant** nella partizione, indipendentemente dal fatto che le pagine vengano utilizzate o meno. Le pagine IAM sono incluse.<br /><br /> Sempre 0 per un indice columnstore.|  
|**used_page_count**|**bigint**|Numero totale di pagine utilizzate per la partizione, Calcolato come **in_row_used_page_count**  +  **lob_used_page_count**  +  **row_overflow_used_page_count**.|  
|**reserved_page_count**|**bigint**|Numero totale di pagine riservate per la partizione Calcolato come **in_row_reserved_page_count**  +  **lob_reserved_page_count**  +  **row_overflow_reserved_page_count**.|  
|**row_count**|**bigint**|Numero approssimato di righe nella partizione.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
|**distribution_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> ID numerico univoco associato alla distribuzione.|  
  
## <a name="remarks"></a>Commenti  
 **sys.dm_db_partition_stats** visualizza informazioni sullo spazio utilizzato per archiviare e gestire i dati LOB all'interno delle righe e i dati di overflow delle righe per tutte le partizioni in un database. Viene visualizzata una riga per partizione.  
  
 I conteggi su cui si basa l'output vengono inseriti nella cache in memoria oppure archiviati su disco in varie tabelle di sistema.  
  
 I dati all'interno di righe, i dati LOB e i dati di overflow della riga rappresentano tre unità di allocazione che compongono una partizione. È possibile eseguire una query sulla vista del catalogo [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) per rilevare i metadati su ciascuna unità di allocazione nel database.  
  
 Se un heap o un indice non è partizionato, esso è composto da una partizione (con numero di partizione = 1). Per tale heap o indice viene pertanto restituita solo una riga. È possibile eseguire una query sulla vista del catalogo [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) per rilevare i metadati su ciascuna partizione di tutte le tabelle e tutti gli indici in un database.  
  
 Il conteggio totale relativo a una tabella specifica o un indice specifico può essere ottenuto tramite l'aggiunta dei conteggi per tutte le partizioni rilevanti.  
  
## <a name="permissions"></a>Autorizzazioni  
 `VIEW DATABASE STATE`Sono necessarie `VIEW DEFINITION` le autorizzazioni e per eseguire una query sulla vista a gestione dinamica **sys.dm_db_partition_stats** . Per ulteriori informazioni sulle autorizzazioni per le viste a gestione dinamica, vedere [funzioni e viste a gestione dinamica &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-all-counts-for-all-partitions-of-all-indexes-and-heaps-in-a-database"></a>R. Restituzione di tutti i conteggi per tutte le partizioni di tutti gli indici e heap in un database  
 Nell'esempio seguente vengono visualizzati tutti i conteggi per tutte le partizioni di tutti gli indici e heap nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats;  
GO  
```  
  
### <a name="b-returning-all-counts-for-all-partitions-of-a-table-and-its-indexes"></a>B. Restituzione di tutti i conteggi per tutte le partizioni di una tabella e dei relativi indici  
 Nell'esempio seguente vengono visualizzati tutti i conteggi per tutte le partizioni della tabella `HumanResources.Employee` e dei relativi indici.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats   
WHERE object_id = OBJECT_ID('HumanResources.Employee');  
GO  
```  
  
### <a name="c-returning-total-used-pages-and-total-number-of-rows-for-a-heap-or-clustered-index"></a>C. Restituzione del numero totale di pagine utilizzate e del numero totale di righe per un heap o un indice cluster  
 Nell'esempio seguente vengono restituiti il numero totale di pagine utilizzate e il numero totale di righe per l'heap o l'indice cluster della tabella `HumanResources.Employee`. Poiché per impostazione predefinita la tabella `Employee` non è partizionata, la somma include solo una partizione.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SUM(used_page_count) AS total_number_of_used_pages,   
    SUM (row_count) AS total_number_of_rows   
FROM sys.dm_db_partition_stats  
WHERE object_id=OBJECT_ID('HumanResources.Employee')    AND (index_id=0 or index_id=1);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  


