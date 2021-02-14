---
description: sys.dm_exec_query_optimizer_info (Transact-SQL)
title: sys.dm_exec_query_optimizer_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_info_TSQL
- dm_exec_query_optimizer_info
- sys.dm_exec_query_optimizer_info_TSQL
- sys.dm_exec_query_optimizer_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_info dynamic management view
ms.assetid: 1d72cef1-22d8-4ae0-91db-6694fe918c9e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3bd179dd4660ce82e6629619c92e0e0e8e429bc1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342956"
---
# <a name="sysdm_exec_query_optimizer_info-transact-sql"></a>sys.dm_exec_query_optimizer_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce statistiche dettagliate sul funzionamento di Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile utilizzare questa vista durante l'ottimizzazione di un carico di lavoro per individuare problemi o miglioramenti per l'ottimizzazione delle query. Ad esempio, è possibile utilizzare il numero totale di ottimizzazioni, il valore del tempo trascorso e il valore di costo finale per confrontare le ottimizzazioni della query per il carico di lavoro corrente con eventuali variazioni rilevate durante il processo di ottimizzazione. Alcuni contatori forniscono dati rilevanti solo per utilizzo diagnostico interno in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questi contatori sono contrassegnati come "Solo per uso interno".  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_exec_query_optimizer_info**.  
  
|Nome|Tipo di dati|Descrizione|  
|----------|---------------|-----------------|  
|**counter**|**nvarchar(4000)**|Nome dell'evento statistiche di Query Optimizer.|  
|**occurrence**|**bigint**|Numero di occorrenze dell'evento di ottimizzazione per il contatore corrente.|  
|**value**|**float**|Valore medio della proprietà per occorrenza dell'evento.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
    
## <a name="remarks"></a>Commenti  
 **sys.dm_exec_query_optimizer_info** contiene le proprietà (contatori) seguenti. Tutti i valori di occorrenza sono cumulativi e vengono impostati su 0 al riavvio del sistema. Tutti i valori dei campi valori vengono impostati su NULL al riavvio del sistema. Tutti i valori delle colonne valori che specificano una media utilizzano il valore di occorrenza della stessa riga del denominatore nel calcolo della media. Tutte le ottimizzazioni delle query vengono misurate quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determina le modifiche apportate **dm_exec_query_optimizer_info**, incluse le query generate dall'utente e dal sistema. L'esecuzione di un piano già memorizzato nella cache non comporta la modifica dei valori in **dm_exec_query_optimizer_info**, ma solo le ottimizzazioni sono significative.  
  
|Contatore|Occorrenza|Valore|  
|-------------|----------------|-----------|  
|optimizations|Numero totale di ottimizzazioni.|Non applicabile|  
|elapsed time|Numero totale di ottimizzazioni.|Tempo medio trascorso per ottimizzazione in una singola istruzione (query), espresso in secondi.|  
|final cost|Numero totale di ottimizzazioni.|Costo medio stimato per piano ottimizzato espresso nelle unità di costo interne.|  
|trivial plan|Solo per uso interno|Solo per uso interno|  
|attività|Solo per uso interno|Solo per uso interno|  
|no plan|Solo per uso interno|Solo per uso interno|  
|search 0|Solo per uso interno|Solo per uso interno|  
|search 0 time|Solo per uso interno|Solo per uso interno|  
|search 0 tasks|Solo per uso interno|Solo per uso interno|  
|search 1|Solo per uso interno|Solo per uso interno|  
|search 1 time|Solo per uso interno|Solo per uso interno|  
|search 1 tasks|Solo per uso interno|Solo per uso interno|  
|search 2|Solo per uso interno|Solo per uso interno|  
|search 2 time|Solo per uso interno|Solo per uso interno|  
|search 2 tasks|Solo per uso interno|Solo per uso interno|  
|gain stage 0 to stage 1|Solo per uso interno|Solo per uso interno|  
|gain stage 1 to stage 2|Solo per uso interno|Solo per uso interno|  
|timeout|Solo per uso interno|Solo per uso interno|  
|memory limit exceeded|Solo per uso interno|Solo per uso interno|  
|insert stmt|Numero di ottimizzazioni per istruzioni INSERT.|Non applicabile|  
|delete stmt|Numero di ottimizzazioni per istruzioni DELETE.|Non applicabile|  
|update stmt|Numero di ottimizzazioni per istruzioni UPDATE.|Non applicabile|  
|contains subquery|Numero di ottimizzazioni per una query contenente almeno una sottoquery.|Non applicabile|  
|unnest failed|Solo per uso interno|Solo per uso interno|  
|tables|Numero totale di ottimizzazioni.|Numero medio di tabelle a cui viene fatto riferimento per query ottimizzata.|  
|hint|Numero di volte in cui un hint specifico è stato specificato. Gli hint conteggiati includono gli hint per la query JOIN, GROUP, UNION e FORCE ORDER, l'opzione SET FORCE PLAN e gli hint di join.|Non applicabile|  
|order hint|Numero di volte in cui un hint per la query FORCE ORDER è stato specificato.|Non applicabile|  
|join hint|Numero di volte in cui l'algoritmo JOIN è stato applicato a un hint di join.|Non applicabile|  
|view reference|Numero di volte in cui è stato fatto riferimento a una vista in una query.|Non applicabile|  
|remote query|Numero di ottimizzazioni in cui la query ha fatto riferimento ad almeno un'origine dei dati remota, ad esempio una tabella con un nome in quattro parti o un risultato OPENROWSET.|Non applicabile|  
|maximum DOP|Numero totale di ottimizzazioni.|Valore MAXDOP effettivo medio per un piano ottimizzato. Per impostazione predefinita, MAXDOP effettivo è determinato dall'opzione di configurazione del server **max degree of parallelism** ed è possibile eseguirne l'override per una query specifica in base al valore dell'hint per la query MAXDOP.|  
|maximum recursion level|Numero di ottimizzazioni in cui è stato specificato un livello MAXRECURSION maggiore di 0 con l'hint per la query.|Livello MAXRECURSION medio nelle ottimizzazioni in cui è stato specificato un livello di ricorsione massimo con l'hint per la query.|  
|indexed views loaded|Solo per uso interno|Solo per uso interno|  
|indexed views matched|Numero di ottimizzazioni in cui è stata trovata la corrispondenza per una o più viste indicizzate.|Numero medio di viste corrispondenti.|  
|indexed views used|Numero di ottimizzazioni in cui una o più viste indicizzate vengono utilizzate nel piano di output dopo aver individuato la corrispondenza.|Numero medio di viste utilizzate.|  
|indexed views updated|Numero di ottimizzazioni di un'istruzione DML che generano un piano che gestisce una o più viste indicizzate.|Numero medio di viste gestite.|  
|dynamic cursor request|Numero di ottimizzazioni in cui è stata specificata una richiesta di cursore dinamico.|Non applicabile|  
|fast forward cursor request|Numero di ottimizzazioni in cui è stata specificata una richiesta di cursore fast forward-only.|Non applicabile|  
|merge stmt|Numero di ottimizzazioni per le istruzioni MERGE.|Non applicabile|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-viewing-statistics-on-optimizer-execution"></a>R. Visualizzazione delle statistiche durante l'esecuzione di Query Optimizer  
 Nell'esempio seguente vengono visualizzate le statistiche di esecuzione di Query Optimizer per l'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT * FROM sys.dm_exec_query_optimizer_info;  
```  
  
### <a name="b-viewing-the-total-number-of-optimizations"></a>B. Visualizzazione del numero totale di ottimizzazioni  
 Nell'esempio seguente viene visualizzato il numero di ottimizzazioni eseguite.  
  
```  
SELECT occurrence AS Optimizations FROM sys.dm_exec_query_optimizer_info  
WHERE counter = 'optimizations';  
```  
  
### <a name="c-average-elapsed-time-per-optimization"></a>C. Tempo medio trascorso per ottimizzazione  
 Nell'esempio seguente viene visualizzato il tempo medio trascorso per ogni ottimizzazione.  
  
```  
SELECT ISNULL(value,0.0) AS ElapsedTimePerOptimization  
FROM sys.dm_exec_query_optimizer_info WHERE counter = 'elapsed time';  
```  
  
### <a name="d-fraction-of-optimizations-that-involve-subqueries"></a>D. Frazione di ottimizzazioni che interessano sottoquery  
 Nell'esempio seguente viene individuata la frazione di query ottimizzate contenente una sottoquery.  
  
```  
SELECT (SELECT CAST (occurrence AS float) FROM sys.dm_exec_query_optimizer_info WHERE counter = 'contains subquery') /  
       (SELECT CAST (occurrence AS float)   
        FROM sys.dm_exec_query_optimizer_info WHERE counter = 'optimizations')  
        AS ContainsSubqueryFraction;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


