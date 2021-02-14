---
title: Gestire SQL Server in Linux con PowerShell
description: Informazioni dettagliate su SQL Server PowerShell ed esempi su come usare Windows con SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: a3492ce1-5d55-4505-983c-d6da8d1a94ad
ms.openlocfilehash: d10b498569bac6a166ded3eaf9bd03f23ea6b718
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345637"
---
# <a name="use-powershell-on-windows-to-manage-sql-server-on-linux"></a>Usare PowerShell in Windows per gestire SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo presenta [SQL Server PowerShell](../powershell/sql-server-powershell.md) e illustra alcuni esempi di come usarlo con SQL Server in Linux. Il supporto di PowerShell per SQL Server è attualmente disponibile in Windows, macOS e Linux. Questo articolo illustra in dettaglio l'uso di un computer Windows per connettersi a un'istanza di SQL Server remota in Linux.

## <a name="install-the-newest-version-of-sql-powershell-on-windows"></a>Installare la versione più recente di SQL PowerShell in Windows

[SQL PowerShell](../powershell/download-sql-server-ps-module.md) in Windows viene gestito in PowerShell Gallery. Quando si lavora con SQL Server, è consigliabile usare sempre la versione più recente del modulo di PowerShell SqlServer.

## <a name="before-you-begin"></a>Prima di iniziare

Vedere i [problemi noti](sql-server-linux-release-notes.md) di SQL Server in Linux.

## <a name="launch-powershell-and-import-the-sqlserver-module"></a>Avviare PowerShell e importare il modulo *sqlserver*

Avviare PowerShell in Windows. Per avviare una nuova sessione di Windows PowerShell, usare <kbd>Win</kbd>+<kbd>R</kbd> nel computer Windows in uso e digitare **PowerShell**.

```
PowerShell
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

Si userà ora PowerShell in Windows per connettersi all'istanza di SQL Server in Linux e visualizzare alcune proprietà del server.

Copiare e incollare i comandi seguenti al prompt di PowerShell. Quando si eseguono questi comandi, PowerShell effettua le operazioni seguenti:
- Visualizzano una finestra di dialogo in cui viene richiesto il nome host o l'indirizzo IP dell'istanza
- Visualizzano la finestra di dialogo *Richiesta credenziali di Windows PowerShell* in cui vengono richieste le credenziali. È possibile usare il *nome utente SQL* e la *password SQL* per connettersi all'istanza di SQL Server in Linux
- Usare il cmdlet **Get-SqlInstance** per connettersi al **server** e visualizzare alcune proprietà

Facoltativamente, è possibile sostituire la variabile `$serverInstance` con l'indirizzo IP o il nome host dell'istanza di SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and get a few properties
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

Un'altra opzione per connettersi all'istanza di SQL Server prevede l'uso del [provider SQL Server PowerShell](../powershell/sql-server-powershell-provider.md).  Questo provider consente di esplorare l'istanza di SQL Server come se si stesse esplorando la struttura ad albero in Esplora oggetti, ma dalla riga di comando.  Per impostazione predefinita, questo provider viene presentato come unità PSDrive denominata `SQLSERVER:\`, che è possibile usare per connettersi alle istanze di SQL Server a cui l'account di dominio ha accesso ed esplorarle.  Per informazioni su come configurare l'autenticazione di Active Directory per SQL Server in Linux, vedere [Passaggi di configurazione](./sql-server-linux-active-directory-auth-overview.md#configuration-steps).

È anche possibile usare l'autenticazione di SQL con il provider SQL Server PowerShell. A questo scopo, usare il cmdlet `New-PSDrive` per creare una nuova unità PSDrive e immettere le credenziali appropriate per la connessione.

Nell'esempio seguente viene illustrato come creare una nuova unità PSDrive usando l'autenticazione SQL.

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

L'output potrebbe essere simile al seguente.  È possibile notare che l'output è simile a quello che verrà visualizzato da SSMS nel nodo Databases.  Visualizza i database utente, ma non i database di sistema.

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

Se è necessario visualizzare tutti i database dell'istanza, è possibile usare il cmdlet [Get-SqlDatabase](/powershell/module/sqlserver/Get-SqlDatabase).

## <a name="examine-sql-server-error-logs"></a>Esaminare i log degli errori di SQL Server

La procedura seguente usa PowerShell in Windows per esaminare i log degli errori connettendosi all'istanza di SQL Server in Linux. Si userà anche il **cmdlet Out-GridView** per visualizzare le informazioni dei log degli errori in una visualizzazione griglia.

Copiare e incollare i comandi seguenti al prompt di PowerShell. L'esecuzione dei comandi potrebbe richiedere alcuni minuti. Questi comandi eseguono le operazioni seguenti:
- Visualizzano una finestra di dialogo in cui viene richiesto il nome host o l'indirizzo IP dell'istanza
- Visualizzano la finestra di dialogo *Richiesta credenziali di Windows PowerShell* in cui vengono richieste le credenziali. È possibile usare il *nome utente SQL* e la *password SQL* per connettersi all'istanza di SQL Server in Linux
- Usare il cmdlet **Get-SqlErrorLog** per connettersi all'istanza di SQL Server in Linux e recuperare i log degli errori da ieri (**Yesterday**)
- Invio dell'output tramite pipe al cmdlet **Out-GridView**

Facoltativamente, è possibile sostituire la variabile `$serverInstance` con l'indirizzo IP o il nome host dell'istanza di SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday | Out-GridView
# done
```
## <a name="see-also"></a>Vedere anche
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)
- [Cmdlet di SqlServer](/powershell/module/sqlserver)