---
title: Rimuovere una replica secondaria da un gruppo di disponibilità
description: 'Passaggi per rimuovere una replica secondaria da un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removesecondaryar.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 35ddc8b6-3e7c-4417-9a0a-d4987a09ddf7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51211eafbc847a5ad7daae6fecc128a489116cd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344604"
---
# <a name="remove-a-secondary-replica-from-an-availability-group-sql-server"></a>Rimuovere una replica secondaria da un gruppo di disponibilità (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come rimuovere una replica secondaria da un gruppo di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
 
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Questa attività è supportata solo nella replica primaria.    
-   È possibile rimuove solo una replica secondaria da un gruppo di disponibilità.  
  
## <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria del gruppo di disponibilità.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per rimuovere una replica secondaria**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Selezionare il gruppo di disponibilità ed espandere il nodo **Repliche di disponibilità** .  
  
4.  Questo passaggio dipende dalla scelta di rimuovere più repliche o una sola replica, come indicato di seguito:  
  
    -   Per rimuovere più repliche, utilizzare il riquadro **Dettagli Esplora oggetti** per visualizzare e selezionare tutte le repliche che si desidera rimuovere. Per altre informazioni, vedere [Utilizzare Dettagli Esplora oggetti per monitorare Gruppi di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Per rimuovere una singola replica, selezionarla nel riquadro **Esplora oggetti** o **Dettagli Esplora oggetti** .  
  
5.  Fare clic con il pulsante destro del mouse sulla replica o sulle repliche secondarie selezionate e scegliere **Rimuovi da gruppo di disponibilità** nel menu dei comandi.  
  
6.  Nella finestra di dialogo **Rimozione delle repliche secondarie dal gruppo di disponibilità** scegliere **OK** per rimuovere tutte le repliche secondarie elencate. Se non si desidera rimuovere tutte le repliche elencate, fare clic su **Annulla**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per rimuovere una replica secondaria**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito:  
  
     ALTER AVAILABILITY GROUP *nome_gruppo* REMOVE REPLICA ON '*nome_istanza*' [,...*n*]  
  
     dove *nome_gruppo* è il nome del gruppo di disponibilità e *nome_istanza* è l'istanza del server in cui si trova la replica secondaria.  
  
     Nell'esempio seguente viene rimossa una replica secondaria dal gruppo di disponibilità *MyAG* . La replica secondaria di destinazione si trova in un'istanza del server denominata *HADR_INSTANCE* in un computer denominato *COMPUTER02*.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE REPLICA ON 'COMPUTER02\HADR_INSTANCE';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per rimuovere una replica secondaria**  
  
1.  Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **Remove-SqlAvailabilityReplica** .  
  
     Ad esempio, il seguente comando rimuove la replica di disponibilità nel server `MyReplica` dal gruppo di disponibilità denominato `MyAg`.  Il comando deve essere eseguito nell'istanza del server che ospita la replica primaria del gruppo di disponibilità.  
  
    ```  
    Remove-SqlAvailabilityReplica `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-replica"></a><a name="PostBestPractices"></a> Completamento: Dopo la rimozione di una replica secondaria  
 Se si specifica una replica che non è attualmente disponibile, quando viene portata online viene rilevato che è stata rimossa.  
  
 La rimozione di una replica ne arresta la ricezione di dati. Dopo la conferma della rimozione dall'archivio globale di una replica secondaria, la replica rimuove le impostazioni del gruppo di disponibilità dai relativi database che rimangono nell'istanza del server locale nello stato RECOVERING.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Aggiungere una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)   
 [Rimuovere un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
