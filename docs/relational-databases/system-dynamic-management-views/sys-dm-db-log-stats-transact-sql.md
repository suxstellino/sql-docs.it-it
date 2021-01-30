---
description: sys.dm_db_log_stats (Transact-SQL)
title: sys.dm_db_log_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_log_stats_TSQL
- sys.dm_db_log_stats
- sys.dm_db_log_stats_TSQL
- dm_db_log_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_stats dynamic management function
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88ac57a5ba41afacafe8565225ed9ce7bca55991
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191190"
---
# <a name="sysdm_db_log_stats-transact-sql"></a>sys.dm_db_log_stats (Transact-SQL)   
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Restituisce gli attributi a livello di riepilogo e le informazioni sui file di log delle transazioni dei database. Utilizzare queste informazioni per il monitoraggio e la diagnostica dell'integrità del log delle transazioni.   
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
 sys.dm_db_log_stats ( database_id )
```  
  
## <a name="arguments"></a>Argomenti  

*database_id* | NULL | **Impostazione predefinita**

ID del database. `database_id` è `int`. Gli input validi sono il numero di ID di un database, `NULL` o `DEFAULT` . Il valore predefinito è `NULL`. `NULL` e `DEFAULT` sono valori equivalenti nel contesto del database corrente.  
È possibile specificare la funzione predefinita [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Quando `DB_ID` si utilizza senza specificare un nome di database, il livello di compatibilità del database corrente deve essere 90 o superiore.

  
## <a name="tables-returned"></a>Tabelle restituite  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id    |**int**    |ID database |  
|recovery_model |**nvarchar(60)**   |   Modello di recupero del database. I valori possibili sono: <br /> SEMPLICE<br /> BULK_LOGGED <br /> FULL |  
|log_min_lsn    |**nvarchar(24)**   |   Numero di sequenza iniziale del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) corrente nel log delle transazioni.|  
|log_end_lsn    |**nvarchar(24)**   |   numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) dell'ultimo record del log nel log delle transazioni.|  
|current_vlf_sequence_number    |**bigint** |   Numero di sequenza del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) corrente al momento dell'esecuzione.|  
|current_vlf_size_mb    |**float**  |   Dimensioni correnti del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) in MB.|   
|total_vlf_count    |**bigint** |   Numero totale di [file di log virtuali (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) nel log delle transazioni. |  
|total_log_size_mb  |**float**  |   Dimensioni totali del log delle transazioni in MB. |  
|active_vlf_count   |**bigint** |   Numero totale di [file di log virtuali attivi (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) nel log delle transazioni.|  
|active_log_size_mb |**float**  |   Dimensioni totali del log delle transazioni attive in MB.|  
|log_truncation_holdup_reason   |**nvarchar(60)**   |   Motivo della rapina del troncamento del log. Il valore è uguale a quello  `log_reuse_wait_desc` della colonna di `sys.databases` .  Per una spiegazione più dettagliata di questi valori, vedere [il log delle transazioni](../../relational-databases/logs/the-transaction-log-sql-server.md). <br />I valori possibili sono: <br />NOTHING<br />CHECKPOINT<br />LOG_BACKUP<br />ACTIVE_BACKUP_OR_RESTORE<br />ACTIVE_TRANSACTION<br />DATABASE_MIRRORING<br />REPLICA<br />DATABASE_SNAPSHOT_CREATION<br />LOG_SCAN<br />AVAILABILITY_REPLICA<br />OLDEST_PAGE<br />XTP_CHECKPOINT<br />ALTRO TEMPORANEO |  
|log_backup_time    |**datetime**   |   Ora ultimo backup del log delle transazioni.|   
|log_backup_lsn |**nvarchar(24)**   |   Ultimo numero di sequenza del file di log del backup del log delle transazioni [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|   
|log_since_last_log_backup_mb   |**float**  |   Dimensioni del log in MB dall'ultimo numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)del backup del log delle transazioni.|  
|log_checkpoint_lsn |**nvarchar(24)**   |   Ultimo numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)del checkpoint.|  
|log_since_last_checkpoint_mb   |**float**  |   Dimensioni del log in MB dall'ultimo numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)del checkpoint.|  
|log_recovery_lsn   |**nvarchar(24)**   |   Numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) di recupero del database. Se `log_recovery_lsn` si verifica prima dell'LSN del checkpoint, `log_recovery_lsn` è il numero LSN di transazione attivo meno recente, in caso contrario `log_recovery_lsn` è l'LSN del checkpoint.|  
|log_recovery_size_mb   |**float**  |   Dimensioni del log in MB dal numero di sequenza del log di recupero del log [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|recovery_vlf_count |**bigint** |   Numero totale di [file di log virtuali (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) da ripristinare se è stato eseguito il failover o il riavvio del server. |  


## <a name="remarks"></a>Commenti
Quando `sys.dm_db_log_stats` viene eseguito su un database che partecipa a un gruppo di disponibilità come replica secondaria, viene restituito solo un subset dei campi descritti in precedenza.  Attualmente, solo `database_id` , `recovery_model` e `log_backup_time` verranno restituiti quando vengono eseguiti su un database secondario.   

## <a name="permissions"></a>Autorizzazioni  
Richiede l' `VIEW DATABASE STATE` autorizzazione nel database.   
  
## <a name="examples"></a>Esempi  

### <a name="a-determining-databases-in-a-ssnoversion-instance-with-high-number-of-vlfs"></a>R. Determinazione dei database in un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza con un numero elevato di VLF   
La query seguente restituisce i database con più di 100 VLF nei file di log. Un numero elevato di VLF può influire sul tempo di avvio, ripristino e ripristino del database.

```sql  
SELECT name AS 'Database Name', total_vlf_count AS 'VLF count' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id) 
WHERE total_vlf_count  > 100;
```   

### <a name="b-determining-databases-in-a-ssnoversion-instance-with-transaction-log-backups-older-than-4-hours"></a>B. Determinazione dei database in un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza con backup del log delle transazioni più vecchi di 4 ore   
La query seguente determina gli ultimi tempi di backup del log per i database nell'istanza di.

```sql  
SELECT name AS 'Database Name', log_backup_time AS 'last log backup time' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id); 
```

## <a name="see-also"></a>Vedere anche  
[Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_info & #40; Transact-SQL & #41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
  
