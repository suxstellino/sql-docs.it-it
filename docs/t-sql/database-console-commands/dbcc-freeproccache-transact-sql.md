---
description: DBCC FREEPROCCACHE (Transact-SQL)
title: DBCC FREEPROCCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREEPROCCACHE_TSQL
- FREEPROCCACHE
- DBCC_FREEPROCCACHE_TSQL
- DBCC FREEPROCCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- freeing procedure cache
- removing procedure cache elements
- deleting procedure cache elements
- DBCC FREEPROCCACHE statement
- procedure cache [SQL Server]
- clearing procedure cache
ms.assetid: 0e09d210-6f23-4129-aedb-3d56b2980683
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a24703ab8d93be9dc3f8b7f5fabb4e8e99e0e74a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750521"
---
# <a name="dbcc-freeproccache-transact-sql"></a>DBCC FREEPROCCACHE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Rimuove tutti gli elementi dalla cache dei piani, rimuove un piano specifico dalla cache dei piani specificando un handle di piani o un handle SQL oppure rimuove tutte le voci della cache associate a un pool di risorse specificato.

> [!NOTE]
> DBCC FREEPROCCACHE non comporta la cancellazione delle statistiche di esecuzione delle stored procedure compilate in modo nativo. La cache delle procedure non contiene informazioni sulle stored procedure compilate in modo nativo. Tutte le statistiche di esecuzione raccolte dalle esecuzioni delle procedure verranno visualizzate nelle DMV delle statistiche di esecuzione: [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) e [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
Sintassi per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

```sql
DBCC FREEPROCCACHE [ ( { plan_handle | sql_handle | pool_name } ) ] [ WITH NO_INFOMSGS ]  
```  

Sintassi per [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:
  
```sql
DBCC FREEPROCCACHE [ ( COMPUTE | ALL ) ] 
     [ WITH NO_INFOMSGS ]   
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 ( { *plan_handle* | *sql_handle* | *pool_name* } )  
*plan_handle* identifica in modo univoco un piano di query per un batch eseguito il cui piano risiede nella cache dei piani. *plan_handle* è un argomento di tipo **varbinary(64)** che può essere ottenuto dagli oggetti a gestione dinamica seguenti:  
 -   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  

*sql_handle* è l'handle SQL del batch da cancellare. *sql_handle* è un argomento di tipo **varbinary(64)** che può essere ottenuto dagli oggetti a gestione dinamica seguenti:  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
 -   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  

*pool_name* è il nome di un pool di risorse di Resource Governor. *pool_name* è un argomento di tipo **sysname** che può essere ottenuto eseguendo una query sulla vista a gestione dinamica [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
 Per associare un gruppo del carico di lavoro di Resource Governor al pool di risorse, eseguire una query sulla vista a gestione dinamica [sys.dm_resource_governor_workload_groups](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md). Per informazioni sul gruppo del carico di lavoro per una sessione, eseguire una query sulla vista a gestione dinamica [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).  

  
 WITH NO_INFOMSGS  
 Disattiva tutti i messaggi informativi.  
  
 COMPUTE  
 Ripulire la cache dei piani di query da ogni nodo di calcolo. Si tratta del valore predefinito.  
  
 ALL  
 Ripulire la cache dei piani di query da ogni nodo di calcolo e dal nodo di controllo.  

> [!NOTE]
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], viene usata l'istruzione `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` per cancellare la cache (dei piani) delle procedure per il database nell'ambito.

## <a name="remarks"></a>Commenti  
Usare DBCC FREEPROCCACHE per cancellare con cautela la cache dei piani. Se si cancella la cache (dei piani) delle procedure, tutti i piani vengono rimossi e le esecuzioni delle query in entrata vengono compilate in un nuovo piano, anziché riutilizzare piani precedentemente memorizzati nella cache. 

Ciò può causare una riduzione improvvisa e temporanea delle prestazioni delle query poiché aumenta il numero di nuove compilazioni. Per ogni archivio cache cancellato nella cache dei piani, il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conterrà il messaggio informativo seguente:

> `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.` 

Questo messaggio viene registrato ogni cinque minuti per tutta la durata dello scaricamento della cache.

La cache delle procedure viene cancellata anche con le seguenti operazioni di riconfigurazione:
-   access check cache bucket count  
-   access check cache quota  
-   clr enabled  
-   cost threshold for parallelism  
-   cross db ownership chaining  
-   index create memory  
-   max degree of parallelism  
-   max server memory  
-   max text repl size  
-   max worker threads  
-   min memory per query  
-   min server memory  
-   query governor cost limit  
-   query wait  
-   remote query timeout  
-   user options  
  
## <a name="result-sets"></a>Set di risultati  
Quando non viene specificata la clausola WITH NO_INFOMSGS, DBCC FREEPROCCACHE restituisce: "Esecuzione DBCC completata. Se sono stati visualizzati messaggi di errore DBCC, rivolgersi all'amministratore di sistema".
  
## <a name="permissions"></a>Autorizzazioni  
Applicabile a: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 
- È necessario disporre dell'autorizzazione ALTER SERVER STATE per il server.  

Applicabile a: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
- Richiede l'appartenenza al ruolo predefinito del server DB_OWNER.  

## <a name="general-remarks-for-sssdw-and-sspdw"></a>Osservazioni generali per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
È possibile eseguire contemporaneamente più comandi DBCC FREEPROCCACHE.
In [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] la cancellazione della cache dei piani può causare un peggioramento temporaneo delle prestazioni delle query poiché le query in entrata compilano un nuovo piano, invece di riutilizzarne uno precedentemente memorizzato nella cache. 

DBCC FREEPROCCACHE (COMPUTE) fa sì che solo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ricompili le query quando vengono eseguite sui nodi di calcolo. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] non ricompilano il piano delle query parallelo generato nel nodo di controllo.
DBCC FREEPROCCACHE può essere annullato durante l'esecuzione.
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Limitazioni e restrizioni per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC FREEPROCCACHE non può essere eseguito all'interno di una transazione.
DBCC FREEPROCCACHE non è supportato in un'istruzione EXPLAIN.
  
## <a name="metadata-for-sssdw-and-sspdw"></a>Metadati per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Quando viene eseguito DBCC FREEPROCCACHE, viene aggiunta una nuova riga alla vista di sistema sys.pdw_exec_requests.

## <a name="examples-ssnoversion"></a>Esempi: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
### <a name="a-clearing-a-query-plan-from-the-plan-cache"></a>R. Cancellazione di un piano di query dalla cache dei piani  
Nell'esempio seguente viene indicato come cancellare un piano di query dalla cache dei piani specificando l'handle del piano di query. Per verificare che la query di esempio si trovi nella cache dei piani, la query viene prima eseguita. Viene eseguita una query sulle viste a gestione dinamica `sys.dm_exec_cached_plans` e `sys.dm_exec_sql_text` per restituire l'handle di piano per la query. 

Il valore dell'handle di piani dal set di risultati viene quindi inserito nell'istruzione `DBCC FREEPROCACHE` per rimuovere solo quel piano dalla cache dei piani.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM Person.Address;  
GO  
SELECT plan_handle, st.text  
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st  
WHERE text LIKE N'SELECT * FROM Person.Address%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
plan_handle                                         text  
--------------------------------------------------  -----------------------------  
0x060006001ECA270EC0215D05000000000000000000000000  SELECT * FROM Person.Address;  
  
(1 row(s) affected)
 ```
 
```sql  
-- Remove the specific plan from the cache.  
DBCC FREEPROCCACHE (0x060006001ECA270EC0215D05000000000000000000000000);  
GO  
```  
  
### <a name="b-clearing-all-plans-from-the-plan-cache"></a>B. Cancellazione di tutti i piani dalla cache dei piani  
Nell'esempio seguente vengono cancellati tutti gli elementi dalla cache dei piani. La clausola WITH `NO_INFOMSGS` viene specificata per impedire la visualizzazione del messaggio di informazioni.
  
```sql  
DBCC FREEPROCCACHE WITH NO_INFOMSGS;  
```  
  
### <a name="c-clearing-all-cache-entries-associated-with-a-resource-pool"></a>C. Cancellazione di tutte le voci di cache associate a un pool di risorse  
Nell'esempio seguente vengono cancellate tutte le voci di cache associate a un pool di risorse specificato. Viene eseguita prima una query sulla vista `sys.dm_resource_governor_resource_pools` per ottenere il valore per *pool_name*.
  
```sql  
SELECT * FROM sys.dm_resource_governor_resource_pools;  
GO  
DBCC FREEPROCCACHE ('default');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-dbcc-freeproccache-basic-syntax-examples"></a>D. Esempi di sintassi di base DBCC FREEPROCCACHE  
Nell'esempio seguente vengono rimosse tutte le cache dei piani di query dai nodi di calcolo. Nonostante il contesto sia impostato su UserDbSales, tutte le cache dei piani di query dai nodi di calcolo vengono rimosse. La clausola WITH NO_INFOMSGS impedisce che i messaggi informativi vengano visualizzati nei risultati.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE) WITH NO_INFOMSGS;
```  
  
 Nell'esempio seguente vengono restituiti gli stessi risultati dell'esempio precedente, ad eccezione del fatto che i messaggi informativi vengono visualizzati nei risultati.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE);  
```  
  
Quando i messaggi informativi vengono richiesti e l'esecuzione ha esito positivo, i risultati della query contengono una riga per ogni nodo di calcolo.
  
### <a name="e-granting-permission-to-run-dbcc-freeproccache"></a>E. Concessione dell'autorizzazione per l'esecuzione di DBCC FREEPROCCACHE  
Nell'esempio seguente all'account di accesso David viene concessa l'autorizzazione per l'esecuzione di DBCC FREEPROCCACHE.  
  
```sql
GRANT ALTER SERVER STATE TO David; 
GO
```  
  
## <a name="see-also"></a>Vedere anche  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[Resource Governor](../../relational-databases/resource-governor/resource-governor.md)  
[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
  
  
