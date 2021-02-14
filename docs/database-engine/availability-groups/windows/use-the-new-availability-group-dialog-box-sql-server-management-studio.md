---
title: Finestra di dialogo Nuovo gruppo di disponibilità in SSMS
description: Informazioni su come usare la finestra di dialogo Nuovo gruppo di disponibilità di SQL Server Management Studio per configurare un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 1b0a6421-fbd4-4bb4-87ca-657f4782c433
author: cawrites
ms.author: chadam
ms.openlocfilehash: ed9c9ec058062dd378bf29f78e1c60c49a654669
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355123"
---
# <a name="use-the-new-availability-group-dialog-box-sql-server-management-studio"></a>Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento sono incluse informazioni sulla modalità di uso della finestra di dialogo **Nuovo gruppo di disponibilità** di [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] per creare un gruppo di disponibilità AlwaysOn nelle istanze di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] abilitate per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Tramite un *gruppo di disponibilità* vengono definiti un set di database utente di cui verrà eseguito il failover come unità singola e un set di partner di failover, noti come *repliche di disponibilità*, che supportano il failover.  
  
> [!NOTE]  
>  Per un'introduzione ai gruppi di disponibilità, vedere [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
> [!NOTE]  
>  Per informazioni su modalità alternative di creazione di un gruppo di disponibilità, vedere [Attività correlate](#RelatedTasks), più avanti in questo argomento.  
  
  
##  <a name="prerequisites"></a><a name="PrerequisitesRestrictions"></a> Prerequisiti  
  
-   Prima di creare un gruppo di disponibilità, verificare che le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospitano repliche di disponibilità si trovino in un nodo del Clustering di failover di Windows Server (Windows Server Failover Clustering, WSFC) diverso all'interno dello stesso cluster di failover WSFC. Inoltre, verificare che ciascuna delle istanze del server sia abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] e soddisfi tutti gli altri prerequisiti di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Per altre informazioni, si consiglia di leggere [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Prima di creare un gruppo di disponibilità, assicurarsi che in ogni istanza del server in cui sarà ospitata una replica di disponibilità sia disponibile un endpoint del mirroring del database completamente funzionante. Per altre informazioni, vedere [Endpoint del mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
-   Per utilizzare la finestra di dialogo **Nuovo gruppo di disponibilità** è necessario conoscere i nomi delle istanze del server in cui saranno ospitate le repliche di disponibilità. Inoltre, è necessario conoscere i nomi di qualsiasi database che si vuole aggiungere al nuovo gruppo di disponibilità, nonché assicurarsi che questi database soddisfino i prerequisiti del database di disponibilità e le restrizioni descritte in [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). Se si immettono valori non validi, il nuovo gruppo di disponibilità non funzionerà.  
  
## <a name="limitations"></a><a name="Limitations"></a> Limitazioni  
 Nella finestra di dialogo **Nuovo gruppo di disponibilità** non è possibile effettuare le operazioni seguenti:  
  
-   Crea un listener del gruppo di disponibilità.    
-   Creare un join delle repliche secondarie al gruppo di disponibilità.    
-   Effettuare la sincronizzazione dei dati iniziale.  
  
 Per informazioni su queste attività di configurazione, vedere [Completamento: Operazioni da effettuare dopo l'utilizzo della finestra di dialogo Nuovo gruppo di disponibilità per creare un gruppo di disponibilità](#FollowUp) più avanti in questo argomento.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.  
  
##  <a name="using-the-new-availability-group-dialog-box-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo della finestra di dialogo Nuovo gruppo di disponibilità (SQL Server Management Studio)  
 **Per creare un gruppo di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server in cui è ospitata la replica primaria e fare clic sul nome del server.  
  
2.  Espandere il nodo **Disponibilità elevata Always On** .  
  
3.  Fare clic con il pulsante destro del mouse su **Gruppi di disponibilità** e selezionare il comando **Nuovo gruppo di disponibilità** .  
  
4.  Tramite questo comando verrà aperta la finestra di dialogo **Nuovo gruppo di disponibilità** .  
  
5.  Nel campo **Nome gruppo di disponibilità** della pagina **Generale** immettere un nome per il nuovo gruppo di disponibilità. Questo nome deve essere un identificatore di SQL Server valido univoco in tutti i gruppi di disponibilità del cluster WSFC. La lunghezza massima consentita per il nome del gruppo di disponibilità è 128 caratteri.  
  
6.  Nella griglia **Database di disponibilità** fare clic su **Aggiungi** e immettere il nome di un database locale che si desidera appartenga a questo gruppo di disponibilità. Ripetere questa operazione per ogni database da aggiungere. Quando si fa clic su **OK**, tramite la finestra di dialogo viene verificato se il database specificato soddisfa i prerequisiti per poter appartenere a un gruppo di disponibilità. Per informazioni sui prerequisiti, vedere [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
7.  Nella griglia **Database di disponibilità** fare clic su **Aggiungi** e immettere il nome di un'istanza del server per ospitare una replica secondaria. Non verrà effettuato il tentativo di connessione a queste istanze da parte della finestra di dialogo. Se si specifica un nome del server errato, verrà aggiunta una replica secondaria, tuttavia non sarà possibile effettuarvi una connessione.  
  
    > [!TIP]  
    >  Se è stata aggiunta una replica e non è possibile connettersi all'istanza del server host, è possibile rimuovere la replica e aggiungerne una nuova. Per altre informazioni, vedere [Rimuovere una replica secondaria da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) e [Aggiungere una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
8.  Nel riquadro **Selezione pagina** della finestra di dialogo fare clic su **Preferenze di backup**. Quindi, nella pagina **Preferenze di backup** specificare il punto in cui devono essere salvati i backup in base al ruolo della replica e assegnare le priorità di backup a ciascuna istanza del server in cui sarà ospitata una replica di disponibilità per questo gruppo di disponibilità. Per altre informazioni, vedere [Proprietà gruppo di disponibilità: Nuovo gruppo di disponibilità &#40;pagina Preferenze di backup&#41;](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md).  
  
9. Per creare il gruppo di disponibilità, fare clic su **OK**. In questo modo, tramite la finestra di dialogo viene verificato se i database specificati soddisfano i prerequisiti.  
  
     Per uscire dalla finestra di dialogo senza creare il gruppo di disponibilità, fare clic su **Annulla**.  
  
##  <a name="follow-up-after-using-the-new-availability-group-dialog-box-to-create-an-availability-group"></a><a name="FollowUp"></a> Completamento: Operazioni da effettuare dopo l'utilizzo della finestra di dialogo Nuovo gruppo di disponibilità per creare un gruppo di disponibilità  
  
-   Sarà necessario connettersi, a sua volta, a ogni istanza del server in cui è ospitata una replica secondaria per il gruppo di disponibilità e completare i passaggi seguenti:  
  
    1.  Creare un join della replica secondaria al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
    2.  Ripristinare i backup correnti di ogni database primario e il relativo log delle transazioni tramite RESTORE WITH NORECOVERY. Per altre informazioni, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
    3.  Creare immediatamente un join di ogni database secondario appena preparato al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
-   È consigliabile creare un listener per il nuovo gruppo di disponibilità. A questo scopo è necessario essere connessi all'istanza del server in cui è ospitata la replica primaria corrente. Per altre informazioni, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare le proprietà della replica e del gruppo di disponibilità**  
  
-   [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurare i criteri di failover flessibili per controllare le condizioni per il failover automatico &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Modificare il periodo di timeout della sessione per una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Per completare la configurazione del gruppo di disponibilità**  
  
-   [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Modalità alternative di creazione di un gruppo di disponibilità**  
  
-   [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Creare un gruppo di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Creare un gruppo di disponibilità &#40;PowerShell SQL Server&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **Per abilitare i gruppi di disponibilità AlwaysOn**  
  
-   [Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Per configurare un endpoint del mirroring del database**  
  
-   [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Per risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn**  
  
-   [Risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
