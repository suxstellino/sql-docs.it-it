---
description: sys.dm_pdw_errors (Transact-SQL)
title: sys.dm_pdw_errors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 944eac31-5691-432b-b9f5-f1e11c05191f
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: a1d5a75794d0063611f798cd826e463eb348debe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142784"
---
# <a name="sysdm_pdw_errors-transact-sql"></a>sys.dm_pdw_errors (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutti gli errori rilevati durante l'esecuzione di una richiesta o di una query.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|error_id|**nvarchar (36)**|Chiave per questa visualizzazione.<br /><br /> ID numerico univoco associato all'errore.|Univoco tra tutti gli errori di query nel sistema.|  
|source|**nvarchar (64)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|tipo|**nvarchar(4000)**|Tipo di errore che si è verificato.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|create_time|**datetime**|Ora in cui si è verificato l'errore.|Minore o uguale all'ora corrente.|  
|pwd_node_id|**int**|Identificatore del nodo specifico, se presente. Per ulteriori informazioni sugli ID dei nodi, vedere [sys.dm_pdw_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).||  
|session_id|**nvarchar(32)**|Identificatore della sessione, se presente. Per ulteriori informazioni sugli ID di sessione, vedere  [sys.dm_pdw_exec_sessions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).||  
|request_id|**nvarchar(32)**|Identificatore della richiesta, se presente. Per ulteriori informazioni sugli ID richiesta, vedere [sys.dm_pdw_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).||  
|spid|**int**|SPID della sessione di SQL Server, se presente.||  
|thread_id|**int**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|dettagli|**nvarchar(4000)**|Include la descrizione completa del testo dell'errore.||  
  
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere la sezione metadati nell'argomento [limiti di capacità](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
