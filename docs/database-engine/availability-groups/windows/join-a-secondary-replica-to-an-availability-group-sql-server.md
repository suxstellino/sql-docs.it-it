---
title: Creare un join di una replica secondaria a un gruppo di disponibilità
description: Passaggi per aggiungere una replica secondaria a un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.joinreplica.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], joining
- Availability Groups [SQL Server], configuring
ms.assetid: e5bd2489-097a-490e-8ea1-34fe48378ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4f9f35186d57900628f39a593e3e338ed89a9d89
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766416"
---
# <a name="join-a-secondary-replica-to-an-always-on-availability-group"></a>Creare un join di una replica secondaria a un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come creare un join di una replica secondaria a un gruppo di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Dopo l'aggiunta di una replica secondaria a un gruppo di disponibilità AlwaysOn, è necessario creare un join della replica secondaria al gruppo di disponibilità. L'operazione di join della replica deve essere eseguita nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui viene ospitata la replica secondaria.  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   La replica primaria del gruppo di disponibilità deve essere attualmente online.    
-   È necessario essere connessi all'istanza del server che ospita una replica secondaria di cui non sia ancora stato creato un join al gruppo di disponibilità.    
-   L'istanza del server locale deve essere in grado di connettersi all'endpoint del mirroring del database dell'istanza del server che ospita la replica primaria.  
  
> [!IMPORTANT]  
>  Se nessuno dei prerequisiti viene soddisfatto, l'operazione di join non viene completata. Al termine di un tentativo di join errato, potrebbe essere necessario connettersi all'istanza del server in cui è ospitata la replica primaria per rimuovere e aggiungere nuovamente la replica secondaria, prima di poter creare un join al gruppo di disponibilità. Per altre informazioni, vedere [Rimuovere una replica secondaria da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) e [Aggiungere una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per creare un join di una replica di disponibilità a un gruppo di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server in cui viene ospitata la replica secondaria e fare clic sul nome del server per espandere il relativo albero.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Selezionare il gruppo di disponibilità della replica secondaria a cui si è connessi.  
  
4.  Fare clic con il pulsante destro del mouse sulla replica secondaria e scegliere **Crea un join del gruppo di disponibilità**.  
  
5.  In questo modo verrà aperta la finestra di dialogo **Creare un join della replica al gruppo di disponibilità** .  
  
6.  Per creare un join della replica secondaria al gruppo di disponibilità, fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per creare un join di una replica di disponibilità a un gruppo di disponibilità**  
  
1.  Connettersi all'istanza del server che ospita la replica secondaria.  
  
2.  Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito:  
  
     ALTER AVAILABILITY GROUP *nome_gruppo* JOIN  
  
     dove *nome_gruppo* è il nome del gruppo di disponibilità.  
  
     Nell'esempio seguente viene creato un join della replica secondaria al gruppo di disponibilità `MyAG`.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG JOIN;  
    ```  
  
    > [!NOTE]  
    >  Per un esempio di questa istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] impiegata in un contesto, vedere [Creare un gruppo di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per creare un join di una replica di disponibilità a un gruppo di disponibilità**  
  
 Nel provider PowerShell per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
1.  Spostarsi nella directory (**cd**) dell'istanza del server che ospita la replica secondaria.  
  
2.  Creare un join della replica secondaria al gruppo di disponibilità eseguendo il cmdlet **Join-SqlAvailabilityGroup** con il nome del gruppo di disponibilità.  
  
     Ad esempio, tramite il comando seguente è possibile creare un join di una replica secondaria ospitata dall'istanza del server presente nel percorso specificato al gruppo di disponibilità denominato `MyAg`.  Questa istanza del server deve ospitare una replica secondaria in questo gruppo di disponibilità.  
  
    ```  
    Join-SqlAvailabilityGroup -Path SQLSERVER:\SQL\SecondaryServer\InstanceName -Name 'MyAg'  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-configure-secondary-databases"></a><a name="FollowUp"></a> Completamento: Configurare i database secondari  
 Per ogni database nel gruppo di disponibilità, è necessario un database secondario nell'istanza del server in cui viene ospitata la replica secondaria. È possibile configurare i database secondari prima o dopo la creazione di un join di una replica secondaria a un gruppo di disponibilità, come indicato di seguito:  
  
1.  Ripristinare i backup dei log e dei database recenti di ogni database primario nell'istanza del server in cui viene ospitata la replica secondaria, utilizzando RESTORE WITH NORECOVERY per ogni operazione di ripristino. Per altre informazioni, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
2.  Creare un join di ogni database secondario al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
