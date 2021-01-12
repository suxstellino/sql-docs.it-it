---
description: sys.dm_db_xtp_memory_consumers (Transact-SQL)
title: sys.dm_db_xtp_memory_consumers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers_TSQL
- sys.dm_db_xtp_memory_consumers_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_memory_consumers dynamic management view
ms.assetid: f7ab2eaf-e627-464d-91fe-0e170b3f37bc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5b27a5202f20e6ced2ad73734688702cfbb7e7a2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099891"
---
# <a name="sysdm_db_xtp_memory_consumers-transact-sql"></a>sys.dm_db_xtp_memory_consumers (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Indica i consumer di memoria a livello di database nel motore di database di [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. Nella vista viene restituita una riga per ogni consumer di memoria utilizzato dal motore di database. Usare questa DMV per vedere come la memoria viene distribuita tra oggetti interni diversi.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|memory_consumer_id|**bigint**|ID interno del consumer di memoria.|  
|memory_consumer_type|**int**|Tipo di consumer di memoria:<br /><br /> 0=Aggregazione. Aggrega l'utilizzo della memoria due o più consumer. Non deve essere visualizzato.<br /><br /> 2=VARHEAP. Tiene traccia dell'utilizzo di memoria per un heap a lunghezza variabile.<br /><br /> 3=HASH. Tiene traccia dell'utilizzo di memoria per un indice.<br /><br /> 5=pool di pagine del DB. Tiene traccia dell'utilizzo di memoria per un pool di pagine del database per le operazioni di runtime. Ad esempio, le variabili di tabella e alcune analisi serializzabili. È disponibile un solo consumer di memoria di questo tipo per database).|  
|memory_consumer_type_desc|**nvarchar (64)**|Tipo di consumer di memoria: VARHEAP, HASH o PGPOOL.<br /><br /> 0: non deve essere visualizzato.<br /><br /> 2 - VARHEAP<br /><br /> 3 - HASH<br /><br /> 5 - PGPOOL|  
|memory_consumer_desc|**nvarchar (64)**|Descrizione dell'istanza del consumer di memoria:<br /><br /> VARHEAP <br />Heap del database. Utilizzo per allocare i dati utente per un database (righe).<br />Heap di sistema del database. Utilizzato per allocare i dati del database che verranno inclusi nei dump della memoria e non includono dati utente.<br />Heap dell'indice dell'intervallo. Heap privato utilizzato dall'indice dell'intervallo per allocare pagine BW.<br /><br /> HASH: nessuna descrizione poiché la object_id indica la tabella e l'index_id indice hash.<br /><br /> PGPOOL: per il database è presente un solo pool di pagine 64K del database del pool di pagine.|  
|object_id|**bigint**|ID oggetto a cui è attribuita la memoria allocata. Un valore negativo per gli oggetti di sistema.|  
|xtp_object_id|**bigint**|ID oggetto per la tabella ottimizzata per la memoria.|  
|index_id|**int**|ID dell'indice del consumer, se disponibile. NULL per le tabelle di base.|  
|allocated_bytes|**bigint**|Numero di byte riservati al consumer.|  
|used_bytes|**bigint**|Byte utilizzati dal consumer. Si applica solo a varheap.|  
|allocation_count|**int**|Numero di allocazioni.|  
|partition_count|**int**|Solo per uso interno.|  
|sizeclass_count|**int**|Solo per uso interno.|  
|min_sizeclass|**int**|Solo per uso interno.|  
|max_sizeclass|**int**|Solo per uso interno.|  
|memory_consumer_address|**varbinary**|Indirizzo interno del consumer. Solo per uso interno.|  
|xtp_object_id|**bigint**|ID oggetto OLTP in memoria che corrisponde alla tabella ottimizzata per la memoria.|  
  
## <a name="remarks"></a>Osservazioni  
 Nell'output gli allocatori a livello di database fanno riferimento a tabelle utente, indici e tabelle di sistema. VARHEAP con object_id = NULL indica la memoria allocata alle tabelle con colonne a lunghezza variabile.  
  
## <a name="permissions"></a>Autorizzazioni  
 Vengono restituite tutte le righe se si dispone dell'autorizzazione VIEW DATABASE STATE per il database corrente. In caso contrario, viene restituito un set di righe vuoto.  
  
 Se non si dispone dell'autorizzazione VIEW DATABASE, verranno restituite tutte le colonne per le righe nelle tabelle per cui si dispone dell'autorizzazione SELECT.  
  
 Vengono restituite le tabelle di sistema solo per gli utenti con l'autorizzazione VIEW DATABASE STATE.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Quando una tabella ottimizzata per la memoria dispone di un indice columnstore, il sistema usa alcune tabelle interne che utilizzano una certa quantità di memoria per tenere traccia dei dati per l'indice columnstore. Per informazioni dettagliate su queste tabelle interne e query di esempio che mostrano il consumo di memoria, vedere [sys.memory_optimized_tables_internal_attributes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md).
 
  
## <a name="examples"></a>Esempi  
  
```  
-- memory consumers (database level)  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_memory_consumers;  
```  
  
## <a name="user-scenario"></a>Scenario utente  
  
```  
-- memory consumers (database level)  
  
select  convert(char(10), object_name(object_id)) as Name,   
convert(char(10),memory_consumer_type_desc ) as memory_consumer_type_desc, object_id,index_id, allocated_bytes,  used_bytes   
from sys.dm_db_xtp_memory_consumers  
```  
  
 Di seguito è riportato l'output con un subset di colonne. Gli allocatori a livello di database fanno riferimento a tabelle utente, indici e tabelle di sistema. VARHEAP con object_id = NULL (ultima riga) fa riferimento alla memoria allocata alle righe di dati delle tabelle (t1 in questo esempio). I byte allocati, una volta convertiti in MB, sono 1340 MB.  
  
```  
Name       memory_consumer_type_desc object_id   index_id    allocated_bytes      used_bytes  
---------- ------------------------- ----------- ----------- -------------------- --------------------  
t3         HASH                      629577281   2           8388608              8388608  
t2         HASH                      597577167   2           8388608              8388608  
t1         HASH                      565577053   2           1048576              1048576  
NULL       HASH                      -6          1           2048                 2048  
NULL       VARHEAP                   -6          NULL        0                    0  
NULL       HASH                      -5          3           8192                 8192  
NULL       HASH                      -5          2           8192                 8192  
NULL       HASH                      -5          1           8192                 8192  
NULL       HASH                      -4          1           2048                 2048  
NULL       VARHEAP                   -4          NULL        0                    0  
NULL       HASH                      -3          1           2048                 2048  
NULL       HASH                      -2          2           8192                 8192  
NULL       HASH                      -2          1           8192                 8192  
NULL       VARHEAP                   -2          NULL        196608               26496  
NULL       HASH                      0           1           2048                 2048  
NULL       PGPOOL                    0           NULL        0                    0  
NULL       VARHEAP                   NULL        NULL        1405943808           1231220560  
  
(17 row(s) affected)  
```  
  
 La memoria totale allocata e utilizzata da questa DMV corrisponde al livello dell'oggetto in [sys.dm_db_xtp_table_memory_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md).  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
        sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1358                 1191  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
