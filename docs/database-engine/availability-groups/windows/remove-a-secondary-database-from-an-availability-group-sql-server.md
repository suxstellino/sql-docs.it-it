---
title: Rimuovere un database secondario da un gruppo di disponibilità
description: Procedura per rimuovere un database secondario da un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.unjoindb.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], databases
ms.assetid: 4e51a570-58d7-4f01-9390-4198f3602576
author: cawrites
ms.author: chadam
ms.openlocfilehash: d6ad224d5c017fa8e8286e245af2e8ca3a96e4c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344560"
---
# <a name="remove-a-secondary-database-from-an-availability-group-sql-server"></a>Rimuovere un database secondario da un gruppo di disponibilità (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come rimuovere un database secondario da un gruppo di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
   
  
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Prerequisiti e restrizioni  
  
-   Questa attività è supportata solo sulle repliche secondarie. È necessario essere connessi all'istanza del server che ospita la replica secondaria da cui verrà rimosso il database.  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per rimuovere un database secondario da un gruppo di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica secondaria da cui si desidera rimuovere uno o più database secondari ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Selezionare il gruppo di disponibilità ed espandere il nodo **Database di disponibilità** .  
  
4.  Questo passaggio dipende dalla scelta di rimuovere uno o più database:  
  
    -   Per rimuovere più database, utilizzare il riquadro **Dettagli Esplora oggetti** per visualizzare e selezionare tutti i database che si desidera rimuovere. Per altre informazioni, vedere [Utilizzare Dettagli Esplora oggetti per monitorare Gruppi di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Per rimuovere un singolo database, selezionarlo nel riquadro **Esplora oggetti** o **Dettagli Esplora oggetti** .  
  
5.  Fare clic con il pulsante destro del mouse sul database o sui database selezionati e scegliere **Rimuovi database secondario** nel menu dei comandi.  
  
6.  Nella finestra di dialogo **Rimuovi database dal gruppo di disponibilità** scegliere **OK** per rimuovere tutti i database elencati. Se non si desidera rimuovere tutti i database elencati, fare clic su **Annulla**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per rimuovere un database secondario da un gruppo di disponibilità**  
  
1.  Connettersi all'istanza del server che ospita la replica secondaria.  
  
2.  Utilizzare la [clausola SET HADR dell'istruzione ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) come indicato di seguito:  
  
     ALTER DATABASE *nome_database* SET HADR OFF  
  
     dove *nome_database* è il nome di un database secondario da rimuovere dal gruppo di disponibilità a cui appartiene.  
  
     L'esempio seguente mostra come rimuovere il database secondario locale *MyDb2* dal relativo gruppo di disponibilità.  
  
    ```  
    ALTER DATABASE MyDb2 SET HADR OFF;  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per rimuovere un database secondario da un gruppo di disponibilità**  
  
1.  Spostarsi nella directory (**cd**) dell'istanza del server che ospita la replica secondaria.  
  
2.  Usare il cmdlet **Remove-SqlAvailabilityDatabase** specificando il nome del database di disponibilità da rimuovere dal gruppo di disponibilità. Quando si è connessi a un'istanza del server che ospita una replica secondaria, solo il database secondario locale verrà rimosso dal gruppo di disponibilità.  
  
     Ad esempio, il comando seguente rimuove il database secondario `MyDb8` dalla replica secondaria ospitata dall'istanza del server denominata `SecondaryComputer\Instance`. La sincronizzazione dei dati con i database secondari rimossi viene terminata. Questo comando non influisce sul database primario né su nessun altro database secondario.  
  
    ```  
    Remove-SqlAvailabilityDatabase `  
    -Path SQLSERVER:\Sql\SecondaryComputer\InstanceName\AvailabilityGroups\MyAg\Databases\MyDb8  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-database-from-an-availability-group"></a><a name="FollowUp"></a> Completamento: Dopo la rimozione di un database secondario da un gruppo di disponibilità  
 Quando un database secondario viene rimosso, non è più unito in join al gruppo di disponibilità e tutte le informazioni relative al database secondario rimosso vengono ignorate dal gruppo di disponibilità. Il database secondario rimosso viene posto nello stato RESTORING.  
  
> [!TIP]  
>  Per un breve intervallo di tempo dopo avere rimosso un database secondario, è possibile riavviare la sincronizzazione dei dati AlwaysOn sul database creando nuovamente un join al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 A questo punto sono disponibili modi alternativi per gestire un database secondario rimosso:  
  
-   Se il database secondario non è più necessario, è possibile eliminarlo.  
  
     Per altre informazioni, vedere [DROP DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-database-transact-sql.md) o [Eliminare un database](../../../relational-databases/databases/delete-a-database.md).  
  
-   Se si desidera accedere a un database secondario dopo che è stato rimosso dal gruppo di disponibilità, è possibile recuperare il database. Tuttavia, se si recupera un database secondario rimosso, saranno online due database indipendenti e divergenti con lo stesso nome. È necessario assicurarsi che i client possano accedere solo al database primario corrente.  
  
     Per altre informazioni, vedere [Recupero di un database senza ripristino dei dati &#40;Transact-SQL&#41;](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Rimuovere un database primario da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
