---
description: sys.dm_fts_outstanding_batches (Transact-SQL)
title: sys.dm_fts_outstanding_batches (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_outstanding_batches
- dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches
dev_langs:
- TSQL
helpviewer_keywords:
- troubleshooting [SQL Server], full-text search
- sys.dm_fts_outstanding_batches dynamic management view
ms.assetid: c4d697ed-c906-4c28-b137-036a25e13c84
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2932f123ac2b7f1125d187c943645c15d09e52d0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347936"
---
# <a name="sysdm_fts_outstanding_batches-transact-sql"></a>sys.dm_fts_outstanding_batches (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni su ogni batch di indicizzazione full-text.  
  
  |Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID del database|  
|catalog_id|**int**|ID del catalogo full-text|  
|table_id|**int**|ID della tabella contenente l'indice full-text.|  
|batch_id|**int**|ID batch|  
|memory_address|**varbinary (8)**|Indirizzo di memoria dell'oggetto batch|  
|crawl_memory_address|**varbinary (8)**|Indirizzo di memoria dell'oggetto ricerca per indicizzazione (oggetto padre)|  
|memregion_memory_address|**varbinary (8)**|Indirizzo di una regione di memoria condivisa in uscita dell'host del daemon di filtri (fdhost.exe)|  
|hr_batch|**int**|Codice relativo all'errore più recente per il batch|  
|is_retry_batch|**bit**|Indica se questo è un batch relativo a un tentativo:<br /><br /> 0 = No<br /><br /> 1 = Sì|  
|retry_hints|**int**|Tipo di tentativo necessario per il batch:<br /><br /> 0 = nessun tentativo<br /><br /> 1 = tentativo multi-thread<br /><br /> 2 = tentativo a thread singolo<br /><br /> 3 = tentativo a thread singolo e multi-thread<br /><br /> 5 = tentativo finale multi-thread<br /><br /> 6 = tentativo finale a thread singolo<br /><br /> 7 = tentativo finale a thread singolo e multi-thread|  
|retry_hints_description|**nvarchar(120)**|Descrizione del tipo di tentativo necessario:<br /><br /> NO RETRY<br /><br /> MULTI THREAD RETRY<br /><br /> SINGLE THREAD RETRY<br /><br /> SINGLE AND MULTI THREAD RETRY<br /><br /> MULTI THREAD FINAL RETRY<br /><br /> SINGLE THREAD FINAL RETRY<br /><br /> SINGLE AND MULTI THREAD FINAL RETRY|  
|doc_failed|**bigint**|Numero di documenti con errore nel batch|  
|batch_timestamp|**timestamp**|Valore del timestamp ottenuto al momento della creazione del batch|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene rilevato il numero di batch attualmente in elaborazione per ogni tabella nell'istanza del server.  
  
```  
SELECT database_id, table_id, COUNT(*) AS batch_count FROM sys.dm_fts_outstanding_batches GROUP BY database_id, table_id ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)  
  
  
