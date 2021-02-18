---
title: Gestione del completamento alla pressione del tasto TAB (SQL Server PowerShell)
description: Informazioni su come controllare il completamento alla pressione del tasto TAB di Windows PowerShell usando nel modo corretto tre variabili nei moduli di SQL Server PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 6296848a-890f-4ad3-8d9f-92ed6a79aa00
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: cca82925c95b360bff522bc813798485628d4382
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339041"
---
# <a name="manage-tab-completion-with-sql-server-powershell"></a>Gestire il completamento alla pressione del tasto TAB (SQL Server PowerShell)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gli snap-in PowerShell di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] introducono tre variabili (**$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** e **$SqlServerIncludeSystemObjects**) per controllare il completamento alla pressione del tasto TAB di Windows PowerShell. Il completamento alla pressione del tasto TAB consente di ridurre la digitazione restituendo tabelle di elementi i cui nomi iniziano con la stringa che si sta digitando.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Con il completamento alla pressione del tasto TAB di Windows PowerShell, dopo aver digitato parte del nome di un percorso o di un cmdlet, è possibile premere il tasto TAB per ottenere un elenco degli elementi il cui nome corrisponde a quanto già digitato. È quindi possibile selezionare l'elemento desiderato dall'elenco senza digitare il resto del nome.  

Se si usa un database che contiene molti oggetti, gli elenchi di completamento alla pressione del tasto TAB possono diventare molto grandi. Anche alcuni tipi di oggetto di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , ad esempio le viste, includono numerosi oggetti di sistema.  

Gli snap-in di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] introducono tre variabili di sistema che possono essere usate per controllare la quantità di informazioni generata dal completamento alla pressione del tasto TAB e da **Get-ChildItem**.

## <a name="sqlservermaximumtabcompletion--n"></a>$SqlServerMaximumTabCompletion =** *n*

Specifica il numero massimo di oggetti da includere in un elenco di completamento alla pressione del tasto TAB. Se si seleziona il tasto TAB in un nodo del percorso con più di *n* oggetti, l'elenco di completamento alla pressione del tasto TAB viene troncato in corrispondenza di *n*. *n* è un numero intero. L'impostazione predefinita è 0, che indica che non esiste alcun limite al numero di oggetti elencati.  

## <a name="sqlservermaximumchilditems--n"></a>$SqlServerMaximumChildItems =** *n*

Specifica il numero massimo di oggetti visualizzati da **Get-ChildItem**. Se **Get-ChildItem** viene eseguito in un nodo del percorso con più di *n* oggetti, l'elenco viene troncato in corrispondenza di *n*. *n* è un numero intero. L'impostazione predefinita è 0, che indica che non esiste alcun limite al numero di oggetti elencati.  

## <a name="sqlserverincludesystemobjects---true--false-"></a>$SqlServerIncludeSystemObjects =** { **$True** | **$False** }

Se **$True**, gli oggetti di sistema vengono visualizzati dal completamento alla pressione del tasto TAB e da **Get-ChildItem**. Se **$False**, non viene visualizzato alcun oggetto di sistema. L'impostazione predefinita è **$False**.  

## <a name="set-the-sql-server-tab-completion-variables"></a>Impostazione delle variabili per il completamento alla pressione del tasto TAB

Per tutte le variabile di cui si desidera modificare il valore predefinito, impostare la variabile sul nuovo valore.  

### <a name="example-powershell"></a>Esempio (PowerShell)

Nell'esempio seguente vengono impostate tutte e tre le variabili e vengono elencate le relative impostazioni:  

```powershell
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```

## <a name="see-also"></a>Vedere anche

- [Installare SQL Server PowerShell](download-sql-server-ps-module.md)
- [SQL Server PowerShell](sql-server-powershell.md)