---
title: Usare i percorsi di SQL Server PowerShell | Microsoft Docs
description: Descrizione di come modificare e recuperare le informazioni usando i cmdlet o i metodi e le proprietà dell'oggetto identificato dal percorso del provider.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: f31d8e2c-8d59-4fee-ac2a-324668e54262
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: 19e50c5c389e5fb29316358940501bf7791bd457
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350121"
---
# <a name="work-with-sql-server-powershell-paths"></a>Utilizzo di percorsi di SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Dopo essere passati a un nodo in un percorso di provider [!INCLUDE[ssDE](../includes/ssde-md.md)] , è possibile eseguire operazioni o recuperare informazioni utilizzando i metodi e le proprietà dell'oggetto di gestione di [!INCLUDE[ssDE](../includes/ssde-md.md)] associato al nodo in questione.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Dopo essere passati a un nodo in un percorso di provider [!INCLUDE[ssDE](../includes/ssde-md.md)] è possibile eseguire due tipi di azioni:  
  
-   È possibile eseguire i cmdlet di Windows PowerShell che operano sui nodi, ad esempio **Rename-Item**.  
  
-   È possibile chiamare i metodi dal modello a oggetti di gestione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] associato, ad esempio SMO. Ad esempio, se si passa al nodo Databases in un percorso, è possibile usare i metodi e le proprietà della classe <xref:Microsoft.SqlServer.Management.Smo.Database> .  
  
 Il provider [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene utilizzato per gestire gli oggetti in un'istanza del [!INCLUDE[ssDE](../includes/ssde-md.md)]. Non viene utilizzato per i dati nei database. Se si è passati a una tabella o una vista, non è possibile utilizzare il provider per selezionare, inserire, aggiornare o eliminare i dati. Usare il cmdlet **Invoke-Sqlcmd** per eseguire una query o per modificare dati in tabelle e viste nell'ambiente di Windows PowerShell. Per altre informazioni, vedere [cmdlet Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd).  
  
##  <a name="listing-methods-and-properties"></a><a name="ListPropMeth"></a> Elenco di metodi e proprietà  
 **Elenco di metodi e proprietà**  
  
 Per visualizzare i metodi e le proprietà disponibili per specifici oggetti o classi di oggetti, usare il cmdlet **Get-Member** .  
  
### <a name="examples-listing-methods-and-properties"></a>Esempi: Elenco di metodi e proprietà  
 In questo esempio viene impostata una variabile di Windows PowerShell sulla classe SMO <xref:Microsoft.SqlServer.Management.Smo.Database> e vengono elencati i metodi e le proprietà:  
  
```  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar | Get-Member -Type Methods  
$MyDBVar | Get-Member -Type Properties  
```  
  
 Si può anche usare **Get-Member** per visualizzare un elenco di proprietà e metodi associati al nodo finale di un percorso di Windows PowerShell.  
  
 In questo esempio si passa al nodo Databases in un percorso SQLSERVER: e vengono elencate le proprietà della raccolta:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-Item . | Get-Member -Type Properties  
```  
  
 In questo esempio si passa al nodo AdventureWorks2012 in un percorso SQLSERVER: e vengono elencate le proprietà dell'oggetto:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
Get-Item . | Get-Member -Type Properties  
```  
  
##  <a name="using-methods-and-properties"></a><a name="UsePropMeth"></a> Utilizzo di metodi e proprietà  
 **Utilizzo di metodi e proprietà SMO**  
  
 Per eseguire operazioni su oggetti di un percorso di provider [!INCLUDE[ssDE](../includes/ssde-md.md)] è possibile utilizzare metodi e proprietà SMO.  
  
### <a name="examples-using-methods-and-properties"></a>Esempi: Utilizzo di metodi e proprietà  
 In questo esempio viene usata la proprietà dello **schema** SMO per ottenere un elenco delle tabelle dallo schema Sales di AdventureWorks2012:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables  
Get-ChildItem | where {$_.Schema -eq "Sales"}  
```  
  
 In questo esempio viene usato il metodo **Script** SMO per generare uno script che contiene le istruzioni **CREATE VIEW** necessarie per ricreare le viste in AdventureWorks2012:  
  
```  
Remove-Item C:\PowerShell\CreateViews.sql  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Views  
foreach ($Item in Get-ChildItem) { $Item.Script() | Out-File -Filepath C:\PowerShell\CreateViews.sql -append }  
```  
  
 In questo esempio viene utilizzato il metodo **Create** SMO per creare un database e quindi viene utilizzata la proprietà **State** per indicare se il database esiste:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar.Parent = (Get-Item ..)  
$MyDBVar.Name = "NewDB"  
$MyDBVar.Create()  
$MyDBVar.State  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Provider PowerShell per SQL Server](sql-server-powershell-provider.md)   
 [Spostarsi all'interno dei percorsi di SQL Server PowerShell](navigate-sql-server-powershell-paths.md)   
 [Conversione di URN in percorsi di provider di SQL Server](/powershell/module/sqlserver/Convert-UrnToPath)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
