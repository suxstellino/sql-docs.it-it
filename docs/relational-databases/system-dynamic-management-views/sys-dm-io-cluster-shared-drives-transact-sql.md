---
description: sys.dm_io_cluster_shared_drives (Transact-SQL)
title: sys.dm_io_cluster_shared_drives (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_cluster_shared_drives_TSQL
- sys.dm_io_cluster_shared_drives
- dm_io_cluster_shared_drives_TSQL
- dm_io_cluster_shared_drives
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_cluster_shared_drives dynamic management view
ms.assetid: c8fcced8-c780-49dc-99bd-6beb3ca532c4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed6f7581fb605009fbbdc25c116e3317a5236ec7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97444268"
---
# <a name="sysdm_io_cluster_shared_drives-transact-sql"></a>sys.dm_io_cluster_shared_drives (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Questa vista restituisce il nome di ognuna delle unità condivise se l'istanza del server corrente è un server di cluster. Se l'istanza del server corrente non è un'istanza cluster, restituisce un set di righe vuoto.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] oggetto da, usare il nome **sys.dm_pdw_nodes_io_cluster_shared_drives**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**DriveName**|**nchar(2)**|Nome dell'unità (lettera di unità) che rappresenta un singolo disco facente parte dell'array di dischi condivisi. La colonna non ammette i valori Null.|  
|**pdw_node_id**|**int**|**Si applica a**: ssPDW<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="remarks"></a>Commenti  
 Se il clustering è attivato, l'istanza del cluster di failover richiede che i file di dati e di log siano disponibili in dischi condivisi, affinché sia possibile accedervi in seguito a un failover dell'istanza in un altro nodo. Ognuna delle righe in questa vista rappresenta un singolo disco condiviso utilizzato dall'istanza cluster di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Solo i dischi inclusi in questa vista possono essere utilizzati per archiviare file di dati o di log per questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I dischi elencati nella vista sono quelli contenuti nel gruppo di risorse cluster associato all'istanza.  
  
> [!NOTE]  
>  Questa vista verrà deprecata in una versione futura. È invece consigliabile utilizzare [sys.dm_io_cluster_valid_path_names &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-valid-path-names-transact-sql.md) .  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre dell'autorizzazione VIEW SERVER STATE per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene utilizzata la vista sys.dm_io_cluster_shared_drives per determinare le unità condivise in un'istanza del server di cluster:  
  
```  
SELECT * FROM sys.dm_io_cluster_shared_drives;  
```  
  
 Set di risultati:  
  
 DriveName  
  
 --------\-  
  
 m  
  
 n  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_io_cluster_valid_path_names &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-valid-path-names-transact-sql.md)   
 [sys.dm_os_cluster_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  
