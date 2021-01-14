---
title: sys.dm_db_xtp_checkpoint_stats (Transact-SQL) | Microsoft Docs
description: Restituisce le statistiche relative alle operazioni checkpoint OLTP in memoria del database corrente. Informazioni su come questa visualizzazione differisce per le versioni di SQL Server.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2fa75ed45028187c4855aaf80f25f9a30c6000c7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171453"
---
# <a name="sysdm_db_xtp_checkpoint_stats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Restituisce le statistiche relative alle operazioni checkpoint OLTP in memoria del database corrente. Se il database non include oggetti OLTP in memoria, viene restituito un set di risultati vuoto.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
```  
USE In_Memory_db_name
SELECT * FROM sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] è sostanzialmente diverso dalle versioni più recenti e viene descritto più in basso nell'argomento all' [SQL Server 2014](#bkmk_2014).**
  
## <a name="sssql15-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive  
 Nella tabella seguente vengono descritte le colonne in `sys.dm_db_xtp_checkpoint_stats` , a partire da **[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]** .  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|Ultimo LSN visualizzato dal controller.|  
|end_of_log_lsn|**numerico (38)**|LSN della fine del log.|  
|bytes_to_end_of_log|**bigint**|Byte di log non elaborati dal controller, corrispondenti ai byte compresi tra `last_lsn_processed` e `end_of_log_lsn` .|  
|log_consumption_rate|**bigint**|Frequenza di utilizzo del log delle transazioni da parte del controller (in KB al secondo).|  
|active_scan_time_in_ms|**bigint**|Tempo impiegato dal controller per l'analisi attiva del log delle transazioni.|  
|total_wait_time_in_ms|**bigint**|Tempo di attesa cumulativo per il controller durante la mancata analisi del log.|  
|waits_for_io|**bigint**|Numero di attese per l'i/o di log incorse dal thread del controller.|  
|io_wait_time_in_ms|**bigint**|Tempo cumulativo trascorso in attesa del log IO dal thread del controller.|  
|waits_for_new_log_count|**bigint**|Numero di attese subite dal thread del controller per la generazione di un nuovo log.|  
|new_log_wait_time_in_ms|**bigint**|Tempo cumulativo trascorso in attesa di un nuovo log da parte del thread del controller.|  
|idle_attempts_count|**bigint**|Numero di volte in cui il controller ha avuto una transizione a uno stato di inattività.|  
|tx_segments_dispatched|**bigint**|Numero di segmenti visualizzati dal controller e inviati ai serializzatori. Segment è una parte contigua del log che costituisce un'unità di serializzazione. È attualmente dimensionato su 1 MB, ma può cambiare in futuro.|  
|segment_bytes_dispatched|**bigint**|Numero totale di byte di byte inviati dal controller ai serializzatori, dal riavvio del database.|  
|bytes_serialized|**bigint**|Numero totale di byte serializzati dopo il riavvio del database.|  
|serializer_user_time_in_ms|**bigint**|Tempo impiegato dai serializzatori in modalità utente.|  
|serializer_kernel_time_in_ms|**bigint**|Tempo impiegato dai serializzatori in modalità kernel.|  
|xtp_log_bytes_consumed|**bigint**|Numero totale di byte di log utilizzati dopo il riavvio del database.|  
|checkpoints_closed|**bigint**|Numero di checkpoint chiusi dopo il riavvio del database.|  
|last_closed_checkpoint_ts|**bigint**|Timestamp dell'ultimo checkpoint chiuso.|  
|hardened_recovery_lsn|**numerico (38)**|Il recupero verrà avviato da questo LSN.|  
|hardened_root_file_guid|**uniqueidentifier**|GUID del file radice che è stato finalizzato in seguito all'ultimo checkpoint completato.|  
|hardened_root_file_watermark|**bigint**|**Solo interno**. Fino a che punto è possibile leggere il file radice fino a (si tratta di un tipo solo internamente pertinente, denominato del BSN).|  
|hardened_truncation_lsn|**numerico (38)**|LSN del punto di troncamento.|  
|log_bytes_since_last_close|**bigint**|Byte dall'ultima chiusura alla fine del log corrente.|  
|time_since_last_close_in_ms|**bigint**|Tempo trascorso dall'ultima chiusura del checkpoint.|  
|current_checkpoint_id|**bigint**|Attualmente vengono assegnati nuovi segmenti a questo checkpoint. Il sistema di checkpoint è una pipeline. Il checkpoint corrente è quello a cui vengono assegnati i segmenti del log. Una volta raggiunto il limite, il checkpoint viene rilasciato dal controller e ne viene creato uno nuovo come corrente.|  
|current_checkpoint_segment_count|**bigint**|Numero di segmenti nel checkpoint corrente.|  
|recovery_lsn_candidate|**bigint**|**Solo internamente**. Candidato da prelevare come RecoveryLSN quando current_checkpoint_id si chiude.|  
|outstanding_checkpoint_count|**bigint**|Numero di checkpoint in attesa di chiusura della pipeline.|  
|closing_checkpoint_id|**bigint**|ID del checkpoint di chiusura.<br /><br /> I serializzatori operano in parallelo, quindi, una volta completati, il checkpoint è un candidato per essere chiuso dal thread di chiusura. Il thread di chiusura può tuttavia chiudersi solo uno alla volta e deve essere in ordine, quindi il Checkpoint di chiusura è quello in cui il thread di chiusura sta lavorando.|  
|recovery_checkpoint_id|**bigint**|ID del checkpoint da utilizzare nel ripristino.|  
|recovery_checkpoint_ts|**bigint**|Timestamp del checkpoint di ripristino.|  
|bootstrap_recovery_lsn|**numerico (38)**|LSN di recupero per il bootstrap.|  
|bootstrap_root_file_guid|**uniqueidentifier**|GUID del file radice per il bootstrap.|  
|internal_error_code|**bigint**|Errore visualizzato da uno dei thread controller, serializer, Close e merge.|
|bytes_of_large_data_serialized|**bigint**|Quantità di dati serializzati. |  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 Nella tabella seguente vengono descritte le colonne di `sys.dm_db_xtp_checkpoint_stats` per **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|Numero di byte di log tra il numero di sequenza del file di log (LSN) corrente e la fine del log del thread.|  
|total_log_blocks_processed|**bigint**|Numero complessivo di blocchi di log elaborati dall'avvio del server.|  
|total_log_records_processed|**bigint**|Numero complessivo di record di log elaborati dall'avvio del server.|  
|xtp_log_records_processed|**bigint**|Numero totale di record di log OLTP in memoria elaborati dall'avvio del server.|  
|total_wait_time_in_ms|**bigint**|Tempo di attesa cumulativo in ms.|  
|waits_for_io|**bigint**|Numero di attese per le operazioni di I/O sui log.|  
|io_wait_time_in_ms|**bigint**|Tempo cumulativo trascorso in attesa delle operazioni di I/O sui log.|  
|waits_for_new_log|**bigint**|Numero di attese per la generazione del nuovo log.|  
|new_log_wait_time_in_ms|**bigint**|Tempo cumulativo trascorso in attesa del nuovo log.|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|Quantità di log generato dall'ultimo checkpoint OLTP in memoria.|  
|ms_since_last_checkpoint|**bigint**|Quantità di tempo, in millisecondi, dall'ultimo checkpoint OLTP in memoria.|  
|checkpoint_lsn|**numerico (38)**|Numero di sequenza del file di log (LSN) di recupero associato all'ultimo checkpoint completato OLTP in memoria.|  
|current_lsn|**numerico (38)**|LSN del record del log attualmente in elaborazione.|  
|end_of_log_lsn|**numerico (38)**|LSN di fine del log.|  
|task_address|**varbinary (8)**|Indirizzo di SOS_Task. Join a sys.dm_os_tasks per ottenere ulteriori informazioni.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW DATABASE STATE` per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
