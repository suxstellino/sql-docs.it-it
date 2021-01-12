---
description: sys.database_files (Transact-SQL)
title: sys.database_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_files
- sys.database_files_TSQL
- database_files
- database_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_files catalog view
ms.assetid: 0f5b0aac-c17d-4e99-b8f7-d04efc9edf44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 603b9d6fd9a2f15a4592a251dd2b9df6ddb44bfa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093234"
---
# <a name="sysdatabase_files-transact-sql"></a>sys.database_files (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni file di un database archiviata nel database stesso. Si tratta di una vista specifica per ogni database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**file_id**|**int**|ID del file all'interno del database.|  
|**file_guid**|**uniqueidentifier**|GUID del file.<br /><br /> NULL = il database è stato aggiornato da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (valido per SQL Server 2005 e versioni precedenti).|  
|**type**|**tinyint**|Tipo di file:<br/><br /> 0 = righe<br /><br/> 1 = Log<br/><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = full-text|  
|**type_desc**|**nvarchar(60)**|Descrizione del tipo di file:<br /><br /> ROWS <br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT|  
|**data_space_id**|**int**|Il valore può essere uguale o maggiore di 0. Un valore uguale a 0 rappresenta il file di log del database, mentre un valore maggiore di 0 rappresenta l'ID del filegroup in cui è archiviato il file di dati.|  
|**nome**|**sysname**|Nome logico del file nel database.|  
|**physical_name**|**nvarchar(260)**|Nome del file del sistema operativo. Se il database è ospitato da una [replica secondaria leggibile](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)AlwaysOn, **physical_name** indica il percorso del file del database di replica primaria. Per il percorso file corretto di un database secondario leggibile, eseguire una query [sys.sysaltfiles](../../relational-databases/system-compatibility-views/sys-sysaltfiles-transact-sql.md).|  
|**state**|**tinyint**|Stato del file:<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|**state_desc**|**nvarchar(60)**|Descrizione dello stato del file:<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Per altre informazioni, vedere [Stati del file](../../relational-databases/databases/file-states.md).|  
|**size**|**int**|Dimensioni del file in pagine da 8 KB.<br /><br /> 0 = Non applicabile<br /><br /> Per uno snapshot del database, il valore size corrisponde allo spazio massimo utilizzabile dallo snapshot per il file.<br /><br /> Per i contenitori del filegroup FILESTREAM, le dimensioni riflettono le dimensioni correnti utilizzate del contenitore.|  
|**max_size**|**int**|Dimensioni massime del file espresse in pagine da 8 KB.<br /><br /> 0 = Non è consentito alcun aumento.<br /><br /> -1 = La dimensione del file aumenterà finché il disco è pieno.<br /><br /> 268435456 = La dimensione del file di log aumenterà fino al valore massimo di 2 TB.<br /><br /> Per i contenitori del filegroup FILESTREAM, max_size riflette la dimensione massima del contenitore.<br /><br /> Si noti che i database aggiornati con dimensioni illimitate del file di log segnaleranno-1 per le dimensioni massime del file di log.|  
|**crescita**|**int**|0 = La dimensione del file è fissa e non aumenterà.<br /><br /> >0 = il file aumenterà automaticamente.<br /><br /> Se is_percent_growth = 0, il valore dell'aumento di dimensioni è espresso in unità di pagine da 8 KB, con arrotondamento ai 64 KB successivi.<br /><br /> Se is_percent_growth = 1, il valore dell'aumento di dimensioni è espresso come percentuale (numero intero).|  
|**is_media_read_only**|**bit**|1 = Il file si trova in un supporto con accesso in sola lettura.<br /><br /> 0 = Il file è memorizzato in un supporto di lettura/scrittura.|  
|**is_read_only**|**bit**|1 = Il file è contrassegnato per l'accesso in sola lettura.<br /><br /> 0 = Il file è contrassegnato in lettura/scrittura.|  
|**is_sparse**|**bit**|1 = il file è di tipo sparse.<br /><br /> 0 = il file non è di tipo sparse.<br /><br /> Per altre informazioni, vedere [Visualizzare le dimensioni del file sparse di uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|**is_percent_growth**|**bit**|1 = L'aumento del file è una percentuale.<br /><br /> 0 = Dimensione dell'aumento assoluto in pagine.|  
|**is_name_reserved**|**bit**|1 = Il nome del file eliminato (name o physical_name) può essere riutilizzato solo dopo il successivo backup del log. Quando si eliminano i file da un database, i nomi logici rimangono in stato riservato fino al successivo backup del log. Questa colonna è rilevante solo nel modello di recupero con registrazione completa e nel modello di recupero con registrazione minima delle operazioni bulk.|  
|**create_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) in corrispondenza del quale il file è stato creato.|  
|**drop_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) in corrispondenza del quale il file è stato eliminato.<br /><br /> 0 = Il nome del file non è disponibile per il riutilizzo.|  
|**read_only_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) in corrispondenza del quale la modalità del filegroup contenente il file è passata da lettura/scrittura a sola lettura (la modifica più recente).|  
|**read_write_lsn**|**numeric(25,0)**|Numero di sequenza del file di log in corrispondenza del quale la modalità del filegroup contenente il file è passata da sola lettura a lettura/scrittura (la modifica più recente).|  
|**differential_base_lsn**|**numeric(25,0)**|Base per backup differenziali. Gli extent di dati modificati dopo tale LSN verranno inclusi in un backup differenziale.|  
|**differential_base_guid**|**uniqueidentifier**|Identificatore univoco del backup di base in base al quale verrà eseguito un backup differenziale.|  
|**differential_base_time**|**datetime**|Tempo corrispondente a differential_base_lsn.|  
|**redo_start_lsn**|**numeric(25,0)**|Numero di sequenza del file di log in corrispondenza del quale deve iniziare l'esecuzione del successivo rollforward.<br /><br /> NULL a meno che state = RESTORING o state = RECOVERY_PENDING.|  
|**redo_start_fork_guid**|**uniqueidentifier**|Identificatore univoco del fork di recupero. Il valore first_fork_guid del successivo backup del log ripristinato deve corrispondere a questo valore. Rappresenta lo stato corrente del file.|  
|**redo_target_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) in corrispondenza del quale è possibile arrestare l'esecuzione del rollforward online sul file.<br /><br /> NULL a meno che state = RESTORING o state = RECOVERY_PENDING.|  
|**redo_target_fork_guid**|**uniqueidentifier**|Fork di recupero in corrispondenza del quale è possibile recuperare il file. Abbinato a redo_target_lsn.|  
|**backup_lsn**|**numeric(25,0)**|Numero di sequenza del file di log del backup dei dati o del backup differenziale del file più recente.|  
  
> [!NOTE]  
>  In caso di eliminazione o ricompilazione di indici di grandi dimensioni oppure di eliminazione o troncamento di tabelle di grandi dimensioni, in [!INCLUDE[ssDE](../../includes/ssde-md.md)] le deallocazioni di pagine effettive e i relativi blocchi associati vengono posticipati fino all'esecuzione del commit della transazione. Le operazioni di eliminazione posticipate non rendono immediatamente disponibile lo spazio allocato. Pertanto, i valori restituiti da sys.database_files subito dopo l'eliminazione o il troncamento di un oggetto di grandi dimensioni potrebbero non rispecchiare lo spazio su disco effettivamente disponibile.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Esempi  
L'istruzione seguente restituisce il nome, le dimensioni del file e la quantità di spazio vuoto per ogni file di database.

```
SELECT name, size/128.0 FileSizeInMB,
size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 
   AS EmptySpaceInMB
FROM sys.database_files;
```
Per altre informazioni sull'uso di [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] , vedere [determinazione delle dimensioni del database in database SQL di Azure V12 nel](/archive/blogs/sqlcat/determining-database-size-in-azure-sql-database-v12) Blog del team di consulenza clienti di SQL.
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di database e file &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Stati di file](../../relational-databases/databases/file-states.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.master_files &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)  
  
