---
description: sys.dm_db_missing_index_groups (Transact-SQL)
title: sys.dm_db_missing_index_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_groups
- sys.dm_db_missing_index_groups_TSQL
- dm_db_missing_index_groups
- dm_db_missing_index_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_groups dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_groups dynamic management view
ms.assetid: 9cc00acd-d83d-49f8-be72-5b2aebed246b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb82dc8e3a8d47c6f497c7caebc8c0aca24f5b34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180606"
---
# <a name="sysdm_db_missing_index_groups-transact-sql"></a>sys.dm_db_missing_index_groups (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Questa DMV restituisce informazioni sugli indici mancanti in un gruppo di indici specifico, ad eccezione degli indici spaziali. 
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**index_group_handle**|**int**|Identifica un gruppo di indici mancanti.|  
|**index_handle**|**int**|Identifica un indice mancante appartenente al gruppo specificato da **index_group_handle**.<br /><br /> Un gruppo di indici contiene un solo indice.|  
  
## <a name="remarks"></a>Commenti  
 Le informazioni restituite da **sys.dm_db_missing_index_groups** vengono aggiornate in caso di ottimizzazione di una query tramite Query Optimizer e non sono persistenti. Le informazioni relative agli indici mancanti vengono mantenute solo fino al riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per mantenere tali informazioni anche dopo il riciclo del server, gli amministratori di database devono eseguirne periodicamente copie di backup.  
  
 Nessuna delle colonne del set di risultati di output è una chiave, ma la relativa combinazione costituisce una chiave di indice.  

  >[!NOTE]
  >Il set di risultati per questa DMV è limitato a 600 righe. Ogni riga contiene un indice mancante. Se sono presenti più di 600 indici mancanti, è necessario indirizzare gli indici mancanti esistenti in modo da poter visualizzare quelli più recenti.
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire query su questa vista a gestione dinamica, è necessario che agli utenti sia stata concessa l'autorizzazione VIEW SERVER STATE o qualsiasi autorizzazione che include l'autorizzazione VIEW SERVER STATE.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_db_missing_index_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
