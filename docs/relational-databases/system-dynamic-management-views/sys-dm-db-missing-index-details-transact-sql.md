---
description: sys.dm_db_missing_index_details (Transact-SQL)
title: sys.dm_db_missing_index_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_missing_index_details
- dm_db_missing_index_details
- sys.dm_db_missing_index_details_TSQL
- dm_db_missing_index_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- missing indexes feature [SQL Server], sys.dm_db_missing_index_details dynamic management view
- sys.dm_db_missing_index_details dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fdce3b3269d0b60c20d82064955c83416c4254eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095182"
---
# <a name="sysdm_db_missing_index_details-transact-sql"></a>sys.dm_db_missing_index_details (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni dettagliate sugli indici mancanti, escludendo gli indici spaziali.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**index_handle**|**int**|Identifica un determinato indice mancante. L'identificatore è univoco nel server. **index_handle** è la chiave di questa tabella.|  
|**database_id**|**smallint**|Identifica il database in cui è archiviata la tabella con l'indice mancante.|  
|**object_id**|**int**|Identifica la tabella in cui l'indice risulta mancante.|  
|**equality_columns**|**nvarchar(4000)**|Elenco delimitato da virgole delle colonne che contribuiscono ai predicati di uguaglianza nel formato seguente:<br /><br /> *Table. Column*  = *constant_value*|  
|**inequality_columns**|**nvarchar(4000)**|Elenco delimitato da virgole delle colonne che contribuiscono ai predicati di disuguaglianza, ad esempio predicati nel formato seguente:<br /><br /> *Table. Column*  >  *constant_value*<br /><br /> Qualsiasi operatore di confronto diverso da "=" esprime disuguaglianza.|  
|**included_columns**|**nvarchar(4000)**|Elenco delimitato da virgole delle colonne necessarie come colonne di copertura per la query. Per altre informazioni sulle colonne di copertura o incluse, vedere [creare indici con colonne incluse](../../relational-databases/indexes/create-indexes-with-included-columns.md).<br /><br /> Per gli indici ottimizzati per la memoria (hash e non cluster ottimizzati per la memoria), ignorare **included_columns**. Tutte le colonne della tabella vengono incluse in ogni indice ottimizzato per la memoria.|  
|**istruzione**|**nvarchar(4000)**|Nome della tabella in cui l'indice risulta mancante.|  
  
## <a name="remarks"></a>Osservazioni  
 Le informazioni restituite da **sys.dm_db_missing_index_details** vengono aggiornate in caso di ottimizzazione di una query tramite Query Optimizer e non sono persistenti. Le informazioni relative agli indici mancanti vengono mantenute solo fino al riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per mantenere tali informazioni anche dopo il riciclo del server, gli amministratori di database devono eseguirne periodicamente copie di backup.  
  
 Per determinare il gruppo di indici mancanti a cui appartiene un determinato indice mancante, è possibile eseguire query sulla DMV **sys.dm_db_missing_index_groups** eseguendone l'equijoin con **sys.dm_db_missing_index_details** in base alla colonna **index_handle**.  

  >[!NOTE]
  >Il set di risultati per questa DMV è limitato a 600 righe. Ogni riga contiene un indice mancante. Se sono presenti più di 600 indici mancanti, è necessario indirizzare gli indici mancanti esistenti in modo da poter visualizzare quelli più recenti. 
  
## <a name="using-missing-index-information-in-create-index-statements"></a>Utilizzo di informazioni sugli indici mancanti in istruzioni CREATE INDEX  
 Per convertire le informazioni restituite da **sys.dm_db_missing_index_details** in un'istruzione create index sia per gli indici ottimizzati per la memoria che per quelli basati su disco, le colonne di uguaglianza devono essere inserite prima delle colonne di disuguaglianza e insieme devono creare la chiave dell'indice. Aggiungere le colonne incluse all'istruzione CREATE INDEX mediante la clausola INCLUDE. Per determinare un ordine efficiente per le colonne di uguaglianza, ordinarle in base alla selettività a partire dalle colonne più selettive, all'estrema sinistra nell'elenco di colonne.  
  
 Per ulteriori informazioni sugli indici ottimizzati per la memoria, vedere [indici per tabelle Memory-Optimized](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).  
  
## <a name="transaction-consistency"></a>Consistenza delle transazioni  
 Se in una transazione viene creata o eliminata una tabella, le righe contenenti le informazioni sugli indici mancanti per gli oggetti eliminati vengono rimosse da questo oggetto a gestione dinamica, mantenendo la consistenza delle transazioni.  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
 [sys.dm_db_missing_index_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
