---
title: sys.dm_db_xtp_checkpoint_files (Transact-SQL) | Microsoft Docs
description: Visualizza informazioni sui file dei checkpoint, incluse le dimensioni del file, il percorso fisico e l'ID transazione. Informazioni su come questa visualizzazione differisce per le versioni di SQL Server.
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bed4fa18b01a295815c07f6a0f37d4bc0b942c16
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092924"
---
# <a name="sysdm_db_xtp_checkpoint_files-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Visualizza informazioni sui file del checkpoint, incluse le dimensioni del file, la posizione fisica e l'ID transazione.  
  
> **Nota:** Per il checkpoint corrente che non è stato chiuso, la colonna di stato di s `ys.dm_db_xtp_checkpoint_files` sarà in costruzione per i nuovi file. Un checkpoint si chiude automaticamente quando è disponibile una crescita sufficiente del log delle transazioni dall'ultimo checkpoint o se si esegue il `CHECKPOINT` comando ([Checkpoint &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)).  
  
 Un filegroup ottimizzato per la memoria utilizza internamente i file di sola aggiunta per archiviare le righe inserite ed eliminate per le tabelle in memoria. Sono disponibili due tipi di file. Un file di dati contiene righe inserite mentre un file differenziale contiene riferimenti a righe eliminate. 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] è sostanzialmente diverso dalle versioni più recenti e viene descritto più in basso nell'argomento all' [SQL Server 2014](#bkmk_2014).  
  
 Per ulteriori informazioni, vedere [creazione e gestione dell'archiviazione per oggetti Memory-Optimized](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
##  <a name="sssql15-and-later"></a><a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e versioni successive  
 Nella tabella seguente vengono descritte le colonne di `sys.dm_db_xtp_checkpoint_files` , a partire da **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]** .  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|container_id|**int**|ID del contenitore (rappresentato come file con tipo FILESTREAM in sys.database_files) di cui fa parte il file di dati o il file differenziale. Join con file_id in [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID del contenitore, di cui fa parte la radice, i dati o il file differenziale. Join con file_guid nella tabella sys.database_files.|  
|checkpoint_file_id|**uniqueidentifier**|GUID del file del checkpoint.|  
|relative_file_path|**nvarchar(256)**|Percorso del file relativo al contenitore a cui è stato eseguito il mapping.|  
|file_type|**smallint**|-1 per la versione gratuita<br /><br /> 0 per il file di dati.<br /><br /> 1 per il file differenziale.<br /><br /> 2 per file radice<br /><br /> 3 per file di dati di grandi dimensioni|  
|file_type_desc|**nvarchar(60)**|GRATUITO: tutti i file mantenuti disponibili sono disponibili per l'allocazione. Il numero di file disponibili può variare a seconda delle esigenze previste dal sistema. La dimensione massima è 1 GB.<br /><br /> I file di dati contengono righe inserite in tabelle ottimizzate per la memoria.<br /><br /> I file DELTA Delta contengono riferimenti a righe nei file di dati che sono stati eliminati.<br /><br /> I file radice radice contengono metadati di sistema per oggetti con ottimizzazione per la memoria e compilati in modo nativo.<br /><br /> I file di dati di grandi dimensioni contengono i valori inseriti nelle colonne (n) varchar (max) e varbinary (max), oltre ai segmenti di colonna che fanno parte di indici columnstore nelle tabelle ottimizzate per la memoria.|  
|internal_storage_slot|**int**|Indice del file nella matrice di archiviazione interna. NULL per la radice o per lo stato diverso da 1.|  
|checkpoint_pair_file_id|**uniqueidentifier**|File differenziale o di dati corrispondente. NULL per la radice.|  
|file_size_in_bytes|**bigint**|Dimensioni del file sul disco.|  
|file_size_used_in_bytes|**bigint**|Per le coppie di file del checkpoint ancora da popolare, questa colonna verrà aggiornata dopo il checkpoint successivo.|  
|logical_row_count|**bigint**|Per i dati, numero di righe inserite.<br /><br /> Per Delta, numero di righe eliminate dopo l'accounting per drop table.<br /><br /> Per root, NULL.|  
|state|**smallint**|0-PRECREATED<br /><br /> 1-IN COSTRUZIONE<br /><br /> 2 - ACTIVE<br /><br /> 3-DESTINAZIONE UNIONE<br /><br /> 8-IN ATTESA DEL TRONCAMENTO DEL LOG|  
|state_desc|**nvarchar(60)**|Precreated: un numero di file di checkpoint è preallocato per ridurre o eliminare le attese di allocazione di nuovi file durante l'esecuzione delle transazioni. Questi file precreati possono variare di dimensione, a seconda delle esigenze stimate del carico di lavoro, ma non contengono dati. Si tratta di un sovraccarico di archiviazione nei database con un filegroup MEMORY_OPTIMIZED_DATA.<br /><br /> IN costruzione: questi file del checkpoint sono in fase di costruzione, ovvero vengono popolati in base ai record di log generati dal database e non fanno ancora parte di un checkpoint.<br /><br /> ATTIVO: contengono le righe inserite o eliminate dai checkpoint chiusi precedenti. Contengono il contenuto delle tabelle lette dall'area in memoria prima di applicare la parte attiva del log delle transazioni al riavvio del database. Si prevede che le dimensioni di questi file del checkpoint siano approssimativamente 2x delle dimensioni in memoria delle tabelle ottimizzate per la memoria, presupponendo che l'operazione di Unione sia in grado di mantenere il carico di lavoro transazionale.<br /><br /> DESTINAZIONE Unione-la destinazione delle operazioni di merge: questi file del checkpoint archiviano le righe di dati consolidate dai file di origine identificati dai criteri di Unione. Una volta installata l'operazione di unione, lo stato di MERGE TARGET diventa ACTIVE.<br /><br /> IN attesa del TRONCAmento del LOG: una volta installato il merge e la destinazione di Unione CFP fa parte del checkpoint durevole, i file del checkpoint di origine Unione passano a questo stato. I file in questo stato sono necessari per la correttezza operativa del database con tabella ottimizzata per la memoria.  Ad esempio, per recuperare da un checkpoint durevole tornando indietro nel tempo.|  
|lower_bound_tsn|**bigint**|Limite inferiore della transazione nel file; null se lo stato non è in (1, 3).|  
|upper_bound_tsn|**bigint**|Limite superiore della transazione nel file; null se lo stato non è in (1, 3).|  
|begin_checkpoint_id|**bigint**|ID del checkpoint iniziale.|  
|end_checkpoint_id|**bigint**|ID del checkpoint finale.|  
|last_updated_checkpoint_id|**bigint**|ID dell'ultimo checkpoint che ha aggiornato il file.|  
|encryption_status|**smallint**|0, 1, 2|  
|encryption_status_desc|**nvarchar(60)**|0 => NON CRITTOGRAFATO<br /><br /> 1 => CRITTOGRAFATO CON LA CHIAVE 1<br /><br /> 2 => CRITTOGRAFATO CON LA CHIAVE 2. Valido solo per i file attivi.|  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 Nella tabella seguente vengono descritte le colonne per `sys.dm_db_xtp_checkpoint_files` **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|container_id|**int**|ID del contenitore (rappresentato come file con tipo FILESTREAM in sys.database_files) di cui fa parte il file di dati o il file differenziale. Join con file_id in [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID del contenitore di cui fa parte il file di dati o il file differenziale.|  
|checkpoint_file_id|**GUID**|ID del file di dati o differenziale.|  
|relative_file_path|**nvarchar(256)**|Percorso del file di dati o differenziale, relativo al percorso del contenitore.|  
|file_type|**tinyint**|0 per il file di dati.<br /><br /> 1 per il file differenziale.<br /><br /> NULL se la colonna contenente gli stati è impostata su 7.|  
|file_type_desc|**nvarchar(60)**|Tipo di file: DATA_FILE, DELTA_FILE o NULL se la colonna di stato è impostata su 7.|  
|internal_storage_slot|**int**|Indice del file nella matrice di archiviazione interna. NULL se la colonna contenente gli stati è impostata su 2 o 3.|  
|checkpoint_pair_file_id|**uniqueidentifier**|File di dati o differenziale corrispondente.|  
|file_size_in_bytes|**bigint**|Dimensioni del file in uso. NULL se la colonna contenente gli stati è impostata su 5, 6 o 7.|  
|file_size_used_in_bytes|**bigint**|Dimensioni utilizzate del file in uso. NULL se la colonna contenente gli stati è impostata su 5, 6 o 7.<br /><br /> Per le coppie di file del checkpoint ancora da popolare, questa colonna verrà aggiornata dopo il checkpoint successivo.|  
|inserted_row_count|**bigint**|Numero di righe del file di dati.|  
|deleted_row_count|**bigint**|Numero di righe eliminate del file differenziale.|  
|drop_table_deleted_row_count|**bigint**|Numero di righe nei file di dati interessati dall'eliminazione di una tabella. Si applica ai file di dati quando la colonna contenente gli stati è uguale a 1.<br /><br /> Mostra i conteggi delle righe eliminate dalle tabelle eliminate. Le statistiche drop_table_deleted_row_count vengono compilate dopo il completamento del Garbage Collection in memoria delle righe delle tabelle eliminate e l'esecuzione di un checkpoint. Se si riavvia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prima che le statistiche delle tabelle eliminate vengono riflesse nella colonna, le statistiche verranno aggiornate durante il recupero. Il processo di recupero non carica le righe delle tabelle eliminate. Le statistiche delle tabelle eliminate vengono compilate durante la fase di caricamento e riportate nella colonna al termine del recupero.|  
|state|**int**|0-PRECREATED<br /><br /> 1-IN COSTRUZIONE<br /><br /> 2 - ACTIVE<br /><br /> 3-DESTINAZIONE UNIONE<br /><br /> 4-ORIGINE UNITA<br /><br /> 5-OBBLIGATORIO PER IL BACKUP/HA<br /><br /> 6-IN TRANSIZIONE ALLA RIMOZIONE DEFINITIVA<br /><br /> 7-RIMOZIONE DEFINITIVA|  
|state_desc|**nvarchar(60)**|Precreated: un piccolo set di coppie di file di dati e differenziali, noto anche come coppie di file di checkpoint (CFPs), viene mantenuto pre-allocato per ridurre o eliminare le attese di allocazione di nuovi file durante l'esecuzione delle transazioni. Hanno dimensioni intere, con file di dati di 128 MB e file differenziali di 8 MB, ma non contengono dati. Il numero di coppie di file di checkpoint è calcolato in base al numero di processori logici o utilità di pianificazione (uno per core, senza limiti) con un minimo di 8. Si tratta di un overhead di archiviazione fisso nei database con tabelle ottimizzate per la memoria.<br /><br /> IN costruzione: set di CFPs che archivia le righe di dati appena inserite ed eventualmente eliminate dall'ultimo checkpoint.<br /><br /> ACTIVE: contengono le righe inserite ed eliminate dai precedenti checkpoint chiusi. Queste coppie di file di checkpoint contengono tutte le righe inserite ed eliminate richieste prima dell'applicazione della parte attiva del log delle transazioni al riavvio del database. Le dimensioni di queste coppie di file di checkpoint saranno all'incirca il doppio delle dimensioni in memoria delle tabelle ottimizzate per la memoria, supponendo che l'operazione di unione sia corrente con il carico di lavoro transazionale.<br /><br /> DESTINAZIONE di MERGE: la PCP archivia le righe di dati consolidate da CFP (s) identificate dai criteri di Unione. Una volta installata l'operazione di unione, lo stato di MERGE TARGET diventa ACTIVE.<br /><br /> ORIGINE unita: una volta installata l'operazione di Unione, i CFPs di origine vengono contrassegnati come origini UNITe. Si noti che l'analizzatore dei criteri di unione può identificare più operazioni di unione ma una coppia di file di checkpoint può partecipare solo a un'operazione di unione.<br /><br /> OBBLIGATORIO per il BACKUP/HA-una volta installato il merge e la destinazione di Unione CFP fa parte del checkpoint durevole, l'origine di merge CFPs la transizione a questo stato. Le coppie di file di checkpoint in questo stato sono necessarie per la correttezza operativa del database in cui sia inclusa una tabella ottimizzata per la memoria.  Ad esempio, per recuperare da un checkpoint durevole tornando indietro nel tempo. Una coppia di file di checkpoint può essere contrassegnata per il processo di Garbage Collection quando il punto di troncamento del log va oltre l'intervallo di transazioni.<br /><br /> IN transizione alla rimozione definitiva: questi CFPs non sono necessari per il motore OLTP In-Memory e possono essere sottoposti a Garbage Collection. Questo stato indica che le coppie di file di checkpoint sono in attesa del thread in background per passare allo stato successivo, ovvero allo stato TOMBSTONE.<br /><br /> Rimozione definitiva: questi CFPs sono in attesa di essere sottoposti a Garbage Collection dal Garbage Collector FILESTREAM. ([sp_filestream_force_garbage_collection &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md))|  
|lower_bound_tsn|**bigint**|Limite inferiore delle transazioni incluse nel file. Null se la colonna contenente gli stati è diversa da 2, 3 o 4.|  
|upper_bound_tsn|**bigint**|Limite superiore delle transazioni incluse nel file. Null se la colonna contenente gli stati è diversa da 2, 3 o 4.|  
|last_backup_page_count|**int**|Conteggio delle pagine logiche determinato nell'ultimo backup. Si applica quando la colonna contenente gli stati è impostata su 2, 3, 4 o 5. NULL se il conteggio delle pagine non è noto.|  
|delta_watermark_tsn|**int**|Transazione dell'ultimo checkpoint che ha scritto in questo file differenziale. Si tratta della filigrana per il file differenziale.|  
|last_checkpoint_recovery_lsn|**nvarchar (23)**|Numero di sequenza del file di log (LSN) di recupero dell'ultimo checkpoint per cui è ancora richiesto il file.|  
|tombstone_operation_lsn|**nvarchar (23)**|Il file verrà eliminato una volta che tombstone_operation_lsn non è più sincronizzato con l'LSN di troncamento del log.|  
|logical_deletion_log_block_id|**bigint**|Si applica solo alla stato 5.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW DATABASE STATE` per il server.  
  
## <a name="use-cases"></a>Casi d'uso  
 È possibile stimare lo spazio di archiviazione usato da OLTP In-Memory come segue:  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
Per visualizzare una suddivisione dell'utilizzo dello spazio di archiviazione in base allo stato e al tipo di file, eseguire la query seguente:
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
