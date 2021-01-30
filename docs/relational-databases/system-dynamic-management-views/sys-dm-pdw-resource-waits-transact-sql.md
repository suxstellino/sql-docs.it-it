---
description: sys.dm_pdw_resource_waits (Transact-SQL)
title: sys.dm_pdw_resource_waits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/26/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a43ce9a2-5261-41e3-97f0-555ba05ebed9
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: f6685853de3fed6397886d654135bb6ce499e7da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203394"
---
# <a name="sysdm_pdw_resource_waits-transact-sql"></a>sys.dm_pdw_resource_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Visualizza le informazioni di attesa per tutti i tipi di risorsa in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Posizione della richiesta nell'elenco di attesa.|ordinale in base 0. Questa operazione non è univoca in tutte le voci di attesa.|  
|session_id|**nvarchar(32)**|ID della sessione in cui si è verificato lo stato di attesa.|Vedere session_id in [sys.dm_pdw_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|tipo|**nvarchar(255)**|Tipo di attesa rappresentata da questa voce.|Valori possibili:<br /><br /> Connessioni<br /><br /> Concorrenza di query locali<br /><br /> Concorrenza di query distribuite<br /><br /> Concorrenza DMS<br /><br /> Concorrenza di backup|  
|object_type|**nvarchar(255)**|Tipo di oggetto interessato dall'attesa.|Valori possibili:<br /><br /> **OGGETTO**<br /><br /> **DATABASE**<br /><br /> **SISTEMA**<br /><br /> **SCHEMA**<br /><br /> **APPLICAZIONE**|  
|object_name|**nvarchar (386)**|Nome o GUID dell'oggetto specificato interessato dall'attesa.|Le tabelle e le viste vengono visualizzate con nomi in tre parti.<br /><br /> Gli indici e le statistiche vengono visualizzati con nomi in quattro parti.<br /><br /> Nomi, entità e database sono nomi di stringa.|  
|request_id|**nvarchar(32)**|ID della richiesta in cui si è verificato lo stato di attesa.|Identificatore QID della richiesta.<br /><br /> Identificatore GUID per le richieste di caricamento.|  
|request_time|**datetime**|Ora in cui è stato richiesto il blocco o la risorsa.||  
|acquire_time|**datetime**|Ora in cui è stato acquisito il blocco o la risorsa.||  
|state|**nvarchar(50)**|Stato dello stato di attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Priorità dell'elemento in attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|concurrency_slots_used|**int**|Interno|Vedere il [monitoraggio attese risorse](#monitor-resource-waits) di seguito|  
|resource_class|**nvarchar (20)**|Interno |Vedere il [monitoraggio attese risorse](#monitor-resource-waits) di seguito|  
  
## <a name="monitor-resource-waits"></a>Monitoraggio attese risorse 
Con l'introduzione dei [gruppi del carico di lavoro](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation), gli slot di concorrenza non sono più applicabili.  Usare la query seguente e la `resources_requested` colonna per comprendere le risorse necessarie per eseguire la richiesta.

```sql
select rw.wait_id
      ,rw.session_id
      ,rw.type
      ,rw.object_type
      ,rw.object_name
      ,rw.request_id
      ,rw.request_time
      ,rw.acquire_time
      ,rw.state
      ,resources_requested = s.effective_request_min_resource_grant_percent
      ,r.group_name
  from sys.dm_workload_management_workload_groups_stats s
  join sys.dm_pdw_exec_requests r on r.group_name = s.name collate SQL_Latin1_General_CP1_CI_AS
  join sys.dm_pdw_resource_waits rw on rw.request_id = r.request_id
```

## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
