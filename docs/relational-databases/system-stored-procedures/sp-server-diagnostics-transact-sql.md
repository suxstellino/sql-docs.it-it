---
description: sp_server_diagnostics (Transact-SQL)
title: sp_server_diagnostics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_server_diagnostics
- sp_server_diagnostics_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_server_diagnostics
ms.assetid: 62658017-d089-459c-9492-c51e28f60efe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b57d7d6f9925c8cd2c4670daa68054dff9f1e871
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190821"
---
# <a name="sp_server_diagnostics-transact-sql"></a>sp_server_diagnostics (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Acquisisce dati diagnostici e informazioni di integrità su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per rilevare potenziali errori. La procedura viene eseguita in modalità di ripetizione e i risultati vengono inviati periodicamente. Può essere richiamata da una connessione normale o di applicazione livello dati.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_server_diagnostics [@repeat_interval =] 'repeat_interval_in_seconds'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @repeat_interval = ] 'repeat_interval_in_seconds'` Indica l'intervallo di tempo in cui il stored procedure viene eseguito ripetutamente per inviare informazioni sull'integrità.  
  
 *repeat_interval_in_seconds* è di **tipo int** e il valore predefinito è 0. I valori di parametro validi sono 0 oppure qualsiasi valore uguale o maggiore di 5. È necessario eseguire la stored procedure per almeno 5 secondi per restituire i dati completi. Il valore minimo per l'esecuzione della stored procedure in modalità di ripetizione è 5 secondi.  
  
 Se questo parametro non viene specificato o se il valore specificato è 0, la stored procedure restituirà i dati una sola volta, quindi verrà chiusa.  
  
 Se il valore specificato è inferiore al valore minimo, verrà generato un errore e non verrà restituito nulla.  
  
 Se il valore specificato è uguale o maggiore di 5, la stored procedure viene eseguita ripetutamente per restituire lo stato di integrità finché non verrà annullato manualmente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
**sp_server_diagnostics** restituisce le informazioni seguenti  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|**create_time**|**datetime**|Indica il timestamp della creazione della riga. Ogni riga di un singolo set di righe dispone dello stesso timestamp.|  
|**component_type**|**sysname**|Indica se la riga contiene informazioni per il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] componente a livello di istanza o per un gruppo di disponibilità always on:<br /><br /> instance<br /><br /> Always On: AvailabilityGroup|  
|**component_name**|**sysname**|Indica il nome del componente o il nome del gruppo di disponibilità:<br /><br /> sistema<br /><br /> resource<br /><br /> query_processing<br /><br /> io_subsystem<br /><br /> eventi<br /><br /> *\<name of the availability group>*|  
|**state**|**int**|Indica lo stato di integrità del componente:<br /><br /> 0<br /><br /> 1<br /><br /> 2<br /><br /> 3|  
|**state_desc**|**sysname**|Descrive la colonna contenente gli stati. Le descrizioni che corrispondono ai valori nella colonna contenente gli stati sono:<br /><br /> 0: Sconosciuto<br /><br /> 1: Pulisci<br /><br /> 2: avviso<br /><br /> 3: errore|  
|**data**|**varchar (max)**|Indica dati specifici del componente.|  
  
 Di seguito sono riportate le descrizioni dei cinque componenti:  
  
-   **System**: raccoglie i dati da una prospettiva di sistema su spinlock, condizioni di elaborazione gravi, attività non cedenti, errori di pagina e utilizzo della CPU. Queste informazioni producono un'indicazione dello stato di integrità complessiva.  
  
-   **risorsa**: raccoglie i dati dal punto di vista delle risorse su memoria fisica e virtuale, pool di buffer, pagine, cache e altri oggetti memoria. Queste informazioni producono un'indicazione dello stato di integrità complessiva.  
  
-   **query_processing**: raccoglie i dati da una prospettiva di elaborazione delle query sui thread di lavoro, le attività, i tipi di attesa, le sessioni con utilizzo intensivo della CPU e le attività di blocco. Queste informazioni producono un'indicazione dello stato di integrità complessiva.  
  
-   **io_subsystem**: raccoglie i dati su io. Oltre ai dati diagnostici, questo componente produce uno stato di integrità di avviso o integro e pulito solo per un sottosistema di IO.  
  
-   **eventi**: raccoglie dati e superfici attraverso la stored procedure sugli errori e gli eventi di interesse registrati dal server, inclusi i dettagli sulle eccezioni del buffer circolare, gli eventi del buffer circolare sul broker di memoria, la memoria insufficiente, il monitoraggio dell'utilità di pianificazione, il pool di buffer, gli spinlock, la sicurezza e la connettività. Gli eventi avranno sempre 0 come stato.  
  
-   **\<name of the availability group>**: Raccoglie i dati per il gruppo di disponibilità specificato (se component_type = "Always On: AvailabilityGroup").  
  
## <a name="remarks"></a>Commenti  
Da una prospettiva di errore, i componenti di elaborazione di query, risorsa e sistema verranno utilizzati per il rilevamento dell'errore mentre i componenti di eventi e io_subsystem verranno utilizzati solo per gli scopi diagnostici.  
  
Nella tabella seguente viene eseguito il mapping dei componenti agli stati di integrità associati.  
  
|Componenti|Pulito (1)|Avviso (2)|Errore (3)|Sconosciuto (0)|  
|----------------|-----------------|-------------------|-----------------|--------------------|  
|sistema|x|x|x||  
|risorse|x|x|x||  
|query_processing|x|x|x||  
|io_subsystem|x|x|||  
|eventi||||x|  
  
La (x) in ogni riga rappresenta gli stati di integrità validi per il componente. Ad esempio, lo stato di io_subsystem potrà essere pulito o avviso. Non verranno visualizzati gli stati di errore.  
 
> [!NOTE]
> L'esecuzione di sp_server_diagnostics procedura interna è implementata in un thread preemptive con priorità alta.
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="examples"></a>Esempio  
È consigliabile utilizzare le sessioni estese per acquisire le informazioni di integrità e salvarle in un file che si trova fuori da SQL Server. Pertanto, è ancora possibile accedervi se si verifica un errore. Nell'esempio seguente viene salvato l'output da una sessione eventi in un file:  
```sql  
CREATE EVENT SESSION [diag]  
ON SERVER  
           ADD EVENT [sp_server_diagnostics_component_result] (set collect_data=1)  
           ADD TARGET [asynchronous_file_target] (set filename='c:\temp\diag.xel');  
GO  
ALTER EVENT SESSION [diag]  
      ON SERVER STATE = start;  
GO  
```  
  
Nella query di esempio riportata di seguito viene letto il file di log della sessione estesa:  
```sql  
SELECT  
    xml_data.value('(/event/@name)[1]','varchar(max)') AS Name  
  , xml_data.value('(/event/@package)[1]', 'varchar(max)') AS Package  
  , xml_data.value('(/event/@timestamp)[1]', 'datetime') AS 'Time'  
  , xml_data.value('(/event/data[@name=''component_type'']/value)[1]','sysname') AS Sysname  
  , xml_data.value('(/event/data[@name=''component_name'']/value)[1]','sysname') AS Component  
  , xml_data.value('(/event/data[@name=''state'']/value)[1]','int') AS State  
  , xml_data.value('(/event/data[@name=''state_desc'']/value)[1]','sysname') AS State_desc  
  , xml_data.query('(/event/data[@name="data"]/value/*)') AS Data  
FROM   
(  
      SELECT  
                        object_name as event  
                        ,CONVERT(xml, event_data) as xml_data  
       FROM    
      sys.fn_xe_file_target_read_file('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\*.xel', NULL, NULL, NULL)  
)   
AS XEventData  
ORDER BY time;  
```  
  
Nell'esempio seguente viene acquisito l'output di sp_server_diagnostics in una tabella in modalità non di ripetizione:  
```sql  
CREATE TABLE SpServerDiagnosticsResult  
(  
      create_time DateTime,  
      component_type sysname,  
      component_name sysname,  
      state int,  
      state_desc sysname,  
      data xml  
);  
INSERT INTO SpServerDiagnosticsResult  
EXEC sp_server_diagnostics; 
```  

La query di esempio seguente legge l'output di riepilogo dalla tabella:  
```sql  
SELECT create_time,
       component_name,
       state_desc 
FROM SpServerDiagnosticsResult;  
``` 

La query di esempio seguente legge parte dell'output dettagliato da ogni componente nella tabella:  
```sql  
-- system
select data.value('(/system/@systemCpuUtilization)[1]','bigint') as 'System_CPU',
   data.value('(/system/@sqlCpuUtilization)[1]','bigint') as 'SQL_CPU',
   data.value('(/system/@nonYieldingTasksReported)[1]','bigint') as 'NonYielding_Tasks',
   data.value('(/system/@pageFaults)[1]','bigint') as 'Page_Faults',
   data.value('(/system/@latchWarnings)[1]','bigint') as 'Latch_Warnings',
   data.value('(/system/@BadPagesDetected)[1]','bigint') as 'BadPages_Detected',
   data.value('(/system/@BadPagesFixed)[1]','bigint') as 'BadPages_Fixed'
from SpServerDiagnosticsResult 
where component_name like 'system'
go

-- Resource Monitor
select data.value('(./Record/ResourceMonitor/Notification)[1]', 'VARCHAR(max)') AS [Notification],
    data.value('(/resource/memoryReport/entry[@description=''Working Set'']/@value)[1]', 'bigint')/1024 AS [SQL_Mem_in_use_MB],
    data.value('(/resource/memoryReport/entry[@description=''Available Paging File'']/@value)[1]', 'bigint')/1024 AS [Avail_Pagefile_MB],
    data.value('(/resource/memoryReport/entry[@description=''Available Physical Memory'']/@value)[1]', 'bigint')/1024 AS [Avail_Physical_Mem_MB],
    data.value('(/resource/memoryReport/entry[@description=''Available Virtual Memory'']/@value)[1]', 'bigint')/1024 AS [Avail_VAS_MB],
    data.value('(/resource/@lastNotification)[1]','varchar(100)') as 'LastNotification',
    data.value('(/resource/@outOfMemoryExceptions)[1]','bigint') as 'OOM_Exceptions'
from SpServerDiagnosticsResult 
where component_name like 'resource'
go

-- Nonpreemptive waits
select waits.evt.value('(@waitType)','varchar(100)') as 'Wait_Type',
   waits.evt.value('(@waits)','bigint') as 'Waits',
   waits.evt.value('(@averageWaitTime)','bigint') as 'Avg_Wait_Time',
   waits.evt.value('(@maxWaitTime)','bigint') as 'Max_Wait_Time'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/topWaits/nonPreemptive/byDuration/wait') AS waits(evt)
where component_name like 'query_processing'
go

-- Preemptive waits
select waits.evt.value('(@waitType)','varchar(100)') as 'Wait_Type',
   waits.evt.value('(@waits)','bigint') as 'Waits',
   waits.evt.value('(@averageWaitTime)','bigint') as 'Avg_Wait_Time',
   waits.evt.value('(@maxWaitTime)','bigint') as 'Max_Wait_Time'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/topWaits/preemptive/byDuration/wait') AS waits(evt)
where component_name like 'query_processing'
go

-- CPU intensive requests
select cpureq.evt.value('(@sessionId)','bigint') as 'SessionID',
   cpureq.evt.value('(@command)','varchar(100)') as 'Command',
   cpureq.evt.value('(@cpuUtilization)','bigint') as 'CPU_Utilization',
   cpureq.evt.value('(@cpuTimeMs)','bigint') as 'CPU_Time_ms'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/cpuIntensiveRequests/request') AS cpureq(evt)
where component_name like 'query_processing'
go

-- Blocked Process Report
select blk.evt.query('.') as 'Blocked_Process_Report_XML'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/blockingTasks/blocked-process-report') AS blk(evt)
where component_name like 'query_processing'
go

-- IO
select data.value('(/ioSubsystem/@ioLatchTimeouts)[1]','bigint') as 'Latch_Timeouts',
   data.value('(/ioSubsystem/@totalLongIos)[1]','bigint') as 'Total_Long_IOs'
from SpServerDiagnosticsResult 
where component_name like 'io_subsystem'
go

-- Event information
select xevts.evt.value('(@name)','varchar(100)') as 'xEvent_Name',
   xevts.evt.value('(@package)','varchar(100)') as 'Package',
   xevts.evt.value('(@timestamp)','datetime') as 'xEvent_Time',
   xevts.evt.query('.') as 'Event Data'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/events/session/RingBufferTarget/event') AS xevts(evt)
where component_name like 'events'
go  
``` 
  
## <a name="see-also"></a>Vedere anche  
 [Failover Policy for Failover Cluster Instances](../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  
