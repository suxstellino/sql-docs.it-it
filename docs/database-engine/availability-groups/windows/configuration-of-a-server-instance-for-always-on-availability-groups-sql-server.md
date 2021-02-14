---
title: Abilitare la funzionalità Gruppi di disponibilità per un'istanza di SQL Server
description: Viene descritto come abilitare la funzionalità Gruppi di disponibilità Always On per un'istanza di SQL Server.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], about
ms.assetid: fad8db32-593e-49d5-989c-39eb8399c416
author: cawrites
ms.author: chadam
ms.openlocfilehash: d40b3e89b9250b148f59ed78765fff9e2eaf47df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349310"
---
# <a name="enable-the-always-on-availability-group-feature-for-a-sql-server-instance"></a>Abilitare la funzionalità Gruppi di disponibilità Always On per un'istanza di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono fornite informazioni sui requisiti per la configurazione di un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] da supportare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Per informazioni di base sui prerequisiti e le limitazioni di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] per i nodi Windows Server Failover Clustering (WSFC) e per le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
   
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Termini e definizioni  
 [Gruppi di disponibilità AlwaysOn](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
 Soluzione di disponibilità elevata e recupero di emergenza che offre una sostituzione di livello enterprise al mirroring del database. Un *gruppo di disponibilità* supporta un ambiente di failover per un set discreto di database utente, noti come *database di disponibilità*, su cui si verifica il failover.  
  
 replica di disponibilità  
 Creazione di un'istanza di un gruppo di disponibilità ospitata da un'istanza specifica di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e che mantiene una copia locale di ogni database di disponibilità che appartiene al gruppo di disponibilità. Sono disponibili due tipi di replica di disponibilità: una *replica primaria* e da una a quattro *repliche secondarie*. Le istanze del server che ospitano le repliche di disponibilità per un determinato gruppo di disponibilità devono risiedere in nodi diversi di un solo cluster WSFC (Windows Server Failover Clustering).  
  
 [endpoint del mirroring del database](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
 È un oggetto di SQL Server che consente la comunicazione di SQL Server nella rete. Per fare parte del mirroring del database e/o di un'istanza del server [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] è richiesto un endpoint speciale e dedicato. Tutte le connessioni del mirroring e del gruppo di disponibilità in un'istanza del server utilizzano lo stesso endpoint del mirroring del database. Si tratta di un endpoint speciale utilizzato solo per ricevere tali connessioni da altre istanze del server.  
  
##  <a name="to-configure-a-server-instance-to-support-always-on-availability-groups"></a><a name="ConfigSI"></a> Per configurare un'istanza del server in modo che supporti i gruppi di disponibilità AlwaysOn  
 Per supportare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], un'istanza del server deve trovarsi in un nodo nel cluster di failover WSFC in cui è ospitato il gruppo di disponibilità, essere abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] e in essa deve essere presente un endpoint del mirroring del database.  
  
1.  Abilitare la funzionalità Gruppi di disponibilità AlwaysOn in ogni istanza del server che deve far parte di uno o più gruppi di disponibilità. Una determinata istanza del server può ospitare solo una singola replica di disponibilità per un gruppo di disponibilità specifico.  
  
2.  Verificare che nell'istanza del server sia incluso un endpoint del mirroring del database.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per abilitare i gruppi di disponibilità AlwaysOn**  
  
-   [Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Per determinare la presenza di un endpoint del mirroring del database**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
 **Per creare un endpoint del mirroring del database**  
  
-   [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   **Blog:**  
  
     [serie di informazioni su HADRON riguardanti l'uso del pool di lavoro per database abilitati HADRON in AlwaysOn](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server AlwaysOn Team Blog: blog ufficiale del team di SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
-   **Video:**  
  
     [Pagina relativa alla prima parte riguardante l'introduzione della soluzione a disponibilità elevata di prossima generazione della serie AlwaysOn di Microsoft SQL Server nome in codice "Denali"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Pagina relativa alla seconda parte riguardante la compilazione di una soluzione a disponibilità elevata critica tramite AlwasyOn della serie AlwaysOn di Microsoft SQL Server nome in codice "Denali"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **White paper:**  
  
     [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Gruppi di disponibilità AlwaysOn: interoperabilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Clustering di failover e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Istanze del cluster di failover Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
