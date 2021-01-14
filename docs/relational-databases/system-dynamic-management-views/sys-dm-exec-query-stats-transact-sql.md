---
description: sys.dm_exec_query_stats (Transact-SQL)
title: sys.dm_exec_query_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_stats_TSQL
- dm_exec_query_stats
- sys.dm_exec_query_stats
- sys.dm_exec_query_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_stats dynamic management view
ms.assetid: eb7b58b8-3508-4114-97c2-d877bcb12964
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9456d3adf0e8e2ea0c3b31d9bb37dae24142dfab
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170693"
---
# <a name="sysdm_exec_query_stats-transact-sql"></a>sys.dm_exec_query_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce dati statistici aggregati sulle prestazioni dei piani di query memorizzati nella cache in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La vista contiene una riga per ogni istruzione di query nel piano memorizzato nella cache e la durata delle righe è legata al piano stesso. Quando un piano viene rimosso dalla cache, le righe corrispondenti vengono eliminate da questa vista.  
  
> [!NOTE]
> - I risultati di **sys.dm_exec_query_stats**  possono variare a seconda dell'esecuzione, perché i dati riflettono solo le query finite e non quelli ancora in corso.
> - Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_exec_query_stats**.    

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**sql_handle**|**varbinary(64)**  |Token che identifica in modo univoco il batch o stored procedure di cui fa parte la query.<br /><br /> È possibile utilizzare **sql_handle**, insieme a **statement_start_offset** e **statement_end_offset**, per recuperare il testo SQL della query chiamando la funzione a gestione dinamica **sys.dm_exec_sql_text**.|  
|**statement_start_offset**|**int**|Indica, in byte e a partire da 0, la posizione iniziale della query descritta dalla riga all'interno del testo del batch o dell'oggetto persistente.|  
|**statement_end_offset**|**int**|Indica, in byte e a partire da 0, la posizione finale della query descritta dalla riga all'interno del testo del batch o dell'oggetto persistente. Per le versioni precedenti [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a, il valore-1 indica la fine del batch. I commenti finali non sono più inclusi.|  
|**plan_generation_num**|**bigint**|Numero di sequenza utilizzabile per distinguere le istanze dei piani dopo una ricompilazione.|  
|**plan_handle**|**varbinary(64)**|È un token che identifica in modo univoco un piano di esecuzione di query per un batch eseguito e il relativo piano risiede nella cache dei piani oppure è attualmente in esecuzione. È possibile passare questo valore alla funzione a gestione dinamica [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) per ottenere il piano di query.<br /><br /> È sempre 0x000 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**creation_time**|**datetime**|Ora di compilazione del piano.|  
|**last_execution_time**|**datetime**|Ora dell'ultimo avvio dell'esecuzione del piano.|  
|**execution_count**|**bigint**|Numero di esecuzioni del piano a partire dall'ultima compilazione.|  
|**total_worker_time**|**bigint**|Quantità totale di tempo di CPU, espresso in microsecondi (con precisione al millisecondo), impiegato per le esecuzioni del piano a partire dalla relativa compilazione.<br /><br /> Per le stored procedure compilate in modo nativo, il valore di **total_worker_time** non può essere accurato se più esecuzioni richiedono meno di 1 millisecondo.|  
|**last_worker_time**|**bigint**|Tempo di CPU, espresso in microsecondi (con precisione al millisecondo), impiegato per l'ultima esecuzione del piano. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Tempo minimo di CPU, espresso in microsecondi (con precisione al millisecondo), mai impiegato dal piano durante una singola esecuzione. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Tempo massimo di CPU, espresso in microsecondi (con precisione al millisecondo), mai impiegato dal piano durante una singola esecuzione. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Numero totale di letture fisiche effettuate dalle esecuzioni del piano a partire dalla relativa compilazione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_physical_reads**|**bigint**|Numero di letture fisiche eseguite durante l'ultima esecuzione del piano.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_physical_reads**|**bigint**|Numero minimo di letture fisiche effettuate dal piano durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_physical_reads**|**bigint**|Numero massimo di letture fisiche effettuate dal piano durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_logical_writes**|**bigint**|Numero totale di scritture logiche effettuate dalle esecuzioni del piano a partire dalla relativa compilazione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_logical_writes**|**bigint**|Numero di pagine del pool di buffer sporcate durante l'ultima esecuzione del piano.<br /><br />Dopo la lettura di una pagina, la pagina diventa Dirty solo la prima volta che viene modificata. Quando una pagina diventa Dirty, questo numero viene incrementato. Le modifiche successive di una pagina già Dirty non influiscono su questo numero.<br /><br />Questo numero sarà sempre 0 quando si esegue una query su una tabella ottimizzata per la memoria.|  
|**min_logical_writes**|**bigint**|Numero minimo di scritture logiche effettuate dal piano durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_logical_writes**|**bigint**|Numero massimo di scritture logiche effettuate dal piano durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_logical_reads**|**bigint**|Numero totale di letture logiche effettuate dalle esecuzioni del piano a partire dalla sua compilazione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_logical_reads**|**bigint**|Numero di letture logiche effettuate durante l'ultima esecuzione del piano.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_logical_reads**|**bigint**|Numero minimo di letture logiche effettuate dal piano durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_logical_reads**|**bigint**|Numero massimo di letture logiche effettuate dal piano durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_clr_time**|**bigint**|Tempo, in microsecondi (con precisione al millisecondo), utilizzato all'interno di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] oggetti Common Language Runtime (CLR) dalle esecuzioni del piano a partire dalla relativa compilazione. Gli oggetti CLR possono essere stored procedure, funzioni, trigger, tipi e aggregazioni.|  
|**last_clr_time**|**bigint**|Tempo, espresso in microsecondi (con precisione al millisecondo), impiegato dall'ultima esecuzione del piano all'interno di oggetti CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Gli oggetti CLR possono essere stored procedure, funzioni, trigger, tipi e aggregazioni.|  
|**min_clr_time**|**bigint**|Tempo minimo di CPU, espresso in microsecondi (con precisione al millisecondo), mai impiegato dal piano durante una singola esecuzione all'interno di oggetti CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Gli oggetti CLR possono essere stored procedure, funzioni, trigger, tipi e aggregazioni.|  
|**max_clr_time**|**bigint**|Tempo massimo di CPU, espresso in microsecondi (con precisione al millisecondo), mai impiegato dal piano durante una singola esecuzione all'interno di oggetti CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Gli oggetti CLR possono essere stored procedure, funzioni, trigger, tipi e aggregazioni.|  
|**total_elapsed_time**|**bigint**|Tempo totale trascorso, espresso in microsecondi (con precisione al millisecondo), per le esecuzioni completate di questo piano.|  
|**last_elapsed_time**|**bigint**|Tempo trascorso, espresso in microsecondi (con precisione al millisecondo), per le ultime esecuzioni completate di questo piano.|  
|**min_elapsed_time**|**bigint**|Tempo minimo trascorso, espresso in microsecondi (con precisione al millisecondo), per un'esecuzione completata di questo piano.|  
|**max_elapsed_time**|**bigint**|Tempo massimo trascorso, espresso in microsecondi (con precisione al millisecondo), per un'esecuzione completata di questo piano.|  
|**query_hash**|**Binario (8)**|Valore hash binario calcolato sulla query che consente di identificare query con logica analoga. È possibile utilizzare il valore hash della query per determinare l'utilizzo delle risorse aggregate per query che differiscono solo per valori letterali.|  
|**query_plan_hash**|**binario (8)**|Valore hash binario calcolato sul piano di esecuzione di query che consente di identificare piani di esecuzioni analoghi. È possibile utilizzare il valore hash del piano di query per individuare il costo cumulativo di query con piani di esecuzione analoghi.<br /><br /> È sempre 0x000 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**total_rows**|**bigint**|Numero totale di righe restituite dalla query. Non può essere null.<br /><br /> È sempre 0 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**last_rows**|**bigint**|Numero di righe restituite durante l'ultima esecuzione della query. Non può essere null.<br /><br /> È sempre 0 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**min_rows**|**bigint**|Numero minimo di righe restituite dalla query durante un'esecuzione. Non può essere null.<br /><br /> È sempre 0 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**max_rows**|**bigint**|Numero massimo di righe restituite dalla query durante un'esecuzione. Non può essere null.<br /><br /> È sempre 0 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**statement_sql_handle**|**varbinary(64)**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Popolato con valori non NULL solo se Query Store è attivato e raccoglie le statistiche per la query specifica.|  
|**statement_context_id**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Popolato con valori non NULL solo se Query Store è attivato e raccoglie le statistiche per la query specifica.|  
|**total_dop**|**bigint**|Somma totale del grado di parallelismo utilizzato dal piano dopo la compilazione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**last_dop**|**bigint**|Grado di parallelismo quando il piano è stato eseguito l'ultima volta. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**min_dop**|**bigint**|Il grado di parallelismo minimo utilizzato da questo piano durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**max_dop**|**bigint**|Massimo grado di parallelismo utilizzato da questo piano durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**total_grant_kb**|**bigint**|Quantità totale di concessione di memoria riservata, in KB, ricevuta dal piano dopo la relativa compilazione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**last_grant_kb**|**bigint**|Quantità di memoria riservata concessa in KB quando il piano è stato eseguito l'ultima volta. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**min_grant_kb**|**bigint**|Quantità minima di concessione di memoria riservata, in KB, ricevuta dal piano durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**max_grant_kb**|**bigint**|Quantità massima di concessioni di memoria riservate in KB che questo piano ha ricevuto durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**total_used_grant_kb**|**bigint**|Quantità totale di concessioni di memoria riservate in KB utilizzate dal piano dopo la relativa compilazione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**last_used_grant_kb**|**bigint**|Quantità di memoria usata in KB quando il piano è stato eseguito l'ultima volta. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**min_used_grant_kb**|**bigint**|Quantità minima di concessione di memoria usata in KB che questo piano ha mai usato durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**max_used_grant_kb**|**bigint**|Quantità massima di concessioni di memoria utilizzata in KB che questo piano ha mai utilizzato durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**total_ideal_grant_kb**|**bigint**|Quantità totale di concessioni di memoria ideali in KB stimate dal piano dopo la relativa compilazione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**last_ideal_grant_kb**|**bigint**|Quantità di concessione di memoria ideale in KB quando il piano è stato eseguito l'ultima volta. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**min_ideal_grant_kb**|**bigint**|Quantità minima di concessione di memoria ideale in KB che questo piano ha mai stimato durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**max_ideal_grant_kb**|**bigint**|Quantità massima di concessione di memoria ideale in KB che questo piano ha mai stimato durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**total_reserved_threads**|**bigint**|Somma totale dei thread paralleli riservati che questo piano ha mai usato dopo la compilazione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**last_reserved_threads**|**bigint**|Numero di thread paralleli riservati quando il piano è stato eseguito l'ultima volta. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**min_reserved_threads**|**bigint**|Il numero minimo di thread paralleli riservati che questo piano ha mai usato durante un'esecuzione.  Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**max_reserved_threads**|**bigint**|Numero massimo di thread paralleli riservati che questo piano ha mai usato durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**total_used_threads**|**bigint**|Somma totale dei thread paralleli utilizzati che questo piano ha mai usato dopo la compilazione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**last_used_threads**|**bigint**|Numero di thread paralleli utilizzati quando il piano è stato eseguito l'ultima volta. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**min_used_threads**|**bigint**|Il numero minimo di thread paralleli utilizzati che questo piano ha mai usato durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**max_used_threads**|**bigint**|Numero massimo di thread paralleli utilizzati utilizzati dal piano durante un'esecuzione. Sarà sempre 0 per l'esecuzione di una query su una tabella ottimizzata per la memoria.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  
|**total_columnstore_segment_reads**|**bigint**|Somma totale dei segmenti columnstore letti dalla query. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_reads**|**bigint**|Numero di segmenti columnstore letti dall'ultima esecuzione della query. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_reads**|**bigint**|Numero minimo di segmenti columnstore letti dalla query durante un'esecuzione. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_reads**|**bigint**|Numero massimo di segmenti columnstore letti dalla query durante un'esecuzione. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**total_columnstore_segment_skips**|**bigint**|Somma totale dei segmenti columnstore ignorati dalla query. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_skips**|**bigint**|Numero di segmenti columnstore ignorati dall'ultima esecuzione della query. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_skips**|**bigint**|Numero minimo di segmenti columnstore ignorati dalla query durante un'esecuzione. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_skips**|**bigint**|Numero massimo di segmenti columnstore ignorati dalla query durante un'esecuzione. Non può essere null.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|
|**total_spills**|**bigint**|Numero totale di pagine distribuite dall'esecuzione di questa query a partire dalla relativa compilazione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Numero di pagine distribuite durante l'ultima esecuzione della query.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Numero minimo di pagine che la query ha mai distribuito durante una singola esecuzione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Numero massimo di pagine che la query ha mai distribuito durante una singola esecuzione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Identificatore del nodo su cui si trova questa distribuzione.<br /><br /> **Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]| 
|**total_page_server_reads**|**bigint**|Numero totale di letture del server di pagine remote eseguite dalle esecuzioni del piano a partire dalla relativa compilazione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure |  
|**last_page_server_reads**|**bigint**|Numero di letture del server della pagina remota effettuate durante l'ultima esecuzione del piano.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure |  
|**min_page_server_reads**|**bigint**|Numero minimo di letture del server della pagina remota effettuate dal piano durante una singola esecuzione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure |  
|**max_page_server_reads**|**bigint**|Numero massimo di letture del server di pagine remote effettuate dal piano durante una singola esecuzione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure |  
> [!NOTE]
> <sup>1</sup> per le stored procedure compilate in modo nativo quando la raccolta delle statistiche è abilitata, il tempo di lavoro viene raccolto in millisecondi. Se la query viene eseguita in meno di un millisecondo, il valore sarà 0.  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
   
## <a name="remarks"></a>Commenti  
 Le statistiche nella vista vengono aggiornate quando viene completata una query.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-finding-the-top-n-queries"></a>R. Ricerca delle prime n query  
 Nell'esempio seguente vengono restituite informazioni sulle prime cinque query classificate in base al tempo medio della CPU. Nell'esempio le query vengono aggregate in base al relativo valore hash del piano, in modo da raggruppare le query logicamente equivalenti in base all'utilizzo di risorse cumulativo.  
  
```sql  
SELECT TOP 5 query_stats.query_hash AS "Query Hash",   
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",  
    MIN(query_stats.statement_text) AS "Statement Text"  
FROM   
    (SELECT QS.*,   
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,  
    ((CASE statement_end_offset   
        WHEN -1 THEN DATALENGTH(ST.text)  
        ELSE QS.statement_end_offset END   
            - QS.statement_start_offset)/2) + 1) AS statement_text  
     FROM sys.dm_exec_query_stats AS QS  
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats  
GROUP BY query_stats.query_hash  
ORDER BY 2 DESC;  
```  
  
### <a name="b-returning-row-count-aggregates-for-a-query"></a>B. Restituzione di aggregazioni relative al conteggio delle righe per una query  
 Nell'esempio seguente vengono restituite le informazioni di aggregazione relative al conteggio delle righe (totale righe, numero minimo righe, numero massimo righe e ultime righe) per le query.  
  
```sql  
SELECT qs.execution_count,  
    SUBSTRING(qt.text,qs.statement_start_offset/2 +1,   
                 (CASE WHEN qs.statement_end_offset = -1   
                       THEN LEN(CONVERT(nvarchar(max), qt.text)) * 2   
                       ELSE qs.statement_end_offset end -  
                            qs.statement_start_offset  
                 )/2  
             ) AS query_text,   
     qt.dbid, dbname= DB_NAME (qt.dbid), qt.objectid,   
     qs.total_rows, qs.last_rows, qs.min_rows, qs.max_rows  
FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS qt   
WHERE qt.text like '%SELECT%'   
ORDER BY qs.execution_count DESC;  
```  
  
## <a name="see-also"></a>Vedi anche  
[Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
[sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)    
[sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_procedure_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)     
[sys.dm_exec_trigger_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)     
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  


