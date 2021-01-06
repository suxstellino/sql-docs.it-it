---
title: Aggiungere un database a un gruppo di disponibilità
description: 'Aggiungere un database a un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 2a54eef8-9e8e-4e04-909c-6970112d55cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: f6af03aebbc04ce45f2dad51025bdce32a1443b5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641218"
---
# <a name="add-a-database-to-an-always-on-availability-group"></a>Aggiungere un database a un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come aggiungere un database a un gruppo di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  

  
## <a name="prerequisites-and-restrictions"></a>Prerequisiti e restrizioni  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria.  
  
-   È necessario che il database risieda nell'istanza del server che ospita la replica primaria e sia conforme ai prerequisiti e alle restrizioni per i database di disponibilità. Per altre informazioni, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  

  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic con il pulsante destro del mouse sul gruppo di disponibilità e selezionare uno dei comandi seguenti:  
  
    -   Per avviare la procedura guidata Aggiungi database a gruppo di disponibilità, selezionare il comando **Aggiungi database** . Per altre informazioni, vedere [Usare la procedura guidata Aggiungi database a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
    -   Per aggiungere uno o più database specificandoli nella finestra di dialogo **Proprietà gruppo di disponibilità** , selezionare il comando **Proprietà** . Di seguito sono indicati i passaggi per l'aggiunta di un database:  
  
        1.  Nel riquadro **Database di disponibilità** fare clic sul pulsante **Aggiungi** . Viene creato e selezionato un campo del database vuoto.  
  
        2.  Immettere il nome di un database che soddisfi i prerequisiti dei database di disponibilità.  
  
         Per aggiungere un altro database, ripetere i passaggi precedenti. Dopo avere specificato i database, fare clic su **OK** per completare l'operazione.  
  
         Dopo avere utilizzato la finestra di dialogo **Proprietà gruppo di disponibilità** per aggiungere un database a un gruppo di disponibilità, è necessario configurare il database secondario corrispondente su ogni istanza del server che ospita una replica secondaria. Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Usare Transact-SQL  

  
1.  Connettersi all'istanza del server che ospita la replica primaria.    
2.  Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito:  
  
     ALTER AVAILABILITY GROUP *nome_gruppo* ADD DATABASE *nome_database* [,...*n*]  
  
     dove *nome_gruppo* è il nome del gruppo di disponibilità e *nome_database* è il nome di un database da aggiungere al gruppo.  
  
     Nell'esempio seguente viene aggiunto il database *MyDb3* al gruppo di disponibilità *MyAG* .  
  
    ```  
    -- Connect to the server instance that hosts the primary replica.  
    -- Add an existing database to the availability group.  
    ALTER AVAILABILITY GROUP MyAG ADD DATABASE MyDb3;  
    GO  
    ```  
  
3.  Dopo avere aggiunto un database a un gruppo di disponibilità, è necessario configurare il database secondario corrispondente su ogni istanza del server che ospita una replica secondaria. Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Usare PowerShell  

  
1.  Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **Add-SqlAvailabilityDatabase** .  
  
     Ad esempio, con il comando seguente viene aggiunto il database secondario `MyDd` al gruppo di disponibilità `MyAG` la cui replica primaria è ospitata da `PrimaryServer\InstanceName`.  
  
    ```  
  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "MyDb"  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
3.  Dopo avere aggiunto un database a un gruppo di disponibilità, è necessario configurare il database secondario corrispondente su ogni istanza del server che ospita una replica secondaria. Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
 Per un esempio completo, vedere [Esempio (PowerShell)](#PSExample), di seguito.  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Esempio (PowerShell)  
 Nel seguente esempio si illustra il processo completo di preparazione di un database secondario da un database nell'istanza del server che ospita la replica primaria di un gruppo di disponibilità, aggiungendo il database a un gruppo di disponibilità (come database primario), quindi creando un join del database secondario al gruppo di disponibilità. Nell'esempio si esegue innanzitutto il backup del database e del relativo log delle transazioni. Successivamente si ripristinano i backup del database e del log nelle istanze del server che ospitano una replica secondaria.  
  
 Nell'esempio viene chiamato due volte **Add-SqlAvailabilityDatabase** , la prima volta nella replica primaria per aggiungere il database al gruppo di disponibilità, successivamente nella replica secondaria per creare un join del database secondario in quella replica al gruppo di disponibilità. Se si dispone di più di una replica secondaria, ripristinare e creare un join del database secondario in ognuna di esse.  
  
```  
$DatabaseBackupFile = "\\share\backups\MyDatabase.bak"  
$LogBackupFile = "\\share\backups\MyDatabase.trn"  
$MyAgPrimaryPath = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
$MyAgSecondaryPath = "SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAg"  
  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "PrimaryServer\InstanceName"  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "PrimaryServer\InstanceName" -BackupAction 'Log'  
  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "SecondaryServer\InstanceName" -NoRecovery  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "SecondaryServer\InstanceName" -RestoreAction 'Log' -NoRecovery  
  
Add-SqlAvailabilityDatabase -Path $MyAgPrimaryPath -Database "MyDatabase"  
Add-SqlAvailabilityDatabase -Path $MyAgSecondaryPath -Database "MyDatabase"  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Usare il Dashboard AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)