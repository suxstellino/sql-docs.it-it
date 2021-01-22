---
title: SQL Server, Database Replica | Microsoft Docs
description: Informazioni sull'oggetto prestazioni SQLServer:Database Replica, che contiene contatori delle prestazioni sui database secondari di un gruppo di disponibilità Always On.
ms.custom: ''
ms.date: 01/13/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SQLServer:Database Replica
- performance counters [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], performance counters
ms.assetid: a5f6bdce-2b13-4924-aaeb-b50b57d624d8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3dff6cb9b3a65ea5f38a97786e10701bc1f31749
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170303"
---
# <a name="sql-server-database-replica"></a>SQL Server, replica di database

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto prestazioni **SQLServer:Database Replica** contiene contatori delle prestazioni che forniscono informazioni sui database secondari di un gruppo di disponibilità Always On in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Questo oggetto è valido solo su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita una replica secondaria.  
  
|Nome contatore|Descrizione|Contenuto in...|  
|------------------|-----------------|--------------|  
|**Byte file ricevuti/sec**|Quantità di dati FILESTREAM ricevuti dalla replica secondaria per il database secondario nell'ultimo secondo.|Replica secondaria|  
|**Coda log in attesa di applicazione**|Numero di blocchi di log in attesa di essere applicati alla replica del database.|Replica secondaria|
|**Coda log pronti per l'applicazione**|Numero di blocchi di log in attesa e pronti per essere applicati alla replica del database.|Replica secondaria|
|**Byte log ricevuti/sec**|Quantità di record del log ricevuti dalla replica secondaria per il database nell'ultimo secondo.|Replica secondaria|  
|**Log rimanente per il rollback**|La quantità di log in KB che deve ancora completare la fase di rollback.|Replica secondaria|  
|**Coda invii log**|Quantità di record di log nei file di log del database primario che non sono stati inviati alla replica secondaria (in KB). Questo valore viene inviato alla replica secondaria dalla replica primaria. Le dimensioni della coda non includono file FILESTREAM inviati a un database secondario.|Replica secondaria|  
|**Transazioni di scrittura con mirroring/sec**|Numero di transazioni che sono state scritte nel database primario e il cui commit viene eseguito solo quando il log è stato inviato al database secondario nell'ultimo secondo.|Replica primaria|  
|**Coda di recupero**|Quantità di record del log nei file di log della replica secondaria che non sono ancora stati sottoposti a rollforward.|Replica secondaria|  
|**Blocchi rollforward/sec**|Numero di volte in cui il thread della fase di rollforward viene bloccato nei blocchi utilizzati dai lettori del database.|Replica secondaria|  
|**Byte rollforward rimanenti**|Quantità di log in KB di cui deve essere eseguito il rollforward per completare la fase di ripristino.|Replica secondaria|  
|**Byte di cui è stato eseguito il rollforward/sec**|Quantità di record del log sottoposti a rollforward nel database secondario nell'ultimo secondo.|Replica secondaria|  
|**Totale log per cui è necessario il rollback**|Kilobyte di log totali che devono essere annullati.|Replica secondaria|  
|**Ritardo transazioni**|Ritardo di attesa totale per il riconoscimento di un commit senza terminazione per tutte le transazioni correnti, in millisecondi. Dividere per *Transazioni di scrittura con mirroring/sec* per ottenere il *ritardo delle transazioni medio*. Per altre informazioni, vedere [SQL Server 2012 AlwaysOn – Part 12 – Performance Aspects and Performance Monitoring II](/archive/blogs/saponsqlserver/sql-server-2012-alwayson-part-12-performance-aspects-and-performance-monitoring-ii) (SQL Server 2012 AlwaysOn – Parte 12 – Aspetti delle prestazioni e monitoraggio delle prestazioni II)|Replica primaria|  
  
## <a name="see-also"></a>Vedere anche
  
 [Monitoraggio dell'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, replica di disponibilità](../../relational-databases/performance-monitor/sql-server-availability-replica.md)   
 [SQL Server, oggetto di database](../../relational-databases/performance-monitor/sql-server-databases-object.md)   
 [Gruppi di disponibilità AlwaysOn di &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
