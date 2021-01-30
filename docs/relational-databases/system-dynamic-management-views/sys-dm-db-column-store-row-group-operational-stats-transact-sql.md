---
description: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4adcc52df1d3039f18ca63cbe93bd355f8df4544
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180660"
---
# <a name="sysdm_db_column_store_row_group_operational_stats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce l'attività corrente del metodo di I/O a livello di riga, di blocco e di accesso per RowGroups compressi in un indice columnstore. Usare **sys.dm_db_column_store_row_group_operational_stats** per tenere traccia del periodo di tempo in cui una query utente deve attendere per leggere o scrivere in un rowgroup compresso o una partizione di un indice columnstore e identificare RowGroups che stanno riscontrando attività di I/o significative o punti sensibili.  
  
 Gli indici columnstore in memoria non vengono visualizzati in questa DMV.  
 
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID della tabella con l'indice columnstore.|  
|**index_id**|**int**|ID dell'indice columnstore.|  
|**partition_number**|**int**|Numero di partizione in base 1 all'interno dell'indice o heap.|  
|**row_group_id**|**int**|ID di rowgroup nell'indice columnstore. Questa operazione è univoca all'interno di una partizione.|  
|**scan_count**|**int**|Numero di analisi eseguite da rowgroup dall'ultimo riavvio di SQL.|  
|**delete_buffer_scan_count**|**int**|Numero di volte in cui è stato utilizzato il buffer di eliminazione per determinare le righe eliminate in questo rowgroup. Ciò include l'accesso alla tabella hash in memoria e al albero b sottostante.|  
|**index_scan_count**|**int**|Numero di volte in cui è stata analizzata la partizione dell'indice columnstore. Questo è lo stesso per tutti i RowGroups della partizione.|  
|**rowgroup_lock_count**|**bigint**|Conteggio cumulativo delle richieste di blocco per questo rowgroup dall'ultimo riavvio di SQL.|  
|**rowgroup_lock_wait_count**|**bigint**|Numero cumulativo di volte in cui il motore di database ha atteso il blocco rowgroup dall'ultimo riavvio di SQL.|  
|**rowgroup_lock_wait_in_ms**|**bigint**|Numero cumulativo di millisecondi di attesa del motore di database in questo blocco rowgroup dall'ultimo riavvio di SQL.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste le autorizzazioni seguenti:  
  
-   Autorizzazione CONTROL per la tabella specificata da object_id.  
  
-   Autorizzazione VIEW DATABASE STATE per la restituzione di informazioni su tutti gli oggetti all'interno del database, tramite il carattere jolly di oggetto @*object_id* = null  
  
 La concessione di VIEW DATABASE STATE consente la restituzione di tutti gli oggetti nel database, indipendentemente dalle eventuali autorizzazioni CONTROL negate per oggetti specifici.  
  
 La negazione di VIEW DATABASE STATE non consente la restituzione di tutti gli oggetti nel database, indipendentemente dalle eventuali autorizzazioni CONTROL concesse per oggetti specifici. Inoltre, quando viene specificato il carattere jolly di database @*database_id*= null, il database viene omesso.  
  
 Per altre informazioni, vedere [viste a gestione dinamica e funzioni &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni a gestione dinamica e DMV correlate all'indice &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  

