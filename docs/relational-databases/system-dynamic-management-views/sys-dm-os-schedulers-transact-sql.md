---
description: sys.dm_os_schedulers (Transact-SQL)
title: sys.dm_os_schedulers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_schedulers
- sys.dm_os_schedulers_TSQL
- sys.dm_os_schedulers
- dm_os_schedulers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_schedulers dynamic management view
ms.assetid: 3a09d81b-55d5-416f-9cda-1a3a5492abe0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a2d8ed3f8eb3aa5a89ee7176863165a2a0a0b74
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171723"
---
# <a name="sysdm_os_schedulers-transact-sql"></a>sys.dm_os_schedulers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per utilità di pianificazione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dove è stato eseguito il mapping di ogni utilità di pianificazione a un singolo processore. Utilizzare questa vista per eseguire il monitoraggio delle condizioni di un'utilità di pianificazione oppure per identificare eventuali attività sfuggite al controllo. Per ulteriori informazioni sulle utilità di pianificazione, vedere la [Guida all'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md).  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_schedulers**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|scheduler_address|**varbinary (8)**|Indirizzo di memoria dell'utilità di pianificazione. Non ammette i valori Null.|  
|parent_node_id|**int**|ID del nodo a cui appartiene l'utilità di pianificazione, definito anche nodo padre. Rappresenta un nodo NUMA (Non-Uniform Memory Access). Non ammette i valori Null.|  
|scheduler_id|**int**|ID dell'utilità di pianificazione. Tutte le utilità di pianificazione utilizzate per eseguire query normali sono identificate da numeri ID minori di 1048576. Quelle con ID maggiori o uguali a 1048576 vengono utilizzate internamente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio l'utilità di pianificazione della connessione amministrativa dedicata. Non ammette i valori Null.|  
|cpu_id|**smallint**|ID CPU assegnato all'utilità di pianificazione.<br /><br /> Non ammette i valori Null.<br /><br /> **Nota:** 255 non indica alcuna affinità come in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Per ulteriori informazioni sull'affinità, vedere [sys.dm_os_threads &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md) .|  
|status|**nvarchar(60)**|Indica lo stato dell'utilità di pianificazione. I possibili valori sono i seguenti:<br /><br /> -NASCOSTO ONLINE<br />-NASCOSTO OFFLINE<br />-VISIBILE ONLINE<br />-VISIBILE OFFLINE<br />-VISIBILE ONLINE (DAC)<br />-HOT_ADDED<br /><br /> Non ammette i valori Null.<br /><br /> Le utilità di pianificazione HIDDEN vengono utilizzate per l'elaborazione delle richieste interne a [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Le utilità di pianificazione VISIBLE vengono utilizzate per l'elaborazione delle richieste dell'utente.<br /><br /> Le utilità di pianificazione OFFLINE eseguono il mapping ai processori che sono offline nella maschera di affinità e che non vengono pertanto utilizzati per l'elaborazione di alcuna richiesta. Le utilità di pianificazione ONLINE eseguono il mapping ai processori che sono online nella maschera di affinità e che sono disponibili per l'elaborazione di thread.<br /><br /> Il valore DAC indica che l'utilità di pianificazione è in esecuzione nell'ambito di una connessione amministrativa dedicata.<br /><br /> HOT ADDED indica le utilità di pianificazione aggiunte in risposta a un evento di aggiunta di CPU a caldo.|  
|is_online|**bit**|Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurato in modo da utilizzare solo alcuni dei processori disponibili nel server, è possibile che sia stato eseguito il mapping di alcune utilità di pianificazione a processori non inclusi nella maschera di affinità. In tal caso, questa colonna restituisce 0, a indicare che l'utilità di pianificazione non viene utilizzata per elaborare query o batch.<br /><br /> Non ammette i valori Null.|  
|is_idle|**bit**|1 = L'utilità di pianificazione è inattiva. Nessun thread di lavoro è in esecuzione. Non ammette i valori Null.|  
|preemptive_switches_count|**int**|Numero di volte in cui i thread di lavoro nell'utilità di pianificazione corrente sono passati alla modalità preemptive.<br /><br /> Per eseguire codice esterno a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio stored procedure estese e query distribuite, è necessario che un thread venga eseguito esternamente al controllo dell'utilità di pianificazione in modalità non preemptive. A tale scopo, un thread di lavoro passa alla modalità preemptive.|  
|context_switches_count|**int**|Numero di cambi di contesto che si sono verificati nell'utilità di pianificazione corrente. Non ammette i valori Null.<br /><br /> Per consentire l'esecuzione di altri thread di lavoro, il thread di lavoro in esecuzione deve cedere il controllo dell'utilità di pianificazione o cambiare contesto.<br /><br /> **Nota:** Se un thread di lavoro produce l'utilità di pianificazione e si inserisce nella coda eseguibile e quindi non trova altri thread di lavoro, il ruolo di lavoro selezionerà se stesso. In questo caso la colonna context_switches_count non viene aggiornata, a differenza di yield_count, che viene aggiornata.|  
|idle_switches_count|**int**|Numero di volte in cui l'utilità di pianificazione è rimasta in attesa di un evento durante il periodo di inattività. Questa colonna è simile a context_switches_count. Non ammette i valori Null.|  
|current_tasks_count|**int**|Numero di attività correnti associate all'utilità di pianificazione corrente. Questo numero include le attività seguenti:<br /><br /> -Attività in attesa di un thread di lavoro per eseguirle.<br />-Attività attualmente in attesa o in esecuzione (in stato sospeso o ESEGUIbile).<br /><br /> Non appena un'attività viene completata, il conteggio viene ridotto. Non ammette i valori Null.|  
|runnable_tasks_count|**int**|Numero di thread di lavoro, con le attività ad essi assegnati, in attesa di essere pianificati nella coda eseguibile. Non ammette i valori Null.|  
|current_workers_count|**int**|Numero di thread di lavoro associati all'utilità di pianificazione corrente. Questo numero include i thread di lavoro a cui non sono state assegnate attività. Non ammette i valori Null.|  
|active_workers_count|**int**|Numero di thread di lavoro attivi. Un thread di lavoro attivo non è mai preemptive, deve disporre sempre di un'attività associata e può essere in esecuzione, eseguibile o sospeso. Non ammette i valori Null.|  
|work_queue_count|**bigint**|Numero di attività nella coda in sospeso in attesa di essere prelevate da un thread di lavoro. Non ammette i valori Null.|  
|pending_disk_io_count|**int**|Numero di I/O in sospeso in attesa di essere completati. Ogni utilità di pianificazione dispone di un elenco di operazioni di I/O in sospeso che vengono verificate per determinarne il completamento ogni volta che si verifica uno scambio di contesto. Il conteggio viene incrementato quando la richiesta viene inserita e viene ridotto quando la richiesta viene completata. Questo numero non indica lo stato degli I/O. Non ammette i valori Null.|  
|load_factor|**int**|Valore interno indicante il fattore di carico nell'utilità di pianificazione corrente. Questo valore viene utilizzato per determinare se una nuova attività deve essere inserita nell'utilità di pianificazione corrente oppure in un'altra utilità di pianificazione. Risulta utile per eseguire attività di debug nel caso in cui il carico delle utilità di pianificazione non sembra essere distribuito in modo uniforme. Il routing viene definito in base al carico dell'utilità di pianificazione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza inoltre un fattore di caricamento di nodi e utilità di pianificazione per semplificare l'individuazione del percorso migliore per acquisire risorse. Quando un'attività viene accodata, il fattore di caricamento viene aumentato. Quando un'attività viene completata, il fattore di caricamento viene ridotto. L'utilizzo dei fattori di caricamento consente l'ottimizzazione del bilanciamento del carico di lavoro da parte del sistema operativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori Null.|  
|yield_count|**int**|Valore interno utilizzato per indicare lo stato dell'utilità di pianificazione corrente. Questo valore viene utilizzato dal sistema di monitoraggio dell'utilità di pianificazione per determinare se un thread di lavoro dell'utilità di pianificazione non restituisce tempestivamente il controllo ad altri thread di lavoro. Non indica il passaggio di un thread di lavoro o di un'attività a un nuovo thread di lavoro. Non ammette i valori Null.|  
|last_timer_activity|**bigint**|Espresso in tick della CPU, indica l'ultimo controllo della coda del timer dell'utilità di pianificazione da parte dell'utilità stessa. Non ammette i valori Null.|  
|failed_to_create_worker|**bit**|Impostare su 1 se risulta impossibile creare un nuovo thread di lavoro nell'utilità di pianificazione corrente. Ciò in genere si verifica a causa di vincoli a livello di memoria. Ammette i valori Null.|  
|active_worker_address|**varbinary (8)**|Indirizzo di memoria del thread di lavoro attivo. Ammette i valori Null. Per ulteriori informazioni, vedere [sys.dm_os_workers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|memory_object_address|**varbinary (8)**|Indirizzo di memoria dell'oggetto memoria dell'utilità di pianificazione. Non ammette i valori Null.|  
|task_memory_object_address|**varbinary (8)**|Indirizzo di memoria dell'oggetto memoria dell'attività. Non ammette i valori Null. Per ulteriori informazioni, vedere [sys.dm_os_memory_objects &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|quantum_length_us|**bigint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Espone il quantum dell'utilità di pianificazione utilizzato da SQLOS.|  
| total_cpu_usage_ms |**bigint**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive <br><br> CPU totale utilizzata dall'utilità di pianificazione come indicato dai thread di lavoro non preemptive. Non ammette i valori Null.|
|total_cpu_idle_capped_ms|**bigint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Indica che la limitazione dipende dall' [obiettivo del livello di servizio](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu#service-level-objective), sarà sempre 0 per le versioni non di Azure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ammette i valori Null.|
|total_scheduler_delay_ms|**bigint**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive <br><br> Il tempo che intercorre tra un thread di lavoro e l'altro. Possono essere causati da processi di lavoro di tipo preemptive che ritardano la pianificazione del successivo thread di lavoro non preemptive o a causa dei thread di pianificazione del sistema operativo di altri processi. Non ammette i valori Null.|
|ideal_workers_limit|**int**|**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e versioni successive <br><br> Il numero di ruoli di lavoro idealmente presenti nell'utilità di pianificazione. Se i thread di lavoro correnti superano il limite dovuto a un carico di attività sbilanciato, quando diventano inattivi, verranno eliminati. Non ammette i valori Null.|
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="examples"></a>Esempi  
  
### <a name="a-monitoring-hidden-and-nonhidden-schedulers"></a>R. Monitoraggio delle utilità di pianificazione nascoste e non nascoste  
 La query seguente restituisce lo stato dei thread di lavoro e delle attività di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per tutte le utilità di pianificazione. La query viene eseguita su un computer dotato di:  
  
-   Due processori (CPU)  
  
-   Due nodi (NUMA)  
  
-   Una CPU per nodo NUMA  
  
-   Maschera di affinità impostata su `0x03`.  
  
```sql  
SELECT  
    scheduler_id,  
    cpu_id,  
    parent_node_id,  
    current_tasks_count,  
    runnable_tasks_count,  
    current_workers_count,  
    active_workers_count,  
    work_queue_count  
  FROM sys.dm_os_schedulers;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
scheduler_id cpu_id parent_node_id current_tasks_count  
------------ ------ -------------- -------------------  
0            1      0              9                    
257          255    0              1                    
1            0      1              10                   
258          255    1              1                    
255          255    32             2                    
  
runnable_tasks_count current_workers_count  
-------------------- ---------------------  
0                    11                     
0                    1                      
0                    18                     
0                    1                      
0                    3                      
  
active_workers_count work_queue_count  
-------------------- --------------------  
6                    0  
1                    0  
8                    0  
1                    0  
1                    0  
```  
  
 L'output contiene le informazioni seguenti:  
  
-   Sono disponibili cinque utilità di pianificazione. Due utilità di pianificazione hanno un valore ID < 1048576. Le utilità di pianificazione con ID >= 1048576 sono note come utilità di pianificazione nascoste. L'utilità di pianificazione `255` rappresenta la connessione amministrativa dedicata. Per ogni istanza è prevista un'utilità di pianificazione DAC. I monitoraggi risorse che coordinano le richieste di memoria utilizzano le utilità di pianificazione `257` e `258`, una per ogni nodo NUMA.  
  
-   Nell'output sono presenti 23 attività in corso, che includono le richieste utente e le attività di gestione delle risorse avviate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esempi di attività di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono RESOURCE MONITOR (una per ogni nodo NUMA), LAZY WRITER (una per ogni nodo NUMA), LOCK MONITOR, CHECKPOINT e LOG WRITER.  
  
-   Sono stati eseguiti i mapping del nodo NUMA `0` alla CPU `1` e del nodo NUMA `1` alla CPU `0`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviato generalmente in un nodo NUMA diverso da 0.  
  
-   Se tramite `runnable_tasks_count` viene restituito `0`, significa che non sono presenti attività in esecuzione attiva. È tuttavia possibile che esistano sessioni attive.  
  
-   All'utilità di pianificazione `255` che rappresenta la connessione DAC sono associati `3` thread di lavoro, che vengono allocati all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e non subiscono modifiche. Tali thread vengono utilizzati esclusivamente per l'elaborazione delle query DAC. Le due attività presenti in questa utilità di pianificazione rappresentano una gestione connessione e un thread di lavoro inattivo.  
  
-   `active_workers_count` rappresenta tutti i thread di lavoro a cui sono associate attività e che sono in esecuzione in modalità non preemptive. Alcune attività, ad esempio i listener di rete, vengono eseguite nell'ambito di una pianificazione preemptive.  
  
-   Le utilità di pianificazione nascoste non elaborano le richieste utente comuni. L'utilità di pianificazione DAC costituisce l'eccezione, poiché dispone di un thread per l'elaborazione delle richieste.  
  
### <a name="b-monitoring-nonhidden-schedulers-in-a-busy-system"></a>B. Monitoraggio delle utilità di pianificazione non nascoste in un sistema a utilizzo intensivo  
 Nella query seguente viene illustrato lo stato delle utilità di pianificazione non nascoste con carichi notevoli, in cui il numero delle richieste è maggiore di quello che può essere gestito dai thread di lavoro disponibili. In questo esempio le attività vengono assegnate a 256 thread di lavoro. Alcune attività sono in attesa dell'assegnazione a un thread di lavoro. Un conteggio di attività eseguibili inferiore indica che più attività sono in attesa di una risorsa.  
  
> [!NOTE]  
>  Per individuare lo stato dei thread di lavoro, è possibile eseguire una query su sys.dm_os_workers. Per ulteriori informazioni, vedere [sys.dm_os_workers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).  
  
 La query è la seguente:  
  
```sql  
SELECT  
    scheduler_id,  
    cpu_id,  
    current_tasks_count,  
    runnable_tasks_count,  
    current_workers_count,  
    active_workers_count,  
    work_queue_count  
  FROM sys.dm_os_schedulers  
  WHERE scheduler_id < 255;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
scheduler_id current_tasks_count runnable_tasks_count  
------------ ------------------- --------------------  
0            144                 0                     
1            147                 1                     
  
current_workers_count active_workers_count work_queue_count  
--------------------- -------------------- --------------------  
128                   125                  16  
128                   126                  19  
```  
  
 Nel risultato seguente sono invece presenti più attività eseguibili, mentre nessuna attività è in attesa di un thread di lavoro. Il valore di `work_queue_count` è `0` per entrambe le utilità di pianificazione.  
  
```  
scheduler_id current_tasks_count runnable_tasks_count  
------------ ------------------- --------------------  
0            107                 98                    
1            110                 100                   
  
current_workers_count active_workers_count work_queue_count  
--------------------- -------------------- --------------------  
128                   104                  0  
128                   108                  0  
```  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
