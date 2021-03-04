---
title: Gestire l'autenticazione a SQL Server in PowerShell
description: Informazioni su come usare l'autenticazione di SQL Server anziché l'autenticazione di Windows (impostazione predefinita) quando ci si connette a un'istanza del motore di database.
titleSuffix: SQL Server on Linux
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 10/14/2020
ms.openlocfilehash: 233c456a2e4c7f41efd6038efccc6b00b68eb72d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838061"
---
# <a name="manage-authentication-to-sql-server-in-powershell"></a>Gestire l'autenticazione a SQL Server in PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Per impostazione predefinita, i componenti PowerShell di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizzano l'autenticazione di Windows in caso di connessione a un'istanza del [!INCLUDE[ssDE](../includes/ssde-md.md)]. È possibile usare l'autenticazione di SQL Server definendo un'unità virtuale PowerShell o specificando i parametri **-Username** e **-Password** per **Invoke-Sqlcmd**.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="permissions"></a>Autorizzazioni

Tutte le azioni eseguibili in un'istanza del [!INCLUDE[ssDE](../includes/ssde-md.md)] vengono controllate dalle autorizzazioni concesse alle credenziali di autenticazione utilizzate per connettersi all'istanza. Per impostazione predefinita, il provider e i cmdlet di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizzano l'account di Windows in esecuzione per eseguire una connessione con autenticazione di Windows al [!INCLUDE[ssDE](../includes/ssde-md.md)].  

Per effettuare una connessione con autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è necessario fornire un ID di accesso per l'autenticazione di SQL Server e una password. Se si usa il provider di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , è necessario associare le credenziali di accesso di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a un'unità virtuale e quindi usare il comando di modifica della directory (**cd**) per stabilire la connessione a tale unità. In Windows PowerShell le credenziali di sicurezza possono essere associate solo a unità virtuali.  

## <a name="sql-server-authentication-using-a-virtual-drive"></a>Autenticazione di SQL Server tramite un'unità virtuale

### <a name="to-create-a-virtual-drive-associated-with-a-sql-server-authentication-login"></a>Per creare un'unità virtuale associata a un accesso di autenticazione di SQL Server

1. Creare una funzione che:

    1. Disponga di parametri per il nome da assegnare all'unità virtuale, l'ID di accesso e il percorso del provider da associare all'unità virtuale.

    2. Usi **read-host** per richiedere all'utente la password.  

    3. Usi **new-object** per creare un oggetto credenziali.  

    4. Usi **new-psdrive** per creare un'unità virtuale con le credenziali fornite.  

2. Richiamare la funzione per creare un'unità virtuale con le credenziali fornite.  

#### <a name="example-virtual-drive"></a>Esempio (unità virtuale)

In questo esempio viene creata una funzione denominata **sqldrive** che può essere utilizzata per creare un'unità virtuale associata all'account di accesso con autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e all'istanza specificati.  
  
 La funzione **sqldrive** richiede di immettere la password per effettuare l'accesso, mascherandola durante la digitazione. Quando si usa il comando di modifica della directory (**cd**) per connettersi a un percorso usando il nome di unità virtuale, tutte le operazioni vengono eseguite usando le credenziali di accesso con autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fornite al momento della creazione dell'unità.  
  
```powershell
## Create a function that specifies the login and prompts for the password.  
  
function sqldrive  
{  
    param( [string]$name, [string]$login = "MyLogin", [string]$root = "SQLSERVER:\SQL\MyComputer\MyInstance" )  
    $pwd = read-host -AsSecureString -Prompt "Password"  
    $cred = new-object System.Management.Automation.PSCredential -argumentlist $login,$pwd  
    New-PSDrive $name -PSProvider SqlServer -Root $root -Credential $cred -Scope 1  
}  
  
## Use the sqldrive function to create a SQLAuth virtual drive.  
sqldrive SQLAuth
  
## Set-Location to the virtual drive, which invokes the supplied authentication credentials.  
sl SQLAuth:
```

## <a name="sql-server-authentication-using-invoke-sqlcmd"></a>Autenticazione di SQL Server tramite Invoke-Sqlcmd

### <a name="to-use-invoke-sqlcmd-with-sql-server-authentication"></a>Per utilizzare Invoke-Sqlcmd con l'autenticazione di SQL Server

1. Usare il parametro **-Username** per specificare un ID di accesso e il parametro **-Password** per specificare la password associata.  

#### <a name="example-invoke-sqlcmd"></a>Esempio (Invoke-Sqlcmd)

In questo esempio viene utilizzato l'host della lettura cmdlet per la richiesta di una password all'utente, quindi viene stabilita la connessione tramite l'autenticazione di SQL Server.  

```powershell
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```

## <a name="see-also"></a>Vedere anche

- [SQL Server PowerShell](sql-server-powershell.md)
- [Provider PowerShell per SQL Server](sql-server-powershell-provider.md)
- [Cmdlet Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd)
- [Usare PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)
