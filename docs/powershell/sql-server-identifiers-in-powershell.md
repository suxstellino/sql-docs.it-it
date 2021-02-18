---
title: Identificatori di SQL Server in PowerShell | Microsoft Docs
description: Informazioni sui percorsi usati dai provider di Windows PowerShell per esporre le gerarchie di dati e sulla necessità di codificare determinati caratteri non supportati da PowerShell in questi percorsi.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- Cmdlets [SQL Server], Encode-Sqlname
- PowerShell [SQL Server], identifiers
- Encode-Sqlname cmdlet
- PowerShell [SQL Server], Encode-Sqlname
- Decode-Sqlname cmdlet
- PowerShell [SQL Server], Decode-Sqlname
- identifiers [SQL Server], PowerShell
- Cmdlets [SQL Server], Decode-Sqlname
ms.assetid: 651099b0-33b4-453a-a864-b067f21eb8b9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: b448e5455b985baffbeb8eb611c3f357e083cff2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354925"
---
# <a name="sql-server-identifiers-in-powershell"></a>Identificatori di SQL Server in PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Il provider di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per Windows PowerShell usa gli identificatori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nei percorsi di Windows PowerShell. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ha identificatori che possono contenere caratteri non supportati nei percorsi di Windows PowerShell. Per tali caratteri è necessario usare caratteri di escape oppure la codifica speciale quando gli identificatori vengono usati nei percorsi di Windows PowerShell.  
  
[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="sql-server-identifiers-in-windows-powershell-paths"></a>Identificatori di SQL Server nei percorsi di Windows PowerShell  

I provider Windows PowerShell espongono le gerarchie di dati tramite una struttura di percorso simile a quella del file system di Windows. Il provider [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implementa i percorsi degli oggetti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per il [!INCLUDE[ssDE](../includes/ssde-md.md)], l'unità è impostata su SQLSERVER: la prima cartella è impostata su \SQL e agli oggetti di database viene fatto riferimento come contenitori ed elementi. Di seguito è riportato il percorso della tabella Vendor nello schema Purchasing del database [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] in un'istanza predefinita del [!INCLUDE[ssDE](../includes/ssde-md.md)]:  
  
```  
SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ha identificatori che sono i nomi degli oggetti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , ad esempio i nomi di tabelle o colonne. Esistono due tipi di identificatori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :  
  
-   Gli identificatori regolari sono limitati a un set di caratteri supportati anche nei percorsi di Windows PowerShell. Questi nomi possono essere utilizzati nei percorsi di Windows PowerShell senza essere modificati.  
  
-   Gli identificatori delimitati possono utilizzare caratteri non supportati nei nomi dei percorsi di Windows PowerShell. Gli identificatori delimitati sono chiamati identificatori tra parentesi se sono racchiusi tra parentesi quadre ([IdentifierName]) e identificatori tra virgolette se sono racchiusi tra virgolette doppie ("IdentifierName"). Se un identificatore delimitato utilizza caratteri non supportati nei percorsi di Windows PowerShell, i caratteri devono essere codificati o devono essere utilizzati caratteri di escape prima di utilizzare l'identificatore come nome di contenitore o di elemento. La codifica è supportata per tutti i caratteri. Per alcuni caratteri, come i due punti (:), non è possibile utilizzare caratteri di escape.  
  
## <a name="sql-server-identifiers-in-cmdlets"></a>Identificatori di SQL Server nei cmdlet  
 Alcuni cmdlet di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dispongono di un parametro che accetta un identificatore come input. I valori del parametro vengono generalmente forniti come costanti di stringa tra virgolette o nelle variabili di stringa. Quando gli identificatori vengono forniti come costanti di stringa o nelle variabili, non si verifica alcun conflitto con il set dei caratteri supportati da Windows PowerShell.  
  
## <a name="sql-server-identifier-tasks"></a>Attività degli identificatori di SQL Server  
  
|Descrizione dell'attività|Articolo|  
|----------------------|-----------|  
|Viene descritto come specificare un nome dell'istanza, includendo il nome del computer in cui è in esecuzione l'istanza.|[Specificare istanze nel provider SQL Server PowerShell](specify-instances-in-the-sql-server-powershell-provider.md)|  
|Viene descritto come specificare la codifica esadecimale per i caratteri negli identificatori delimitati che non sono supportati nei percorsi di Windows PowerShell. Viene inoltre descritto come decodificare i caratteri esadecimali.|[Codificare e decodificare identificatori di SQL Server](encode-and-decode-sql-server-identifiers.md)|  
|Viene descritto come utilizzare il carattere di escape di Windows PowerShell per i caratteri non supportati nei percorsi di PowerShell.|[Identificatori di escape di SQL Server](escape-sql-server-identifiers.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Provider PowerShell per SQL Server](sql-server-powershell-provider.md)   
 [SQL Server PowerShell](sql-server-powershell.md)   
 [Identificatori del database](../relational-databases/databases/database-identifiers.md)  
  
  
