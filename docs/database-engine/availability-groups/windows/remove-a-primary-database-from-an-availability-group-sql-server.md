---
title: Rimuovere un database da un gruppo di disponibilità
description: Passaggi per rimuovere un database primario da un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removeprimarydb.f1
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 6d4ca31e-ddf0-44bf-be5e-a5da060bf096
author: cawrites
ms.author: chadam
ms.openlocfilehash: ca813d490f97bf8e501ada801599cef74e2b9c05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351031"
---
# <a name="remove-a-primary-database-from-an-always-on-availability-group"></a>Rimuovere un database primario da un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come rimuovere il database primario e il database o i database secondari corrispondenti da un gruppo di disponibilità Always On usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Prerequisiti e restrizioni  
  
-   Questa attività può essere eseguita solo sulle repliche primarie. È necessario essere connessi all'istanza del server che ospita la replica primaria.  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per rimuovere un database di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria del database o dei database da rimuovere ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Selezionare il gruppo di disponibilità ed espandere il nodo **Database di disponibilità** .  
  
4.  Questo passaggio dipende dalla scelta di rimuovere uno o più database:  
  
    -   Per rimuovere più database, utilizzare il riquadro **Dettagli Esplora oggetti** per visualizzare e selezionare tutti i database che si desidera rimuovere. Per altre informazioni, vedere [Utilizzare Dettagli Esplora oggetti per monitorare Gruppi di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Per rimuovere un singolo database, selezionarlo nel riquadro **Esplora oggetti** o **Dettagli Esplora oggetti** .  
  
5.  Fare clic con il pulsante destro del mouse sul database o sui database selezionati e scegliere **Rimuovere il database dal gruppo di disponibilità** nel menu dei comandi.  
  
6.  Nella finestra di dialogo **Rimuovi i database dal gruppo di disponibilità** scegliere **OK** per rimuovere tutti i database elencati. Se non si desidera rimuoverli tutti, scegliere **Annulla**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per rimuovere un database di disponibilità**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito:  
  
     ALTER AVAILABILITY GROUP *group_name* REMOVE DATABASE *availability_database_name*  
  
     dove *group_name* è il nome del gruppo di disponibilità e *database_name* è il nome di un database da aggiungere al gruppo.  
  
     Nell'esempio seguente viene rimosso un database denominato `Db6` dal gruppo di disponibilità `MyAG` .  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE DATABASE Db6;  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per rimuovere un database di disponibilità**  
  
1.  Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **Remove-SqlAvailabilityDatabase** specificando il nome del database di disponibilità da rimuovere dal gruppo di disponibilità. Quando si è connessi all'istanza del server che ospita la replica primaria, il database primario e i database secondari corrispondenti vengono tutti rimossi dal gruppo di disponibilità.  
  
     Ad esempio, il seguente comando rimuove il database di disponibilità `MyDb9` dal gruppo di disponibilità denominato `MyAg`. Dal momento che il comando viene eseguito nell'istanza del server che ospita la replica primaria, il database primario e tutti i relativi database secondari corrispondenti vengono rimossi dal gruppo di disponibilità. La sincronizzazione dei dati non verrà più eseguita per questo database in nessuna replica secondaria.  
  
    ```  
    Remove-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\PrimaryComputer\InstanceName\AvailabilityGroups\MyAg\AvailabilityDatabases\MyDb9
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-an-availability-database-from-an-availability-group"></a><a name="FollowUp"></a> Completamento: Dopo la rimozione di un database di disponibilità da un gruppo di disponibilità  
 La rimozione di un database di disponibilità dal relativo gruppo di disponibilità termina la sincronizzazione dati tra il database primario precedente e i database secondari corrispondenti. Il database primario precedente rimane online. Ogni database secondario corrispondente viene posto nello stato RESTORING.  
  
 A questo punto sono disponibili modi alternativi per gestire un database secondario rimosso:  
  
-   Se un determinato database secondario non è più necessario, è possibile eliminarlo.  
  
     Per altre informazioni, vedere [Eliminare un database](../../../relational-databases/databases/delete-a-database.md).  
  
-   Se si desidera accedere a un database secondario dopo che è stato rimosso dal gruppo di disponibilità, è possibile recuperare il database. Tuttavia, se si recupera un database secondario rimosso, saranno online due database indipendenti e divergenti con lo stesso nome. È necessario assicurarsi che i client possano accedere a uno solo di essi, di solito al database primario più recente.  
  
     Per altre informazioni, vedere [Recupero di un database senza ripristino dei dati &#40;Transact-SQL&#41;](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Rimuovere un database secondario da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
