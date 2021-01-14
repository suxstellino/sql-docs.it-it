---
description: sys.resource_governor_workload_groups (Transact-SQL)
title: sys.resource_governor_workload_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_workload_groups
- resource_governor_workload_groups_TSQL
- sys.resource_governor_workload_groups_TSQL
- resource_governor_workload_groups
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_workload_groups catalog view
ms.assetid: 619ba4b7-868f-4784-b527-ec1dfd703c4f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 70718b01e07cc883a030107e461e4390ca4e7cac
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169639"
---
# <a name="sysresource_governor_workload_groups-transact-sql"></a>sys.resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce la configurazione memorizzata del gruppo del carico di lavoro in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ogni gruppo del carico di lavoro può essere sottoscritto a un unico pool di risorse.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|group_id|**int**|ID univoco del gruppo del carico di lavoro. Non ammette i valori Null.|  
|name|**sysname**|Nome del gruppo del carico di lavoro. Non ammette i valori Null.|  
|importance|**sysname**|**Nota:** L'importanza si applica solo ai gruppi del carico di lavoro nello stesso pool di risorse.<br /><br /> L'importanza relativa di una richiesta in questo gruppo del carico di lavoro. L'importanza è una delle seguenti, con il valore medio predefinito: LOW, MEDIum, HIGH.<br /><br /> Non ammette i valori Null.|  
|request_max_memory_grant_percent|**int**|Massima concessione di memoria, espressa in percentuale, per una singola richiesta. Il valore predefinito è 25. Non ammette i valori Null.<br /><br /> **Nota:** Se questa impostazione è superiore al 50%, le query di grandi dimensioni vengono eseguite una alla volta. Pertanto, durante l'esecuzione della query c'è maggiore rischio di ricevere un errore di memoria insufficiente.|  
|request_max_cpu_time_sec|**int**|Limite massimo di utilizzo della CPU, in secondi, per un'unica richiesta. Il valore predefinito, 0, non specifica alcun limite. Non ammette i valori Null.<br /><br /> **Nota:** Per altre informazioni, vedere [classe di evento CPU Threshold superata](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|request_memory_grant_timeout_sec|**int**|Timeout di concessione di memoria, in secondi, per una singola richiesta. Il valore predefinito, 0 utilizza un calcolo interno basato sul costo della query. Non ammette i valori Null.|  
|max_dop|**int**|Massimo grado di parallelismo per il gruppo del carico di lavoro. Il valore predefinito, 0, utilizza le impostazioni globali. Non ammette i valori Null.<br /><br /> **Nodo:** Questa impostazione eseguirà l'override dell'opzione di query **MAXDOP**.|  
|group_max_requests|**int**|Numero massimo di richieste concorrenti. Il valore predefinito, 0, non specifica alcun limite. Non ammette i valori Null.|  
|pool_id|**int**|ID del pool di risorse utilizzato dal gruppo del carico di lavoro.|  
|external_pool_id|**int**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.<br /><br /> ID del pool di risorse esterne utilizzato dal gruppo del carico di lavoro.|  
  
## <a name="remarks"></a>Commenti  
 La vista del catalogo visualizza i metadati memorizzati. Per visualizzare la configurazione in memoria, utilizzare la vista a gestione dinamica corrispondente [sys.dm_resource_governor_workload_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md).  
  
 La configurazione archiviata e in memoria può essere diversa se è stata modificata la configurazione di Resource Governor senza applicare l'istruzione ALTER RESOURCE GOVERNOR RECONFIGURE.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione VIEW ANY DEFINITION per visualizzare i contenuti e l'autorizzazione CONTROL SERVER per modificarli.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo di Resource Governor &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)  
  
  
