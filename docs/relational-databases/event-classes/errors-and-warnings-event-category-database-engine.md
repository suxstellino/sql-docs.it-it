---
description: Categoria di eventi Errori e avvisi (Motore di database)
title: Categoria di eventi Errori e avvisi
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Errors and Warnings event category [SQL Server]
- SQL Server event classes, Errors and Warnings event category
- event classes [SQL Server], Errors and Warnings event category
ms.assetid: 249c19b5-af68-4433-80f6-337395176641
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 16d74c7559f1645a84cffb251d6a7fbc62ada36b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183734"
---
# <a name="errors-and-warnings-event-category-database-engine"></a>Categoria di eventi Errori e avvisi (Motore di database)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La categoria di eventi **Errori e avvisi** include eventi generali relativi a errori e avvisi.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Classe di evento Attention](../../relational-databases/event-classes/attention-event-class.md)|Indica che è si è verificato un evento **Attention** .|  
|[Classe di evento Background Job Error](../../relational-databases/event-classes/background-job-error-event-class.md)|Indica l'interruzione anomala di un processo in background.|  
|[Classe di evento Bitmap Warning](../../relational-databases/event-classes/bitmap-warning-event-class.md)|Indica che il filtro bitmap è stato disabilitato in una query.|  
|[Classe di evento Blocked Process Report](../../relational-databases/event-classes/blocked-process-report-event-class.md)|Indica che un'attività è rimasta bloccata per un periodo di tempo maggiore di quello specificato.|  
|[Classe di evento CPU Threshold Exceeded](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)|Indica che il Resource Governor rileva una query che supera la soglia della CPU specificata.|  
|[Classe di evento ErrorLog](../../relational-databases/event-classes/errorlog-event-class.md)|Indica che sono stati registrati eventi di errore nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Classe di evento EventLog](../../relational-databases/event-classes/eventlog-event-class.md)|Indica la registrazione di eventi nel registro eventi di Windows.|  
|[Classe di evento Exception](../../relational-databases/event-classes/exception-event-class.md)|Indica la generazione di un'eccezione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Classe di evento Exchange Spill](../../relational-databases/event-classes/exchange-spill-event-class.md)|Indica che i buffer di comunicazione in un piano di query parallelo sono stati scritti nel database tempdb.|  
|[Classe di evento Execution Warnings](../../relational-databases/event-classes/execution-warnings-event-class.md)|Indica la generazione di avvisi di concessione di memoria durante l'esecuzione di un'istruzione o di una stored procedure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Classe di evento Hash Warning](../../relational-databases/event-classes/hash-warning-event-class.md)|Indica il verificarsi di una ricorsione di hash o un hash bailout durante un'operazione di hashing.|  
|[Classe di evento Missing Column Statistics](../../relational-databases/event-classes/missing-column-statistics-event-class.md)|Indica che le statistiche di colonna utili per Query Optimizer non sono disponibili.|  
|[Classe di evento Missing Join Predicate](../../relational-databases/event-classes/missing-join-predicate-event-class.md)|Indica che è in esecuzione una query senza predicato di join.|  
|[Classe di evento Sort Warnings](../../relational-databases/event-classes/sort-warnings-event-class.md)|Indica le operazioni di ordinamento per cui la memoria disponibile risulta insufficiente.|  
|[Classe di evento User Error Message](../../relational-databases/event-classes/user-error-message-event-class.md)|Visualizza messaggi di errore per gli utenti.|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
