---
description: sys.dm_db_log_info (Transact-SQL)
title: sys.dm_db_log_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_info
- sys.dm_db_log_info_TSQL
- dm_db_log_info
- dm_db_log_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_info dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 171cada53538f8e5b615e6096a169b0976b57532
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236053"
---
# <a name="sysdm_db_log_info-transact-sql"></a>sys.dm_db_log_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Restituisce le informazioni del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) del log delle transazioni. Nota tutti i file di log delle transazioni vengono combinati nell'output della tabella. Ogni riga nell'output rappresenta un VLF nel log delle transazioni e fornisce informazioni rilevanti per tale VLF nel log.

## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_db_log_info ( database_id )  
``` 

## <a name="arguments"></a>Argomenti  
 *database_id* | NULL | PREDEFINITA  
 ID del database. *database_id* è di **tipo int**. Gli input validi sono il numero di ID di un database, NULL o predefinito. Il valore predefinito è NULL. NULL e DEFAULT sono valori equivalenti nel contesto del database corrente.
 
 Specificare NULL per restituire le informazioni VLF del database corrente.

 È possibile specificare la funzione predefinita [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Quando `DB_ID` si utilizza senza specificare un nome di database, il livello di compatibilità del database corrente deve essere 90 o superiore.  

## <a name="table-returned"></a>Tabella restituita  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID del database.|
|file_id|**smallint**|ID file del log delle transazioni.|  
|vlf_begin_offset|**bigint** |Percorso di offset del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) dall'inizio del file di log delle transazioni.|
|vlf_size_mb |**float** |dimensioni del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) in MB, arrotondate a 2 posizioni decimali.|     
|vlf_sequence_number|**bigint** |numero di sequenza del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) nell'ordine creato. Usato per identificare in modo univoco VLF nel file di log.|
|vlf_active|**bit** |Indica se il [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) è in uso o meno. <br />0: VLF non è in uso.<br />1-VLF è attivo.|
|vlf_status|**int** |Stato del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). I valori possibili includono <br />0-VLF è inattivo <br />1-VLF è inizializzato ma non usato <br /> 2: VLF è attivo.|
|vlf_parity|**tinyint** |Parità del [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). Utilizzato internamente per determinare la fine del log all'interno di un VLF.|
|vlf_first_lsn|**nvarchar (48)** |Numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) del primo record del log nel [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|
|vlf_create_lsn|**nvarchar (48)** |Numero di sequenza del file di [log (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) del record di log che ha creato il [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|
|vlf_encryptor_thumbprint|**varbinary(20)**| **Si applica a:** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] <br><br> Mostra l'identificazione digitale del VLF se VLF è crittografato con [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md); in caso contrario, null. |

## <a name="remarks"></a>Commenti
La `sys.dm_db_log_info` funzione a gestione dinamica sostituisce l' `DBCC LOGINFO` istruzione.    
 
## <a name="permissions"></a>Autorizzazioni  
Richiede l' `VIEW DATABASE STATE` autorizzazione nel database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-determing-databases-in-a-sql-server-instance-with-high-number-of-vlfs"></a>R. Definizione di database in un'istanza di SQL Server con un numero elevato di VLF
La query seguente determina i database con più di 100 VLF nei file di log, che possono influire sul tempo di avvio, ripristino e ripristino del database.

```sql
SELECT [name], COUNT(l.database_id) AS 'vlf_count' 
FROM sys.databases s
CROSS APPLY sys.dm_db_log_info(s.database_id) l
GROUP BY [name]
HAVING COUNT(l.database_id) > 100
```

### <a name="b-determing-the-position-of-the-last-vlf-in-transaction-log-before-shrinking-the-log-file"></a>B. Definizione della posizione dell'ultimo `VLF` nel log delle transazioni prima della compattazione del file di log

È possibile utilizzare la query seguente per determinare la posizione dell'ultimo VLF attivo prima di eseguire SHRINKFILE sul log delle transazioni per determinare se il log delle transazioni può essere compattato.

```sql
USE AdventureWorks2016
GO

;WITH cte_vlf AS (
SELECT ROW_NUMBER() OVER(ORDER BY vlf_begin_offset) AS vlfid, DB_NAME(database_id) AS [Database Name], vlf_sequence_number, vlf_active, vlf_begin_offset, vlf_size_mb
    FROM sys.dm_db_log_info(DEFAULT)),
cte_vlf_cnt AS (SELECT [Database Name], COUNT(vlf_sequence_number) AS vlf_count,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 0) AS vlf_count_inactive,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS vlf_count_active,
    (SELECT MIN(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_min_vlf_active,
    (SELECT MIN(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS min_vlf_active,
    (SELECT MAX(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_max_vlf_active,
    (SELECT MAX(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS max_vlf_active
    FROM cte_vlf
    GROUP BY [Database Name])
SELECT [Database Name], vlf_count, min_vlf_active, ordinal_min_vlf_active, max_vlf_active, ordinal_max_vlf_active,
((ordinal_min_vlf_active-1)*100.00/vlf_count) AS free_log_pct_before_active_log,
((ordinal_max_vlf_active-(ordinal_min_vlf_active-1))*100.00/vlf_count) AS active_log_pct,
((vlf_count-ordinal_max_vlf_active)*100.00/vlf_count) AS free_log_pct_after_active_log
FROM cte_vlf_cnt
GO
```

## <a name="see-also"></a>Vedere anche  
[Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)

