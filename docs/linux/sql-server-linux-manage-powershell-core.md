---
title: Gestire SQL Server in Linux con PowerShell Core
description: È possibile ottenere informazioni su SQL Server PowerShell esaminando un paio di esempi sull'uso di SQL Server PowerShell con PowerShell Core (PS Core) in macOS e Linux.
ms.date: 04/22/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: vanto
ms.openlocfilehash: a65ad9ad35e4feb2a55ffbdc259601a7b16106f9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094556"
---
# <a name="manage-sql-server-on-linux-with-powershell-core"></a>Gestire SQL Server in Linux con PowerShell Core

Questo articolo presenta [SQL Server PowerShell](../powershell/sql-server-powershell.md) e illustra alcuni esempi di come usarlo con PowerShell Core (PS Core) in macOS e Linux. PowerShell Core è ora un progetto open source in [GitHub](https://github.com/powershell/powershell).

## <a name="cross-platform-editor-options"></a>Opzioni dell'editor multipiattaforma

Tutti i passaggi di PowerShell Core seguenti funzioneranno in un terminale normale. In alternativa, è possibile eseguirli da un terminale all'interno di VS Code o di Azure Data Studio.  Sia VS Code che Azure Data Studio sono disponibili in macOS e Linux.  Per altre informazioni su Azure Data Studio, vedere [questo argomento di avvio rapido](../azure-data-studio/quickstart-sql-server.md).  È anche possibile usare l'[estensione PowerShell](../azure-data-studio/extensions/powershell-extension.md).

## <a name="installing-powershell-core"></a>Installazione di PowerShell Core

Per altre informazioni sull'installazione di PowerShell Core in varie piattaforme supportate e sperimentali, vedere gli articoli seguenti:

- [Installazione di PowerShell Core in Windows](/powershell/scripting/install/installing-powershell-core-on-windows)
- [Installazione di PowerShell Core in Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Installazione di PowerShell Core in macOS](/powershell/scripting/install/installing-powershell-core-on-macos)
- [Installazione di PowerShell Core in ARM](/powershell/scripting/install/powershell-core-on-arm)

## <a name="install-the-sqlserver-module"></a>Installare il modulo SqlServer

Il modulo `SqlServer` viene mantenuto in [PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer/). Quando si lavora con SQL Server, è consigliabile usare sempre la versione più recente del modulo di PowerShell SqlServer.

Per installare il modulo SqlServer, aprire una sessione di PowerShell Core ed eseguire il codice seguente:

```powerhsell
Install-Module -Name SqlServer
```

Per altre informazioni su come installare il modulo SqlServer da PowerShell Gallery, vedere questa [pagina](../powershell/download-sql-server-ps-module.md).

## <a name="using-the-sqlserver-module"></a>Uso del modulo SqlServer

Avviare PowerShell Core.  Se si usa macOS o Linux, aprire una *sessione del terminale* sul computer e digitare **pwsh** per avviare una nuova sessione di PowerShell Core.  In Windows usare <kbd>Win</kbd>+<kbd>R</kbd> e digitare `pwsh` per avviare una nuova sessione di PowerShell Core.

```
pwsh
```

SQL Server fornisce un modulo di PowerShell denominato **SqlServer**. È possibile usare il modulo **SqlServer** per importare i componenti di SQL Server (provider e cmdlet di SQL Server) in un ambiente o uno script di PowerShell.

Copiare e incollare il comando seguente al prompt di PowerShell per importare il modulo **SqlServer** nella sessione di PowerShell corrente:

```powershell
Import-Module SqlServer
```

Digitare il comando seguente al prompt di PowerShell per verificare che il modulo di **SqlServer** sia stato importato correttamente:

```powershell
Get-Module -Name SqlServer
```

PowerShell dovrebbe visualizzare informazioni simili all'output seguente:

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Connettersi a SQL Server e ottenere informazioni sul server

La procedura seguente usa PowerShell Core per connettersi all'istanza di SQL Server in Linux e visualizzare alcune proprietà del server.

Copiare e incollare i comandi seguenti al prompt di PowerShell. Quando si eseguono questi comandi, PowerShell effettua le operazioni seguenti:
- Visualizzano una finestra di dialogo in cui viene richiesto il nome host o l'indirizzo IP dell'istanza
- Visualizza la finestra di dialogo *PowerShell credential request* (Richiesta credenziali di PowerShell), in cui vengono richieste le credenziali. È possibile usare il *nome utente SQL* e la *password SQL* per connettersi all'istanza di SQL Server in Linux
- Usare il cmdlet **Get-SqlInstance** per connettersi al **server** e visualizzare alcune proprietà

Facoltativamente, è possibile sostituire la variabile `$serverInstance` con l'indirizzo IP o il nome host dell'istanza di SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and return a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

PowerShell dovrebbe visualizzare informazioni simili all'output seguente:

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------                   -------    ------------ -----------  ------------ ----------------
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu
```

> [!NOTE]
> Se non viene visualizzato nulla per questi valori, la connessione all'istanza di SQL Server di destinazione probabilmente non è riuscita. Verificare che sia possibile usare le stesse informazioni di connessione per connettersi da SQL Server Management Studio. Rivedere poi i [consigli per la risoluzione dei problemi di connessione](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="using-the-sql-server-powershell-provider"></a>Uso del provider SQL Server PowerShell

Un'altra opzione per connettersi all'istanza di SQL Server prevede l'uso del [provider SQL Server PowerShell](../powershell/sql-server-powershell-provider.md).  L'uso del provider consente di esplorare l'istanza di SQL Server come se si stesse esplorando la struttura ad albero in Esplora oggetti, ma nella riga di comando.  Per impostazione predefinita, questo provider viene presentato come unità PSDrive denominata `SQLSERVER:\`, che è possibile usare per connettersi alle istanze di SQL Server a cui l'account di dominio ha accesso ed esplorarle.  Per informazioni su come configurare l'autenticazione di Active Directory per SQL Server in Linux, vedere [Passaggi di configurazione](./sql-server-linux-active-directory-auth-overview.md#configuration-steps).

È anche possibile usare l'autenticazione di SQL con il provider SQL Server PowerShell. A questo scopo, usare il cmdlet `New-PSDrive` per creare una nuova unità PSDrive e immettere le credenziali appropriate per la connessione.

Nell'esempio seguente viene illustrato come creare una nuova unità PSDrive usando l'autenticazione di SQL.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

Per verificare che l'unità sia stata creata, eseguire il cmdlet `Get-PSDrive`.

```powershell
Get-PSDrive
```

Dopo aver creato la nuova unità PSDrive, è possibile iniziare a esplorarla.

```powershell
dir SQLonDocker:\Databases
```

L'output potrebbe essere simile al seguente.  È possibile notare che questo output è simile a quello che verrà visualizzato da SSMS nel nodo Databases.  Visualizza i database utente, ma non i database di sistema.

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

Se è necessario visualizzare tutti i database dell'istanza, è possibile usare il cmdlet `Get-SqlDatabase`.

## <a name="get-databases"></a>Ottenere i database

Un cmdlet importante da conoscere è `Get-SqlDatabase`.  Per molte operazioni che coinvolgono un database o gli oggetti all'interno di un database, è possibile usare il cmdlet `Get-SqlDatabase`.  Se si specificano i valori per entrambi i parametri `-ServerInstance` e `-Database`, verrà recuperato solo l'oggetto di database.  Se tuttavia si specifica solo il parametro `-ServerInstance`, verrà restituito un elenco completo di tutti i database di tale istanza.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

# Connect to the Instance and retrieve all databases
Get-SqlDatabase -ServerInstance ServerB -Credential $credential
```

Di seguito è riportato un esempio di output restituito dal comando Get-SqlDatabase precedente:

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.88 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.63 MB Simple       140 sa
master               Normal        6.00 MB  600.00 KB Simple       140 sa
model                Normal       16.00 MB    5.70 MB Full         140 sa
msdb                 Normal       15.50 MB    1.14 MB Simple       140 sa
tempdb               Normal       16.00 MB    5.49 MB Simple       140 sa

```

## <a name="examine-sql-server-error-logs"></a>Esaminare i log degli errori di SQL Server

La procedura seguente usa PowerShell Core per esaminare i log degli errori connettendosi all'istanza di SQL Server in Linux.

Copiare e incollare i comandi seguenti al prompt di PowerShell. L'esecuzione dei comandi potrebbe richiedere alcuni minuti. Questi comandi eseguono i passaggi seguenti:
- Visualizzano una finestra di dialogo in cui viene richiesto il nome host o l'indirizzo IP dell'istanza
- Visualizzano la finestra di dialogo *PowerShell credential request* (Richiesta credenziali di PowerShell), in cui vengono richieste le credenziali. È possibile usare il *nome utente SQL* e la *password SQL* per connettersi all'istanza di SQL Server in Linux
- Usare il cmdlet **Get-SqlErrorLog** per connettersi all'istanza di SQL Server in Linux e recuperare i log degli errori da ieri (**Yesterday**)

Facoltativamente, è possibile sostituire la variabile `$serverInstance` con l'indirizzo IP o il nome host dell'istanza di SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday
# done
```

## <a name="explore-cmdlets-currently-available-in-ps-core"></a>Esplorare i cmdlet attualmente disponibili in PS core
Nonostante il modulo SqlServer abbia attualmente 109 cmdlet disponibili in Windows PowerShell, in PSCore ne sono disponibili solo 62. Di seguito è riportato un elenco completo dei 62 cmdlet attualmente disponibili.  Per una documentazione dettagliata di tutti i cmdlet del modulo SqlServer, vedere le [informazioni di riferimento sui cmdlet](/powershell/module/sqlserver/) di SqlServer.

Il comando seguente consente di visualizzare tutti i cmdlet disponibili nella versione di PowerShell in uso.

```powershell
Get-Command -Module SqlServer -CommandType Cmdlet |
Sort-Object -Property Noun |
SELECT Name
```

- ConvertFrom-EncodedSqlName
- ConvertTo-EncodedSqlName
- Get-SqlAgent
- Get-SqlAgentJob
- Get-SqlAgentJobHistory
- Get-SqlAgentJobSchedule
- Get-SqlAgentJobStep
- Get-SqlAgentSchedule
- Invoke-SqlAssessment
- Get-SqlAssessmentItem
- Remove-SqlAvailabilityDatabase
- Resume-SqlAvailabilityDatabase
- Add-SqlAvailabilityDatabase
- Suspend-SqlAvailabilityDatabase
- New-SqlAvailabilityGroup
- Set-SqlAvailabilityGroup
- Remove-SqlAvailabilityGroup
- Switch-SqlAvailabilityGroup
- Join-SqlAvailabilityGroup
- Revoke-SqlAvailabilityGroupCreateAnyDatabase
- Grant-SqlAvailabilityGroupCreateAnyDatabase
- New-SqlAvailabilityGroupListener
- Set-SqlAvailabilityGroupListener
- Add-SqlAvailabilityGroupListenerStaticIp
- Set-SqlAvailabilityReplica
- Remove-SqlAvailabilityReplica
- New-SqlAvailabilityReplica
- Set-SqlAvailabilityReplicaRoleToSecondary
- New-SqlBackupEncryptionOption
- Get-SqlBackupHistory
- Invoke-Sqlcmd
- New-SqlCngColumnMasterKeySettings
- Remove-SqlColumnEncryptionKey
- Get-SqlColumnEncryptionKey
- Remove-SqlColumnEncryptionKeyValue
- Add-SqlColumnEncryptionKeyValue
- Get-SqlColumnMasterKey
- Remove-SqlColumnMasterKey
- New-SqlColumnMasterKey
- Get-SqlCredential
- Set-SqlCredential
- New-SqlCredential
- Remove-SqlCredential
- New-SqlCspColumnMasterKeySettings
- Get-SqlDatabase
- Restore-SqlDatabase
- Backup-SqlDatabase
- Set-SqlErrorLog
- Get-SqlErrorLog
- New-SqlHADREndpoint
- Set-SqlHADREndpoint
- Get-SqlInstance
- Add-SqlLogin
- Remove-SqlLogin
- Get-SqlLogin
- Set-SqlSmartAdmin
- Get-SqlSmartAdmin
- Read-SqlTableData
- Write-SqlTableData
- Read-SqlViewData
- Read-SqlXEvent
- Convert-UrnToPath

## <a name="see-also"></a>Vedere anche
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)