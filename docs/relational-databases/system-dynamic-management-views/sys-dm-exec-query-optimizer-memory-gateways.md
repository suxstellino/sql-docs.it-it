---
title: sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)
description: Restituisce lo stato corrente dei semafori di risorsa utilizzati per limitare l'ottimizzazione delle query simultanee
ms.custom: seo-dt-2019
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ff3adb52a968957faead5453e266ec733df38f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198531"
---
# <a name="sysdm_exec_query_optimizer_memory_gateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Restituisce lo stato corrente dei semafori di risorsa utilizzati per limitare l'ottimizzazione delle query simultanee.

|Colonna|Tipo|Descrizione|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|ID pool di risorse in Resource Governor|  
|**nome**|**sysname**|Nome del controllo di compilazione (Gateway Small, gateway medio, Big gateway)|
|**max_count**|**int**|Numero massimo configurato di compilazioni simultanee|
|**active_count**|**int**|Numero attualmente attivo di compilazioni in questo controllo|
|**waiter_count**|**int**|Il numero di attese in questo controllo|
|**threshold_factor**|**bigint**|Fattore di soglia che definisce la parte di memoria massima utilizzata dall'ottimizzazione delle query.  Per il gateway di piccole dimensioni, threshold_factor indica l'utilizzo massimo della memoria ottimizzatore in byte per una query prima che sia necessaria per ottenere un accesso nel gateway di piccole dimensioni.  Per il gateway medio e di grandi dimensioni, threshold_factor Mostra la parte della memoria totale del server disponibile per questo controllo. Viene usato come divisore durante il calcolo della soglia di utilizzo della memoria per il controllo.|
|**threshold**|**bigint**|Memoria soglia successiva in byte.  La query è necessaria per ottenere un accesso a questo gateway se il consumo di memoria raggiunge questa soglia.  "-1" se la query non è necessaria per ottenere l'accesso a questo gateway.|
|**is_active**|**bit**|Indica se la query è necessaria per passare il controllo corrente.|


## <a name="permissions"></a>Autorizzazioni  
SQL Server richiede l'autorizzazione VIEW SERVER STATE per il server.

Il database SQL di Azure richiede l'autorizzazione VIEW DATABASE STATE nel database.


## <a name="remarks"></a>Commenti  
SQL Server usa un approccio con gateway a livelli per limitare il numero di compilazioni simultanee consentite.  Vengono usati tre gateway, tra cui Small, medium e Big. I gateway consentono di evitare l'esaurimento delle risorse di memoria complessive da parte della memoria di compilazione più ampia, richiedendo i consumer.

Attende che un gateway provochi una compilazione posticipata. Oltre ai ritardi nella compilazione, alle richieste limitate viene associato un RESOURCE_SEMAPHORE_QUERY_COMPILE accumulo di tipo wait. Il tipo di attesa RESOURCE_SEMAPHORE_QUERY_COMPILE potrebbe indicare che le query utilizzano una grande quantità di memoria per la compilazione e che la memoria è stata esaurita o in alternativa è disponibile una quantità di memoria sufficiente nel complesso, ma le unità disponibili in un gateway specifico sono esaurite. L'output di **sys.dm_exec_query_optimizer_memory_gateways** può essere usato per risolvere i problemi relativi agli scenari in cui la memoria disponibile non è sufficiente per compilare un piano di esecuzione della query.  

## <a name="examples"></a>Esempi  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>R. Visualizzazione delle statistiche sui semafori delle risorse  
Quali sono le statistiche correnti del gateway di memoria dell'utilità di ottimizzazione per questa istanza di SQL Server?

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[Come usare il comando DBCC MEMORYSTATUS per monitorare l'utilizzo della memoria su SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005) 
 La [compilazione di query di grandi dimensioni attende il RESOURCE_SEMAPHORE_QUERY_COMPILE SQL Server 2014](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)
