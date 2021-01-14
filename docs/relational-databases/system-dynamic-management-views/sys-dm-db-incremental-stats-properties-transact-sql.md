---
description: sys.dm_db_incremental_stats_properties (Transact-SQL)
title: sys.dm_db_incremental_stats_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_incremental_stats_properties
- sys.dm_db_incremental_stats_properties_TSQL
- dm_db_incremental_stats_properties
- dm_db_incremental_stats_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_incremental_stats_properties
ms.assetid: aa0db893-34d1-419c-b008-224852e71307
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27585efd99f537e6b2f6d3082c341533e8bbea3b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172093"
---
# <a name="sysdm_db_incremental_stats_properties-transact-sql"></a>sys.dm_db_incremental_stats_properties (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le proprietà di statistiche incrementali per l'oggetto di database specificato (tabella) nel database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrente. L'uso di `sys.dm_db_incremental_stats_properties` (che contiene un numero di partizione) è simile a `sys.dm_db_stats_properties` usato per le statistiche non incrementali. 
  
  Questa funzione è stata introdotta in [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] Service Pack 2 e [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Service Pack 1.
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_db_incremental_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Argomenti  
 *object_id*  
 ID dell'oggetto nel database corrente per il quale sono richieste le proprietà di una delle relative statistiche incrementali. *object_id* è di **tipo int**.  
  
 *stats_id*  
 ID delle statistiche per l'oggetto *object_id* specificato. L'ID delle statistiche può essere ottenuto dalla DMV [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* è di tipo **int**.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID dell'oggetto (tabella) per cui restituire le proprietà dell'oggetto statistiche.|  
|stats_id|**int**|ID dell'oggetto statistiche. Il valore è univoco all'interno della tabella. Per altre informazioni, vedere [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|
|partition_number|**int**|Numero di partizione che contiene la parte della tabella.|  
|last_updated|**datetime2**|Data e ora dell'ultimo aggiornamento dell'oggetto statistiche. Per altre informazioni, vedere la sezione [Osservazioni](#Remarks) più avanti nella pagina.|  
|rows|**bigint**|Numero totale di righe della tabella al momento dell'ultimo aggiornamento delle statistiche. Se le statistiche vengono filtrate o corrispondono a un indice filtrato, il numero di righe potrebbe essere inferiore al numero di righe della tabella.|  
|rows_sampled|**bigint**|Numero totale di righe campionate per i calcoli statistici.|  
|steps|**int**|Numero di intervalli nell'istogramma. Per altre informazioni, vedere [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Numero totale di righe nella tabella prima dell'applicazione dell'espressione di filtro (per statistiche filtrate). Se le statistiche non vengono filtrate, unfiltered_rows corrisponde al valore restituito nella colonna rows.|  
|modification_counter|**bigint**|Numero totale di modifiche per la colonna iniziale delle statistiche, la colonna in cui viene compilato l'istogramma, dall'ultimo aggiornamento delle statistiche.<br /><br /> Questa colonna non contiene informazioni per le tabelle ottimizzate per la memoria.|  
  
## <a name="remarks"></a><a name="Remarks"></a> Osservazioni  
 `sys.dm_db_incremental_stats_properties` restituisce un set di righe vuoto in base a una delle condizioni seguenti:  
  
-   `object_id` o `stats_id` è NULL.   
-   L'oggetto specificato non viene trovato oppure non corrisponde a una tabella con statistiche incrementali.  
-   L'ID delle statistiche specificato non corrisponde alle statistiche esistenti per l'ID oggetto specificato.  
-   L'utente corrente non dispone delle autorizzazioni per visualizzare l'oggetto statistiche.
 
 Questo comportamento consente l'utilizzo sicuro di `sys.dm_db_incremental_stats_properties` in caso di applicazione incrociata in viste quali `sys.objects` e `sys.stats`. Questo metodo può restituire le proprietà per le statistiche che corrispondono a ogni partizione. Per visualizzare le proprietà per le statistiche unite combinate in tutte le partizioni, usare sys.dm_db_stats_properties. 

La data di aggiornamento delle statistiche viene archiviata nell'[oggetto BLOB di statistiche](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) insieme all'[istogramma](../../relational-databases/statistics/statistics.md#histogram) e al [vettore di densità](../../relational-databases/statistics/statistics.md#density), non nei metadati. Quando non viene letto alcun dato per generare i dati delle statistiche, il BLOB di statistiche non viene creato, la data non è disponibile e la colonna *last_updated* è null. È il caso delle statistiche filtrate per le quali il predicato non restituisce alcuna riga o delle nuove tabelle vuote.

## <a name="permissions"></a>Autorizzazioni  
 L'utente deve avere autorizzazioni di selezione per le colonne delle statistiche o essere proprietario della tabella o membro del ruolo predefinito del server `sysadmin`, del ruolo predefinito del database `db_owner` o del ruolo predefinito del database `db_ddladmin`.  
  
## <a name="examples"></a>Esempi  

### <a name="a-simple-example"></a>R. Esempio semplice
L'esempio seguente restituisce le statistiche per la tabella `PartitionTable` descritta nell'argomento [Creare tabelle e indici partizionati](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md).

```sql
SELECT * FROM sys.dm_db_incremental_stats_properties (object_id('PartitionTable'), 1);
``` 

Per altre informazioni sull'utilizzo, vedere  [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md).
  
## <a name="see-also"></a>Vedere anche  
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Funzioni e viste a gestione dinamica relative agli oggetti &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
