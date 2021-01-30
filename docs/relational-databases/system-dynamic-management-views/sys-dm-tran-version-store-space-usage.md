---
description: sys.dm_tran_version_store_space_usage (Transact-SQL)
title: sys.dm_tran_version_store_space_usage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_version_store_space_usage_TSQL
- sys.dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store_space_usage dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c36dce670a89883b436421ae26c36d54308f2457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99132099"
---
# <a name="sysdm_tran_version_store_space_usage-transact-sql"></a>sys.dm_tran_version_store_space_usage (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Restituisce una tabella in cui viene visualizzato lo spazio totale in tempdb utilizzato dai record dell'archivio versioni per ogni database. **sys.dm_tran_version_store_space_usage** è efficiente e non costoso da eseguire, in quanto non consente di spostarsi tra i singoli record dell'archivio versioni e restituisce lo spazio di archiviazione delle versioni aggregato utilizzato in tempdb per database.
  
Ogni record con versione viene archiviato come dati binari, insieme ad alcune informazioni di rilevamento o di stato. Analogamente ai record nelle tabelle di database, i record inclusi nell'archivio delle versioni vengono archiviati in pagine da 8192 byte. In caso di dimensioni superiori a 8192 byte, il record verrà suddiviso in due record distinti.  
  
Poiché il record con versione viene archiviato come dato binario, non si verificheranno problemi in presenza di diverse regole di confronto di database diversi. Utilizzare **sys.dm_tran_version_store_space_usage** per monitorare e pianificare le dimensioni di tempdb in base all'utilizzo dello spazio di archiviazione delle versioni dei database in un'istanza di SQL Server.
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database.|  
|**reserved_page_count**|**bigint**|Conteggio totale delle pagine riservate in tempdb per i record dell'archivio versioni del database.|  
|**reserved_space_kb**|**bigint**|Spazio totale utilizzato in kilobyte in tempdb per i record dell'archivio versioni del database.|  
  
## <a name="permissions"></a>Autorizzazioni  
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   

## <a name="examples"></a>Esempio  
La query seguente può essere utilizzata per determinare lo spazio utilizzato in tempdb, dall'archivio delle versioni di ogni database in un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza di. 
  
```sql  
SELECT 
  DB_NAME(database_id) as 'Database Name',
  reserved_page_count,
  reserved_space_kb 
FROM sys.dm_tran_version_store_space_usage;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Database Name            reserved_page_count reserved_space_kb  
------------------------ -------------------- -----------  
msdb                      0                    0             
AdventureWorks2016        10                   80             
AdventureWorks2016DW      0                    0             
WideWorldImporters        20                   160             
```
 
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative alle transazioni &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
