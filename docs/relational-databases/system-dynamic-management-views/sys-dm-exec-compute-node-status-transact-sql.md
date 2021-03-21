---
description: sys.dm_exec_compute_node_status (Transact-SQL)
title: sys.dm_exec_compute_node_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_COMPUTE_NODE_STATUS_TSQL
- DM_EXEC_COMPUTE_NODE_STATUS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_compute_node_status
- sys.dm_exec_compute_node_status management view
ms.assetid: b606f91f-3a08-4a4f-bb57-32ae155b3738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5878301bed9e18f2ae21f6706ad0eefa1318dbd0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750291"
---
# <a name="sysdm_exec_compute_node_status-transact-sql"></a>sys.dm_exec_compute_node_status (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Include informazioni aggiuntive sulle prestazioni e lo stato di tutti i nodi di base. Elenca una riga per nodo.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|compute_node_id|`int`|ID numerico univoco associato al nodo.|Univoco nel cluster con scalabilità orizzontale indipendentemente dal tipo.|  
|process_id|`int`|||  
|process_name|`nvarchar(255)`|Nome logico del nodo.|Qualsiasi stringa di lunghezza appropriata.|  
|allocated_memory|`bigint`|Memoria allocata totale in questo nodo.||  
|available_memory|`bigint`|Memoria totale disponibile in questo nodo.||  
|process_cpu_usage|`bigint`|Utilizzo totale della CPU del processo, in cicli.||  
|total_cpu_usage|`bigint`|Utilizzo totale della CPU, in cicli.||  
|thread_count|`bigint`|Numero totale di thread in uso in questo nodo.||  
|handle_count|`bigint`|Numero totale di handle in uso in questo nodo.||  
|total_elapsed_time|`bigint`|Tempo totale trascorso dall'avvio o dal riavvio del sistema.|Tempo totale trascorso dall'avvio o dal riavvio del sistema. Se total_elapsed_time supera il valore massimo per un numero intero (24,8 giorni in millisecondi), si verificherà un errore di materializzazione causato da un overflow. Il valore massimo in millisecondi equivale a 24,8 giorni.|  
|is_available|`bit`|Flag che indica se il nodo è disponibile.||  
|sent_time|`datetime`|Ora dell'ultima invio di un pacchetto di rete||  
|received_time|`datetime`|Ultima volta in cui un pacchetto di rete è stato inviato da questo nodo.||  
|error_id|`nvarchar(36)`|Identificatore univoco dell'ultimo errore che si è verificato in questo nodo.||
|compute_pool_id|`int`|Identificatore univoco per il pool.|

## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
