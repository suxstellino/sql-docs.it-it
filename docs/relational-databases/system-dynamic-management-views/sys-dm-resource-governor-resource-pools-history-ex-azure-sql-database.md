---
description: sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)
title: sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/05/2021
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor
- sys.resource_governor_TSQL
- resource_governor
- resource_governor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor catalog view
ms.assetid: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current
ms.openlocfilehash: 32082b09a5e583e68f2f06794d80d1d3f21b4e76
ms.sourcegitcommit: 11ca2305a8d7e420daf772eb97861706c9e08e31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97928440"
---
# <a name="sysdm_resource_governor_resource_pools_history_ex-transact-sql"></a>sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

Ogni riga rappresenta uno snapshot periodico delle statistiche del pool di risorse nel database SQL di Azure. Quando il motore di database viene avviato, viene eseguita una snapshot e, a intervalli di pochi secondi, successivamente. L'intervallo tra l'oggetto corrente e lo snapshot precedente può variare e viene fornito nella `duration_ms` colonna. Vengono restituiti gli snapshot più recenti disponibili, fino a 128 snapshot per ogni pool di risorse.

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pool_id**|INT|ID del pool di risorse. Non ammette i valori Null.
|**nome**|sysname|Nome del pool di risorse. Non ammette i valori Null.|
|**snapshot_time**|datetime2|Data/ora dello snapshot delle statistiche del pool di risorse|
|**duration_ms**|INT|Durata tra lo snapshot corrente e quello precedente|
|**statistics_start_time**|datetime2|Ora di reimpostazione delle statistiche per questo pool. Non ammette i valori Null.|
|**active_session_count**|INT|Totale sessioni attive nello snapshot corrente|
|**active_worker_count**|INT|Totale processi di lavoro nello snapshot corrente|
|**delta_cpu_usage_ms**|INT|Utilizzo CPU in millisecondi dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_cpu_usage_preemptive_ms**|INT|Chiamate Win32 di tipo preemptive non regolate dal RG della CPU SQL, dall'ultimo snapshot|
|**used_data_space_kb**|bigint|Spazio totale usato nei database utente associati al pool di utenti|
|**allocated_disk_space_kb**|bigint|Dimensioni totali del file di dati dei database utente nel pool di utenti associato|
|**target_memory_kb**|bigint|Quantità di memoria di destinazione, in kilobyte, che il pool di risorse sta cercando di ottenere. Si basa sulle impostazioni correnti e sullo stato del server. Non ammette i valori Null.|
|**used_memory_kb**|bigint|Quantità di memoria utilizzata, in kilobyte, per il pool di risorse. Non ammette i valori Null.|
|**cache_memory_kb**|bigint|Utilizzo corrente della memoria cache totale in kilobyte. Non ammette i valori Null.|
|**compile_memory_kb**|bigint|Utilizzo corrente della memoria prelevata totale in kilobyte (KB). La maggioranza dell'utilizzo avviene per la compilazione e l'ottimizzazione, ma può includere anche altri utenti della memoria. Non ammette i valori Null.|
|**active_memgrant_count**|bigint|Il conteggio corrente delle concessioni di memoria. Non ammette i valori Null.|
|**active_memgrant_kb**|bigint|La somma, in kilobyte (KB), delle concessioni correnti di memoria. Non ammette i valori Null.|
|**used_memgrant_kb**|bigint|Il totale corrente della memoria usata (prelevata) dalle concessioni di memoria. Non ammette i valori Null.|
|**delta_memgrant_timeout_count**|INT|numero di timeout delle concessioni di memoria nel pool di risorse in questo periodo. Non ammette i valori Null.|
|**delta_memgrant_waiter_count**|INT|Il conteggio delle query attualmente in sospeso nelle concessioni di memoria. Non ammette i valori Null.|
|**delta_out_of_memory_count**|INT|Numero di allocazioni di memoria non riuscite nel pool dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_read_io_queued**|INT|Il totale di letture IOs accodate dopo l'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**delta_read_io_issued**|INT|Totale dei dispositivi IOs letti emessi dall'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**delta_read_io_completed**|INT|Totale IOs di lettura completati dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_read_io_throttled**|INT|Il totale di letture IOs limitate da snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**delta_read_bytes**|bigint|Numero totale di byte letti dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_read_io_stall_ms**|INT|Tempo totale (in millisecondi) tra l'arrivo e il completamento dell'i/o di lettura dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_read_io_stall_queued_ms**|INT|Tempo totale (in millisecondi) tra l'arrivo di i/o di lettura e il problema dall'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O. Delta_read_io_stall_queued_ms diverso da zero significa che i/o sono interessati da RG.|
|**delta_write_io_queued**|INT|Totale di scritture IOs accodate dall'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**delta_write_io_issued**|INT|Il totale degli i/o di scrittura emessi dall'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**delta_write_io_completed**|INT|Totale di operazioni di scrittura IOs completate dall'ultimo snapshot. Non ammette i valori Null|
|**delta_write_io_throttled**|INT|Il totale degli i/o di scrittura limitati dall'ultimo snapshot. Non ammette i valori Null|
|**delta_write_bytes**|bigint|Numero totale di byte scritti dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_write_io_stall_ms**|INT|Tempo totale (in millisecondi) tra l'arrivo e il completamento i/o di scrittura dall'ultimo snapshot. Non ammette i valori Null.|
|**delta_write_io_stall_queued_ms**|INT|Tempo totale (in millisecondi) tra l'arrivo di i/o di scrittura e il problema dall'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**delta_io_issue_delay_ms**|INT|Tempo totale (in millisecondi) tra il problema pianificato e il problema effettivo dell'i/o dopo l'ultimo snapshot. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**max_iops_per_volume**|INT|Numero massimo di i/o al secondo (IOPS) per ogni impostazione del volume del disco per questo pool. Ammette i valori Null. Null se il pool di risorse non è governato per l'I/O.|
|**max_memory_kb**|bigint|Quantità massima di memoria, in kilobyte, disponibile per il pool di risorse. Si basa sulle impostazioni correnti e sullo stato del server. Non ammette i valori Null.
|**max_log_rate_kb**|bigint|Frequenza massima di log (kilo-byte al sec) al livello del pool di risorse.|
|**max_data_space_kb**|bigint|Impostazione limite massimo di archiviazione del pool elastico per questo pool elastico, espressa in kilobyte.|
|**max_session**|INT|Limite della sessione per il pool|
|**max_worker**|INT|Limite di lavoro per il pool|
|**min_cpu_percent**|INT|Configurazione corrente della larghezza di banda media garantita della CPU per tutte le richieste nel pool di risorse, in caso di contesa di CPU. Non ammette i valori Null.|
|**max_cpu_percent**|INT|Configurazione corrente per la larghezza di banda media massima della CPU concessa per tutte le richieste nel pool di risorse, in caso di contesa di CPU. Non ammette i valori Null.|
|**cap_cpu_percent**|INT|Limite di utilizzo massimo della larghezza di banda della CPU concesso per tutte le richieste nel pool di risorse. Limita il livello massimo della larghezza di banda della CPU al livello specificato. L'intervallo consentito per il valore è compreso tra 1 e 100. Non ammette i valori Null.|
|**min_vcores**|Decimal (5, 2)|Configurazione corrente della larghezza di banda media garantita della CPU per tutte le richieste nel pool di risorse, in caso di contesa di CPU.  In unità di vcore|
|**max_vcores**|Decimal (5, 2)|Configurazione corrente per la larghezza di banda media massima della CPU concessa per tutte le richieste nel pool di risorse, in caso di contesa di CPU.  In unità di vcore|
|**cap_vcores**|Decimal (5, 2)|Limite di utilizzo massimo della larghezza di banda della CPU concesso per tutte le richieste nel pool di risorse.  In unità in vcore|
|**instance_cpu_count**|INT|Numero di CPU configurate per l'istanza|
|**instance_cpu_percent**|Decimal (5, 2)|Percentuale CPU configurata per l'istanza|
|**instance_vcores**|Decimal (5, 2)|Numero di Vcore configurati per l'istanza|
|**delta_log_bytes_used**|Decimal (5, 2)|Totale generazione log (in byte) a livello di pool dall'ultimo snapshot|
|**avg_login_rate_percent**|Decimal (5, 2)|Numero di accessi dall'ultimo snapshot rispetto al limite di accesso|
|**delta_vcores_used**|Decimal (5, 2)|Utilizzo di calcolo nel conteggio di Vcore dall'ultimo snapshot.|
|**cap_vcores_used_percent**|Decimal (5, 2)|Utilizzo medio del calcolo espresso in percentuale del limite del pool.|
|**instance_vcores_used_percent**|Decimal (5, 2)|Utilizzo medio del calcolo in percentuale del limite dell'istanza di SQL.|
|**avg_data_io_percent**|Decimal (5, 2)|Utilizzo I/O medio espresso in percentuale sulla base del limite del pool.|
|**avg_log_write_percent**|Decimal (5, 2)|Utilizzo delle risorse di scrittura medio espresso in percentuale del limite del pool.|
|**avg_storage_percent**|Decimal (5, 2)|Utilizzo di spazio di archiviazione medio espresso in percentuale del limite di archiviazione del pool.|
|**avg_allocated_storage_percent**|Decimal (5, 2)|Percentuale di spazio dati allocato da tutti i database nel pool elastico. Questo è il rapporto tra lo spazio dati allocato e le dimensioni massime dei dati per il pool elastico. Per ulteriori informazioni, vedere la pagina relativa alla gestione dello spazio file nel database SQL|
|**max_worker_percent**|Decimal (5, 2)|Numero massimo di ruoli di lavoro simultanei (richieste) espresso in percentuale sulla base del limite del pool.|
|**max_session_percent**|Decimal (5, 2)|Numero massimo di sessioni simultanee espresso in percentuale sulla base del limite del pool.|
|||

## <a name="permissions"></a>Autorizzazioni

Questa vista richiede l'autorizzazione VIEW SERVER STATE.

## <a name="remarks"></a>Commenti

Gli utenti possono accedere a questa vista a gestione dinamica per monitorare il consumo di risorse quasi in tempo reale per il pool di carico di lavoro degli utenti e per i pool interni di sistema dell'istanza del database SQL di Azure

> [!IMPORTANT]
> La maggior parte dei dati esposti da questa DMV è destinata al consumo interno ed è soggetta a modifiche.

## <a name="examples"></a>Esempi

Nell'esempio seguente vengono restituiti i dati di frequenza massima dei log e il consumo a ogni snapshot da parte del pool di utenti  

```sql
select snapshot_time, name, max_log_rate_kb, delta_log_bytes_used from sys.dm_resource_governor_resource_pools_history_ex where name like 'UserPool%' order by snapshot_time desc
```

Nell'esempio seguente vengono restituite informazioni simili sys.elastic_pool_resource_stats senza la necessità di connettersi al database master logico

```sql
select snapshot_time, name, cap_vcores_used_percent,
  avg_data_io_percent,  
  avg_log_write_percent,
  avg_storage_percent,
  avg_allocated_storage_percent,
  max_data_space_kb,
  max_worker_percent,
  max_session_percent
    from sys.dm_resource_governor_resource_pools_history_ex where name like 'UserPool%' order by snapshot_time desc
```

## <a name="see-also"></a>Vedere anche

- [Governance della frequenza di log delle traduzioni](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Limiti delle risorse di DTU per pool elastico](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- [Limiti delle risorse di vCore per pool elastico](/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)