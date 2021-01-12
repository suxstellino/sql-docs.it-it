---
description: sys.database_event_session_actions (database SQL di Azure)
title: sys.database_event_session_actions (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
ms.assetid: 32494df1-7ab7-4b88-a858-6b1021d67433
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d6b142c7a0a6d63fb6e0c2dcc59a5f2f701c2cfd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102809"
---
# <a name="sysdatabase_event_session_actions-azure-sql-database"></a>sys.database_event_session_actions (database SQL di Azure)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce una riga per ogni azione su ogni evento di una sessione dell'evento.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 e a tutte le versioni successive.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID della sessione dell'evento. Non ammette i valori Null.|  
|event_id|**int**|ID dell'evento. L'ID è univoco all'interno dell'oggetto della sessione di evento. Non ammette i valori Null.|  
|name|**sysname**|Nome dell'azione. Ammette i valori Null.|  
|Pacchetto|**sysname**|Nome del pacchetto eventi che contiene l'evento. Ammette i valori Null.|  
|module|**sysname**|Nome del modulo contenente l'evento. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il server.  
  
## <a name="remarks"></a>Osservazioni  
 Questa vista ha le cardinalità della relazione seguenti.  
  
| Da | A | Relationship |
| ---- | -- | ------------ |
|sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.sys. database_event_sessions. event_session_id|Molti-a-uno|  
|sys.database_event_session_actions sys.database_event_session_actions.event_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.database_event_session_events sys.database_event_session_events.event_session_id<br /><br /> sys.database_event_session_events sys.database_event_session_events.event_id|Molti-a-uno|  
  
  
