---
title: sys.database_automatic_tuning_options (Transact-SQL) | Microsoft Docs
description: Informazioni su come visualizzare le opzioni di ottimizzazione automatica in un database SQL. Vedere autorizzazioni obbligatorie e visualizzare altre risorse disponibili.
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0dbedd65c5f98016e4e001fd863845079da68578
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210382"
---
# <a name="sysdatabase_automatic_tuning_options-transact-sql"></a>tuning_options automatico sys. database \_ \_ (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Restituisce le opzioni di ottimizzazione automatica per il database.  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**nvarchar(128)**|Nome dell'opzione di ottimizzazione automatica. Per le opzioni disponibili, fare riferimento a [ALTER DATABASE SET AUTOMATIC_TUNING &#40;&#41;Transact-SQL ](../../t-sql/statements/alter-database-transact-sql-set-options.md) .|  
|**desired_state**|**smallint**|Indica la modalità operativa desiderata per l'opzione di ottimizzazione automatica, impostata in modo esplicito dall'utente.<br />0 = OFF<br />1 = ON|  
|**desired_state_desc**|**nvarchar(60)**|Descrizione testuale della modalità operativa desiderata dell'opzione di ottimizzazione automatica.<br />OFF<br />ON|  
|**actual_state**|**smallint**|Indica la modalità operativa dell'opzione di ottimizzazione automatica.<br />0 = OFF<br />1 = ON|  
|**actual_state_desc**|**nvarchar(60)**|Descrizione testuale della modalità operativa effettiva dell'opzione di ottimizzazione automatica.<br />OFF<br />ON|  
|**reason**|**smallint**|Indica perché gli Stati effettivi e desiderati sono diversi.<br />2 = DISABILITATO<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|Descrizione testuale del motivo per cui gli Stati effettivi e desiderati sono diversi.<br />DISABLEd = l'opzione è disabilitata dal sistema<br />QUERY_STORE_OFF = Query Store è disattivato<br />QUERY_STORE_READ_ONLY = Query Store è in modalità di sola lettura<br />NOT_SUPPORTED = disponibile solo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition| 
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Vedere anche  
 [Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 
