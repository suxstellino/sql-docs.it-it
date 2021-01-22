---
title: Monitoraggio dell'utilizzo della memoria
description: "Monitorare un'istanza di SQL Server per verificare che l'utilizzo della memoria rientri negli intervalli standard. Usare i contatori Memoria: Byte disponibili e Memoria: Pagine/sec."
ms.custom: ''
ms.date: 01/20/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tuning databases [SQL Server], memory
- monitoring server performance [SQL Server], memory usage
- isolating memory [SQL Server]
- paging rate [SQL Server]
- memory [SQL Server], monitoring usage
- monitoring [SQL Server], memory usage
- low-memory conditions
- database monitoring [SQL Server], memory usage
- available memory [SQL Server]
- page faults [SQL Server]
- monitoring performance [SQL Server], memory usage
- server performance [SQL Server], memory
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8954573b0c32eef45ec655b27d26e03e72be96ed
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688762"
---
# <a name="monitor-memory-usage"></a>Monitorare l'utilizzo della memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Monitorare periodicamente un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per verificare che l'utilizzo della memoria rientri negli intervalli standard. 

## <a name="configuring-ssnoversion-max-memory"></a>Configurazione della [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] memoria massima

Per impostazione predefinita, un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza può nel tempo utilizzare la maggior parte della memoria del sistema operativo Windows disponibile nel server. Una volta acquisita la memoria, questa non verrà rilasciata a meno che non venga rilevato un numero eccessivo di richieste di memoria. Questo si verifica in base alla progettazione e non indica una perdita di memoria nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processo. Utilizzare [l'opzione **max server memory**](../../database-engine/configure-windows/server-memory-server-configuration-options.md) per limitare la quantità di memoria che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è consentito acquisire per la maggior parte degli utilizzi. Per ulteriori informazioni, vedere la [Guida all'architettura di gestione della memoria](../../relational-databases/memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).

In SQL Server in Linux [impostare il limite di memoria](../../linux/sql-server-linux-performance-best-practices.md#advanced-configuration) con lo strumento MSSQL-conf e l' [impostazione Memory. memorylimitmb](../../linux/sql-server-linux-configure-mssql-conf.md#memorylimit).  

## <a name="monitor-operating-system-memory"></a>Monitorare la memoria del sistema operativo   
 Per eseguire il monitoraggio di una condizione di memoria insufficiente, utilizzare i contatori di Windows Server seguenti. È possibile eseguire query su molti contatori di memoria del sistema operativo tramite le viste a gestione dinamica [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) e [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).

-   **Memoria: Byte disponibili**  
Questo contatore indica il numero di byte di memoria attualmente disponibili per l'utilizzo da processi. I valori bassi del contatore **byte disponibili** possono indicare una carenza complessiva della memoria del sistema operativo. È possibile eseguire query su questo valore tramite T-SQL utilizzando [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).available_physical_memory_kb.

-   **Memoria: Pagine/sec**  
Questo contatore indica il numero di pagine recuperate dal disco a causa di errori di pagina hardware o scritte su disco per liberare spazio nel working set a causa di errori di pagina. Un valore elevato del contatore **Pagine/sec** può indicare un paging eccessivo.  

-   **Memoria: errori di pagina/sec** Questo contatore indica la frequenza degli errori di pagina per tutti i processi, inclusi i processi di sistema. Una frequenza bassa ma diversa da zero di paging su disco (e di conseguenza errori di pagina) è tipica, anche se il computer dispone di una quantità di memoria disponibile. Microsoft Windows Virtual Memory Manager (VMM) sottrae pagine a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e agli altri processi in quanto riduce le dimensioni dei set di lavoro di tali processi. L'attività di VMM tende quindi a causare errori di pagina.  

-   **Processo: errori di pagina/sec** Questo contatore indica la frequenza degli errori di pagina per un determinato processo utente. Monitoraggio **processo: errori di pagina/sec** per determinare se l'attività del disco è causata dal paging da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per determinare se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un altro processo provoca un paging eccessivo, monitorare il contatore **Processo: Errori di pagina/sec** alla ricerca dell'istanza del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per ulteriori informazioni sulla risoluzione di un paging eccessivo, vedere la documentazione del sistema operativo.  
  
## <a name="isolating-memory-used-by-ssnoversion"></a>Isolamento della memoria utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 

 Per monitorare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'utilizzo della memoria, utilizzare i seguenti [SQL Server contatori di oggetti](use-sql-server-objects.md). Molti SQL Server contatori di oggetti possono essere sottoposti a query tramite le viste a gestione dinamica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) o [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md).

 Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestisce i requisiti di memoria in modo dinamico, in base alle risorse di sistema disponibili. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necessita di una maggior quantità di memoria, richiede al sistema operativo di determinare se è disponibile memoria fisica e utilizza la memoria disponibile. Se la memoria disponibile è insufficiente per il sistema operativo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rilascerà la memoria al sistema operativo fino a quando non viene rilevata la condizione di memoria insufficiente o fino a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] raggiungere il limite di **memoria minima del server** . Tuttavia, è possibile eseguire l'override dell'opzione per usare la memoria in modo dinamico usando le opzioni di configurazione del server **min server** Memory e **max server memory** . Per altre informazioni, vedere [Opzioni per la memoria server](../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
 Per monitorare la quantità di memoria usata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esaminare i contatori delle prestazioni seguenti:  
  
-   **SQL Server: Gestione memoria: Memoria totale server (KB)**  
Questo contatore indica la quantità di memoria del sistema operativo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è attualmente stato eseguito il commit del gestore della memoria [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È previsto che questo numero aumenti come richiesto dall'attività effettiva e aumenterà dopo SQL Server avvio. Eseguire una query su questo contatore usando la vista a gestione dinamica [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) , osservando la colonna **committed_kb** .

-   **SQL Server: gestore della memoria: memoria del server di destinazione (KB)**  
Questo contatore indica una quantità ideale di memoria che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere utilizzata in base al carico di lavoro recente. Confrontare con la **memoria totale del server** dopo un periodo di operazione normale per determinare se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è allocata una quantità di memoria desiderata. Dopo l'operazione tipica, la memoria **totale del server** e la memoria del **server di destinazione** devono essere simili. Se la **memoria totale del server** è significativamente inferiore rispetto alla **memoria del server di destinazione**, è possibile che si verifichi un utilizzo eccessivo della [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] memoria. Durante un periodo di tempo dopo l'avvio di, la memoria [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **totale del server** dovrebbe essere inferiore alla **memoria del server di destinazione**, in quanto la **memoria totale del server** aumenta. Eseguire una query su questo contatore usando la vista a gestione dinamica [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) , osservando la colonna **committed_target_kb** . Per ulteriori informazioni e procedure consigliate per la configurazione della memoria, vedere [Opzioni di configurazione della memoria del server](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).

-   **Processo: Working set**  
Questo contatore indica la quantità di memoria fisica attualmente utilizzata da un processo, in base al sistema operativo. Osservare l'istanza sqlservr.exe di questo contatore. Eseguire una query su questo contatore usando la vista a gestione dinamica [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) , osservando la colonna **physical_memory_in_use_kb** .

-   **Processo: byte privati**  
Questo contatore indica la quantità di memoria richiesta da un processo per il proprio utilizzo al sistema operativo. Osservare l'istanza sqlservr.exe di questo contatore. Poiché questo contatore include tutte le allocazioni di memoria richieste da sqlservr.exe, incluse quelle non limitate dall' [opzione **max server memory**](../../database-engine/configure-windows/server-memory-server-configuration-options.md), questo contatore può segnalare valori maggiori [dell'opzione **max server memory**](../../database-engine/configure-windows/server-memory-server-configuration-options.md).

-   **SQL Server: Gestione buffer: Pagine di database**  
Questo contatore indica il numero di pagine nel pool di buffer con contenuto del database. Non include altri tipi di memoria del pool non buffer all'interno del SQL Server processo. Eseguire una query su questo contatore utilizzando la vista a gestione dinamica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) .

-   **SQL Server: Gestione buffer: Percentuale riscontri cache del buffer**  
Questo contatore è specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È consigliabile un rapporto di 90 o superiore. Un valore maggiore di 90 indica che più del 90% di tutte le richieste di dati sono state soddisfatte dalla cache dei dati in memoria senza dover leggere dal disco. Per ulteriori informazioni su Gestione buffer SQL Server, vedere l' [oggetto SQL Server buffer Manager](sql-server-buffer-manager-object.md). Eseguire una query su questo contatore utilizzando la vista a gestione dinamica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) .  
 
-   **SQL Server: Gestione buffer: permanenza presunta delle pagine**  
Questo contatore misura la quantità di tempo in secondi per cui la pagina meno recente rimane nel pool di buffer. Per i sistemi che usano un'architettura NUMA, questa è la media tra tutti i nodi NUMA. Si tratta di un valore più elevato in crescita. Un DIP improvviso indica una varianza significativa dei dati all'interno e all'esterno del pool di buffer, che indica che il carico di lavoro non è stato in grado di sfruttare completamente i dati già in memoria. Ogni nodo NUMA dispone di un proprio nodo del pool di buffer. Nei server con più di un nodo NUMA, visualizzare la permanenza presunta delle pagine del nodo del pool di buffer usando **SQL Server: nodo buffer:** permanenza presunta delle pagine. Eseguire una query su questo contatore utilizzando la vista a gestione dinamica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) .

  
## <a name="examples"></a>Esempi 

### <a name="determining-current-memory-allocation"></a>Determinazione dell'allocazione di memoria corrente  
 Le query seguenti restituiscono informazioni sulla memoria attualmente allocata.  
  
```  
SELECT
(total_physical_memory_kb/1024) AS Total_OS_Memory_MB,
(available_physical_memory_kb/1024)  AS Available_OS_Memory_MB
FROM sys.dm_os_sys_memory;

SELECT  
(physical_memory_in_use_kb/1024) AS Memory_used_by_Sqlserver_MB,  
(locked_page_allocations_kb/1024) AS Locked_pages_used_by_Sqlserver_MB,  
(total_virtual_address_space_kb/1024) AS Total_VAS_in_MB,
process_physical_memory_low,  
process_virtual_memory_low  
FROM sys.dm_os_process_memory;  
```  

### <a name="determining-current-sql-server-memory-utilization"></a>Determinazione dell'utilizzo corrente della memoria SQL Server   
 La query seguente restituisce informazioni sull'utilizzo della memoria SQL Server corrente.  
```  
SELECT
sqlserver_start_time,
(committed_kb/1024) AS Total_Server_Memory_MB,
(committed_target_kb/1024)  AS Target_Server_Memory_MB
FROM sys.dm_os_sys_info;
```   

### <a name="determining-page-life-expectancy"></a>Determinazione della permanenza presunta delle pagine
 Nella query seguente vengono utilizzati **sys.dm_os_performance_counters** per osservare il valore di permanenza presunta delle **pagine** dell'istanza di SQL Server a livello generale di gestione buffer e a ogni livello di nodo NUMA.
```
SELECT
CASE instance_name WHEN '' THEN 'Overall' ELSE instance_name END AS NUMA_Node, cntr_value AS PLE_s
FROM sys.dm_os_performance_counters    
WHERE counter_name = 'Page life expectancy';
```

## <a name="see-also"></a>Vedi anche
- [sys.dm_os_sys_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md)
- [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md)
- [sys.dm_os_sys_info (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)
- [sys.dm_os_performance_counters (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)
- [Oggetto Memory Manager di SQL Server](sql-server-memory-manager-object.md)
- [Oggetto Buffer Manager di SQL Server](sql-server-buffer-manager-object.md)   
- [Opzioni di configurazione della memoria del server](../../database-engine/configure-windows/server-memory-server-configuration-options.md)
- [Guida sull'architettura di gestione della memoria](../../relational-databases/memory-management-architecture-guide.md)
