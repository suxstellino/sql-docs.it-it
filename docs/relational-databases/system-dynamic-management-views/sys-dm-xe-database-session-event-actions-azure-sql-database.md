---
description: sys.dm_xe_database_session_event_actions (database SQL di Azure)
title: sys.dm_xe_database_session_event_actions
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 48519fd9-c7c2-434b-848d-ccbf41133fdd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 7219cba5d913ce2478f731de02b60705df27faae
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101379"
---
# <a name="sysdm_xe_database_session_event_actions-azure-sql-database"></a>sys.dm_xe_database_session_event_actions (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce informazioni sulle azioni di sessione di evento. Le azioni vengono eseguite quando vengono generati gli eventi. Questa vista di gestione aggrega le statistiche sul numero di volte che viene eseguita un'azione e la fase di esecuzione totale dell'azione.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 e a qualsiasi versione futura.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Indirizzo di memoria della sessione dell'evento. Non ammette i valori Null.|  
|action_name|**nvarchar(60)**|Nome dell'azione. Non ammette i valori Null.|  
|action_package_guid|**uniqueidentifier**|GUID per il pacchetto che contiene l'azione. Non ammette i valori Null.|  
|event_name|**nvarchar(60)**|Nome dell'evento associato all'azione. Non ammette i valori Null.|  
|event_package_guid|**uniqueidentifier**|GUID per il pacchetto che contiene l'evento. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.event_session_address|sys.dm_xe_database_sessions. Address|Molti-a-uno|  
|sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.action_name<br /><br /> sys.dm_xe_session_event_actions.action_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_database_session_events sys.dm_xe_database_session_events.event_package_guid|Molti-a-uno|  
|sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.event_name<br /><br /> sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.event_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_objects.package_guid|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  
