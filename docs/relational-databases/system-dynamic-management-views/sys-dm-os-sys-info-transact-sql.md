---
description: sys.dm_os_sys_info (Transact-SQL)
title: sys.dm_os_sys_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_sys_info_TSQL
- dm_os_sys_info
- dm_os_sys_info_TSQL
- sys.dm_os_sys_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_info dynamic management view
- time [SQL Server], instance started
- starting time
ms.assetid: 20f6bc9c-839a-4fa4-b3f3-a6c47d1b69af
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eede187e90a838ae0e0a2c0051a5b4d496c7b4b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096535"
---
# <a name="sysdm_os_sys_info-transact-sql"></a>sys.dm_os_sys_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce svariate informazioni utili sul computer e sulle risorse disponibili e utilizzate da [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].  
  
> **Nota:** Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_sys_info**.  
  
|Nome colonna|Tipo di dati|Descrizione e note specifiche della versione |  
|-----------------|---------------|-----------------|  
|**cpu_ticks**|**bigint**|Specifica il conteggio corrente dei tick della CPU. I tick della CPU vengono recuperati dal contatore RDTSC del processore. Si tratta di un contatore a incremento progressivo costante. Non ammette i valori NULL.|  
|**ms_ticks**|**bigint**|Specifica il numero di millisecondi dall'avvio del computer. Non ammette i valori NULL.|  
|**cpu_count**|**int**|Specifica il numero di CPU logiche nel sistema. Non ammette i valori NULL.|  
|**hyperthread_ratio**|**int**|Specifica il rapporto del numero dei core logici o fisici esposti da un pacchetto del processore fisico. Non ammette i valori NULL.|  
|**physical_memory_in_bytes**|**bigint**|**Si applica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] da a [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Specifica la quantità totale di memoria fisica disponibile nel computer. Non ammette i valori NULL.|  
|**physical_memory_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità totale di memoria fisica disponibile nel computer. Non ammette i valori NULL.|  
|**virtual_memory_in_bytes**|**bigint**|**Si applica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] da a [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Quantità di memoria virtuale disponibile per il processo in modalità utente. Questo valore può essere utilizzato per determinare se SQL Server è stato avviato tramite il parametro /3gb.|  
|**virtual_memory_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità totale di spazio degli indirizzi virtuali disponibile per il processo in modalità utente. Non ammette i valori NULL.|  
|**bpool_committed**|**int**|**Si applica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] da a [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Rappresenta la memoria di cui è stato eseguito il commit in kilobyte (KB) nel gestore della memoria. Non include la memoria riservata nel gestore della memoria. Non ammette i valori NULL.|  
|**committed_kb**|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Rappresenta la memoria di cui è stato eseguito il commit in kilobyte (KB) nel gestore della memoria. Non include la memoria riservata nel gestore della memoria. Non ammette i valori NULL.|  
|**bpool_commit_target**|**int**|**Si applica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] da a [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Rappresenta la quantità di memoria, in kilobyte (KB), che può essere utilizzata dal gestore della memoria di SQL Server.|  
|**committed_target_kb**|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Rappresenta la quantità di memoria, in kilobyte (KB), che può essere utilizzata dal gestore della memoria di SQL Server. La quantità di destinazione viene calcolata tramite una vasta gamma di input quali:<br /><br /> -lo stato corrente del sistema, incluso il relativo carico<br /><br /> -la memoria richiesta dai processi correnti<br /><br /> -la quantità di memoria installata nel computer<br /><br /> -parametri di configurazione<br /><br /> Se **committed_target_kb** è maggiore di **committed_kb**, il gestore della memoria tenterà di ottenere memoria aggiuntiva. Se **committed_target_kb** è inferiore **committed_kb**, il gestore della memoria tenterà di compattare la quantità di memoria di cui è stato eseguito il commit. Il **committed_target_kb** include sempre la memoria rubata e riservata. Non ammette i valori NULL.|  
|**bpool_visible**|**int**|**Si applica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] da a [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Numero di buffer da 8 KB nel pool di buffer a cui è possibile accedere direttamente nello spazio degli indirizzi virtuali di processo. Se non si utilizza AWE (Address Windowing Extensions), quando il pool di buffer raggiunge la memoria massima (bpool_committed = bpool_commit_target) il valore di bpool_visible è uguale al valore di bpool_committed. Quando si utilizza AWE in una versione a 32 bit di SQL Server, bpool_visible rappresenta la dimensione della finestra di mapping AWE utilizzata per accedere alla memoria fisica allocata dal pool di buffer. Poiché le dimensioni di questa finestra di mapping sono associate allo spazio degli indirizzi di processo, la quantità visibile sarà inferiore a quella di cui è stato eseguito il commit ed è possibile che risulti ulteriormente ridotta dai componenti interni che utilizzano la memoria per fini diversi dalla visualizzazione delle pagine di database. Se il valore di bpool_visible è troppo basso, è possibile che vengano visualizzati errori di memoria insufficiente.|  
|**visible_target_kb**|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Equivale a **committed_target_kb**. Non ammette i valori NULL.|  
|**stack_size_in_bytes**|**int**|Specifica le dimensioni dello stack di chiamate per ogni thread creato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori NULL.|  
|**os_quantum**|**bigint**|Rappresenta il quantum per un'attività non preemptive misurato in millisecondi. Quantum (in secondi) = velocità di clock **os_quantum** /CPU. Non ammette i valori NULL.|  
|**os_error_mode**|**int**|Viene specificata la modalità di errore per il processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori NULL.|  
|**os_priority_class**|**int**|Specifica la classe di priorità per il processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ammette valori Null.<br /><br /> 32 = Normale (nel log degli errori viene indicato che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviato con valore base di priorità normale (=7).)<br /><br /> 128 = Alta (nel log degli errori viene indicato che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione con valore base di priorità alta.  (=13).)<br /><br /> Per altre informazioni, vedere [Configurare l'opzione di configurazione del server priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).|  
|**max_workers_count**|**int**|Rappresenta il numero massimo di thread di lavoro che è possibile creare. Non ammette i valori NULL.|  
|**scheduler_count**|**int**|Rappresenta il numero di utilità di pianificazione utente configurate nel processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori NULL.|  
|**scheduler_total_count**|**int**|Rappresenta il numero totale di utilità di pianificazione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori NULL.|  
|**deadlock_monitor_serial_number**|**int**|Specifica l'ID della sequenza corrente di monitoraggio dei deadlock. Non ammette i valori NULL.|  
|**sqlserver_start_time_ms_ticks**|**bigint**|Rappresenta il numero di **ms_tick** al momento dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ultimo avvio di. Da confrontare con la colonna ms_ticks corrente. Non ammette i valori NULL.|  
|**sqlserver_start_time**|**datetime**|Specifica la data e l'ora dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ultimo avvio del sistema locale. Non ammette i valori NULL.|  
|**affinity_type**|**int**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Viene specificato il tipo di affinità di processo CPU server attualmente in uso. Non ammette i valori NULL. Per ulteriori informazioni, vedere [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).<br /><br /> 1 = MANUAL<br /><br /> 2 = AUTO|  
|**affinity_type_desc**|**varchar(60)**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Descrive la colonna **affinity_type** . Non ammette i valori NULL.<br /><br /> MANUAL = l'affinità è stata impostata per almeno una CPU.<br /><br /> AUTO = in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile spostare liberamente i thread tra CPU.|  
|**process_kernel_time_ms**|**bigint**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Tempo totale in millisecondi impiegato da tutti i thread di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità kernel. Questo valore può essere maggiore di un singolo clock del processore perché è incluso il tempo di tutti i processori nel server. Non ammette i valori NULL.|  
|**process_user_time_ms**|**bigint**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Tempo totale in millisecondi impiegato da tutti i thread di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità utente. Questo valore può essere maggiore di un singolo clock del processore perché è incluso il tempo di tutti i processori nel server. Non ammette i valori NULL.|  
|**time_source**|**int**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Indica l'API utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per recuperare il tempo di clock. Non ammette i valori NULL.<br /><br /> 0 = QUERY_PERFORMANCE_COUNTER<br /><br /> 1 = MULTIMEDIA_TIMER|  
|**time_source_desc**|**nvarchar(60)**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Descrive la colonna **Time_Source** . Non ammette i valori NULL.<br /><br /> QUERY_PERFORMANCE_COUNTER = l'API [QueryPerformanceCounter](/windows/win32/api/profileapi/nf-profileapi-queryperformancecounter) Recupera il tempo di clock.<br /><br /> MULTIMEDIA_TIMER = l'API del [timer multimediale](/previous-versions//ms713418(v=vs.85)) che recupera il tempo di clock.|  
|**virtual_machine_type**|**int**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Indica se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione in un ambiente virtualizzato.  Non ammette i valori NULL.<br /><br /> 0 = NONE<br /><br /> 1 = HYPERVISOR<br /><br /> 2 = OTHER|  
|**virtual_machine_type_desc**|**nvarchar(60)**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> Descrive la colonna **virtual_machine_type** . Non ammette i valori NULL.<br /><br /> NONE = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in esecuzione all'interno di una macchina virtuale.<br /><br /> HYPERVISOR = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione in una macchina virtuale ospitata da un sistema operativo che esegue hypervisor (un sistema operativo host che utilizza la virtualizzazione assistita da hardware).<br /><br /> OTHER = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione all'interno di una macchina virtuale ospitata da un sistema operativo che non utilizza assistente hardware, ad esempio Microsoft Virtual PC.|  
|**softnuma_configuration**|**int**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e versioni successive.<br /><br /> Specifica il modo in cui vengono configurati i nodi NUMA. Non ammette i valori NULL.<br /><br /> 0 = OFF indica l'impostazione predefinita dell'hardware<br /><br /> 1 = soft-NUMA automatizzato<br /><br /> 2 = Soft-NUMA manuale tramite il registro di sistema|  
|**softnuma_configuration_desc**|**nvarchar(60)**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e versioni successive.<br /><br /> DISATTIVATO = la funzionalità Soft-NUMA è disattivata<br /><br /> ON = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determina automaticamente le dimensioni dei nodi NUMA per soft-NUMA<br /><br /> MANUAL = configurazione soft-NUMA manualmente|
|**process_physical_affinity**|**nvarchar (3072)** |**Si applica a:** A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] .<br /><br />Informazioni ancora disponibili. |
|**sql_memory_model**|**int**|**Si applica a:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 e versioni successive.<br /><br />Specifica il modello di memoria utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per allocare memoria. Non ammette i valori NULL.<br /><br />1 = modello di memoria convenzionale<br />2 = blocco di pagine in memoria<br /> 3 = pagine di grandi dimensioni in memoria|
|**sql_memory_model_desc**|**nvarchar(120)**|**Si applica a:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 e versioni successive.<br /><br />Specifica il modello di memoria utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per allocare memoria. Non ammette i valori NULL.<br /><br />  =  Convenzionale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Usa il modello di memoria convenzionale per allocare memoria. Si tratta di un modello di memoria SQL predefinito quando l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio non dispone dei privilegi di blocco delle pagine in memoria durante l'avvio.<br />  =  LOCK_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] USA blocco di pagine in memoria per allocare memoria. Gestione memoria SQL predefinita quando SQL Server account del servizio possiede il privilegio blocco di pagine in memoria durante SQL Server avvio.<br />   =  LARGE_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza pagine di grandi dimensioni in memoria per allocare memoria. SQL Server utilizza l'allocatore di pagine di grandi dimensioni per allocare memoria solo con Enterprise Edition quando l'account del servizio SQL Server dispone del privilegio blocco di pagine in memoria durante l'avvio del server e quando il flag di traccia 834 è attivato.|
|**pdw_node_id**|**int**|**Si applica a:** [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
|**socket_count** |**int** | **Si applica a:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 e versioni successive.<br /><br />Specifica il numero di socket del processore disponibili nel sistema. |  
|**cores_per_socket** |**int** | **Si applica a:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 e versioni successive.<br /><br />Specifica il numero di processori per socket disponibili nel sistema. |  
|**numa_node_count** |**int** | **Si applica a:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 e versioni successive.<br /><br />Specifica il numero di nodi NUMA disponibili nel sistema. Questa colonna include i nodi NUMA fisici e i nodi NUMA soft. |  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
