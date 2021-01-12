---
description: sys.dm_os_volume_stats (Transact-SQL)
title: sys.dm_os_volume_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_volume_stats_TSQL
- dm_os_volume_stats
- sys.dm_os_volume_stats
- sys.dm_os_volume_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_volume_stats dynamic management function
ms.assetid: fa1c58ad-8487-42ad-956c-983f2229025f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 32deca4f06709806d90ee7d25e25105f191ad23e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098822"
---
# <a name="sysdm_os_volume_stats-transact-sql"></a>sys.dm_os_volume_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-2008R2SP1-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-2008R2sp1-xxxx-xxxx-xxx-md.md)]

  Restituisce informazioni sul volume del sistema operativo (directory) in cui sono archiviati i database e i file specificati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilizzare questa funzione a gestione dinamica per verificare gli attributi dell'unità disco fisica o restituire informazioni sullo spazio libero disponibile relative alla directory.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_os_volume_stats (database_id, file_id)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 *database_id*  
 ID del database. *database_id* è di tipo **int** e non prevede alcun valore predefinito. Non può essere NULL.  
  
 *file_id*  
 ID del file. *file_id* è di **tipo int** e non prevede alcun valore predefinito. Non può essere NULL.  
  
## <a name="table-returned"></a>Tabella restituita  
  
||||  
|-|-|-|  
|**Colonna**|**Tipo di dati**|**Descrizione**|  
|**database_id**|**int**|ID del database. Non può essere null.|  
|**file_id**|**int**|ID del file. Non può essere null.|  
|**volume_mount_point**|**nvarchar(512)**|Punto di montaggio in corrispondenza del quale si trova la radice del volume. Può restituire una stringa vuota. Restituisce null nel sistema operativo Linux.|  
|**volume_id**|**nvarchar(512)**|ID di volume del sistema operativo. Può restituire una stringa vuota. Restituisce null nel sistema operativo Linux.|  
|**logical_volume_name**|**nvarchar(512)**|Nome del volume logico. Può restituire una stringa vuota. Restituisce null nel sistema operativo Linux.|  
|**file_system_type**|**nvarchar(512)**|Tipo di volume del file system (ad esempio, NTFS, FAT, RAW). Può restituire una stringa vuota. Restituisce null nel sistema operativo Linux.|  
|**total_bytes**|**bigint**|Dimensioni totali del volume, espresse in byte. Non può essere null.|  
|**available_bytes**|**bigint**|Spazio libero disponibile nel volume. Non può essere null.|  
|**supports_compression**|**tinyint**|Indica se il volume supporta la compressione eseguita dal sistema operativo. Non può essere null in Windows e restituisce null nel sistema operativo Linux.|  
|**supports_alternate_streams**|**tinyint**|Indica se il volume supporta flussi alternativi. Non può essere null in Windows e restituisce null nel sistema operativo Linux.|  
|**supports_sparse_files**|**tinyint**|Indica se il volume supporta i file sparse.  Non può essere null in Windows e restituisce null nel sistema operativo Linux.|  
|**is_read_only**|**tinyint**|Indica se il volume è attualmente contrassegnato come di sola lettura. Non può essere null.|  
|**is_compressed**|**tinyint**|Indica se il volume è attualmente compresso. Non può essere null in Windows e restituisce null nel sistema operativo Linux.|  
|**incurs_seek_penalty**|**tinyint**|Indica il tipo di archiviazione che supporta questo volume. I valori possibili sono:<br /><br />0: nessun rigore di ricerca su questo volume, in genere quando il dispositivo di archiviazione è PMM o SSD<br /><br />1: ricerca di una penalità su questo volume, in genere quando il dispositivo di archiviazione è HDD<br /><br />2: non è possibile determinare il tipo di archiviazione quando il volume si trova in un percorso UNC o in condivisioni montate<br /><br />NULL: non è possibile determinare il tipo di archiviazione nel sistema operativo Linux<br /><br />**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] )|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW SERVER STATE`.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-return-total-space-and-available-space-for-all-database-files"></a>R. Restituzione dello spazio totale e dello spazio disponibile per tutti i file di database  
 Nell'esempio seguente vengono restituiti lo spazio totale e lo spazio disponibile (in byte) per tutti i file di database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT f.database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.master_files AS f  
CROSS APPLY sys.dm_os_volume_stats(f.database_id, f.file_id);  
```  
  
### <a name="b-return-total-space-and-available-space-for-the-current-database"></a>B. Restituzione dello spazio totale e dello spazio disponibile per il database corrente  
 Nell'esempio seguente vengono restituiti lo spazio totale e lo spazio disponibile (in byte) per i file nel database corrente.  
  
```sql  
SELECT database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.database_files AS f  
CROSS APPLY sys.dm_os_volume_stats(DB_ID(f.name), f.file_id);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.master_files &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  
