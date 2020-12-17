---
title: sys.dm_pdw_nodes_exec_query_statistics_xml (Transact-SQL) | Microsoft Docs
description: Vista a gestione dinamica che restituisce il piano di esecuzione di query per le richieste in corso. Utilizzare questa DMV per recuperare Showplan XML con statistiche temporanee.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 65791195a30dcd23b184dbceeafdd786b64f857e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644087"
---
# <a name="dm_pdw_nodes_exec_query_statistics_xml-transact-sql"></a>dm_pdw_nodes_exec_query_statistics_xml (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Restituisce il piano di esecuzione della query per le richieste in corso. Utilizzare questa DMV per recuperare Showplan XML con statistiche temporanee.

## <a name="table-returned"></a>Tabella restituita

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|
|node_id|**int**|ID numerico univoco associato al nodo.|
|session_id|**smallint**|ID della sessione. Non ammette i valori NULL.|
|request_id|**int**|ID della richiesta. Non ammette i valori NULL.|
|sql_handle|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure di cui fa parte la query. Ammette valori Null.|
|plan_handle|**varbinary(64)**|Token che identifica in modo univoco un piano di esecuzione della query per un batch attualmente in esecuzione. Ammette valori Null.|
|query_plan|**xml**|Contiene la rappresentazione Showplan di runtime del piano di esecuzione della query specificato con *plan_handle* contenenti statistiche parziali. La rappresentazione Showplan è in formato XML. Viene generato un piano per ogni batch contenente ad esempio istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, chiamate di stored procedure e chiamate di funzioni definite dall'utente. Ammette valori Null.|

## <a name="remarks"></a>Osservazioni
Si applicano le stesse osservazioni in [sys.dm_exec_query_statistics_xml](./sys-dm-exec-query-statistics-xml-transact-sql.md) .   

## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW SERVER STATE` per il server.  

## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>Passaggi successivi
 Per altri suggerimenti sullo sviluppo, vedere [Panoramica dello sviluppo di analisi delle sinapsi di Azure](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).