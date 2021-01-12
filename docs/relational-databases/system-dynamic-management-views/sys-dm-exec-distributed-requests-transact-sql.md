---
description: sys.dm_exec_distributed_requests (Transact-SQL)
title: sys.dm_exec_distributed_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_DISTRIBUTED_REQUESTS
- DM_EXEC_DISTRIBUTED_REQUESTS_TSQL
- SYS.DM_EXEC_DISTRIBUTED_REQUESTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- sys.dm_exec_distributed_sql_requests management view
- PolyBase
- dm_exec_distributed_sql_requests management view
ms.assetid: c041d416-d8c6-435e-a563-6a310abd33e3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7c201260484919e307b96cd37240179a4e1ddbec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094064"
---
# <a name="sysdm_exec_distributed_requests-transact-sql"></a>sys.dm_exec_distributed_requests (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Include informazioni su tutte le richieste attualmente o di recente attive nelle query di base. Elenca una riga per ogni richiesta/query.  
  
 In base all'ID della sessione e della richiesta, un utente può recuperare le richieste distribuite effettive generate per l'esecuzione tramite sys.dm_exec_distributed_requests. Ad esempio, una query che include le normali tabelle SQL ed esterne verrà scomposta in varie istruzioni/richieste eseguite nei vari nodi di calcolo. Per tenere traccia dei passaggi distribuiti in tutti i nodi di calcolo, viene introdotto un ID di esecuzione globale che può essere usato per tenere traccia di tutte le operazioni sui nodi di calcolo associati rispettivamente a una richiesta e a un operatore specifici.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|sql_handle|**varbinary(64)**|Chiave per questa visualizzazione. ID numerico univoco associato alla richiesta.|Univoco tra tutte le richieste nel sistema.|  
|execution_id|**nvarchar (32**|ID numerico univoco associato alla sessione in cui è stata eseguita la query.||  
|status|**nvarchar (32**|Stato corrente della richiesta.|' Pending ',' autorizzazione ',' AcquireSystemResources ',' inizializzazione ',' Plan ',' parsing ',' AquireResources ',' running ',' Canceling ',' complete ',' failed ',' annullato '.|  
|error_id|**nvarchar (36)**|ID univoco dell'errore associato alla richiesta, se disponibile.|Impostare su NULL se non si è verificato alcun errore.|  
|start_time|**datetime**|Ora in cui è stata avviata l'esecuzione della richiesta.|0 per le richieste in coda; in caso contrario, DateTime valido minore o uguale all'ora corrente.|  
|end_time|**datetime**|Ora di completamento della compilazione della richiesta da parte del motore.|Null per le richieste in coda o attive; in caso contrario, un valore DateTime valido minore o uguale all'ora corrente.|  
|total_elapsed_time|**int**|Tempo trascorso nell'esecuzione dall'avvio della richiesta, in millisecondi.|Compreso tra 0 e la differenza tra start_time e end_time. Se total_elapsed_time supera il valore massimo per un numero intero, total_elapsed_time continuerà a essere il valore massimo. Questa condizione genererà l'avviso "è stato superato il valore massimo". Il valore massimo in millisecondi equivale a 24,8 giorni.|  
  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
