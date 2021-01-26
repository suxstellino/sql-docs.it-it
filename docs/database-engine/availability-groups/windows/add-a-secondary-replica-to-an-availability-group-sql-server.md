---
title: Aggiungere una replica secondaria a un gruppo di disponibilità
description: Informazioni su come aggiungere una replica secondaria a un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o la Creazione guidata gruppo di disponibilità in SQL Server Management Studio (SSMS).
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 6669dcce-85f9-495f-aadf-7f62cff4a9da
author: cawrites
ms.author: chadam
ms.openlocfilehash: d5be1ee9057711414850e35d4226ad5c3f3346fd
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765502"
---
# <a name="add-a-secondary-replica-to-an-always-on-availability-group"></a>Aggiungere una replica secondaria a un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come aggiungere una replica secondaria a un gruppo di disponibilità Always On esistente usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  

  
##  <a name="prerequisites-and-restrictions"></a><a name="PrerequisitesRestrictions"></a> Prerequisiti e restrizioni  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria.  
  
 Per altre informazioni, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  

##  <a name="security"></a><a name="Security"></a> Sicurezza  
  
###  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  

[!INCLUDE[Freshness](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per aggiungere una replica**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic con il pulsante destro del mouse sul gruppo di disponibilità e selezionare uno dei comandi seguenti:  
  
    -   Per avviare la procedura guidata Aggiungi replica a gruppo di disponibilità, selezionare il comando **Aggiungi replica** . Per altre informazioni, vedere [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
    -   In alternativa, selezionare il comando **Proprietà** per aprire la finestra di dialogo **Proprietà gruppo di disponibilità** . I passaggi per l'aggiunta di una replica in questa finestra di dialogo sono indicati di seguito:  
  
        1.  Nel riquadro **Repliche di disponibilità** della finestra di dialogo fare clic sul pulsante **Aggiungi** . Verrà creata e selezionata una replica in cui il campo Istanza del server vuoto è selezionato.  
  
        2.  Immettere il nome di un'istanza del server che soddisfa i prerequisiti per ospitare una replica di disponibilità.  
  
         Per aggiungere repliche aggiuntive, ripetere i passaggi precedenti. Dopo avere specificato le repliche, fare clic su **OK** per completare l'operazione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per aggiungere una replica**  
  
1.  Connettersi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita la replica primaria.  
  
2.  Aggiungere la nuova replica secondaria al gruppo di disponibilità utilizzando la clausola ADD REPLICA ON dell'istruzione ALTER AVAILABILITY GROUP. Le opzioni ENDPOINT_URL, AVAILABILITY_MODE e FAILOVER_MODE sono obbligatorie in una clausola ADD REPLICA ON. Le altre opzioni di replica, BACKUP_PRIORITY, SECONDARY_ROLE, PRIMARY_ROLE e SESSION_TIMEOUT, sono facoltative. Per altre informazioni, vedere [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
     Ad esempio, nell'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente viene creata una nuova replica per un gruppo di disponibilità denominato `MyAG` sull'istanza del server predefinita ospitata da `COMPUTER04`, il cui URL dell'endpoint è `TCP://COMPUTER04.Adventure-Works.com:5022'`. Questa replica supporta il failover manuale e la modalità di disponibilità con commit asincrono.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG ADD REPLICA ON 'COMPUTER04'   
       WITH (  
             ENDPOINT_URL = 'TCP://COMPUTER04.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per aggiungere una replica**  
  
1.  Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **New-SqlAvailabilityReplica** .  
  
     Ad esempio, il seguente comando aggiunge una replica di disponibilità per un gruppo di disponibilità esistente denominato `MyAg`. Questa replica supporta il failover manuale e la modalità di disponibilità con commit asincrono. Nel ruolo secondario, questa replica supporterà le connessioni con accesso in lettura consentendo all'utente di ripartire il carico dell'elaborazione di sola lettura in questa replica.  
  
    ```  
    $agPath = "SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
    $endpointURL = "TCP://PrimaryServerName.domain.com:5022"  
    $failoverMode = "Manual"  
    $availabilityMode = "AsynchronousCommit"  
    $secondaryReadMode = "AllowAllConnections"  
  
    New-SqlAvailabilityReplica -Name SecondaryServer\Instance `   
    -EndpointUrl $endpointURL `   
    -FailoverMode $failoverMode `   
    -AvailabilityMode $availabilityMode `   
    -ConnectionModeInSecondaryRole $secondaryReadMode `   
    -Path $agPath  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-adding-a-secondary-replica"></a><a name="FollowUp"></a> Completamento: Dopo l'aggiunta di una replica secondaria  
 Per aggiungere una replica per un gruppo di disponibilità esistente, è necessario effettuare i passaggi seguenti:  
  
1.  Connettersi all'istanza del server che ospiterà la nuova replica secondaria.  
  
2.  Creare un join della nuova replica secondaria al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
3.  Per ogni database nel gruppo di disponibilità, creare un database secondario nell'istanza del server che ospita la replica secondaria. Per altre informazioni, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
4.  Creare un join dei nuovi database secondari al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per gestire una replica di disponibilità**  
  
-   [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Rimuovere una replica secondaria da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Modificare il periodo di timeout della sessione per una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
-   [Modificare il periodo di timeout della sessione per una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Usare il Dashboard AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
