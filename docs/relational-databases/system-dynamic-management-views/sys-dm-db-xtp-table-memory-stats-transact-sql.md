---
description: sys.dm_db_xtp_table_memory_stats (Transact-SQL)
title: sys.dm_db_xtp_table_memory_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_table_memory_stats_TSQL
- dm_db_xtp_table_memory_stats
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats
ms.assetid: ad0efc06-3d9c-4861-9dfa-a7a87822d0c8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 12e1f0e140593526314ce12a38df96effadaafb7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099855"
---
# <a name="sysdm_db_xtp_table_memory_stats-transact-sql"></a>sys.dm_db_xtp_table_memory_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Restituisce le statistiche di utilizzo della memoria per ogni tabella di [!INCLUDE[hek_2](../../includes/hek-2-md.md)] (utente e di sistema) nel database corrente. Le tabelle di sistema dispongono di ID oggetto negativi e vengono utilizzate per archiviare le informazioni di runtime per il motore di [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. Diversamente dagli oggetti utente, le tabelle di sistema sono interne e presenti solo in memoria e pertanto non sono visibili tramite le viste del catalogo. Le tabelle di sistema vengono utilizzate per archiviare informazioni quali i metadati di tutti i file di dati/differenziali in memoria, le richieste di tipo merge, filigrane per file differenziali per filtrare le righe, le tabelle eliminate e le informazioni rilevanti per il recupero e i backup. Poiché il motore di [!INCLUDE[hek_2](../../includes/hek-2-md.md)] può avere fino a un massimo di 8.192 coppie di dati e file differenziali, la memoria utilizzata dalle tabelle di sistema può essere costituita da pochi megabyte per i database in memoria di grandi dimensioni.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID oggetto della tabella. NULL per le tabelle di sistema OLTP in memoria.|  
|memory_allocated_for_table_kb|**bigint**|Memoria allocata per la tabella.|  
|memory_used_by_table_kb|**bigint**|Memoria utilizzata dalla tabella, incluse le versioni di riga.|  
|memory_allocated_for_indexes_kb|**bigint**|Memoria allocata per gli indici della tabella.|  
|memory_used_by_indexes_kb|**bigint**|Memoria utilizzata per gli indici della tabella.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Vengono restituite tutte le righe se si dispone dell'autorizzazione VIEW DATABASE STATE per il database corrente. In caso contrario, viene restituito un set di righe vuoto.  
  
 Se non si dispone dell'autorizzazione VIEW DATABASE, verranno restituite tutte le colonne per le righe nelle tabelle per cui si dispone dell'autorizzazione SELECT.  
  
 Vengono restituite le tabelle di sistema solo per gli utenti con l'autorizzazione VIEW DATABASE STATE.  
  
## <a name="examples"></a>Esempi  
 È possibile eseguire una query sulla seguente DMV per ottenere la memoria allocata per tabelle e indici nel database:  
  
```  
-- finding memory for objects  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
 Per trovare la memoria per tutti gli oggetti nel database:  
  
```  
SELECT SUM( memory_allocated_for_indexes_kb + memory_allocated_for_table_kb) AS  
 memoryallocated_objects_in_kb   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
## <a name="user-scenario"></a>Scenario utente  
 Creare in primo luogo le seguenti tabelle di un database denominato HkDb1.  
  
```  
-- set max server memory to 4 GB  
EXEC sp_configure 'max server memory (MB)', 4048  
go  
  
RECONFIGURE  
go  
  
-- create a resource pool for database with memory-optimized objects  
CREATE RESOURCE POOL PoolHkDb1 WITH (MAX_MEMORY_PERCENT = 50);  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
go  
  
--bind the pool to the database  
EXEC sp_xtp_bind_db_resource_pool 'HkDb1', 'PoolHkdb1'  
go  
  
-- take database offline/online to associate the pool  
use master  
go  
  
alter database HkDb1 set offline  
go  
alter database HkDb1 set online  
go  
  
USE HkDb1  
go  
  
CREATE TABLE dbo.t1 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t1_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t2 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t2_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t3 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t3_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 1000000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
-- load 150K rows  
declare @i int = 0  
while (@i <= 150000)  
begin  
       insert t1 values (@i, 'a', replicate ('b', 8000))  
       set @i += 1;  
end  
go  
```  
  
 Quando i dati vengono caricati in una tabella, è possibile vedere le tabelle definite dall'utente e la quantità di spazio di archiviazione che utilizzano. Ad esempio, ogni riga di una tabella potrebbe essere di circa 8070 byte (le dimensioni di allocazione sono 8 K (8192 byte)). È possibile visualizzare gli indici per ogni tabella e la quantità di spazio di archiviazione che utilizzano. Ad esempio, 1 MB è 100 K voci arrotondate alla successiva potenza di 2 (2**17) = 131072 di 8 byte ciascuno. Una tabella può non includere un indice, nel qual caso verrà mostrata l'allocazione di memoria per l'indice. Altre righe possono rappresentare le tabelle di sistema  
  
```  
select convert(char(10), object_name(object_id)) as Name,*   
from sys.dm_db_xtp_table_memory_stats  
```  
  
 Di seguito è riportato l'output, in due parti:  
  
```  
Name       object_id   memory_allocated_for_table_kb memory_used_by_table_kb  
---------- ----------- ----------------------------- -----------------------  
t3         629577281   0                             0  
t1         565577053   1372928                       1202351  
t2         597577167   0                             0  
NULL       -6          0                             0  
NULL       -5          0                             0  
NULL       -4          0                             0  
NULL       -3          0                             0  
NULL       -2          192                           25  
  
memory_allocated_for_indexes_kb memory_used_by_indexes_kb  
------------------------------- -------------------------  
8192                            8192  
1024                            1024  
8192                            8192  
2                               2  
24                              24  
2                               2  
2                               2  
16                              16  
```  
  
 L'output di  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
       sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
```  
  
 è  
  
```  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1357                 1191  
```  
  
 Successivamente, esaminare l'output del pool di risorse. Si noti che la memoria utilizzata dal pool è 1356 MB.  
  
```  
select pool_id,convert(char(10), name) as Name, min_memory_percent, max_memory_percent,   
   max_memory_kb/1024 as max_memory_mb  
from sys.dm_resource_governor_resource_pools  
  
select used_memory_kb/1024 as used_memory_mb ,target_memory_kb/1024 as target_memory_mb  
from sys.dm_resource_governor_resource_pools  
```  
  
 l'output è il seguente:  
  
```  
pool_id     Name       min_memory_percent max_memory_percent max_memory_mb  
----------- ---------- ------------------ ------------------ --------------------  
1           internal   0                  100                3845  
2           default    0                  100                3845  
259         PoolHkDb1  0                  100                3845  
  
used_memory_mb       target_memory_mb  
-------------------- --------------------  
125                  3845  
32                   3845  
1356                 3845  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
