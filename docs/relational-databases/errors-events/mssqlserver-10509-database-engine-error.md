---
description: MSSQLSERVER_10509
title: MSSQLSERVER_10509 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10509 (Database Engine error)
ms.assetid: e9dd5357-ee3d-420a-9a89-d12ab5404e73
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 71f607c8573ad9c309f6b4dc4af2de8690590fe2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197453"
---
# <a name="mssqlserver_10509"></a>MSSQLSERVER_10509
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|10509|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|PG_INVALID_STMT|  
|Testo del messaggio|Impossibile creare la guida di piano '%.\*ls' perché l'istruzione specificata da **\@stmt** o **\@statement_start_offset** contiene un errore di sintassi o non può essere usata in una guida di piano. Specificare una singola istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] valida o una posizione di inizio valida dell'istruzione nel batch. Per ottenere una posizione di inizio valida, eseguire una query sulla colonna statement_start_offset nella funzione a gestione dinamica sys.dm_exec_query_stats.|  
  
## <a name="explanation"></a>Spiegazione  
L'istruzione specificata da **\@stmt** o **\@statement_start_offset** contiene un errore di sintassi o non può essere usata in una guida di piano.  
  
## <a name="user-action"></a>Azione dell'utente  
Specificare una singola istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] valida o una posizione di inizio valida dell'istruzione nel batch. Per ottenere una posizione di inizio valida, eseguire una query sulla colonna statement_start_offset nella funzione a gestione dinamica sys.dm_exec_query_stats.  
  
## <a name="see-also"></a>Vedere anche  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[Guide di piano](~/relational-databases/performance/plan-guides.md)  
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
