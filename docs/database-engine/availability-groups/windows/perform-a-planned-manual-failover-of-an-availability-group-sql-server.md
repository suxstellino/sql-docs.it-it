---
title: Eseguire un failover manuale pianificato di un gruppo di disponibilità
description: Questo argomento descrive come eseguire un failover manuale pianificato di un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 10/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.manualfailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 419f655d-3f9a-4e7d-90b9-f0bab47b3178
author: cawrites
ms.author: chadam
ms.openlocfilehash: e5efd8802ea57a6575f9e960b44ffffd9a32f83e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350785"
---
# <a name="perform-a-planned-manual-failover-of-an-always-on-availability-group-sql-server"></a>Eseguire un failover manuale pianificato di un gruppo di disponibilità Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
In questo argomento viene illustrato come eseguire un failover manuale senza perdite di dati ( *failover manuale pianificato*) in un gruppo di disponibilità AlwaysOn tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Per un gruppo di disponibilità il failover si verifica al livello di una replica di disponibilità. Un failover manuale pianificato, ad esempio un failover del gruppo di disponibilità AlwaysOn, comporta il passaggio della replica primaria precedente al ruolo secondario. Il failover attualmente comporta il passaggio della replica primaria precedente al ruolo secondario.  
  
Un failover manuale pianificato è supportato solo quando la replica principale e la replica secondaria di destinazione sono in esecuzione in modalità commit sincrono e sono attualmente sincronizzate. Un failover manuale pianificato mantiene tutti i dati presenti nei database secondari associati al gruppo di disponibilità nella replica secondaria di destinazione. Dopo che la replica primaria precedente è passata al ruolo secondario, i relativi database diventano database secondari. Iniziano quindi a eseguire la sincronizzazione con i nuovi database primari. Dopo la transizione di tutti i database allo stato SYNCHRONIZED, la nuova replica secondaria diventa idonea a fungere da destinazione di un futuro failover manuale pianificato.  
  
> [!NOTE]  
>  Se la replica primaria e le repliche secondarie sono configurate per la modalità di failover automatico, dopo la sincronizzazione, la replica secondaria può anche fungere da destinazione per un failover automatico. Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare 

>[!IMPORTANT]
>Esistono procedure specifiche per eseguire il failover di un gruppo di disponibilità per la scalabilità in lettura senza usare uno strumento di gestione cluster. Quando un gruppo di disponibilità contiene CLUSTER_TYPE = NONE, seguire le procedure descritte in [Eseguire il failover di una replica primaria in un gruppo di disponibilità per scalabilità in lettura](#fail-over-the-primary-replica-on-a-read-scale-availability-group).

###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni 
  
- Un comando del failover viene restituito non appena la replica secondaria di destinazione ha accettato il comando. Tuttavia, il recupero del database si verifica in modo asincrono dopo che il gruppo di disponibilità ha completato il failover. 
- La coerenza tra i database all'interno del gruppo di disponibilità potrebbe non essere mantenuta nel failover. 
  
    > [!NOTE] 
    >  Il supporto delle transazioni distribuite e tra database varia in base alle versioni di SQL Server e del sistema operativo. Per altre informazioni, vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
  
###  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Prerequisiti e restrizioni 
  
-   La replica secondaria di destinazione e la replica primaria devono essere entrambe in esecuzione in modalità di disponibilità con commit sincrono. 
-   La replica secondaria di destinazione deve essere attualmente sincronizzata con la replica primaria. Per tutti i database secondari della replica secondaria deve esser creato un join al gruppo di disponibilità. Devono essere sincronizzati anche con i database primari corrispondenti (ovvero i database secondari locali devono essere SINCRONIZZATI). 
  
    > [!TIP] 
    >  Per determinare la conformità del failover di una replica secondaria, eseguire una query della colonna **is_failover_ready** nella DMV [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md). Oppure è possibile esaminare la colonna **Conformità Failover** del [dashboard del gruppo AlwaysOn](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md). 
-   Questa attività è supportata solo nella replica secondaria di destinazione. È necessario essere connessi all'istanza del server che ospita la replica secondaria di destinazione. 
  
###  <a name="security"></a><a name="Security"></a> Sicurezza 
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni 
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità. È necessaria anche l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP oppure l'autorizzazione CONTROL SERVER. 
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio 
 Per eseguire manualmente il failover di un gruppo di disponibilità: 
  
1. In Esplora oggetti connettersi a un'istanza del server che ospita una replica secondaria del gruppo di disponibilità di cui eseguire il failover. Espandere l'albero di server. 
  
2. Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità** . 
  
3. Fare clic con il pulsante destro del mouse sul gruppo di disponibilità di cui eseguire il failover e selezionare **Failover**. 
  
4. Avvio della procedura guidata Gruppo di disponibilità di failover. Per altre informazioni, vedere [Usare la procedura guidata Failover gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md). 
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Usare Transact-SQL 
 Per eseguire manualmente il failover di un gruppo di disponibilità: 
  
1. Connettersi all'istanza del server che ospita la replica secondaria di destinazione. 
  
2. Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito: 
  
     ALTER AVAILABILITY GROUP *nome_gruppo* FAILOVER 
  
     Nell'istruzione *nome_gruppo* è il nome del gruppo di disponibilità. 
  
     Nell'esempio seguente viene eseguito il failover manuale del gruppo di disponibilità *MyAg* alla replica secondaria connessa: 
  
    ```  
    ALTER AVAILABILITY GROUP MyAg FAILOVER;  
    ```  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Usare PowerShell 
 Per eseguire manualmente il failover di un gruppo di disponibilità: 
  
1. Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica secondaria di destinazione. 
  
2. Usare il cmdlet **Switch-SqlAvailabilityGroup** . 
  
    > [!NOTE] 
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere la [guida SQL Server PowerShell](../../../powershell/sql-server-powershell.md). 
  
     Nell'esempio seguente viene eseguito il failover manuale del gruppo di disponibilità *MyAg* alla replica secondaria con il percorso specificato: 
  
    ```  
    Switch-SqlAvailabilityGroup -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg  
    ```  
  
    Per impostare e usare il provider PowerShell per SQL Server: 
  
    -   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md) 
    -   [Visualizzare la Guida di SQL Server PowerShell](../../../powershell/sql-server-powershell.md) 

##  <a name="follow-up-after-you-manually-fail-over-an-availability-group"></a><a name="FollowUp"></a> Completamento: dopo il failover manuale su un gruppo di disponibilità 
 Se è stato eseguito il failover al di fuori del [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] del gruppo di disponibilità, modificare i voti del quorum dei nodi di clustering di failover Windows Server per riflettere la nuova configurazione del gruppo di disponibilità. Per altre informazioni, vedere [Clustering di failover Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md). 

<a name = "ReadScaleOutOnly"><a/>

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Eseguire il failover della replica primaria in un gruppo di disponibilità per scalabilità in lettura

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="see-also"></a>Vedere anche 

 * [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
 * [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 
 * [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) 
  
