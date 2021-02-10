---
description: sys.dm_db_page_info (Transact-SQL)
title: sys.dm_db_page_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_page_info
- sys.dm_db_page_info_TSQL
- dm_db_page_info
- dm_db_page_info_TSQL
- dbcc page
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_page_info dynamic management view
author: bluefooted
ms.author: pamela
manager: amitban
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: df904c938418b6d57bc5aac286e482311de4554a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071425"
---
# <a name="sysdm_db_page_info-transact-sql"></a>sys.dm_db_page_info (Transact-SQL)

[!INCLUDE[tsql-appliesto-ssver15-asdb-xxxx-xxx](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

Restituisce informazioni su una pagina in un database.  La funzione restituisce una riga che contiene le informazioni di intestazione della pagina, tra cui `object_id` , `index_id` e `partition_id` .  Questa funzione sostituisce l'uso di `DBCC PAGE` nella maggior parte dei casi.

> [!NOTE]
> `sys.dm_db_page_info` è attualmente supportato solo in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive.


## <a name="syntax"></a>Sintassi   
```  
sys.dm_db_page_info ( DatabaseId, FileId, PageId, Mode )  
``` 

## <a name="arguments"></a>Argomenti  
*DatabaseID* | NULL | PREDEFINITA     
ID del database. *DatabaseID* è di **smallint**. L'input valido è il numero ID di un database. Il valore predefinito è NULL. Tuttavia, se si invia un valore NULL per questo parametro, verrà generato un errore.
 
*Fileid* | NULL | PREDEFINITA   
ID del file. *Fileid* è di **tipo int**.  Input valido è il numero ID di un file nel database specificato da *DatabaseID*. Il valore predefinito è NULL. Tuttavia, se si invia un valore NULL per questo parametro, verrà generato un errore.

*Pageid* | NULL | PREDEFINITA   
ID della pagina.  *Pageid* è di **tipo int**.  Input valido è il numero ID di una pagina nel file specificato da *fileid*. Il valore predefinito è NULL. Tuttavia, se si invia un valore NULL per questo parametro, verrà generato un errore.

*Modalità* | NULL | PREDEFINITA   
Determina il livello di dettaglio nell'output della funzione. ' LIMITED ' restituirà valori NULL per tutte le colonne Description,' detailed ' compilerà le colonne Description.  Il valore predefinito è' LIMITED '.

## <a name="table-returned"></a>Tabella restituita  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id |INT |ID database |
|file_id |INT |ID file |
|page_id |INT |ID pagina |
|page_header_version |INT |Versione intestazione pagina |
|page_type |INT |Tipo di pagina |
|page_type_desc |nvarchar (64) |Descrizione del tipo di pagina |
|page_type_flag_bits |nvarchar (64) |Digitare i bit del flag nell'intestazione di pagina |
|page_type_flag_bits_desc |nvarchar (64) |Digitare la descrizione bits del flag nell'intestazione di pagina |
|page_flag_bits |nvarchar (64) |Flag bits nell'intestazione di pagina |
|page_flag_bits_desc |nvarchar(256) |Descrizione bits del flag nell'intestazione di pagina |
|page_lsn |nvarchar (64) |Numero di sequenza del file di log/timestamp |
|page_level |INT |Livello della pagina nell'indice (foglia = 0) |
|object_id |INT |ID dell'oggetto proprietario della pagina |
|index_id |INT |ID dell'indice (0 per le pagine di dati dell'heap) |
|partition_id |bigint |ID della partizione |
|alloc_unit_id |bigint |ID dell'unità di allocazione |
|is_encrypted |bit |Bit per indicare se la pagina è crittografata o meno |
|has_checksum |bit |Bit per indicare se la pagina contiene un valore di checksum |
|checksum |INT |Archivia il valore di checksum utilizzato per rilevare il danneggiamento dei dati |
|is_iam_pg |bit |Bit per indicare se la pagina è una pagina IAM  |
|is_mixed_ext |bit |Bit per indicare se allocato in un extent misto |
|has_ghost_records |bit |Bit per indicare se la pagina contiene record fantasma <br> Un record fantasma è uno che è stato contrassegnato per l'eliminazione ma che è ancora stato rimosso.|
|has_version_records |bit |Bit per indicare se la pagina contiene record versione utilizzati per il [recupero accelerato del database](../backup-restore/restore-and-recovery-overview-sql-server.md#adr) |
|pfs_page_id |INT |ID pagina della pagina PFS corrispondente |
|pfs_is_allocated |bit |Bit per indicare se la pagina è contrassegnata come allocata nella pagina PFS corrispondente |
|pfs_alloc_percent |INT |Percentuale di allocazione come indicato dal byte PFS corrispondente |
|pfs_status |nvarchar (64) |Byte PFS |
|pfs_status_desc |nvarchar (64) |Descrizione del byte PFS |
|gam_page_id |INT |ID pagina della pagina GAM corrispondente |
|gam_status |bit |Bit per indicare se allocato in GAM |
|gam_status_desc |nvarchar (64) |Descrizione del bit di stato GAM |
|sgam_page_id |INT |ID pagina della pagina SGAM corrispondente |
|sgam_status |bit |Bit per indicare se allocato in SGAM |
|sgam_status_desc |nvarchar (64) |Descrizione del bit di stato di SGAM |
|diff_map_page_id |INT |ID pagina della pagina della mappa di bit differenziale corrispondente |
|diff_status |bit |Bit per indicare se lo stato diff è stato modificato |
|diff_status_desc |nvarchar (64) |Descrizione del bit di stato diff |
|ml_map_page_id |INT |ID pagina della pagina bitmap di registrazione minima corrispondente |
|ml_status |bit |Bit per indicare se la pagina è con registrazione minima |
|ml_status_desc |nvarchar (64) |Descrizione del bit di stato di registrazione minimo |
|prev_page_file_id |SMALLINT |ID file di pagina precedente |
|prev_page_page_id |INT |ID pagina pagina precedente |
|next_page_file_id |SMALLINT |ID file della pagina successiva |
|next_page_page_id |INT |ID pagina pagina successiva |
|fixed_length |SMALLINT |Lunghezza delle righe a dimensione fissa |
|slot_count |SMALLINT |Numero totale di slot (usati e non usati) <br> Per una pagina di dati, questo numero è equivalente al numero di righe. |
|ghost_rec_count |SMALLINT |Numero di record contrassegnati come Ghost nella pagina <br> Un record fantasma è uno che è stato contrassegnato per l'eliminazione ma che è ancora stato rimosso. |
|free_bytes |SMALLINT |Numero di byte disponibili nella pagina |
|free_data_offset |INT |Offset dello spazio disponibile alla fine dell'area dati |
|reserved_bytes |SMALLINT |Numero di byte liberi riservati da tutte le transazioni (se heap) <br> Numero di righe fantasma (se foglia dell'indice) |
|reserved_bytes_by_xdes_id |SMALLINT |Spazio fornito da m_xdesID per m_reservedCnt <br> Solo a scopo di debug |
|xdes_id |nvarchar (64) |Ultima transazione aggiunta come contributo da m_reserved <br> Solo a scopo di debug |
||||

## <a name="remarks"></a>Commenti
La `sys.dm_db_page_info` funzione a gestione dinamica restituisce informazioni sulla pagina `page_id` , ad esempio,, `file_id` e così via, `index_id` `object_id` presenti in un'intestazione di pagina. Queste informazioni sono utili per la risoluzione dei problemi e il debug di diverse prestazioni (contesa di blocco e Latch) e problemi di danneggiamento.

`sys.dm_db_page_info` può essere usato al posto dell' `DBCC PAGE` istruzione in molti casi, ma restituisce solo le informazioni di intestazione di pagina, non il corpo della pagina. `DBCC PAGE` sarà comunque necessario per i casi d'uso in cui è necessario l'intero contenuto della pagina.

## <a name="using-in-conjunction-with-other-dmvs"></a>Utilizzo in combinazione con altri DMV
Uno dei principali casi d'uso di `sys.dm_db_page_info` consiste nell'aggiungerlo ad altri DMV che espongono informazioni sulle pagine.  Per semplificare il caso d'uso, è stata aggiunta una nuova colonna denominata `page_resource` che espone le informazioni della pagina in un formato esadecimale a 8 byte. Questa colonna è stata aggiunta a `sys.dm_exec_requests` e `sys.sysprocesses` e verrà aggiunta ad altri DMV in futuro, in base alle esigenze.

Una nuova funzione, `sys.fn_PageResCracker` , accetta `page_resource` come input e restituisce una singola riga che contiene `database_id` , `file_id` e `page_id` .  Questa funzione può quindi essere usata per facilitare i join tra `sys.dm_exec_requests` o `sys.sysprocesses` e `sys.dm_db_page_info` .

## <a name="permissions"></a>Autorizzazioni  
Richiede l' `VIEW DATABASE STATE` autorizzazione nel database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-all-the-properties-of-a-page"></a>R. Visualizzazione di tutte le proprietà di una pagina
Con la query seguente viene restituita una riga con tutte le informazioni di pagina per una determinata `database_id` combinazione di, `file_id` , `page_id` con la modalità predefinita (' Limited ')

```sql
SELECT *  
FROM sys.dm_db_page_info (5, 1, 15, DEFAULT)
```

### <a name="b-using-sysdm_db_page_info-with-other-dmvs"></a>B. Uso di sys.dm_db_page_info con altri DMV 

La query seguente restituisce una riga per ogni `wait_resource` esposizione eseguita da `sys.dm_exec_requests` quando la riga contiene un valore non null `page_resource`

```sql
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 'LIMITED') AS page_info
```

## <a name="see-also"></a>Vedere anche  
[Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)     
[sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md)


