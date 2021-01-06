---
title: Creare una risorsa DTC cluster per un gruppo di disponibilità
description: Questo argomento illustra la configurazione completa di una risorsa DTC cluster per un gruppo di disponibilità AlwaysOn di SQL Server.
ms.custom: seodec18
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 0e332aa4-2c48-4bc4-a404-b65735a02cea
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c976a81277f18f89486343af32c96509f8383fda
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643272"
---
# <a name="create-clustered-dtc-resource-for-an-always-on-availability-group"></a>Creare una risorsa DTC cluster per un gruppo di disponibilità Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

Questo argomento illustra la configurazione completa di una risorsa DTC cluster per un gruppo di disponibilità AlwaysOn di SQL Server. Il completamento della configurazione può richiedere fino a un'ora. 

La procedura dettagliata crea una risorsa DTC cluster e gruppi di disponibilità di SQL Server per l'allineamento con i requisiti in [Inserire DTC nel cluster per i gruppi di disponibilità di SQL Server](../../../database-engine/availability-groups/windows/cluster-dtc-for-sql-server-2016-availability-groups.md).

La procedura dettagliata usa PowerShell e gli script Transact-SQL (T-SQL),  molti dei quali richiedono l'abilitazione della **modalità SQLCMD** .  Per altre informazioni sulla **modalità SQLCMD**, vedere [Attivazione di scripting SQLCMD nell'editor di query](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md).  Il modulo PowerShell **FailoverClusters** deve essere importato.  Per altre informazioni sull'importazione di un modulo di PowerShell, vedere [Importazione di un modulo di PowerShell](/powershell/scripting/developer/module/importing-a-powershell-module).  Questa procedura dettagliata si basa sui seguenti criteri:
- Sono stati soddisfatti tutti i requisiti elencati in [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
- Il dominio è `contoso.lab`.
- L'utente ha l'autorizzazione per la creazione di oggetti computer nell'unità organizzativa in cui verrà creata la risorsa nome di rete DTC.
- L'utente è un utente di dominio con diritti di amministratore su tutti i nodi del cluster.
- Una condivisione file denominata `sqlbackups` è stata creata per i backup.
- Le istanze predefinite di SQL Server sono denominate `SQLNODE1` e `SQLNODE2`.
- Lo stesso account di servizio è usato in tutte le istanze di SQL Server.
- L'utente è un membro del ruolo sysadmin fisso di SQL Server in tutte le istanze di SQL Server.
- Il risultato predefinito delle transazioni che DTC non riesce a risolvere verrà impostato su `presume commit`.
- L'endpoint del mirroring userà la porta `5022`.
- Non esistono altri gruppi di disponibilità o risorse DTC cluster.
- Dettagli dei cluster (esistente):
  - Nome: `Cluster`
  - Nome della rete: `Cluster Network 1`
  - Nodi: `SQLNODE1, SQLNODE2`
  - Archiviazione condivisa: `Cluster Disk 3` (con `SQLNODE1` proprietario)
- Dettagli dei cluster (da creare):
  - Risorsa nome di rete: `DTCnet1`
  - Risorsa nome di rete DTC: `DTC1`
  - Risorsa disco fisico DTC: `DTCDisk1`
  - Risorsa IP e subnet DTC: `192.168.2.54`, `255.255.255.0`
  - Risorsa IP DTC: `DTCIP1`

## <a name="1-check-operating-system"></a>1. Controllare il sistema operativo
Per le transazioni distribuite supportate, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] deve essere in esecuzione in Windows Server 2012 R2 o Windows Server 2016.  Per Windows Server 2012 R2, è necessario installare l'aggiornamento in KB3090973 disponibile all'indirizzo [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  Questo script controlla la versione del sistema operativo e se deve essere installato l'hotfix 3090973.  Eseguire il seguente script di PowerShell in `SQLNODE1`.

```powershell  
# A few OS checks

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {

    # At least 2012 R2
    $os = (Get-WmiObject -class Win32_OperatingSystem -ComputerName $node).caption;
    IF($os -like "*2012 R2*" -or $os -like "*2016*")
    {
        Write-Host "$os is supported on $node.";
    }
    ELSE
    {
        Write-Host "STOP!  $os is not supported on $node.";
    }
    
    # KB 3090973
    IF($os -like "*2012 R2*")
    {
        $kb = Get-Hotfix -ComputerName $node | Where {$_.HotFixID -eq 'KB3090973'};
        IF($kb)
        {
            Write-Host "KB3090973 is installed on $node."
        }
        ELSE
        {
            Write-Host "HotFixID KB3090973 must be applied on $node.  See https://support.microsoft.com/kb/3090973 for additional information and to download the hotfix.";
        }
    }
    ELSE
    {
        Write-Host "KB3090973 is not applicable to $os on $node."
    }
}
```  
## <a name="2---configure-firewall-rules"></a>2.   Configurare le regole del firewall
Questo script configura il firewall per consentire il traffico DTC, in ogni SQL Server che ospita una replica del gruppo di disponibilità, nonché in qualsiasi altro server impegnato nella transazione distribuita.  Lo script configura anche il firewall per consentire le connessioni per l'endpoint del mirroring del database.  Eseguire il seguente script di PowerShell in `SQLNODE1`.

```powershell  
# Configure Firewall

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {
    Get-NetFirewallRule -CimSession $node -DisplayGroup 'Distributed Transaction Coordinator' -Enabled False -ErrorAction SilentlyContinue | Enable-NetFirewallRule;
    New-NetFirewallRule -CimSession $node -DisplayName 'SQL Server Mirroring' -Description 'Port 5022 for SQL Server Mirroring' -Action Allow -Direction Inbound -Protocol TCP -LocalPort 5022 -RemotePort Any -LocalAddress Any -RemoteAddress Any;
    };
```  
## <a name="3--configure-in-doubt-xact-resolution"></a>3.  Configurare **in-doubt xact resolution** 
Questo script configura l'opzione di configurazione del server **in-doubt xact resolution** per "presupporre il commit" per le transazioni in dubbio.  Eseguire lo script T-SQL seguente in SQL Server Management Studio (SSMS) su `SQLNODE1` in **modalità SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Configure in-doubt xact resolution on all SQL Server instances to presume commit
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------
```

## <a name="4-create-test-databases"></a>4. Creare database di test
Lo script crea un database denominato `AG1` in `SQLNODE1` e un database denominato `dtcDemoAG1` in `SQLNODE2`.  Eseguire lo script T-SQL seguente in SSMS su `SQLNODE1` in **modalità SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- On SQLNODE1 
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'AG1')
BEGIN
    DROP DATABASE AG1;
END
GO

CREATE DATABASE AG1;
ALTER DATABASE AG1 SET RECOVERY FULL WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::AG1 to sa;
GO

USE AG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );

INSERT Names
VALUES ('AG1', GETDATE());
GO


-- Against SQNODE2
:connect SQLNODE2
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'dtcDemoAG1')
BEGIN
    DROP DATABASE dtcDemoAG1;
END
GO

CREATE DATABASE dtcDemoAG1;
ALTER DATABASE dtcDemoAG1 SET RECOVERY SIMPLE WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::dtcDemoAG1 to sa;
GO

USE dtcDemoAG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );
GO    
----------------------------------------------------------------
```
## <a name="5---create-endpoints"></a>5.   Creare gli endpoint
Questo script crea un endpoint denominato `AG1_endpoint` che è in ascolto sulla porta TCP `5022`.  Eseguire lo script T-SQL seguente in SSMS su `SQLNODE1` in **modalità SQLCMD**.

```sql  
/**********************************************
Execute on SQLNODE1 in SQLCMD mode
**********************************************/

-- Create endpoint on server instance that hosts the primary replica:
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------
```

## <a name="6---prepare-databases-for-availability-group"></a>6.   Preparare i database per il gruppo di disponibilità
Lo script esegue il backup di `AG1` su `SQLNODE1` e lo ripristina in `SQLNODE2`.  Eseguire lo script T-SQL seguente in SSMS su `SQLNODE1` in **modalità SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Backup database
BACKUP DATABASE AG1 
TO DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH FORMAT, STATS = 10;

-- Backup transaction log
BACKUP LOG AG1
TO DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH FORMAT, STATS = 10;
GO


-- Restore database and logs on secondary WITH NORECOVERY
:connect SQLNODE2
USE [master]
RESTORE DATABASE AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH NORECOVERY, STATS = 10;
GO
```

## <a name="7---create-availability-group"></a>7.   Creare un gruppo di disponibilità
[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] devono essere creati con il comando **CREATE AVAILABILITY GROUP** e la clausola **WITH DTC_SUPPORT = PER_DB**.  Attualmente non è possibile modificare un gruppo di disponibilità esistente.  La Creazione guidata Gruppo di disponibilità non consente di abilitare il supporto DTC per un nuovo gruppo di disponibilità.  Lo script seguente crea il nuovo gruppo di disponibilità e un join del database secondario.  Eseguire lo script T-SQL seguente in SSMS su `SQLNODE1` in **modalità SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

--  Create Availability Group on SQLNODE1 
USE master;
CREATE AVAILABILITY GROUP DTCAG1
WITH (DTC_SUPPORT = PER_DB) 
FOR DATABASE AG1
REPLICA ON 
  'SQLNODE1' WITH
     (
     ENDPOINT_URL = 'TCP://SQLNODE1.contoso.lab:5022', 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ),
  'SQLNODE2' WITH 
     (
     ENDPOINT_URL = 'TCP://SQLNODE2.contoso.lab:5022',
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ); 
GO


-- SQLNODE2
-- Join secondary replica to the Availability Group 
:connect SQLNODE2
ALTER AVAILABILITY GROUP DTCag1 JOIN;

-- Join database to the Availability Group
ALTER DATABASE AG1 SET HADR AVAILABILITY GROUP = DTCAG1;
GO
```

> [!IMPORTANT]
> Non è possibile abilitare DTC in un oggetto [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] esistente.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] accetta la sintassi seguente per un gruppo di disponibilità esistente:  
> 
> USE master;    
> ALTER AVAILABILITY GROUP \<availability_group\>  
> SET (DTC_Support = Per_DB)  
> 
> Tuttavia, non verrà apportata alcuna modifica alla configurazione.  È possibile confermare la configurazione **dtc_support** con la query T-SQL seguente:  
> 
> SELECT name, dtc_support FROM sys.availability_groups  
> 
> L'unico modo per abilitare il supporto DTC in un gruppo di disponibilità consiste nel creare un gruppo di disponibilità con Transact-SQL.
 
## <a name="8--prepare-cluster-resources"></a><a name="ClusterDTC"></a>8.  Preparare le risorse cluster

Questo script prepara le risorse dipendenti DTC: IP e disco.  L'archiviazione condivisa verrà aggiunta al cluster Windows.  Verranno create risorse di rete e quindi verrà creato il DTC e impostato come risorsa al gruppo di disponibilità.  Eseguire il seguente script di PowerShell in `SQLNODE1`. Grazie ad [Allan Hirt](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) per lo script.

```powershell  
# Create a clustered Microsoft Distributed Transaction Coordinator properly in the resource group with SQL Server

\<#----------------------------------- Begin User Input -----------------------------------#>
$AGgrp = "DTCag1";                          # Name of the WSFC resource group that will contain the DTC resource

$WSFC = (Get-Cluster).Name;                 # Windows Failover Cluster name
$DTCnetwk = "Cluster Network 1"             # WSFC Network to use for the DTC IP address

$ClusterAvailableDisk = "Cluster Disk 3";   # Designated disk that can support failover clustering and is visible to all nodes, but not yet part of the set of clustered disks
$DTCdisk = "DTCDisk1";                      # Name of the disk to be used with DTC

$DTCipresnm = "DTCIP1";                     # WSFC Friendly Name of the DTC's IP resource 
$DTCipaddr = "192.168.2.54";                # IP address of the DTC resource 
$DTCsubnet = "255.255.255.0";               # Subnet for the DTC IP address 
$DTCnetnm = "DTCNet1";                      # WSFC Friendly Name of the Network Name resource
$DTCresnm = "DTC1";                         # Name of the WSFC DTC Network Name resource; Name must be unique in AD
\<#------------------------------------ End User Input ------------------------------------#>


# Make a new disk available for use in a failover cluster.
Get-ClusterAvailableDisk | Where {$_.Name -eq $ClusterAvailableDisk} | Add-ClusterDisk;

# Rename disk
$resource = Get-ClusterResource $ClusterAvailableDisk; $resource.Name = $DTCdisk;

# Create the IP resource
Add-ClusterResource -Name $DTCipresnm -ResourceType "IP Address" -Group $AGgrp;

# Set the network to use, IP address, and subnet
# All three have to be configured at the same time
$DTCIPres = Get-ClusterResource $DTCipresnm;
$ntwk = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Network,$DTCnetwk;
$ipaddr = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Address,$DTCipaddr;
$subnet = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,SubnetMask,$dtcsubnet;

$setdtcipparams = $ntwk,$ipaddr,$subnet;
$setdtcipparams | Set-ClusterParameter;

# Create the Network Name resource
Add-ClusterResource $DTCnetnm -ResourceType "Network Name" -Group $AGgrp;

# Set the value for the Network Name resource
Get-ClusterResource $DTCnetnm | Set-ClusterParameter DnsName $DTCresnm;

# Add the IP address as a depenency of the Network Name resource
Add-ClusterResourceDependency $DTCnetnm $DTCipresnm;

# Create the Distributed Transaction Coordinator resource
Add-ClusterResource $DTCresnm -ResourceType "Distributed Transaction Coordinator" -Group $AGgrp;

# Add the Network Name as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCnetnm;

# Move the disk into the resource group with SQL Server
Move-ClusterResource -Name $DTCdisk -Group $AGgrp;

# Add the disk as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCdisk;

# Bring the IP resource online
Start-ClusterResource $DTCipresnm;

# Bring the Network Name resource online
Start-ClusterResource $DTCnetnm;

# Bring the DTC resource online
Start-ClusterResource $DTCresnm;
```  

## <a name="9---enable-network-dtc"></a>9.   Abilitare l'accesso di rete DTC 

Lo script seguente abilita l'accesso di rete DTC per consentire l'integrazione dei computer remoti nelle transazioni distribuite attraverso la rete per il servizio DTC cluster.  Eseguire il seguente script di PowerShell in `SQLNODE1`.

```powershell  
# Enable Network DTC access for the clustered DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

# Enter Name of DTC resource

$DtcName = "DTC1";
\<# ------- End of User Input ------- #>

[bool]$restart = 0;
$node = (Get-ClusterResource -Name $DtcName).OwnerNode.Name;
$DtcSettings = Get-DtcNetworkSetting -DtcName $DtcName;

IF ($DtcSettings.InboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -InboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($DtcSettings.OutboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -OutboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($restart -eq 1)
{
    Stop-Dtc -CimSession $node -DtcName $DTCname -Confirm:$false;
    Start-Dtc -CimSession $node -DtcName $DTCname;
}
```  

## <a name="10--disable-and-stop-the-local-dtc-service-on-each-node"></a>10.  Disabilitare e arrestare il servizio DTC locale in ogni nodo

Per garantire che le transazioni distribuite usino la risorsa DTC cluster, disabilitare il DTC locale in entrambi i nodi.  Lo script seguente disabilita e arresta il servizio DTC locale in ogni nodo.  Eseguire il seguente script di PowerShell in `SQLNODE1`.
```powershell  
# Disable local DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$DTCname = 'Local';
$nodes = (Get-ClusterNode).Name;

 foreach ($node in $nodes) {

    $service = Get-WmiObject -class Win32_Service -computername $node -Filter "Name='MSDTC'";
    IF ($service.StartMode -ne 'Disabled')
    {
        $service.ChangeStartMode('Disabled');
    }
    
    IF ($service.State -ne 'Stopped')
    {
        $service.StopService();
    }
}
```  

## <a name="11--cycle-the-ssnoversion-service-for-each-instance"></a>11.  Arrestare e riavviare il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ogni istanza

Dopo aver completato la configurazione del servizio DTC cluster, è necessario arrestare e riavviare ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel gruppo di disponibilità per assicurarsi che [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sia registrato per l'uso del servizio DTC.

La prima volta in cui il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] richiede una transazione distribuita, viene eseguita la registrazione a un servizio DTC. Il servizio SQL Server continuerà a usare quel servizio DTC fino a quando non viene riavviato. Se è disponibile un servizio DTC cluster, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verrà registrato per il servizio DTC cluster. Se non è disponibile un servizio DTC cluster, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verrà registrato per il servizio DTC locale. Per verificare che [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] venga registrato per il servizio DTC cluster, arrestare e riavviare ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 

Seguire le istruzioni contenute nello script T-SQL riportato di seguito:
```sql  
/*
Gracefully cycle the SQL Server service and failover the Availability Group
    a.  On SQLNODE2, cycle the SQL Server service from SQL Server Configuration Manger

    b.  On SQLNODE2 failover the Availability Group to SQLNODE2
        Execute T-SQL script, below, on SQLNODE2 (Use Results to Text)

    c.  On SQLNODE1, cycle the SQL Server service from SQL Server Configuration Manger

    d.  On SQLNODE1 failover the Availability Group to SQLNODE1 once the databases are back in sync.
        Execute T-SQL script, below, on SQLNODE1 (Use Results to Text)
*/

SET NOCOUNT ON;

-- Ensure replica is secondary
IF (
SELECT rs.is_primary_replica 
    FROM sys.availability_groups ag
    JOIN sys.dm_hadr_database_replica_states rs
    ON  ag.group_id = rs.group_id
    WHERE ag.name = N'DTCag1'
    AND rs.is_local = 1) = 0
BEGIN
    -- Wait for SYNCHRONIZED state
    DECLARE @ctr tinyint = 0;
    declare @msg varchar(128);
    WHILE (SELECT synchronization_state 
        FROM sys.availability_groups ag
        JOIN sys.dm_hadr_database_replica_states rs
        ON  ag.group_id = rs.group_id
        WHERE ag.name = N'DTCag1'
        AND rs.is_primary_replica = 0
        AND rs.is_local = 1) <> 2
    BEGIN
        WAITFOR DELAY '00:00:01'
        SET @ctr += 1
        SET @msg = 'Waiting for databases to become synchronized. Duration in seconds: ' + cast(@ctr AS varchar(3))
        RAISERROR (@msg, 0, 1) WITH NOWAIT
    END

    ALTER AVAILABILITY GROUP DTCAG1 FAILOVER;
    SELECT 'Failover complete' AS [Sucess]
END
ELSE BEGIN
    SELECT 'This instance is the primary replica.  Connect to the secondary replica and try again.' AS [Error]
END

```

## <a name="12--test-configuration"></a>12.  Configurazione di test

Questo test usa un server collegato da `SQLNODE1` a `SQLNODE2` per creare un transazione distribuita.  Verificare che la replica primaria del gruppo di disponibilità si trovi in `SQLNODE1`. Per testare la configurazione:

- Creare server collegati
- Eseguire una transazione distribuita

### <a name="create-linked-servers"></a>Creare server collegati  
Lo script seguente crea due server collegati in `SQLNODE1`.  Eseguire lo script T-SQL seguente in SSMS su `SQLNODE1`.

```sql  
-- SQLNODE1
IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE1')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE1';   
END

IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE2')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE2';   
END
 ```

### <a name="execute-a-distributed-transaction"></a>Eseguire una transazione distribuita
Questo script restituirà prima le statistiche delle transazioni DTC correnti,  quindi eseguirà una transazione distribuita usando database su `SQLNODE1` e `SQLNODE2`.  Infine, lo script restituirà nuovamente le statistiche delle transazioni DTC, che ora dovrebbero mostrare un conteggio superiore.  Connettersi fisicamente a `SQLNODE1` ed eseguire lo script T-SQL seguente in SSMS su `SQLNODE1` in **modalità SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
    Must be physically connected to SQLNODE1
*******************************************************************/

USE AG1;
SET NOCOUNT ON;

-- Get Baseline
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;

SET XACT_ABORT ON
BEGIN DISTRIBUTED TRANSACTION
    INSERT INTO SQLNODE1.[AG1].[dbo].[Names] VALUES ('TestValue1', GETDATE());
    INSERT INTO SQLNODE2.[dtcDemoAG1].[dbo].[Names] VALUES ('TestValue2', GETDATE());
COMMIT TRAN
GO

-- Review DTC Transaction Statistics
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;
```

> [!IMPORTANT]
> L'istruzione `USE AG1` deve essere eseguita per verificare che il contesto del database sia impostato su `AG1`.  In caso contrario, viene visualizzato il messaggio di errore seguente: "Il contesto della transazione è in uso in un'altra sessione".