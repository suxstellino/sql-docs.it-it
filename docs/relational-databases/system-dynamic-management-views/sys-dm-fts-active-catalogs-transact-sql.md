---
description: sys.dm_fts_active_catalogs (Transact-SQL)
title: sys.dm_fts_active_catalogs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_active_catalogs_TSQL
- dm_fts_active_catalogs
- dm_fts_active_catalogs_TSQL
- sys.dm_fts_active_catalogs
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_active_catalogs dynamic management view
ms.assetid: 40ab5453-040c-4d2e-bb49-e340cf90c3ee
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 581dcf8dc3dace674f7112680981da638cf658ca
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838614"
---
# <a name="sysdm_fts_active_catalogs-transact-sql"></a>sys.dm_fts_active_catalogs (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni sui cataloghi full-text caratterizzati da attività di popolamento in corso nel server.  
  
> [!NOTE]
>  Le colonne seguenti verranno rimosse in una versione futura di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : is_paused, previous_status, previous_status_description, row_count_in_thousands, status, status_description e worker_count. Evitare di utilizzare queste colonne in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui vengono utilizzate.  
  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database contenente il catalogo full-text attivo.|  
|**catalog_id**|**int**|ID del catalogo full-text attivo.|  
|**memory_address**|**varbinary (8)**|Indirizzo dei buffer di memoria allocati per l'attività di popolamento correlata al catalogo full-text.|  
|**nome**|**nvarchar(128)**|Nome del catalogo full-text attivo.|  
|**is_paused**|**bit**|Indica se il popolamento del catalogo full-text attivo è stato sospeso.|  
|**Stato**|**int**|Stato corrente del catalogo full-text. I tipi validi sono:<br /><br /> 0 = Inizializzazione in corso<br /><br /> 1 = Pronto<br /><br /> 2 = sospeso<br /><br /> 3 = Errore temporaneo<br /><br /> 4 = Rimontaggio necessario<br /><br /> 5 = Chiusura<br /><br /> 6 = In stato di inattività per backup<br /><br /> 7 = Il backup viene eseguito tramite il catalogo<br /><br /> 8 = Il catalogo è danneggiato|  
|**status_description**|**nvarchar(120)**|Descrizione dello stato corrente del catalogo full-text attivo.|  
|**previous_status**|**int**|Stato precedente del catalogo full-text. I tipi validi sono:<br /><br /> 0 = Inizializzazione in corso<br /><br /> 1 = Pronto<br /><br /> 2 = sospeso<br /><br /> 3 = Errore temporaneo<br /><br /> 4 = Rimontaggio necessario<br /><br /> 5 = Chiusura<br /><br /> 6 = In stato di inattività per backup<br /><br /> 7 = Il backup viene eseguito tramite il catalogo<br /><br /> 8 = Il catalogo è danneggiato|  
|**previous_status_description**|**nvarchar(120)**|Descrizione dello stato precedente del catalogo full-text attivo.|  
|**worker_count**|**int**|Numero di thread che elaborano il catalogo full-text.|  
|**active_fts_index_count**|**int**|Numero di indici full-text che vengono popolati.|  
|**auto_population_count**|**int**|Numero di tabelle in cui è in corso il popolamento automatico del catalogo full-text.|  
|**manual_population_count**|**int**|Numero di tabelle in cui il popolamento manuale è in corso per il catalogo full-text.|  
|**full_incremental_population_count**|**int**|Numero di tabelle in cui è in corso il popolamento completo o incrementale del catalogo full-text.|  
|**row_count_in_thousands**|**int**|Numero stimato di righe (in migliaia) in tutti gli indici full-text del catalogo full-text.|  
|**is_importing**|**bit**|Indica se il catalogo full-text viene importato:<br /><br /> 1 = Il catalogo viene importato.<br /><br /> 2 = Il catalogo non viene importato.|  
  
## <a name="remarks"></a>Commenti  
 La colonna is_importing è stata introdotta in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
   
## <a name="physical-joins"></a>Join fisici  
 ![Join significativi di questa DMV](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-active-catalogs-1.gif "Join significativi di questa DMV")  
  
## <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|Uno-a-uno|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|Uno-a-uno|  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite le informazioni sui cataloghi full-text attivi nel database corrente.  
  
```  
SELECT catalog.name, catalog.is_importing, catalog.auto_population_count,  
  OBJECT_NAME(population.table_id) AS table_name,  
  population.population_type_description, population.is_clustered_index_scan,  
  population.status_description, population.completion_type_description,  
  population.queued_population_type_description, population.start_time,  
  population.range_count   
FROM sys.dm_fts_active_catalogs catalog   
CROSS JOIN sys.dm_fts_index_population population   
WHERE catalog.database_id = population.database_id   
AND catalog.catalog_id = population.catalog_id   
AND catalog.database_id = (SELECT dbid FROM sys.sysdatabases WHERE name = DB_NAME());  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
