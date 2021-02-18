---
title: Considerazioni sulla riproduzione di tracce
titleSuffix: SQL Server Profiler
description: Individuare le operazioni, le stored procedure, i modelli e le attività di log che impediscono a SQL Server Profiler di riprodurre le tracce.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 73fa339f-b71a-4be4-97ca-d4ae84c8b90b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.openlocfilehash: d06b9769a76d7e6555e0dc671782fcd062b4574f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349427"
---
# <a name="considerations-for-replaying-traces-sql-server-profiler"></a>Considerazioni relative alla riproduzione di tracce (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] non può riprodurre i tipi di tracce seguenti:  
  
-   Tracce contenenti replica transazionale e altre attività del log delle transazioni. Questi eventi verranno ignorati. Gli altri tipi di replica non contrassegnano il log delle transazioni e pertanto non subiscono alcun effetto.  
  
-   Tracce contenenti operazioni che coinvolgono identificatori univoci globali (GUID). Questi eventi verranno ignorati.  
  
-   Tracce contenenti operazioni su colonne **text**, **ntext** e **image** che coinvolgono l'utilità **bcp** , istruzioni BULK INSERT, READTEXT, WRITETEXT e UPDATETEXT e operazioni full-text. Questi eventi verranno ignorati.  
  
-   Tracce contenenti operazioni di associazione della sessione, ovvero le stored procedure di sistema **sp_getbindtoken** e **sp_bindsession** . Questi eventi verranno ignorati.  
  
> [!NOTE]  
>  Se non si usa il modello di riproduzione preconfigurato (**TSQL_Replay**) e non si acquisiscono tutti i dati necessari, la traccia non verrà riprodotta in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Per altre informazioni, vedere [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md).  
  
 Per informazioni sulle autorizzazioni necessarie per riprodurre una traccia, vedere [Autorizzazioni necessarie per l'esecuzione di SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [Guida di riferimento alle classi di evento SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [sp_getbindtoken &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getbindtoken-transact-sql.md)   
 [sp_bindsession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)  
  
  
