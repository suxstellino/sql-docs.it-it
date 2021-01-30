---
description: sys.dm_xe_database_sessions (database SQL di Azure)
title: sys.dm_xe_database_sessions (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
ms.assetid: 33ea5179-16bb-4abd-96cc-9bc696e80987
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 0e70d8d2f3dc731f29eda1f96b8ad88d51964313
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99129873"
---
# <a name="sysdm_xe_database_sessions-azure-sql-database"></a>sys.dm_xe_database_sessions (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce informazioni sugli eventi di sessione. Gli eventi sono punti di esecuzione discreti. I predicati possono essere applicati agli eventi per arrestarne la generazione se l'evento non contiene le informazioni necessarie.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 e a tutte le versioni successive.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Indirizzo di memoria della sessione dell'evento. Non ammette i valori Null.|  
|event_name|**nvarchar(60)**|Nome dell'evento associato all'azione. Non ammette i valori Null.|  
|event_package_guid|**uniqueidentifier**|GUID per il pacchetto che contiene l'evento. Non ammette i valori Null.|  
|event_predicate|**nvarchar(2048)**|Rappresentazione XML dell'albero predicativo applicato all'evento. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
A partire da 2015-07-13,' sys.dm_xe_objects ' è uno di questi DMV XEvent che non contengono ' _database ' nel nome. Non è un errore di digitazione o errore nella colonna a destra della tabella seguente. Il nome è lo stesso in Microsoft SQL Server e nel database SQL di Azure.  
  
|Da|A|Relationship|  
|--------|------|----------------|  
|sys.dm_xe_database_session_events sys.dm_xe_database_session_events.event_session_address|sys.dm_xe_database_sessions. Address|Molti-a-uno|  
|sys. dm _xe_database_session_events. event_package_guid, sys. dm _xe_database_session_events. event_name|sys.dm_xe_objects.name, sys.dm_xe_objects.package_guid|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
[Eventi estesi nel database SQL di Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)  
[Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
