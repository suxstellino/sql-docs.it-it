---
title: sys.dm_db_rda_migration_status (Transact-SQL) | Microsoft Docs
description: Informazioni su come sys.dm_db_rda_migration_status contiene una riga per ogni batch di dati migrati da ogni tabella abilitata per l'estensione nell'istanza locale di SQL Server.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: language-reference
f1_keywords:
- sys.dm_db_rda_migration_status
- sys.dm_db_rda_migration_status_TSQL
- dm_db_rda_migration_status
- dm_db_rda_migration_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_rda_migration_status dynamic management view
ms.assetid: faf3901c-a0e0-4e0c-8b1b-86d9f15f34dd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: eac014075edd5663b490791f6c49fc0b865afd2a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171633"
---
# <a name="stretch-database---sysdm_db_rda_migration_status"></a>Stretch Database sys.dm_db_rda_migration_status
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contiene una riga per ogni batch di dati migrati da ogni tabella abilitata per l'estensione nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . I batch sono identificati in base all'ora di inizio e all'ora di fine.  
  
 **sys.dm_db_rda_migration_status** viene definito come ambito del contesto di database corrente. Assicurarsi di trovarsi nel contesto del database delle tabelle abilitate per l'estensione per le quali si desidera visualizzare lo stato di migrazione.  
  
 In [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] l'output di **sys.dm_db_rda_migration_status** è limitato a 200 righe.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**table_id**|**int**|ID della tabella da cui sono state migrate le righe.|  
|**database_id**|**int**|ID del database da cui sono state migrate le righe.|  
|**migrated_rows**|**bigint**|Numero di righe migrate in questo batch.|  
|**start_time_utc**|**datetime**|Ora UTC in cui è stato avviato il batch.|  
|**end_time_utc**|**datetime**|Ora UTC in cui è stato completato il batch.|  
|**error_number**|**int**|Se il batch ha esito negativo, il numero di errore dell'errore che si è verificato. in caso contrario, null.|  
|**error_severity**|**int**|Se il batch ha esito negativo, la gravità dell'errore che si è verificato. in caso contrario, null.|  
|**error_state**|**int**|Se il batch ha esito negativo, lo stato dell'errore che si è verificato. in caso contrario, null.<br /><br /> Il **ERROR_STATE** indica la condizione o la posizione in cui si è verificato l'errore.|  
  
## <a name="see-also"></a>Vedere anche  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
