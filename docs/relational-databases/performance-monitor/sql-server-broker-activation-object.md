---
title: Oggetto Broker Activation di SQL Server | Microsoft Docs
description: Informazioni sull'oggetto prestazione SQLServer:Broker Activation, che include contatori delle prestazioni che contengono informazioni sull'attivazione tramite stored procedure.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Broker Activation
- Broker Activation object
ms.assetid: cd9b6880-c924-42c7-b333-09c303317c0b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8c257bee159905042ddece42102ef0a55a3512f1
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505865"
---
# <a name="sql-server-broker-activation-object"></a>Oggetto Attivazione Broker di SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto prestazione **SQLServer:Attivazione Broker** include contatori delle prestazioni che contengono informazioni sull'attivazione tramite stored procedure. Nella tabella seguente sono elencati i contatori inclusi nell'oggetto.  
  
|Contatori dell'oggetto Attivazione Broker di SQL Server|Descrizione|  
|-------------------------------------------|-----------------|  
|**Stored procedure richiamate/sec**|Questo contatore indica il numero totale di stored procedure di attivazione richiamate al secondo da tutti i monitoraggi di coda dell'istanza.|  
|**Totale limite attività raggiunto**|Questo contatore indica il numero totale di casi in cui un monitoraggio di coda non ha potuto avviare nuove attività in quanto è stato già raggiunto il numero massimo di attività in esecuzione per la coda.|  
|**Limite attività raggiunto/sec**|Questo contatore indica il numero di casi al secondo in cui un monitoraggio di coda non ha potuto avviare nuove attività in quanto è stato già raggiunto il numero massimo di attività in esecuzione per la coda.|  
|**Attività interrotte/sec**|Questo contatore indica il numero di stored procedure di attivazione terminate con un errore o interrotte da un monitoraggio di coda per la mancata ricezione di messaggi.|  
|**Attività in esecuzione**|Questo contatore indica il numero di stored procedure di attivazione attualmente in esecuzione.|  
|**Attività avviate/sec**|Questo contatore indica il numero totale di stored procedure di attivazione avviate al secondo da tutti i monitoraggi di coda dell'istanza.|  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_broker_activated_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-activated-tasks-transact-sql.md)   
 [sys.dm_broker_queue_monitors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-queue-monitors-transact-sql.md)   
 [Monitorare l'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
