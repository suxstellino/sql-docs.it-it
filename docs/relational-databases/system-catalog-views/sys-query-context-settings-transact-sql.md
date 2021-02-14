---
description: sys.query_context_settings (Transact-SQL)
title: sys.query_context_settings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS
- QUERY_CONTEXT_SETTINGS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_context_settings catalog view
ms.assetid: 3c1887df-6bd8-491e-82fc-d25ad9589faf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 63410c44833d6ce22ac3e1ddc2772f4edb7c5ac4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337019"
---
# <a name="sysquery_context_settings-transact-sql"></a>sys.query_context_settings (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene informazioni sulla semantica che influiscono sulle impostazioni di contesto associate a una query. In sono disponibili diverse impostazioni di contesto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che influiscono sulla semantica di query (definendo il risultato corretto della query). Lo stesso testo di query compilato con impostazioni diverse può produrre risultati diversi (a seconda dei dati sottostanti).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**context_settings_id**|**bigint**|Chiave primaria. Questo valore è esposto nel codice XML Showplan per le query.|  
|**set_options**|**varbinary (8)**|Maschera di bit che riflette lo stato di diverse opzioni SET. Per ulteriori informazioni, vedere [sys.dm_exec_plan_attributes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).|  
|**language_id**|**smallint**|ID della lingua. Per ulteriori informazioni, vedere [ linguaggisys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|**date_format**|**smallint**|Formato della data. Per altre informazioni, vedere [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|**date_first**|**tinyint**|Primo valore di data. Per altre informazioni, vedere [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|**Stato**|**varbinary (2)**|Campo della maschera di maschera che indica il tipo di query o il contesto in cui è stata eseguita la query. <br />Il valore della colonna può essere una combinazione di più flag (espressa in formato esadecimale):<br /><br /> 0x0-query regolare (nessun flag specifico)<br /><br /> 0x1-query eseguita tramite una delle stored procedure delle API del cursore<br /><br /> 0x2-query per la notifica<br /><br /> 0x4-query interna<br /><br /> 0x8-query con parametri automatico senza parametrizzazione universale<br /><br /> 0x10-query di aggiornamento recupero cursori<br /><br /> 0x20-query utilizzata nelle richieste di aggiornamento del cursore<br /><br /> 0x40-il set di risultati iniziale viene restituito all'apertura di un cursore (recupero automatico del cursore)<br /><br /> 0x80-query crittografata<br /><br /> 0x100-query nel contesto del predicato di sicurezza a livello di riga|  
|**required_cursor_options**|**int**|Opzioni di cursore specificate dall'utente, ad esempio il tipo di cursore.|  
|**acceptable_cursor_options**|**int**|Opzioni di cursore che potrebbero essere convertite in modo implicito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per supportare l'esecuzione dell'istruzione.|  
|**merge_action_type**|**smallint**|Tipo di piano di esecuzione del trigger utilizzato come risultato di un'istruzione **merge** .<br /><br /> 0 indica un piano non trigger, un piano di trigger che non viene eseguito come risultato di un'istruzione **merge** o un piano di trigger che viene eseguito come risultato di un'istruzione **merge** che specifica solo un'azione **Delete** .<br /><br /> 1 indica un piano di trigger di **inserimento** che viene eseguito come risultato di un'istruzione **merge** .<br /><br /> 2 indica un piano di trigger di **aggiornamento** che viene eseguito come risultato di un'istruzione **merge** .<br /><br /> 3 indica un piano di trigger **Delete** che viene eseguito come risultato di un'istruzione **merge** contenente un'azione di **inserimento** o **aggiornamento** corrispondente.<br /><br /> <br /><br /> Per i trigger annidati eseguiti da azioni di propagazione, questo valore è l'azione dell'istruzione **merge** che ha causato l'esecuzione di Cascade.|  
|**default_schema_id**|**int**|ID dello schema predefinito, utilizzato per risolvere i nomi non completi.|  
|**is_replication_specific**|**bit**|Utilizzato per la replica.|  
|**is_contained**|**varbinary (1)**|1 indica un database indipendente.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **View database state** .  
  
## <a name="see-also"></a>Vedere anche  
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Stored procedure di Archivio query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
