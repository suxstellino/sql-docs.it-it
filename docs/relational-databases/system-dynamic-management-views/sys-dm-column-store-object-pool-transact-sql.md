---
description: sys.dm_column_store_object_pool (Transact-SQL)
title: sys.dm_column_store_object_pool (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a8a58ca7-0a7d-4786-bfd9-e8894bd345dd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9045d0b9ecda2e20bf9013d0c875adb09eeae67c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095241"
---
# <a name="sysdm_column_store_object_pool-transact-sql"></a>sys.dm_column_store_object_pool (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

 Restituisce i conteggi dei diversi tipi di utilizzo del pool di memoria oggetto per gli oggetti dell'indice columnstore.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|INT|ID del database. Questa operazione è univoca all'interno di un'istanza di un database SQL Server o di un server di database SQL di Azure. |  
|**object_id**|INT|ID dell'oggetto. L'oggetto è uno dei object_types. | 
|**index_id**|INT|ID dell'indice columnstore.|  
|**partition_number**|bigint|Numero di partizione in base 1 all'interno dell'indice o heap. Ogni tabella o vista include almeno una partizione.| 
|**column_id**|INT|ID della colonna columnstore. Questo valore è NULL per DELETE_BITMAP.| 
|**row_group_id**|INT|ID di rowgroup.|
|**object_type**|SMALLINT|1 = COLUMN_SEGMENT<br /><br /> 2 = COLUMN_SEGMENT_PRIMARY_DICTIONARY<br /><br /> 3 = COLUMN_SEGMENT_SECONDARY_DICTIONARY<br /><br /> 4 = COLUMN_SEGMENT_BULKINSERT_DICTIONARY<br /><br /> 5 = COLUMN_SEGMENT_DELETE_BITMAP|  
|**object_type_desc**|nvarchar(60)|COLUMN_SEGMENT-un segmento di colonna. `object_id` ID del segmento. Un segmento archivia tutti i valori di una colonna all'interno di un rowgroup. Se, ad esempio, una tabella contiene 10 colonne, saranno presenti 10 segmenti di colonna per ogni rowgroup. <br /><br /> COLUMN_SEGMENT_PRIMARY_DICTIONARY: dizionario globale che contiene informazioni di ricerca per tutti i segmenti di colonna della tabella.<br /><br /> COLUMN_SEGMENT_SECONDARY_DICTIONARY: un dizionario locale associato a una colonna.<br /><br /> COLUMN_SEGMENT_BULKINSERT_DICTIONARY: un'altra rappresentazione del dizionario globale. Questa operazione fornisce una ricerca inversa del valore da dictionary_id. Utilizzato per creare segmenti compressi come parte del motore di tupla o del caricamento bulk.<br /><br /> COLUMN_SEGMENT_DELETE_BITMAP: bitmap che tiene traccia delle eliminazioni di segmenti. Esiste una bitmap Delete per partizione.|  
|**access_count**|INT|Numero di accessi in lettura o scrittura a questo oggetto.|  
|**memory_used_in_bytes**|bigint|Memoria utilizzata da questo oggetto nel pool di oggetti.|  
|**object_load_time**|Datetime|Tempo di clock per il momento in cui object_id è stato introdotto nel pool di oggetti.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="see-also"></a>Vedere anche  
  
 [Funzioni a gestione dinamica e DMV correlate all'indice &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)  
 [Indici columnstore: Panoramica](../../relational-databases/indexes/columnstore-indexes-overview.md) 
  
