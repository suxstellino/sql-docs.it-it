---
title: Oggetto Processi di SQL Server Agent | Microsoft Docs
description: Informazioni sull'oggetto prestazione Jobs di SQL Server Agent, che include contatori delle prestazioni che contengono informazioni sui processi di questo servizio.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLAgent:Jobs
- Jobs object
ms.assetid: 225b5e2d-4a78-4178-b2b6-b419df83c4aa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34b4a1e9fc276da5e629d7521609bca5570a5798
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505930"
---
# <a name="sql-server-agent-jobs-object"></a>Oggetto Processi di SQL Server Agent
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto prestazioni **Processi** di SQL Server Agent include contatori delle prestazioni che forniscono informazioni relative ai processi di SQL Server Agent. Nella tabella seguente sono elencati i contatori inclusi nell'oggetto.  
  
 Nella tabella seguente sono illustrati i contatori di **SQLAgent:Processi** .  
  
|Nome|Descrizione|  
|----------|-----------------|  
|**Processi attivi**|Questo contatore indica il numero di processi attualmente in esecuzione.|  
|**Processi non riusciti**|Questo contatore indica il numero di processi chiusi con un errore.|  
|**Percentuale processi completati**|Questo contatore indica la percentuale di processi eseguiti completati correttamente.|  
|**Processi attivati/minuto**|Questo contatore indica il numero di processi avviati nell'ultimo minuto.|  
|**Processi in coda**|Questo contatore indica il numero di processi pronti per l'esecuzione in SQL Server Agent, ma la cui esecuzione non è ancora stata avviata.|  
|**Processi completati**|Questo contatore indica il numero di processi chiusi correttamente.|  
  
 Per ogni contatore nell'oggetto sono disponibili le istanze seguenti:  
  
|Istanza|Descrizione|  
|--------------|-----------------|  
|**_Total**|Informazioni relative a tutti i processi.|  
|**Avvisi**|Informazioni relative ai processi avviati da avvisi.|  
|**Altro**|Informazioni relative a processi non avviati da avvisi o pianificazioni. In genere, si tratta di processi avviati manualmente tramite **sp_start_job**.|  
|**Pianificazioni**|Informazioni relative ai processi avviati da pianificazioni.|  
  
## <a name="see-also"></a>Vedere anche  
 [Implementazione di processi](../../ssms/agent/implement-jobs.md)   
 [Utilizzo degli oggetti prestazioni](../../ssms/agent/use-performance-objects.md)   
 [Monitorare l'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
