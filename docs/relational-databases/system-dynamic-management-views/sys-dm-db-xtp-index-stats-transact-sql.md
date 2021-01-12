---
description: sys.dm_db_xtp_index_stats (Transact-SQL)
title: sys.dm_db_xtp_index_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_index_stats
- dm_db_xtp_index_stats
- sys.dm_db_xtp_index_stats_TSQL
- dm_db_xtp_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_index_stats dynamic management view
ms.assetid: 8d0a50b8-2015-4576-930f-e3307dfc888e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0916891a629298602c274d4cf787e6bd9e1a8a9f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099913"
---
# <a name="sysdm_db_xtp_index_stats-transact-sql"></a>sys.dm_db_xtp_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Contiene le statistiche raccolte dall'ultimo riavvio del database.  
  
 Per ulteriori informazioni, vedere [OLTP in memoria &#40;In-Memory&#41;di ottimizzazione ](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md) e [linee guida per l'utilizzo di indici nelle tabelle Memory-Optimized](/previous-versions/sql/sql-server-2016/dn133166(v=sql.130)).  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|ID dell'oggetto a cui appartiene l'indice.|  
|xtp_object_id|**bigint**|ID interno corrispondente alla versione corrente dell'oggetto.<br /><br /> Nota: si applica a [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] .|  
|index_id|**bigint**|ID dell'indice. index_id è un valore univoco solo nell'ambito dell'oggetto.|  
|scans_started|**bigint**|Numero di analisi degli indici OLTP in memoria eseguite. Ogni istruzione select, insert, update o delete richiede un'analisi dell'indice.|  
|scans_retries|**bigint**|Numero di analisi degli indici che è necessario ritentare.|  
|rows_returned|**bigint**|Numero cumulativo di righe restituite dopo la creazione della tabella o l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|rows_touched|**bigint**|Numero cumulativo di righe cui è stato eseguito l'accesso dopo la creazione della tabella o l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|rows_expiring|**bigint**|Solo per uso interno.|  
|rows_expired|**bigint**|Solo per uso interno.|  
|rows_expired_removed|**bigint**|Solo per uso interno.|  
|phantom_scans_started|**bigint**|Solo per uso interno.|  
|phatom_scans_retries|**bigint**|Solo per uso interno.|  
|phantom_rows_touched|**bigint**|Solo per uso interno.|  
|phantom_expiring_rows_encountered|**bigint**|Solo per uso interno.|  
|phantom_expired_rows_encountered|**bigint**|Solo per uso interno.|  
|phantom_expired_removed_rows_encountered|**bigint**|Solo per uso interno.|  
|phantom_expired_rows_removed|**bigint**|Solo per uso interno.|  
|object_address|**varbinary (8)**|Solo per uso interno.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il database corrente.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
