---
description: sys.pdw_loader_backup_run_details (Transact-SQL)
title: sys.pdw_loader_backup_run_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 04fc004f-ee15-4d7a-be08-78357aa99b55
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9f00da4659e4254b7f9a48bea10d201e10e5c9ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186124"
---
# <a name="syspdw_loader_backup_run_details-transact-sql"></a>sys.pdw_loader_backup_run_details (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene ulteriori informazioni dettagliate, oltre alle informazioni contenute in [sys.pdw_loader_backup_runs &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md), sulle operazioni di backup e ripristino in corso e completate in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e sulle operazioni di backup, ripristino e caricamento in corso e completate in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . Le informazioni vengono mantenute tra un riavvio di sistema e l'altro.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|run_id|**int**|Identificatore univoco per l'esecuzione di un backup o un ripristino specifico.<br /><br /> run_id e pdw_node_id formano la chiave per questa visualizzazione.||  
|pdw_node_id|**int**|Identificatore univoco di un nodo Appliance per il quale questo record include i dettagli.<br /><br /> run_id e pdw_node_id formano la chiave per questa visualizzazione.|Vedere node_id in [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|status|**nvarchar (16)**|Stato corrente dell'esecuzione.|' CANCELLED ',' COMPLETED ',' FAILED ',' QUEUED ',' RUNNING '|  
|start_time|**datetime**|Ora di inizio dell'operazione su questo nodo specifico.||  
|end_time|**datetime**|Ora in cui l'operazione è terminata in questo nodo specifico, se disponibile.||  
|total_elapsed_time|**int**|Tempo totale in cui l'operazione è stata eseguita in questo nodo specifico.|Se total_elapsed_time supera il valore massimo per un numero intero (24,8 giorni in millisecondi), si verificherà un errore di materializzazione causato da un overflow.<br /><br /> Il valore massimo in millisecondi equivale a 24,8 giorni.|  
|progress|**int**|Stato dell'operazione espressa come percentuale.|Da 0 a 100|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
