---
title: 'Eseguire il backup di più database: Archiviazione BLOB di Azure'
description: In questo articolo vengono illustrati alcuni script di esempio che possono essere usati per automatizzare i backup in SQL Server nel servizio Archiviazione BLOB di Azure tramite cmdlet di PowerShell.
titleSuffix: PowerShell
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: f7008339-e69d-4e20-9265-d649da670460
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3fb357b16572f34c906942e79be2d8488b49a15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347721"
---
# <a name="back-up-multiple-databases-to-azure-blob-storage---powershell"></a>Backup di più database nel servizio di archiviazione BLOB di Azure - PowerShell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento vengono illustrati alcuni script di esempio che possono essere usati per automatizzare i backup nel servizio Archiviazione BLOB di Azure tramite i cmdlet di PowerShell.  
  
## <a name="overview-of-powershell-cmdlets-for-backup-and-restore"></a>Panoramica dei cmdlet di PowerShell per il backup e il ripristino

**Backup-SqlDatabase** e **Restore-SqlDatabase** sono i due principali cmdlet disponibili per eseguire operazioni di backup e ripristino.

Sono inoltre disponibili altri cmdlet che possono essere necessari per automatizzare i backup nell'archiviazione BLOB di Windows Azure, come il set di cmdlet **SqlCredential**.

Per un elenco di tutti i cmdlet disponibili, vedere [Cmdlet di SqlServer](/powershell/module/sqlserver).
  
> [!TIP]  
> I cmdlet **SqlCredential** vengono usati nelle operazioni di backup e ripristino negli scenari del servizio Archiviazione BLOB di Azure. Per altre informazioni sulle credenziali e sul relativo uso, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).
  
### <a name="powershell-for-multi-database-multi-instance-backup-operations"></a>PowerShell per le operazioni di backup di più istanze di più database

Le sezioni seguenti includono alcuni script per diverse operazioni quali la creazione di credenziali SQL in più istanze di SQL Server, il backup di tutti i database utente in un'istanza di SQL Server e simili. È possibile utilizzare questi script per automatizzare e pianificare le operazioni di backup in base ai requisiti dell'ambiente. Gli script forniti di seguito sono esempi e possono essere modificati o estesi per l'ambiente in uso.  
  
Di seguito sono riportate alcune considerazioni per gli script di esempio:  
  
- SQL Server PowerShell implementa cmdlet per spostarsi all'interno della struttura del percorso che rappresenta la gerarchia di oggetti supportati da un provider PowerShell. Quando si passa a un nodo nel percorso, è possibile utilizzare altri cmdlet per eseguire operazioni di base sull'oggetto corrente.

  Per altre informazioni, vedere [Navigate SQL Server PowerShell Paths](../../powershell/navigate-sql-server-powershell-paths.md).

- Cmdlet **Get-ChildItem**: le informazioni restituite da **Get-ChildItem** dipendono dalla posizione in un percorso di SQL Server PowerShell. Ad esempio, se il percorso è a livello di computer, questo cmdlet restituisce tutte le istanze del motore di database di SQL Server installate nel computer. Oppure, se il percorso è a livello di oggetto come i database, restituisce un elenco di oggetti di database. Per impostazione predefinita il cmdlet **Get-ChildItem** non restituisce oggetti di sistema. Usare il parametro `–Force` per visualizzare gli oggetti di sistema.

- Un account di archiviazione di Azure e le credenziali SQL sono prerequisiti necessari per tutte le operazioni di backup e ripristino nel servizio di archiviazione BLOB di Azure.
  
### <a name="create-a-sql-credential-on-all-instances-of-sql-server"></a>Creare le credenziali SQL in tutte le istanze di SQL Server

Lo script seguente può essere usato per creare credenziali SQL generiche in tutte le istanze di SQL Server in un computer. Se sono già presenti credenziali esistenti con lo stesso nome in una delle istanze del computer, lo script mostra l'errore e continua.  
  
```powershell
# load the sqlps module
import-module sqlps  
  
# set parameters
$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$storageKey = "<myStorageAccessKey>"  
$secureString = ConvertTo-SecureString $storageKey -AsPlainText -Force  
$credentialName = "myCredential-$(Get-Random)"

Write-Host "Generate credential: " $credentialName
  
#cd to sql server and get instances  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and create a SQL credential, output any errors
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials"
        New-SqlCredential -Name $credentialName -Identity $storageAccount -Secret $secureString -Path $path -ea Stop | Out-Null
        Write-Host "...generated credential $($path)\$($credentialName)."  }
    catch { Write-Host $_.Exception.Message } }
```

> [!NOTE]
> L'istruzione `-ea Stop | Out-Null` viene usata per l'output dell'eccezione definita dall'utente. Se si preferiscono i messaggi di errore predefiniti di PowerShell, questa istruzione può essere rimossa. 

### <a name="remove-a-sql-credential-from-all-instances-of-sql-server"></a>Rimuovere credenziali SQL da tutte le istanze di SQL Server

Lo script seguente può essere usato per rimuovere credenziali specifiche da tutte le istanze di SQL Server installate nel computer. Se le credenziali non esistono in un'istanza specifica, viene visualizzato un messaggio di errore e lo script continua finché non vengono controllate tutte le istanze.  
  
```powershell
import-module sqlps

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$credentialName = "<myCredential>"

Write-Host "Delete credential: " $credentialName

cd $sqlPath
$instances = Get-ChildItem

#loop through instances and delete a SQL credential
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials\$($credentialName)"
        Remove-SqlCredential -Path $path -ea Stop | Out-Null
        Write-Host "...deleted credential $($path)."  }
    catch { Write-Host $_.Exception.Message } }
```  
  
### <a name="full-backup-for-all-databases"></a>Backup completo per tutti i database

Lo script seguente consente di creare i backup di tutti i database presenti nel computer, sia database utente che database di sistema **msdb** . Nello script vengono esclusi tramite filtro i database di sistema **tempdb** e **model** .  
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $backupUrlContainer
  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and backup all databases (excluding tempdb and model)
foreach ($instance in $instances)  {
    $path = "$($sqlPath)\$($instance.DisplayName)\databases"
    $databases = Get-ChildItem -Force -Path $path | Where-object {$_.name -ne "tempdb" -and $_.name -ne "model"}

    foreach ($database in $databases) {
        try {
            $databasePath = "$($path)\$($database.Name)"
            Write-Host "...starting backup: " $databasePath
            Backup-SqlDatabase -Database $database.Name -Path $path -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
            Write-Host "...backup complete."  }
        catch { Write-Host $_.Exception.Message } } }
```  
  
### <a name="full-backup-for-system-databases-only-on-a-specific-instance-of-sql-server"></a>Backup completo per i database di sistema solo in un'istanza specifica di SQL Server

Lo script seguente può essere utilizzato per eseguire il backup dei database **master** e **msdb** in un'istanza denominata di SQL Server. Lo stesso script può essere usato per qualsiasi istanza modificando il valore del parametro dell'istanza. L'istanza predefinita di SQL Server è denominata `DEFAULT`.
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$instanceName = "DEFAULT"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $instanceName " to " $backupUrlContainer
  
cd "$($sqlPath)\$($instanceName)"

#loop through instance and backup specific databases
$databases = "master", "msdb"  
foreach ($database in $databases) {
    try {
        Write-Host "...starting backup: " $database
        Backup-SqlDatabase -Database $database -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
        Write-Host "...backup complete." }
    catch { Write-Host $_.Exception.Message } }
```  
  
## <a name="see-also"></a>Vedere anche

[Backup e ripristino di SQL Server con il servizio Archiviazione BLOB di Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)

[Procedure consigliate e risoluzione dei problemi per il backup di SQL Server nell'URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)