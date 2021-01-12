---
description: sys.query_store_plan (Transact-SQL)
title: sys.query_store_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b13c6f42eb967c6ef5d01e189860c9de3204646
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095468"
---
# <a name="sysquery_store_plan-transact-sql"></a>sys.query_store_plan (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene informazioni su ogni piano di esecuzione associato a una query.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|
|**plan_id**|**bigint**|Chiave primaria.|  
|**query_id**|**bigint**|Chiave esterna. Si unisce a [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md).|  
|**plan_group_id**|**bigint**|ID del gruppo di piani. Per le query di cursori sono in genere necessari più piani (popolamento e recupero). Popolare e recuperare i piani compilati insieme si trovano nello stesso gruppo.<br /><br /> 0 indica che il piano non è in un gruppo.|  
|**engine_version**|**nvarchar(32)**|Versione del motore utilizzata per compilare il piano nel formato **' Major. minor. Build. Revision '** .|  
|**compatibility_level**|**smallint**|Livello di compatibilità del database a cui si fa riferimento nella query.|  
|**query_plan_hash**|**binario (8)**|Hash MD5 del piano singolo.|  
|**query_plan**|**nvarchar(max)**|Showplan XML per il piano di query.|  
|**is_online_index_plan**|**bit**|Il piano è stato usato durante la compilazione di un indice online. <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**is_trivial_plan**|**bit**|Il piano è un piano semplice (output nella fase 0 di Query Optimizer). <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**is_parallel_plan**|**bit**|Il piano è parallelo. <br/>**Nota:** Azure sinapsi Analytics restituirà sempre uno (1).|  
|**is_forced_plan**|**bit**|Il piano è contrassegnato come forzato quando l'utente esegue stored procedure **sys.sp_query_store_force_plan**. Il meccanismo di forzatura *non garantisce* che venga utilizzato esattamente questo piano per la query a cui fa riferimento **query_id**. L'utilizzo forzato del piano determina la compilazione di una query e in genere produce esattamente lo stesso piano o simile al piano a cui fa riferimento **plan_id**. Se l'utilizzo forzato del piano ha esito negativo, **force_failure_count** viene incrementato e **last_force_failure_reason** viene popolato con il motivo dell'errore. <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**is_natively_compiled**|**bit**|Il piano include procedure con ottimizzazione per la memoria compilate in modo nativo. (0 = FALSE, 1 = TRUE). <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**force_failure_count**|**bigint**|Numero di volte in cui il piano forzato non è riuscito. Può essere incrementato solo quando la query viene ricompilata (*non a ogni esecuzione*). Viene reimpostato su 0 ogni volta che **is_plan_forced** viene modificato da **false** a **true**. <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**last_force_failure_reason**|**int**|Motivo per cui la forzatura del piano non è riuscita.<br /><br /> 0: nessun errore. in caso contrario, numero di errore dell'errore che ha causato l'esito negativo della forzatura<br /><br /> 8637: ONLINE_INDEX_BUILD<br /><br /> 8683: INVALID_STARJOIN<br /><br /> 8684: TIME_OUT<br /><br /> 8689: NO_DB<br /><br /> 8690: HINT_CONFLICT<br /><br /> 8691: SETOPT_CONFLICT<br /><br /> 8694: DQ_NO_FORCING_SUPPORTED<br /><br /> 8698: NO_PLAN<br /><br /> 8712: NO_INDEX<br /><br /> 8713: VIEW_COMPILE_FAILED<br /><br /> \<other value>: GENERAL_FAILURE <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Descrizione testuale del last_force_failure_reason_desc.<br /><br /> ONLINE_INDEX_BUILD: la query tenta di modificare i dati mentre la tabella di destinazione include un indice compilato online<br /><br /> INVALID_STARJOIN: il piano contiene una specifica StarJoin non valida<br /><br /> TIME_OUT: l'ottimizzatore ha superato il numero di operazioni consentite durante la ricerca del piano specificato dal piano forzato<br /><br /> NO_DB: un database specificato nel piano non esiste<br /><br /> HINT_CONFLICT: non è possibile compilare la query perché il piano è in conflitto con un hint per la query<br /><br /> DQ_NO_FORCING_SUPPORTED: non è possibile eseguire la query perché il piano è in conflitto con l'utilizzo di query distribuite o di operazioni full-text.<br /><br /> NO_PLAN: query processor non è in grado di generare il piano di query perché non è stato possibile verificare che il piano forzato sia valido per la query<br /><br /> NO_INDEX: l'indice specificato nel piano non esiste più<br /><br /> VIEW_COMPILE_FAILED: non è stato possibile forzare il piano di query a causa di un problema in una vista indicizzata a cui si fa riferimento nel piano<br /><br /> GENERAL_FAILURE: errore forzato generale (non trattato con i motivi precedenti) <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre *None*.|  
|**count_compiles**|**bigint**|Pianificare le statistiche di compilazione.|  
|**initial_compile_start_time**|**datetimeoffset**|Pianificare le statistiche di compilazione.|  
|**last_compile_start_time**|**datetimeoffset**|Pianificare le statistiche di compilazione.|  
|**last_execution_time**|**datetimeoffset**|L'ora dell'ultima esecuzione fa riferimento all'ora dell'ultima fine della query o del piano.|  
|**avg_compile_duration**|**float**|Pianificare le statistiche di compilazione. <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**last_compile_duration**|**bigint**|Pianificare le statistiche di compilazione. <br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|  
|**plan_forcing_type**|**int**|Tipo di forzatura del piano.<br /><br />0: NESSUNA<br /><br />1: MANUALE<br /><br />2: AUTO|  
|**plan_forcing_type_desc**|**nvarchar(60)**|Descrizione del testo plan_forcing_type.<br /><br />NONE: nessuna forzatura del piano<br /><br />MANUALE: piano forzato dall'utente<br /><br />AUTOMATICO: piano forzato dall'ottimizzazione automatica|  

## <a name="plan-forcing-limitations"></a>Limitazioni per l'uso forzato dei piani
Query Store è dotato di un meccanismo per imporre a Query Optimizer l'uso di determinati piani di esecuzione. Esistono tuttavia alcune limitazioni che possono impedire l'imposizione di un piano. 

In primo luogo, se il piano contiene i costrutti seguenti:
* Istruzione Insert bulk.
* Riferimento a una tabella esterna
* Query distribuita o operazioni full-text
* Uso di query globali 
* Cursori dinamici o keyset 
* Specifica di join a stella non valida 

> [!NOTE]
> Il database SQL di Azure e il piano di supporto SQL Server 2019 forzano i cursori statici e veloci.

In secondo luogo, quando gli oggetti su cui si basa il piano non sono più disponibili:
* Database (se il database da cui ha avuto origine il piano non esiste più)
* Indice (non più esistente o disabilitato)

Infine, problemi del piano stesso:
* Piano non valido per query
* Numero di operazioni consentite per Query Optimizer superato
* XML del piano formato in modo non corretto

## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **View database state** .  
  
## <a name="see-also"></a>Vedere anche  
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Stored procedure di Query Store &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
