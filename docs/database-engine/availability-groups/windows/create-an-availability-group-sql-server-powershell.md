---
title: Creare un gruppo di disponibilità con PowerShell
description: Informazioni su come usare i cmdlet di PowerShell per creare e configurare un gruppo di disponibilità Always On usando PowerShell in SQL Server 2019 (15.x).
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: bc69a7df-20fa-41e1-9301-11317c5270d2
author: cawrites
ms.author: chadam
ms.openlocfilehash: dc9f2cfe1dddb809d42ba6dc4df5ae21e4feaad9
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765637"
---
# <a name="create-an-always-on-availability-group-using-powershell"></a>Creare un gruppo di disponibilità Always On con PowerShell
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento si illustra come usare i cmdlet di PowerShell per creare e configurare un gruppo di disponibilità AlwaysOn usando PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Un *gruppo di disponibilità* permette di definire un set di database utente di cui sarà eseguito il failover come unità singola e un set di partner di failover, noti come *repliche di disponibilità*, che supportano il failover.  
  
> [!NOTE]  
> Per un'introduzione ai gruppi di disponibilità, vedere [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
> [!NOTE]  
> In alternativa all'uso dei cmdlet di PowerShell, è possibile usare la procedura guidata Crea gruppo di disponibilità o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Per altre informazioni, vedere [Usare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md) o [Creare un gruppo di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  

## <a name="before-you-begin"></a>Prima di iniziare
### <a name="prerequisites-restrictions-and-recommendations"></a><a name="PrerequisitesRestrictions"></a> Prerequisiti, restrizioni e raccomandazioni  

- Prima di creare un gruppo di disponibilità, verificare che le istanze host di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si trovino ognuna in un nodo WSCF (Windows Server Failover Clustering) diverso di un singolo cluster di failover WSFC. Inoltre, verificare che le istanze del server abbiano soddisfatto gli altri prerequisiti dell'istanza del server, che tutti gli altri requisiti di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] siano soddisfatti e che si tengano presenti i consigli. Per altre informazioni, si consiglia di leggere [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  

### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.  

## <a name="using-powershell-to-create-and-configure-an-availability-group"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell per creare e configurare un gruppo di disponibilità  
 
Nella tabella seguente sono elencate le attività di base necessarie per la configurazione di un gruppo di disponibilità e sono indicate le attività supportate dai cmdlet di PowerShell. È necessario eseguire le attività [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nell'ordine con cui sono elencate nella tabella.  
  
|Attività|Cmdlet di PowerShell (se disponibile) o istruzione Transact-SQL|Posizione in cui eseguire l'attività|  
|----------|--------------------------------------------------------------------|---------------------------------|  
|Creare un endpoint del mirroring del database (una volta per ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] )|**New-SqlHadrEndPoint**|Eseguire in ogni istanza del server in cui non è presente l'endpoint del mirroring del database.<br /><br />Per modificare un endpoint del mirroring del database esistente, usare **Set-SqlHadrEndpoint**.|  
|Creare un gruppo di disponibilità|Usare innanzitutto il cmdlet **New-SqlAvailabilityReplica** con il parametro **-AsTemplate** per creare un oggetto della replica di disponibilità in memoria per ognuna delle due repliche di disponibilità da includere nel gruppo di disponibilità.<br /><br /> Creare quindi il gruppo di disponibilità tramite il cmdlet **New-SqlAvailabilityGroup** e facendo riferimento agli oggetti replica di disponibilità.|Eseguire nell'istanza del server che dovrà ospitare la replica primaria iniziale.|  
|Creare un join della replica secondaria al gruppo di disponibilità|**Join-SqlAvailabilityGroup**|Eseguire in ogni istanza del server in cui è ospitata una replica secondaria.|  
|Preparare il database secondario|**Backup-SqlDatabase** e **Restore-SqlDatabase**|Creare i backup nell'istanza del server in cui è ospitata la replica primaria.<br /><br /> Ripristinare i backup in ogni istanza del server in cui è ospitata una replica secondaria, usando il parametro di ripristino **NoRecovery** . Se i percorsi di file differiscono tra i computer in cui sono ospitate la replica primaria e la replica secondaria di destinazione, usare anche il parametro di ripristino **RelocateFile** .|  
|Avviare la sincronizzazione dei dati creando un join di ogni database secondario al gruppo di disponibilità|**Add-SqlAvailabilityDatabase**|Eseguire in ogni istanza del server in cui è ospitata una replica secondaria.|  
  
> [!NOTE]
> Per eseguire attività specifiche, impostare la directory (**cd**) sull'istanza o sulle istanze del server indicate.  

## <a name="using-powershell"></a>Utilizzo di PowerShell

Impostare e usare il [provider SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md). 

> [!NOTE]  
> Per visualizzare la sintassi e un esempio di un cmdlet specifico, usare il cmdlet **Get-Help** nell'ambiente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  

1. Spostarsi nella directory (**cd**) dell'istanza del server che deve ospitare la replica primaria.  
  
1. Creare un oggetto replica di disponibilità in memoria per la replica primaria.  
  
1. Creare un oggetto replica di disponibilità in memoria per ognuna delle repliche secondarie.  
  
1. Creare il gruppo di disponibilità.  
  
    > [!NOTE]  
    > La lunghezza massima consentita per il nome del gruppo di disponibilità è 128 caratteri.  

1. Creare un join della nuova replica secondaria al gruppo di disponibilità. Vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
1. Per ogni database nel gruppo di disponibilità, creare un database secondario ripristinando i backup recenti del database primario, usando RESTORE WITH NORECOVERY.  
  
1. Creare un join di ogni nuovo database secondario al gruppo di disponibilità. Vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
1. (Facoltativo) Usare il comando **dir** di Windows per verificare il contenuto del nuovo gruppo di disponibilità.  
  
> [!NOTE]  
> Se gli account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] delle istanze del server sono eseguiti con account utente di dominio diversi, in ogni istanza del server creare un account di accesso per l'altra istanza del server e concedere a questo account l'autorizzazione CONNECT per l'endpoint del mirroring del database locale.  

### <a name="example"></a><a name="ExampleConfigureGroup"></a>Esempio
Nell'esempio di PowerShell seguente si crea e si configura un gruppo di disponibilità semplice denominato `<myAvailabilityGroup>` con due repliche di disponibilità e un database di disponibilità. Esempio:  

1. Viene eseguito il backup di `<myDatabase>` e del relativo log delle transazioni.  

1. Ripristina `<myDatabase>` e il relativo log delle transazioni, usando l'opzione **-NoRecovery** .  

1. Viene creata una rappresentazione in memoria della replica primaria, che sarà ospitata dall'istanza locale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (denominata `PrimaryComputer\Instance`).  

1. Viene creata una rappresentazione in memoria della replica secondaria, che sarà ospitata da un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (denominata `SecondaryComputer\Instance`).  

1. Viene creato un nuovo gruppo di disponibilità denominato `<myAvailabilityGroup>`.  

1. Viene creato un join della replica secondaria al gruppo di disponibilità.  

1. Viene creato un join del database secondario al gruppo di disponibilità.  

```powershell
# Backup my database and its log on the primary  
Backup-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.bak" `  
    -ServerInstance "PrimaryComputer\Instance"  
  
Backup-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.log" `  
    -ServerInstance "PrimaryComputer\Instance" `  
    -BackupAction Log   
  
# Restore the database and log on the secondary (using NO RECOVERY)  
Restore-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.bak" `  
    -ServerInstance "SecondaryComputer\Instance" `  
    -NoRecovery  
  
Restore-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.log" `  
    -ServerInstance "SecondaryComputer\Instance" `  
    -RestoreAction Log `  
    -NoRecovery  
  
# Create an in-memory representation of the primary replica.  
$primaryReplica = New-SqlAvailabilityReplica `  
    -Name "PrimaryComputer\Instance" `  
    -EndpointURL "TCP://PrimaryComputer.domain.com:5022" `  
    -AvailabilityMode "SynchronousCommit" `  
    -FailoverMode "Automatic" `  
    -Version 12 `  
    -AsTemplate  
  
# Create an in-memory representation of the secondary replica.  
$secondaryReplica = New-SqlAvailabilityReplica `  
    -Name "SecondaryComputer\Instance" `  
    -EndpointURL "TCP://SecondaryComputer.domain.com:5022" `  
    -AvailabilityMode "SynchronousCommit" `  
    -FailoverMode "Automatic" `  
    -Version 12 `  
    -AsTemplate  
  
# Create the availability group  
New-SqlAvailabilityGroup `  
    -Name "<myAvailabilityGroup>" `  
    -Path "SQLSERVER:\SQL\PrimaryComputer\Instance" `  
    -AvailabilityReplica @($primaryReplica,$secondaryReplica) `  
    -Database "<myDatabase>"  
  
# Join the secondary replica to the availability group.  
Join-SqlAvailabilityGroup -Path "SQLSERVER:\SQL\SecondaryComputer\Instance" -Name "<myAvailabilityGroup>"  
  
# Join the secondary database to the availability group.  
Add-SqlAvailabilityDatabase -Path "SQLSERVER:\SQL\SecondaryComputer\Instance\AvailabilityGroups\<myAvailabilityGroup>" -Database "<myDatabase>"  
```  
  
## <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare un'istanza del server per i gruppi di disponibilità AlwaysOn**  
  
- [Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
- [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](~/database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
 **Per configurare le proprietà della replica e del gruppo di disponibilità**  
  
- [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
- [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
- [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
- [Configurare i criteri di failover flessibili per controllare le condizioni per il failover automatico &#40;Gruppi di disponibilità AlwaysOn&#41;](~/database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
- [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
- [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
- [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
- [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
- [Modificare il periodo di timeout della sessione per una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Per completare la configurazione del gruppo di disponibilità**  
  
- [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
- [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
- [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
- [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Modalità alternative di creazione di un gruppo di disponibilità**  
  
- [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
- [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
- [Creare un gruppo di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
 **Per risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn**  
  
- [Risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
- [Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità Always On&#41;](~/database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
## <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
- **Blog:**  
  
     [serie di informazioni su HADRON riguardanti l'uso del pool di lavoro per database abilitati HADRON in AlwaysOn](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [Pagina relativa alla configurazione di AlwaysOn con SQL Server PowerShell](/archive/blogs/sqlalwayson/configuring-alwayson-with-sql-server-powershell)  
  
     [SQL Server AlwaysOn Team Blog: blog ufficiale del team di SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
- **Video:**  
  
     [Pagina relativa alla prima parte riguardante l'introduzione della soluzione a disponibilità elevata di prossima generazione della serie AlwaysOn di Microsoft SQL Server nome in codice "Denali"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Pagina relativa alla seconda parte riguardante la compilazione di una soluzione a disponibilità elevata critica tramite AlwasyOn della serie AlwaysOn di Microsoft SQL Server nome in codice "Denali"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
- **White paper:**  
  
     [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Vedere anche  
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)