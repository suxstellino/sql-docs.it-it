---
title: Avviare lo spostamento dei dati su un database secondario
description: Informazioni su come avviare lo spostamento dei dati per un database secondario che fa parte di un gruppo di disponibilità Always On in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], databases
ms.assetid: 498eb3fb-6a43-434d-ad95-68a754232c45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 262e797573690f8a648b552e782b0161f41ca7cc
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643000"
---
# <a name="start-data-movement-on-an-always-on-secondary-database-sql-server"></a>Avviare lo spostamento dati su un database secondario AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono fornite informazioni relative all'avvio della sincronizzazione dati dopo l'aggiunta di un database a un gruppo di disponibilità AlwaysOn. Per ogni nuova replica primaria, è necessario preparare i database secondari nelle istanze del server che ospitano le repliche secondarie. È quindi necessario eseguire manualmente il join di ciascuno di questi database secondari al gruppo di disponibilità.  
  
> [!NOTE]  
>  Se i percorsi dei file sono identici in ogni istanza del server che ospita una replica di disponibilità per un gruppo di disponibilità, è possibile che la sincronizzazione dati venga avviata automaticamente dalla [Creazione guidata Gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), dalla procedura guidata [Aggiungi database a gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)o dalla procedura guidata [Aggiungi database a gruppo di disponibilità](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md) .  
  
 Per avviare manualmente la sincronizzazione dati, è necessario connettersi a ogni istanza del server in cui è ospitata una replica secondaria per il gruppo di disponibilità e completare i passaggi seguenti:  
  
1.  Ripristinare i backup correnti di ogni database primario e il relativo log delle transazioni tramite RESTORE WITH NORECOVERY. È possibile utilizzare uno dei metodi alternativi seguenti:  
  
    -   Ripristinare manualmente un backup recente del database primario utilizzando RESTORE WITH NORECOVERY, quindi ripristinare ogni successivo backup del log utilizzando RESTORE WITH NORECOVERY. Eseguire questa sequenza di ripristino in ogni istanza del server che ospita una replica secondaria per il gruppo di disponibilità.  
  
         **Per altre informazioni:**  
  
         [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
    -   Se vengono aggiunti uno o più database primari per il log shipping a un gruppo di disponibilità, è possibile eseguire la migrazione di uno o più database secondari corrispondenti dal log shipping ai gruppi di disponibilità AlwaysOn. Per la migrazione di un database secondario per il log shipping è necessario che venga utilizzato lo stesso nome di database del database primario e che il database risieda in un'istanza del server in cui è ospitata una replica secondaria per il gruppo di disponibilità. Il gruppo di disponibilità deve inoltre essere configurato in modo che la replica primaria sia quella preferita per i backup e sia un candidato per l'esecuzione di backup, ovvero che la relativa priorità di backup sia >0. Dopo l'esecuzione del processo di backup sul database primario, sarà necessario disabilitare il processo di backup e al termine dell'esecuzione del processo di ripristino su un determinato database secondario, disabilitare il processo di ripristino.  
  
        > [!NOTE]  
        >  Dopo aver creato tutti i database secondari per il gruppo di disponibilità, se si desidera eseguire backup nelle repliche secondarie, sarà necessario configurare nuovamente le preferenze di backup automatico del gruppo di disponibilità.  
  
         **Per altre informazioni:**  
  
         [Prerequisiti per la migrazione dal log shipping ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
         [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
2.  Creare al più presto un join di ogni database secondario appena preparato al gruppo di disponibilità.  
  
     **Per altre informazioni:**  
  
     [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> Attività correlate  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi database a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
