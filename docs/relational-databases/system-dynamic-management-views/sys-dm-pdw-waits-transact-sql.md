---
description: sys.dm_pdw_waits (Transact-SQL)
title: sys.dm_pdw_waits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5130e498-1c77-4ae3-a80b-9aae396494e9
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: ff7327a914539c5dc2a2b691015ce7b9df55bf97
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138478"
---
# <a name="sysdm_pdw_waits-transact-sql"></a>sys.dm_pdw_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutti gli Stati di attesa rilevati durante l'esecuzione di una richiesta o di una query, inclusi i blocchi, le attese sulle code di trasmissione e così via.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|ID numerico univoco associato allo stato di attesa.<br /><br /> Chiave per questa visualizzazione.|Univoco in tutte le attese del sistema.|  
|session_id|**nvarchar(32)**|ID della sessione in cui si è verificato lo stato di attesa.|Vedere session_id in [sys.dm_pdw_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|tipo|**nvarchar(255)**|Tipo di attesa rappresentata da questa voce.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|object_type|**nvarchar(255)**|Tipo di oggetto interessato dall'attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|object_name|**nvarchar (386)**|Nome o GUID dell'oggetto specificato interessato dall'attesa.||  
|request_id|**nvarchar(32)**|ID della richiesta in cui si è verificato lo stato di attesa.|Vedere request_id in [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|request_time|**datetime**|Ora in cui è stato richiesto lo stato di attesa.||  
|acquire_time|**datetime**|Ora in cui è stato acquisito il blocco o la risorsa.||  
|state|**nvarchar(50)**|Stato dello stato di attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Priorità dell'elemento in attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)   
 [sys.dm_pdw_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-wait-stats-transact-sql.md)  
  
  
