---
description: sys.database_event_session_fields (database SQL di Azure)
title: sys.database_event_session_fields (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
ms.assetid: 9b5c94d6-612c-4e0f-976d-ac6ba55da3ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dafeecb84d666b7899890a75f24c1ecfd8a1cb51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210320"
---
# <a name="sysdatabase_event_session_fields-azure-sql-database"></a>sys.database_event_session_fields (database SQL di Azure)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce una riga per ogni colonna personalizzabile che è impostata in modo esplicito su eventi e destinazioni.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 e a tutte le versioni successive.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID della sessione dell'evento. Non ammette i valori Null.|  
|object_id|**int**|ID dell'oggetto a cui è associato il campo. Non ammette i valori Null.|  
|name|**sysname**|Nome del campo. Non ammette i valori Null.|  
|Valore|**sql_variant**|Valore del campo. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il server.  
  
## <a name="remarks"></a>Commenti  
 Questa vista ha le cardinalità della relazione seguenti.  
  
| Da | A | Relationship |
| ---- | -- | ------------ |
|sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.database_event_sessions sys.database_event_sessions.event_session_id|Molti-a-uno|  
|sys.database_event_session_actions sys.database_event_session_actions.event_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.object_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.database_event_session_events sys.database_event_session_events.event_session_id<br /><br /> sys.database_event_session_events sys.database_event_session_events.event_id|Molti-a-uno|  
|sys.database_event_session_actions sys.database_event_session_actions.event_session_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.object_id|sys.database_event_session_targets sys.database_event_session_targets.event_session_id<br /><br /> sys.database_event_session_targets sys.database_event_session_targets.target_id|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  
