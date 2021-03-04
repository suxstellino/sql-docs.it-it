---
title: Scaricare il modulo PowerShell di SQL Server
description: Informazioni su come installare il modulo SqlServer PowerShell, che fornisce i cmdlet che supportano le funzionalità SQL più recenti e contiene inoltre le versioni aggiornate dei cmdlet nel modulo SQLPS.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: af56a516a526e2363846b199b29f4dd3783cde73
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835388"
---
# <a name="install-the-sql-server-powershell-module"></a>Installare il modulo SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questo articolo include indicazioni per l'installazione del modulo PowerShell **SqlServer**.

## <a name="powershell-modules-for-sql-server"></a>Moduli PowerShell per SQL Server

Esistono due moduli SQL Server PowerShell:

- **SqlServer**: il modulo SqlServer include nuovi cmdlet per il supporto delle nuove funzionalità SQL. Il modulo contiene anche le versioni aggiornate dei cmdlet in **SQLPS**. Per scaricare il modulo SqlServer, passare al [modulo SqlServer in PowerShell Gallery](https://www.powershellgallery.com/packages/Sqlserver).

- **SQLPS**: SQLPS è il modulo usato da [SQL Agent](sql-server-powershell.md#sql-server-agent) per eseguire i processi dell'agente nei passaggi del processo dell'agente usando il sottosistema PowerShell.

> [!NOTE]
> Le versioni del modulo **SqlServer** disponibili in PowerShell Gallery supportano il controllo delle versioni e richiedono PowerShell versione 5.0 o successiva.

Per gli argomenti della Guida, vedere:

- Cmdlet di [SqlServer](/powershell/module/sqlserver).
- Cmdlet di [SQLPS](/powershell/module/sqlps).

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

[SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) non installa nessuno dei due moduli di PowerShell. Per usare PowerShell con SSMS, installare il modulo **SqlServer** da [PowerShell Gallery](https://www.powershellgallery.com/packages/Sqlserver).

> [!NOTE]
> Con SSMS 16.x, una versione precedente del modulo **SqlServer** è inclusa in SQL Server Management Studio (SSMS)

## <a name="azure-data-studio"></a>Azure Data Studio

[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) non installa nessuno dei due moduli di PowerShell. Per usare PowerShell con Azure Data Studio, installare il modulo **SqlServer** da [PowerShell Gallery](https://www.powershellgallery.com/packages/Sqlserver).

È possibile usare l'[estensione di PowerShell](../azure-data-studio/extensions/powershell-extension.md), che offre supporto avanzato per l'editor PowerShell in Azure Data Studio.

## <a name="installing-or-updating-the-sqlserver-module"></a>Installazione o aggiornamento del modulo SqlServer

Per installare il modulo **SqlServer** da PowerShell Gallery, avviare una sessione di [PowerShell](/powershell/scripting/overview) come amministratore. È anche possibile avviare Azure Data Studio come amministratore ed eseguire questi comandi in una sessione di PowerShell nel terminale integrato.

È inoltre possibile usare *Install-Module SQLServer -Scope CurrentUser* per l'esecuzione con autorizzazioni elevate. Questo cmdlet è utile per gli utenti che non sono amministratori nell'ambiente in uso. Tuttavia, poiché l'ambito è limitato all'utente corrente, altri utenti nello stesso computer non possono usare il modulo.

### <a name="install-the-sqlserver-module"></a>Installare il modulo SqlServer

Eseguire il comando seguente nella sessione di PowerShell per installare il modulo SqlServer per tutti gli utenti:

```powershell
Install-Module -Name SqlServer
```

### <a name="to-view-the-versions-of-the-sqlserver-module-installed"></a>Per visualizzare le versioni del modulo SqlServer installate

Eseguire il comando seguente per visualizzare le versioni del modulo SqlServer installate

```powershell
Get-Module SqlServer -ListAvailable
```

### <a name="install-for-the-current-user-rather-than-as-an-administrator"></a>Installare per l'utente corrente anziché come amministratore

Se non è possibile eseguire la sessione di PowerShell come amministratore, eseguire l'installazione per l'utente corrente usando il comando seguente:

```powershell
Install-Module -Name SqlServer -Scope CurrentUser
```

### <a name="to-overwrite-a-previous-version-of-the-sqlserver-module"></a>Per sovrascrivere una versione precedente del modulo SqlServer

È anche possibile usare il comando `Install-Module` per sovrascrivere una versione precedente.

```powershell
Install-Module -Name SqlServer -AllowClobber
```

> [!Note]
> PowerShell usa sempre il modulo più recente installato.

### <a name="update-the-installed-version-of-the-sqlserver-module"></a>Aggiornare la versione installata del modulo SqlServer

Quando sono disponibili versioni aggiornate del modulo **SqlServer**, è possibile installare la versione più recente usando il comando seguente:

```powershell
Install-Module -Name SqlServer -AllowClobber
```

È possibile usare il comando `Update-Module` per installare la versione più recente del modulo SQLServer di PowerShell, ma le versioni precedenti non vengono rimosse. La versione più recente viene installata affiancata per consentire di provare la versione più recente e mantenere installati i moduli precedenti.

Tuttavia, se non si vuole mantenere le versioni precedenti del modulo, è possibile usare il comando `Uninstall-Module` per rimuovere le versioni precedenti.

È possibile usare il comando seguente per verificare se è installata più di una versione:

```powershell
Get-Module SqlServer -ListAvailable
```

Per rimuovere le versioni precedenti, è possibile usare il comando seguente:

```powershell
Uninstall-module -Name SQLServer -RequiredVersion "<version number>" -AllowClobber
```

### <a name="troubleshooting"></a>Risoluzione dei problemi

Se si verificano problemi durante l'installazione, vedere la [documentazione per l'installazione del modulo](https://www.powershellgallery.com/packages/PowerShellGet/2.2.1) e la [guida di riferimento per l'installazione del modulo](/powershell/module/powershellget/Install-Module).

## <a name="using-a-specific-version-of-the-sqlserver-module"></a>Uso di una versione specifica del modulo SqlServer

Per usare una versione specifica del modulo, importarlo con un numero di versione specifico con il comando seguente:

```powershell
Import-Module SqlServer -Version 21.1.18218
```

## <a name="pre-release-versions-of-the-sqlserver-module"></a>Versioni non definitive del modulo SqlServer

Versioni non definitive (o di "anteprima") del modulo SqlServer possono essere disponibili in PowerShell Gallery.

> [!IMPORTANT]
> Queste versioni possono essere individuate e installate usando i cmdlet aggiornati *Find-Module* e *Install-Module* inclusi nel modulo [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet) passando l'opzione *-AllowPrerelease*. Per usare questi cmdlet, installare il modulo PowerShellGet, quindi aprire una nuova sessione.

### <a name="to-discover-pre-release-versions-of-the-sqlserver-module"></a>Per individuare le versioni non definitive del modulo SqlServer

Per individuare la versione non definitiva (anteprima) del modulo SqlServer, eseguire il comando seguente:

```powershell
Find-Module SqlServer -AllowPrerelease
```

### <a name="to-install-a-specific-pre-release-version-of-the-sqlserver-module"></a>Per installare una specifica versione non definitiva del modulo SqlServer

Per installare una specifica versione non definitiva del modulo, installarla con un numero di versione specifico.

È possibile provare a usare il comando seguente:

```powershell
Install-Module SqlServer -RequiredVersion 21.1.18218-preview -AllowPrerelease
```

## <a name="sql-server-powershell-on-linux"></a>SQL Server PowerShell in Linux

Per informazioni su come installare SQL Server PowerShell in Linux, vedere [Gestire SQL Server in Linux con PowerShell Core](../linux/sql-server-linux-manage-powershell-core.md).

## <a name="other-modules"></a>Altri moduli

- [Az.Sql](https://www.powershellgallery.com/packages/Az.Sql/) - Cmdlet del servizio SQL per Azure Resource Manager in Windows PowerShell e PowerShell Core.

- [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc/) - Modulo con risorse DSC per la distribuzione e la configurazione di Microsoft SQL Server.

## <a name="cmdlet-reference"></a>Informazioni di riferimento sui cmdlet

- [Cmdlet di SqlServer](/powershell/module/sqlserver)
- [Cmdlet di SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Passaggi successivi

- [SQL Server PowerShell](sql-server-powershell.md)
- [Cmdlet di SQL Server PowerShell](/powershell/module/sqlserver)
- [Usare PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)