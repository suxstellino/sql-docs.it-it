---
title: Criteri per visualizzare l'integrità di un gruppo di disponibilità
description: Usare i criteri Always On o PowerShell per determinare l'integrità operativa di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6f1bcbc3-1220-4071-8e53-4b957f5d3089
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9b06dac9dc00fd405fd28f1428a0833d968fabe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340834"
---
# <a name="use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server"></a>Usare i criteri Always On per visualizzare l'integrità di un gruppo di disponibilità (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Questo argomento illustra come determinare l'integrità operativa di un gruppo di disponibilità Always On usando i criteri Always On in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Per informazioni sulla gestione basata su criteri di Always On, vedere [Criteri Always On per problemi operativi con gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
> [!IMPORTANT]  
>  Per i criteri Always On, i nomi delle categorie vengono usati come ID. La modifica del nome di una categoria Always On causa l'interruzione della funzionalità di valutazione dell'integrità. Quindi, i nomi di categoria Always On non devono mai essere modificati.  
  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre delle autorizzazioni CONNECT, VIEW SERVER STATE e VIEW ANY DEFINITION.  
  
##  <a name="using-the-always-on-dashboard"></a><a name="SSMSProcedure"></a> Utilizzo del Dashboard Always On  
 **Per aprire il Dashboard Always On**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita una delle repliche di disponibilità. Per visualizzare informazioni su tutte le repliche di disponibilità di un determinato gruppo, connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Fare clic sul nome del server per espandere il relativo albero.  
  
3.  Espandere il nodo **Disponibilità elevata Always On** .  
  
     Fare clic con il pulsante destro del mouse sul nodo **Gruppi di disponibilità** o espandere il nodo e fare clic con il pulsante destro del mouse su un gruppo di disponibilità specifico.  
  
4.  Selezionare il comando **Mostra dashboard** .  
  
 Per informazioni su come usare il Dashboard Always On, vedere [Usare il Dashboard Always On &#40;SQL Server Management Studio&#41;](~/database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Use Always On policies to view the health of an availability group**  
  
1.  Impostare il valore predefinito (**cd**) su un'istanza del server che ospita una delle repliche di disponibilità. Per visualizzare informazioni su tutte le repliche di disponibilità di un determinato gruppo, connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Usare i cmdlet seguenti:  
  
     **Test-SqlAvailabilityGroup**  
     Valuta l'integrità di un gruppo di disponibilità valutando i criteri della gestione basata su criteri di SQL Server. È necessario disporre delle autorizzazioni CONNECT, VIEW SERVER STATE e VIEW ANY DEFINITION per eseguire questo cmdlet.  
  
     Ad esempio, il comando seguente mostra tutti i gruppi di disponibilità con stato di integrità "Error" nell'istanza del server `Computer\Instance`.  
  
    ```  
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups `   
    | Test-SqlAvailabilityGroup | Where-Object { $_.HealthState -eq "Error" }  
    ```  
  
     **Test-SqlAvailabilityReplica**  
     Valuta l'integrità delle repliche di disponibilità valutando i criteri della gestione basata su criteri di SQL Server. È necessario disporre delle autorizzazioni CONNECT, VIEW SERVER STATE e VIEW ANY DEFINITION per eseguire questo cmdlet.  
  
     Ad esempio, il seguente comando valuta l'integrità della replica di disponibilità denominata `MyReplica` nel gruppo di disponibilità `MyAg` e restituisce un breve riepilogo.  
  
    ```  
    Test-SqlAvailabilityReplica `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
     **Test-SqlDatabaseReplicaState**  
     Valuta l'integrità di un database di disponibilità su tutte le repliche di disponibilità aggiunte valutando i criteri della gestione basata su criteri di SQL Server.  
  
     Ad esempio, il seguente comando valuta l'integrità di tutte i database di disponibilità nel gruppo di disponibilità `MyAg` e restituisce un breve riepilogo per ognuno di essi.  
  
    ```  
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\DatabaseReplicaStates `   
     | Test-SqlDatabaseReplicaState  
    ```  
  
     Questi cmdlet accettano le opzioni seguenti:  
  
    |Opzione|Descrizione|  
    |------------|-----------------|  
    |**AllowUserPolicies**|Esegue i criteri utente trovati nelle categorie dei criteri Always On.|  
    |**InputObject**|Raccolta di oggetti che rappresentano gruppi di disponibilità, repliche di disponibilità o stati dei database di disponibilità. Il cmdlet calcolerà l'integrità degli oggetti specificati.|  
    |**NoRefresh**|Quando questo parametro è impostato, il cmdlet non aggiorna manualmente gli oggetti specificati dal parametro **-Path** o **-InputObject** .|  
    |**Percorso**|Percorso del gruppo di disponibilità, una o più repliche di disponibilità o stato del cluster della replica di database del database di disponibilità (a seconda del cmdlet utilizzato). Questo parametro è facoltativo. Se non specificato, il valore predefinito di questo parametro sarà il percorso di lavoro corrente.|  
    |**ShowPolicyDetails**|Mostra il risultato di ogni valutazione dei criteri eseguita da questo cmdlet. Il cmdlet restituisce un oggetto per ogni valutazione dei criteri e questo oggetto contiene campi che descrivono i risultati della valutazione, ovvero se i criteri sono stati soddisfatti, il nome e la categoria dei criteri e così via.|  
  
     Ad esempio, il comando **Test-SqlAvailabilityGroup** seguente specifica il parametro **-ShowPolicyDetails** per mostrare i risultati della valutazione di ogni criterio eseguita da questo cmdlet per ogni criterio della gestione basata su criteri eseguito sul gruppo di disponibilità denominato `MyAg`.  
  
    ```  
    Test-SqlAvailabilityGroup `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\AgName `  
    -ShowPolicyDetails  
  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Visualizzare la Guida di SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
 **Post pubblicati nel blog del team di SQL Server Always On sul monitoraggio dell'integrità Always On con PowerShell**:  
  
-   [Pagina relativa alla prima parte riguardante la panoramica su cmdlet di base](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)  
  
-   [Pagina relativa alla seconda parte riguardante l'utilizzo avanzato di cmdlet](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)  
  
-   [Pagina relativa alla terza parte riguardante un'applicazione di monitoraggio semplice](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)  
  
-   [Pagina relativa alla quarta parte riguardante l'integrazione con SQL Server Agent](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Amministrazione di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Monitoraggio di Gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Criteri AlwaysOn per problemi operativi con gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
