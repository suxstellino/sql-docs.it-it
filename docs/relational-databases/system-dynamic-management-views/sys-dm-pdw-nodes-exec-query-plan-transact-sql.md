---
title: sys.dm_pdw_nodes_exec_query_plan (Transact-SQL) | Microsoft Docs
description: Vista a gestione dinamica che restituisce lo Showplan in formato XML per il batch specificato dall'handle del piano. Il piano specificato tramite l'handle di piano può essere memorizzato nella cache o in esecuzione.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: f78f40922db54563206bfe4c9fb4966958b41840
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99141201"
---
# <a name="syspdw_nodes_dm_exec_query_plan-transact-sql"></a>sys.pdw_nodes_dm_exec_query_plan (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Restituisce il piano Showplan in formato XML per il batch specificato dall'handle di piano. Il piano specificato tramite l'handle di piano può essere memorizzato nella cache o in esecuzione.  

> [!note] 
> In sinapsi SQL l'aggiunta di spazi vuoti in una query costituisce una modifica di query che comporta il ricalcolo dell'hash della query e il precedente piano di esecuzione memorizzato nella cache.


## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|ID numerico univoco associato al nodo.| 
|**dbid**|**smallint**|ID del database di contesto attivo al momento della compilazione dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] corrispondente a questo piano. Per le istruzioni SQL non pianificate e preparate, l'ID del database in cui sono state compilate le istruzioni.<br /><br /> La colonna ammette i valori Null.|  
|**objectid**|**int**|ID dell'oggetto (ad esempio, stored procedure o funzione definita dall'utente) per il piano della query. Per i batch ad hoc e preparati, questa colonna è **null**.<br /><br /> La colonna ammette i valori Null.|  
|**number**|**smallint**|Valore intero della stored procedure numerata. Per i batch ad hoc e preparati, questa colonna è **null**.<br /><br /> La colonna ammette i valori Null.| 
|**crittografati**|**bit**|Indica se la stored procedure corrispondente è crittografata.<br /><br /> 0 = non crittografata<br /><br /> 1 = crittografata<br /><br /> La colonna non ammette i valori Null.|  
|**query_plan**|**xml**|Contiene la rappresentazione Showplan della fase di compilazione del piano di esecuzione della query specificato con *plan_handle*. La rappresentazione Showplan è in formato XML. Viene generato un piano per ogni batch contenente ad esempio istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, chiamate di stored procedure e chiamate di funzioni definite dall'utente.<br /><br /> La colonna ammette i valori Null.|  
  
## <a name="remarks"></a>Commenti  
Si applicano le stesse osservazioni in [sys.dm_exec_query_plan](./sys-dm-exec-query-plan-transact-sql.md) .  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiedere il ruolo del server **sysadmin** o `VIEW SERVER STATE` l'autorizzazione per il server.  
  
## <a name="see-also"></a>Vedi anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>Passaggi successivi
 Per altri suggerimenti sullo sviluppo, vedere [Panoramica dello sviluppo di analisi delle sinapsi di Azure](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).
