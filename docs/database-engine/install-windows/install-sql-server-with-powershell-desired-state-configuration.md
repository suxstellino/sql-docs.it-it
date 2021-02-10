---
title: 'Installare: PowerShell Desired State Configuration'
description: Installare SQL Server usando PowerShell DSC e ottenere informazioni sull'installazione iniziale di un'istanza autonoma di SQL Server 2017 in Windows Server 2016.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.devlang: PowerShell
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
author: randomnote1
ms.author: dareist
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a1105db9984a8ff52485247e6699c578ec350986
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061446"
---
# <a name="install-sql-server-with-powershell-desired-state-configuration"></a>Installare SQL Server con PowerShell Desired State Configuration (DSC)

È mai successo di spostarsi all'interno dell'interfaccia di installazione di SQL Server usando i soliti pulsanti e immettendo le solite informazioni senza soffermarsi un attimo a pensare? Poi, quando l'installazione è terminata, ci si rende conto di aver dimenticato di specificare il gruppo di amministratori di database nel ruolo **sysadmin**. Era quindi necessario eseguire queste operazioni:
* Passare alla modalità utente singolo.
* Aggiungere gli utenti o i gruppi appropriati.
* Portare di nuovo SQL Server in modalità multiutente.
* Eseguire il test. 

Ora, però, il livello di confidenza dell'intera installazione è compromesso. Ci si chiede se non si sarà dimenticato qualcos'altro... senza potersi togliere il dubbio.

Leggere [Panoramica di PowerShell Desired State Configuration (DSC)](/powershell/scripting/dsc/overview/overview). Con DSC è possibile creare un modello di configurazione che può essere riutilizzato in centinaia o migliaia di server. A seconda della build, può essere necessario modificare alcuni parametri di installazione, ma non è un grosso problema perché è comunque possibile mantenere tutte le impostazioni standard. In questo modo si elimina la possibilità di dimenticare di immettere un parametro importante.

Questo articolo descrive l'installazione iniziale di un'istanza autonoma di SQL Server 2017 in Windows Server 2016 tramite la risorsa DSC **SqlServerDsc**. È utile avere una conoscenza pregressa di DSC perché in questo articolo il funzionamento di DSC non viene spiegato.

Per questa procedura dettagliata sono necessari gli elementi seguenti:

- Un computer che esegue Windows Server 2016.
- Il supporto di installazione di SQL Server 2017.
- La risorsa DSC **SqlServerDsc**.

## <a name="prerequisites"></a>Prerequisiti

Nella maggior parte dei casi, per gestire i prerequisiti si usa DSC ma ai fini di questa dimostrazione, i prerequisiti verranno gestiti manualmente.

## <a name="install-the-sqlserverdsc-dsc-resource"></a>Installare la risorsa DSC SqlServerDsc

Scaricare la risorsa DSC [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc) dalla [PowerShell Gallery](https://www.powershellgallery.com/) usando il cmdlet [Install-Module](/powershell/module/powershellget/Install-Module). 

> [!NOTE]
> Per installare il modulo, assicurarsi di eseguire PowerShell **come amministratore**.

```PowerShell
Install-Module -Name SqlServerDsc
```

### <a name="get-the-sql-server-2017-installation-media"></a>Ottenere il supporto di installazione di SQL Server 2017
Scaricare il supporto di installazione di SQL Server 2017 nel server. Per questa demo è stato scaricato SQL Server 2017 Enterprise da una sottoscrizione di Visual Studio e il file ISO è stato copiato in `C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso`.

A questo punto, è necessario estrarre il file ISO in una directory:

```PowerShell
New-Item -Path C:\SQL2017 -ItemType Directory
$mountResult = Mount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso' -PassThru
$volumeInfo = $mountResult | Get-Volume
$driveInfo = Get-PSDrive -Name $volumeInfo.DriveLetter
Copy-Item -Path ( Join-Path -Path $driveInfo.Root -ChildPath '*' ) -Destination C:\SQL2017\ -Recurse
Dismount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso'
```

## <a name="create-the-configuration"></a>Creare configurazione

### <a name="configuration"></a>Configurazione

Creare la funzione di configurazione che verrà chiamata per generare i documenti [MOF (Managed Object Format)](/windows/desktop/WmiSdk/managed-object-format--mof-):

```PowerShell
Configuration SQLInstall
{...}
```

### <a name="modules"></a>Moduli

Importare i moduli nella sessione corrente. Questi moduli indicano al documento di configurazione come compilare i documenti MOF e al motore DSC come applicare i documenti MOF al server:

```PowerShell
Import-DscResource -ModuleName SqlServerDsc
```

### <a name="resources"></a>Risorse

#### <a name="net-framework"></a>.NET Framework

SQL Server si basa su .NET Framework. È quindi necessario assicurarsi che sia installato prima di installare SQL Server. La risorsa **WindowsFeature** viene usata per installare la funzionalità di Windows **Net-Framework-45-Core**:

```PowerShell
WindowsFeature 'NetFramework45'
{
     Name = 'Net-Framework-45-Core'
     Ensure = 'Present'
}
```

#### <a name="sqlsetup"></a>SqlSetup

La risorsa **SqlSetup** viene usata per indicare a DSC come installare SQL Server. I parametri necessari per un'installazione di base sono:

- **InstanceName**. Nome dell’istanza. Usare **MSSQLSERVER** per un'istanza predefinita.
- **Features**. Le funzionalità da installare. In questo esempio, verrà installata solo la funzionalità **SQLEngine**.
- **SourcePath**. Il percorso del supporto di installazione di SQL. In questo esempio, il supporto di installazione di SQL si trova in `C:\SQL2017`. Una condivisione di rete consente di ridurre al minimo lo spazio usato nel server.
- **SQLSysAdminAccounts**. Gli utenti o i gruppi che devono essere membri del ruolo **sysadmin**. In questo esempio, viene concesso l'accesso **sysadmin** al gruppo Administrators locale. 

> [!NOTE]
> Questa configurazione non è consigliabile in un ambiente a sicurezza elevata.

L'elenco completo e la descrizione dei parametri di **SqlSetup** è disponibile nel [repository di SqlServerDsc in GitHub](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlsetup).

La risorsa **SqlSetup** installa solo SQL Server e **non** mantiene le impostazioni applicate, ad esempio le risorse **SQLSysAdminAccounts** specificate al momento dell'installazione. Un amministratore può aggiungere o rimuovere informazioni di accesso dal ruolo **sysadmin**, ma questa operazione non interessa la risorsa **SqlSetup**. Se si vuole che DSC applichi l'appartenenza al ruolo **sysadmin**, usare la risorsa [SqlServerRole](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlserverrole).

#### <a name="finish-configuration"></a>Terminare la configurazione

```PowerShell
Configuration SQLInstall
{
     Import-DscResource -ModuleName SqlServerDsc

     node localhost
     {
          WindowsFeature 'NetFramework45'
          {
               Name   = 'NET-Framework-45-Core'
               Ensure = 'Present'
          }

          SqlSetup 'InstallDefaultInstance'
          {
               InstanceName        = 'MSSQLSERVER'
               Features            = 'SQLENGINE'
               SourcePath          = 'C:\SQL2017'
               SQLSysAdminAccounts = @('Administrators')
               DependsOn           = '[WindowsFeature]NetFramework45'
          }
     }
}
```

## <a name="build-and-deploy"></a>Eseguire la compilazione e la distribuzione

### <a name="compile-the-configuration"></a>Compilare la configurazione

Eseguire lo script di configurazione:

```PowerShell
. .\SQLInstallConfiguration.ps1
```

Eseguire la funzione di configurazione:

```PowerShell
SQLInstall
```

Nella directory di lavoro viene creata una directory denominata **SQLInstall**, che contiene il file **localhost.mof**. Esaminare il contenuto del file MOF, che illustra la configurazione DSC compilata.

### <a name="deploy-the-configuration"></a>Distribuire la configurazione

Per iniziare la distribuzione DSC di SQL Server, chiamare il cmdlet **Start-DscConfiguration**. Al cmdlet vengono forniti i parametri seguenti:

- **Path**. Percorso della cartella contenente i documenti MOF da distribuire, Un esempio è `C:\SQLInstall`.
- **Wait**. Attendere il completamento del processo di configurazione.
- **Force**. Eseguire l'override di tutte le configurazioni DSC esistenti.
- **Verbose**. Visualizzare l'output dettagliato. Utile quando si effettua il push di una configurazione per la prima volta per agevolare la risoluzione dei problemi.

```PowerShell
Start-DscConfiguration -Path C:\SQLInstall -Wait -Force -Verbose
```

Man mano che la configurazione viene applicata, l'output dettagliato illustra ciò che accade. A meno che non si verifichino errori, indicati da testo rosso, quando sullo schermo compare il messaggio **Operation 'Invoke CimMethod' complete** (Operazione Invoke CimMethod completata), l'installazione di SQL Server è completata.

## <a name="validate-installation"></a>Convalidare l'installazione

### <a name="dsc"></a>DSC

I cmdlet [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/test-dscconfiguration) sono in grado di determinare se lo stato corrente del server corrisponde a quello desiderato. In questo caso, si tratta dell'installazione di SQL Server. Il risultato di **Test-DscConfiguration** deve essere **True**:

```PowerShell
PS C:\> Test-DscConfiguration
True
```

### <a name="services"></a>Servizi

L'elenco dei servizi ora restituisce i servizi di SQL Server:

```PowerShell
PS C:\> Get-Service -Name *SQL*
Status  Name           DisplayName
------  ----           -----------
Running MSSQLSERVER    SQL Server (MSSQLSERVER)
Stopped SQLBrowser     SQL Server Browser
Running SQLSERVERAGENT SQL Server Agent (MSSQLSERVER)
Running SQLTELEMETRY   SQL Server CEIP service (MSSQLSERVER)
Running SQLWriter      SQL Server VSS Writer
```

### <a name="sql-server"></a>SQL Server

```PowerShell
PS C:\> & sqlcmd -S $env:COMPUTERNAME
1> SELECT @@SERVERNAME
2> GO
1> quit
```

## <a name="see-also"></a>Vedere anche

[Panoramica di PowerShell DSC](/powershell/scripting/dsc/overview/overview)

[Installare SQL Server al prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

[Installare SQL Server tramite un file di configurazione](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)
