---
description: sys.dm_db_xtp_object_stats (Transact-SQL)
title: sys.dm_db_xtp_object_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_object_stats_TSQL
- sys.dm_db_xtp_object_stats
- dm_db_xtp_object_stats
- sys.dm_db_xtp_object_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_object_stats dynamic management view
ms.assetid: 07300b59-3cab-4d3e-8138-5ea8f584f88f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 65852e4ecb6c26ca59ab0edffc18c1ea0c0ffd1c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099853"
---
# <a name="sysdm_db_xtp_object_stats-transact-sql"></a>sys.dm_db_xtp_object_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Restituisce il numero di righe modificate dalle operazioni per ciascun oggetto [!INCLUDE[hek_2](../../includes/hek-2-md.md)] dopo l'ultimo riavvio del database. Le statistiche vengono aggiornate durante l'esecuzione dell'operazione, indipendentemente dal fatto che per la transazione venga eseguito il commit o il rollback.  
  
 sys.dm_db_xtp_object_stats consente di identificare le tabelle ottimizzate per la memoria che vengono sostanzialmente modificate. È possibile considerare di rimuovere gli indici inutilizzati o poco utilizzati sulla tabella, in quanto ogni indice influisce sulle prestazioni. Se sono presenti indici hash è necessario periodicamente rivalutare il conteggio dei bucket. Per ulteriori informazioni, vedere [Determining the Correct Bucket Count for Hash Indexes](/previous-versions/sql/).  
  
 sys.dm_db_xtp_object_stats consente di identificare le tabelle ottimizzate per la memoria che restituiscono conflitti di tipo scrittura-scrittura, i quali possono influire sulle prestazioni dell'applicazione. Ad esempio, nel caso di una logica di riesecuzione della transazione, la stessa istruzione potrebbe dover essere eseguita più volte. Inoltre, è possibile utilizzare queste informazioni per identificare le tabelle e quindi la logica di business che richiedono la gestione degli errori di scrittura-scrittura.  
  
 La vista contiene una riga per ogni tabella con ottimizzazione per la memoria nel database.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|L'ID dell'oggetto.|  
|row_insert_attempts|**bigint**|Numero di righe inserite nella tabella dopo l'ultimo riavvio del database dalle transazioni su cui è stato eseguito il commit e da quelle interrotte.|  
|row_update_attempts|**bigint**|Numero di righe aggiornate nella tabella dopo l'ultimo riavvio del database dalle transazioni su cui è stato eseguito il commit e da quelle interrotte.|  
|row_delete_attempts|**bigint**|Numero di righe eliminate dalla tabella dopo l'ultimo riavvio del database dalle transazioni su cui è stato eseguito il commit e da quelle interrotte.|  
|write_conflicts|**bigint**|Numero di conflitti di scrittura che si sono verificati dopo l'ultimo riavvio del database.|  
|unique_constraint_violations|**bigint**|Numero di violazioni di vincolo UNIQUE che si sono verificate dopo l'ultimo riavvio del database.|  
|object_address|**varbinary (8)**|Solo per uso interno.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il database corrente.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
