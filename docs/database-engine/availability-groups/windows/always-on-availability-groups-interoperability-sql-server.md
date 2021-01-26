---
title: 'Gruppi di disponibilità Always On: interoperabilità'
description: Descrive le diverse funzionalità che possono e non possono funzionare insieme a un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 04/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], interoperability
ms.assetid: daf87f90-2623-42ca-912c-b8f07d210510
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1c0322b1b00b3f5cbff775d3c248ee81916f48a9
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765137"
---
# <a name="always-on-availability-groups-interoperability-sql-server"></a>Gruppi di disponibilità Always On: interoperabilità (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In questo argomento viene documentata l'interoperabilità di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con altre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] funzionalità di.

## <a name="features-that-interoperate-with-always-on-availability-groups"></a><a name="Interop"></a> Funzionalità che interagiscono con i gruppi di disponibilità Always On

Nella tabella seguente sono elencate le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] funzionalità di interoperabilità con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Un collegamento nella colonna **Ulteriori informazioni** indica che per una determinata funzionalità esistono le considerazioni di interoperabilità.

|Funzionalità|Altre informazioni|
|:------|:---------------|
|Change Data Capture|[Replica, Rilevamento modifiche, Change Data Capture e Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Rilevamento modifiche|[Replica, Rilevamento modifiche, Change Data Capture e Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Database indipendenti|[Database indipendenti con gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/contained-databases-with-always-on-availability-groups-sql-server.md)|
|Crittografia del database|[Database crittografati con gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)|
|Snapshot del database|[Snapshot del database con gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)|
|FILESTREAM e tabelle FileTable|[FILESTREAM e FileTable con gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md)|
|Ricerca full-text|Nota: gli indici full-text sono sincronizzati con i database secondari Always On.|
|Log shipping|[Prerequisiti per la migrazione dal log shipping ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)|
|Archivio BLOB remoto (RBS)|[Archivio BLOB remoti &#40;RBS&#41; e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)|
|Replica|[Configurare la replica per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)<br /><br /> [Gestione di un database di pubblicazione AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)<br /><br /> [Replica, Rilevamento modifiche, Change Data Capture e Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)<br /><br /> [Sottoscrittori della replica e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)|
|Analysis Services|[Analysis Services con i gruppi di disponibilità AlwaysOn](../../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md)|
|Reporting Services|Utilizzare solo repliche secondarie come origine dei dati di Reporting Services e ridurre il carico nella replica primaria di lettura e scrittura.<br /><br /> [Reporting Services con i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)|
|Broker di servizio|[Service Broker con i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)|
|SQL Server Agent|&nbsp;|

## <a name="features-that-interoperate-with-always-on-availability-groups-with-restrictions"></a><a name="restrictions"></a> Funzionalità in grado di interagire con i gruppi di disponibilità AlwaysOn con restrizioni

Le funzionalità seguenti interagiscono con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con restrizioni specifiche. Per informazioni dettagliate, vedere gli argomenti collegati.

- Transazioni tra database/distribuite ([!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] e Windows Server 2016). Per altre informazioni, vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)
- L'[agente di raccolta dati del sistema di statistiche query](../../../relational-databases/data-collection/system-data-collection-set-reports.md#Query) non può essere eseguito in modo affidabile in un ambiente con database secondari non leggibili. Per usare l'agente di raccolta dati del sistema di statistiche query, impostare tutte le repliche del gruppo di disponibilità secondario in modo da consentire l'[accesso in lettura](configure-read-only-access-on-an-availability-replica-sql-server.md). 

## <a name="features-that-do-not-interoperate-with-always-on-availability-groups"></a><a name="NoInterop"></a> Funzionalità prive di interoperabilità con i gruppi di disponibilità AlwaysOn

[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] non è in grado di interagire con le funzionalità:

- Mirroring del database. Per altre informazioni, vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

## <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato

- **Blog:**

  [Guida alla migrazione: Migrazione al clustering di failover e ai gruppi di disponibilità di SQL Server 2012 da distribuzioni clustering e mirroring precedenti](/archive/blogs/sqlalwayson/now-available-migration-guide-migrating-to-sql-server-2012-failover-clustering-and-availability-groups-from-prior-clustering-and-mirroring-deployments)
  [Blog del team di SQL Server Always On: blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)
  [Blog dei tecnici del Servizio Supporto Tecnico per SQL Server](/archive/blogs/psssql/)

- **White paper:**

  [Guida alla migrazione: Migrazione a gruppi di disponibilità Always On da distribuzioni precedenti che combinano mirroring del database e log shipping](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))
  [Guida alle soluzioni Microsoft SQL Server Always On per la disponibilità elevata e il ripristino di emergenza](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))
  [White paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)
  [White paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)

## <a name="see-also"></a>Vedere anche

[Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
[Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)