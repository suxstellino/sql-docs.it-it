---
title: Prerequisiti per convertire il log shipping in gruppi di disponibilità
description: Descrizione dei prerequisiti necessari per convertire il log shipping in un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: 2738ce65-205e-4682-92d8-dc7e37c58b2b
author: cawrites
ms.author: chadam
ms.openlocfilehash: b6b06ecbd40dd54c527c7d32a8943a57aecbeafa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348417"
---
# <a name="prerequisites-to-convert-log-shipping-to-always-on-availability-groups"></a>Prerequisiti per convertire il log shipping in gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono descritti i prerequisiti per convertire un database primario per il log shipping insieme a uno o più dei relativi database secondari in un database primario AlwaysOn e nei relativi database secondari.  
  
> [!NOTE]  
>  In un gruppo di disponibilità è possibile configurare qualsiasi database primario o secondario (possibilmente leggibile) come un database primario per il log shipping.  
  
  
##  <a name="availability-group-prerequisites"></a><a name="AGPrereqsRealAddress"></a> Prerequisiti dei gruppi di disponibilità  
 Per consentire l'esecuzione dei processi di backup sulla replica primaria del gruppo di disponibilità, usare le impostazioni di backup seguenti per i gruppi di disponibilità AlwaysOn:  
  
|Proprietà|Impostazione|  
|--------------|-------------|  
|Preferenza di backup automatico del gruppo di disponibilità.|Solo nella replica primaria|  
|Priorità di backup della replica primaria.|>0|  
  
 **Per altre informazioni:**  
  
 [Visualizzazione delle Proprietà dei gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
 [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
##  <a name="log-shipping-prerequisites"></a><a name="LogShipPrereqs"></a> Prerequisiti per il log shipping  
  
-   È necessario che il database primario per il log shipping risieda nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita la replica primaria iniziale/corrente del gruppo di disponibilità.  
  
-   Per fare in modo che un determinato database secondario per il log shipping venga convertito in un database secondario AlwaysOn, tale database dovrà:  
  
    -   Utilizzare lo stesso nome del database primario.  
  
    -   Risiedere su un'istanza del server in cui viene ospitata una replica secondaria del gruppo di disponibilità.  
  
 Dopo l'esecuzione del processo di backup sul database primario, disabilitare il processo di backup e al termine dell'esecuzione del processo di ripristino su un determinato database secondario, disabilitare il processo di ripristino.  
  
 Dopo avere creato tutti i database secondari per il gruppo di disponibilità, se si desidera eseguire backup sulle repliche secondarie, è necessario configurare nuovamente la preferenza di backup automatico del gruppo di disponibilità.  
  
 **Per altre informazioni:**  
  
 [Converting a log shipping configuration to Availability Group](/archive/blogs/sqlalwayson/converting-a-logshipping-configuration-to-availability-group) (Conversione di una configurazione per il log shipping in un gruppo di disponibilità) (blog su SQL Server)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Log shipping**  
  
-   [Aggiornamento del log shipping a SQL Server 2016 &#40;Transact-SQL&#41;](../../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Rimuovere il log shipping &#40;SQL Server&#41;](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
 **Gruppi di disponibilità AlwaysOn**  
  
-   [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Creare un gruppo di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Creare un gruppo di disponibilità &#40;PowerShell SQL Server&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
-   [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   **Blog:**  
  
     [Converting a logshipping configuration to Availability Group](/archive/blogs/sqlalwayson/converting-a-logshipping-configuration-to-availability-group)  
  
     [Pagina relativa all'aggiunta di un database primario e di database secondari per il log shipping in un gruppo di disponibilità esistente](/archive/blogs/sqlalwayson/add-a-log-shipping-primary-database-and-secondary-databases-to-an-existing-availability-group)  
  
     [SQL Server AlwaysOn Team Blog: blog ufficiale del team di SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
-   **White paper:**  
  
     [Pagina relativa alla guida alla migrazione in cui viene illustrata la migrazione in gruppi di disponibilità AlwaysOn da distribuzioni precedenti in cui vengono combinati il mirroring del database e il log shipping](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))  
  
     [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Monitoraggio di Gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
