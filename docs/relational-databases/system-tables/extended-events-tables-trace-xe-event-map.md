---
description: Tabelle eventi estesi - trace_xe_event_map
title: trace_xe_event_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trace_xe_event_map_TSQL
- trace_xe_event_map
dev_langs:
- TSQL
helpviewer_keywords:
- trace_xe_event_map
- extended events [SQL Server], tables
ms.assetid: 537aa292-3540-47e8-be28-56dc01abc343
author: cawrites
ms.author: chadam
ms.openlocfilehash: 72394c7ec68c6abd2815edec6ec3ddabe135e09b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207213"
---
# <a name="extended-events-tables---trace_xe_event_map"></a>Tabelle eventi estesi - trace_xe_event_map
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni evento degli eventi estesi di cui è stato eseguito il mapping a una classe di evento di Traccia SQL. Questa tabella è archiviata nel database master, nello schema sys.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|trace_event_id|**smallint**|ID della classe di evento di Traccia SQL di cui viene eseguito il mapping.|  
|package_name|**nvarchar(60)**|Nome del pacchetto degli eventi estesi in cui si trova l'evento di cui è stato eseguito il mapping.|  
|xe_event_name|**nvarchar(60)**|Nome dell'evento degli eventi estesi di cui è stato eseguito il mapping alla classe di evento di Traccia SQL.|  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare la query seguente per identificare gli eventi degli eventi estesi equivalenti alle classi di evento di Traccia SQL:  
  
```  
SELECT te.name, xe.package_name, xe.xe_event_name  
FROM sys.trace_events AS te  
LEFT JOIN sys.trace_xe_event_map AS xe  
   ON te.trace_event_id = xe.trace_event_id  
WHERE xe.trace_event_id IS NOT NULL  
```  
  
 Non tutte le classi di evento dispongono di eventi degli eventi estesi equivalenti. È possibile utilizzare la query seguente per elencare le classi di evento che non dispongono di eventi estesi equivalenti:  
  
```  
SELECT te.trace_event_id, te.name  
FROM sys.trace_events AS te  
LEFT JOIN sys.trace_xe_event_map AS xe  
   ON te.trace_event_id = xe.trace_event_id  
WHERE xe.trace_event_id IS NULL  
```  
  
 Nella query precedente, la maggior parte delle classi di evento restituite sono relative al controllo. Si consiglia di utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit per il controllo. Per creare un controllo, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit vengono utilizzati gli eventi estesi. Per altre informazioni, vedere [SQL Server Audit &#40;Motore di database&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
## <a name="see-also"></a>Vedere anche  
 [trace_xe_action_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-action-map.md)  
  
  
