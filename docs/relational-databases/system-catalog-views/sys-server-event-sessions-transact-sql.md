---
description: sys.server_event_sessions (Transact-SQL)
title: sys.server_event_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_event_sessions
- server_event_sessions_TSQL
- sys.server_event_sessions_TSQL
- sys.server_event_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_sessions catalog view
- xe
ms.assetid: 796f3093-6a3e-4d67-8da6-b9810ae9ef5b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9addba51e1a23fa9cf430d91aced04126a7f03e5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093063"
---
# <a name="sysserver_event_sessions-transact-sql"></a>sys.server_event_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elenca tutte le definizioni di sessione dell'evento che esistono in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID univoco della sessione dell'evento. Non ammette i valori Null.|  
|name|**sysname**|Nome definito dall'utente per identificare la sessione eventi. il nome è univoco. Non ammette i valori Null.|  
|event_retention_mode|**nchar(1)**|Determina la modalità di gestione della perdita di eventi. L'impostazione predefinita è S. Non sono ammessi valori Null. I valori validi sono i seguenti:<br /><br /> S. Esegue il mapping a event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> M. Esegue il mapping a event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> N. Esegue il mapping a event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|Descrive la modalità di gestione della perdita di eventi. L'impostazione predefinita è ALLOW_SINGLE_EVENT_LOSS. Non ammette i valori Null. I valori validi sono i seguenti:<br /><br /> ALLOW_SINGLE_EVENT_LOSS. Gli eventi possono essere persi dalla sessione. Gli eventi singoli vengono eliminati solo quando tutti i buffer dell'evento sono completi. La perdita di eventi singoli quando i buffer sono completi lascia spazio a caratteristiche di prestazioni accettabili di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], minimizzando la perdita nel flusso dell'evento elaborato.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS. I buffer di eventi completi possono essere persi dalla sessione. Il numero di eventi persi dipende dalla dimensione della memoria allocata alla sessione, dalla partizione della memoria e dalla dimensione degli eventi nel buffer. Questa opzione minimizza l'impatto sulle prestazioni nel server quando i buffer degli eventi vengono completati rapidamente. Tuttavia, molti eventi della sessione possono essere perduti.<br /><br /> NO_EVENT_LOSS. Non è consentita alcuna perdita di eventi. Questa opzione assicura che tutti gli eventi generati siano mantenuti. L'utilizzo di questa opzione forza tutte le attività che attivano eventi ad aspettare fino a che lo spazio è disponibile in un buffer degli eventi. Ciò può determinare una riduzione rilevabile del livello delle prestazioni quando la sessione dell'evento è attiva.|  
|max_dispatch_latency|**int**|Quantità di tempo, espresso in millisecondi, durante il quale gli eventi verranno trattenuti in memoria prima di essere resi disponibili alle destinazioni della sessione. I valori validi sono compresi tra 0 e 2147483648 e 0. Il valore 0 indica che la latenza di recapito è infinita. Ammette i valori Null.|  
|max_memory|**int**|La quantità di memoria allocata alla sessione per la memorizzazione degli eventi nel buffer. Il valore predefinito è 4 MB. Ammette i valori Null.|  
|max_event_size|**int**|La quantità di memoria riservata per eventi che non si adattano ai buffer di sessione dell'evento. Se max_event_size supera la dimensione del buffer calcolata, due buffer aggiuntivi di max_event_size sono allocati alla sessione dell'evento. Ammette i valori Null.|  
|memory_partition_mode|**nchar(1)**|Percorso della memoria dove i buffer dell'evento vengono creati. La modalità della partizione predefinita è G. Non ammette valori Null. memory_partition_mode è uno dei seguenti:<br /><br /> G - NONE<br /><br /> C - PER_CPU<br /><br /> N - PER_NODE|  
|memory_partition_mode_desc|**sysname**|Il valore predefinito è NONE. Non ammette i valori Null. I valori validi sono i seguenti:<br /><br /> NONE. All'interno di un'istanza di SQL Server viene creato un unico set di buffer.<br /><br /> PER_CPU. Viene creato un set di buffer per CPU.<br /><br /> PER_NODE. Viene creato un set di buffer per ogni nodo NUMA (non-uniform memory access).|  
|track_causality|**bit**|Abilita o disabilita il rilevamento della causalità. Se è impostato su 1 (ON), il rilevamento viene abilitato e gli eventi correlati su connessioni server diverse possono essere correlati. L'impostazione predefinita è 0 (OFF). Non ammette i valori Null.|  
|startup_state|**bit**|Valore che determina se la sessione viene avviata automaticamente all'avvio del server. Il valore predefinito è 0. Non ammette i valori Null. È uno dei seguenti valori:<br /><br /> 0 (OFF). La sessione non inizia all'avvio del server.<br /><br /> 1 (ON). La sessione dell'evento inizia all'avvio del server.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo degli eventi estesi &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  
