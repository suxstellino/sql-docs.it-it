---
description: sys.dm_io_virtual_file_stats (Transact-SQL)
title: sys.dm_io_virtual_file_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_io_virtual_file_stats
- sys.dm_io_virtual_file_stats_TSQL
- sys.dm_io_virtual_file_stats
- dm_io_virtual_file_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_virtual_file_stats dynamic management function
ms.assetid: fa3e321f-6fe5-45ff-b397-02a0dd3d6b7d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 11acce4555f26a6b2455d12a074881fa20f99a50
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750261"
---
# <a name="sysdm_io_virtual_file_stats-transact-sql"></a>sys.dm_io_virtual_file_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Restituisce le statistiche di I/O per i file di log e di dati. Questa funzione a gestione dinamica sostituisce la funzione [fn_virtualfilestats](../../relational-databases/system-functions/sys-fn-virtualfilestats-transact-sql.md) .  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da, usare il nome **sys.dm_pdw_nodes_io_virtual_file_stats**. 

## <a name="syntax"></a>Sintassi  
  
```  
-- Syntax for SQL Server and Azure SQL Database

sys.dm_io_virtual_file_stats (   
    { database_id | NULL },  
    { file_id | NULL }  
)  
```  

```  
-- Syntax for Azure Synapse Analytics

sys.dm_pdw_nodes_io_virtual_file_stats
```
  
## <a name="arguments"></a>Argomenti  


 *database_id* | NULL

 **SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure

 ID del database. *database_id* è di tipo int e non prevede alcun valore predefinito. Gli input validi sono il numero di ID di un database o NULL. Se si specifica NULL, vengono restituiti tutti i database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 È possibile specificare la funzione predefinita [DB_ID](../../t-sql/functions/db-id-transact-sql.md).  
  
*file_id* | NULL

**SI APPLICA A:** SQL Server (a partire dalla versione 2008), database SQL di Azure
 
ID del file. *file_id* è di tipo int e non prevede alcun valore predefinito. Gli input validi sono il numero di ID di un file o NULL. Se si specifica NULL, vengono restituiti tutti i file nel database.  
  
 È possibile specificare la funzione predefinita [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) e fa riferimento a un file nel database corrente.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Non **si applica a:**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> nome del database.</br></br>Per Azure sinapsi Analytics, si tratta del nome del database archiviato nel nodo identificato da pdw_node_id. Ogni nodo dispone di un database tempdb con 13 file. Ogni nodo dispone anche di un database per ogni distribuzione e ogni database di distribuzione contiene 5 file. Se, ad esempio, ogni nodo contiene 4 distribuzioni, i risultati mostrano 20 file di database di distribuzione per ogni pdw_node_id. 
|**database_id**|**smallint**|ID del database.|  
|**file_id**|**smallint**|ID di file.|  
|**sample_ms**|**bigint**|Numero di millisecondi dall'avvio del computer. È possibile utilizzare questa colonna per confrontare output diversi di questa funzione.</br></br>Il tipo di dati è **int** per [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] through [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**num_of_reads**|**bigint**|Numero di letture eseguite nel file.|  
|**num_of_bytes_read**|**bigint**|Numero totale di byte letti nel file.|  
|**io_stall_read_ms**|**bigint**|Tempo totale di attesa degli utenti, in millisecondi, per il completamento delle operazioni di lettura nel file.|  
|**num_of_writes**|**bigint**|Numero di scritture eseguite nel file.|  
|**num_of_bytes_written**|**bigint**|Numero totale di byte scritti nel file.|  
|**io_stall_write_ms**|**bigint**|Tempo totale di attesa degli utenti, in millisecondi, per il completamento delle operazioni di scrittura nel file.|  
|**io_stall**|**bigint**|Tempo totale di attesa degli utenti, in millisecondi, per il completamento delle operazioni di I/O nel file.|  
|**size_on_disk_bytes**|**bigint**|Numero di byte utilizzati nel disco per il file. Per i file sparse, questo numero corrisponde al numero effettivo di byte nel disco utilizzati per gli snapshot di database.|  
|**file_handle**|**varbinary**|Handle di file Windows per il file.|  
|**io_stall_queued_read_ms**|**bigint**|Non **si applica a:**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] fino a [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br /> Latenza di I/O totale introdotta dalla governance delle risorse di I/O per le letture. Non ammette i valori Null. Per ulteriori informazioni, vedere [sys.dm_resource_governor_resource_pools &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).|  
|**io_stall_queued_write_ms**|**bigint**|Non **si applica a:**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] fino a [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br />  Latenza di I/O totale introdotta dalla governance delle risorse di I/O per le scritture. Non ammette i valori Null.|
|**pdw_node_id**|**int**|**Si applica a:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]</br></br>Identificatore del nodo per la distribuzione.
 
## <a name="remarks"></a>Commenti
I contatori vengono inizializzati su Empty ogni volta che viene avviato il servizio SQL Server (MSSQLSERVER).
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE. Per altre informazioni, vedere [viste a gestione dinamica e funzioni &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Esempi  

### <a name="a-return-statistics-for-a-log-file"></a>R. Restituisce le statistiche per un file di log

**Si applica a:** SQL Server (a partire da 2008), database SQL di Azure

 Nell'esempio seguente vengono restituite le statistiche per il file di log nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT * FROM sys.dm_io_virtual_file_stats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="b-return-statistics-for-file-in-tempdb"></a>B. Restituisce le statistiche per il file in tempdb

**Si applica a:** Analisi delle sinapsi di Azure

```sql
SELECT * FROM sys.dm_pdw_nodes_io_virtual_file_stats 
WHERE database_name = 'tempdb' AND file_id = 2;

```

## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica correlate a I O &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  

