---
title: Eseguire Windows PowerShell da SQL Server Management Studio| Microsoft Docs
description: Informazioni su come avviare una sessione di Windows PowerShell da Esplora oggetti in SQL Server Management Studio, con il percorso preimpostato sulla posizione degli oggetti di propria scelta.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 1f841825-da1f-4062-9a81-3cdbab03845b
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 3e74949152adb8ecadc720ced14ff3b29b196fd6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338166"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Esecuzione di Windows PowerShell da SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile avviare sessioni di Windows PowerShell da **Esplora oggetti** in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] avvia Windows PowerShell, carica il modulo **SqlServer** e imposta il contesto del percorso al nodo associato nell'albero di **Esplora oggetti**.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Quando si specifica l'esecuzione di PowerShell per un oggetto in **Esplora oggetti**, [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] inizia una sessione di Windows PowerShell in cui sono stati caricati e registrati gli snap-in PowerShell di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Il percorso della sessione è preimpostato sulla posizione dell'oggetto su cui si è fatto clic con il pulsante destro del mouse in Esplora oggetti. Ad esempio, facendo clic con il pulsante destro del mouse sull'oggetto del database [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] in Esplora oggetti e selezionando **Avvia PowerShell**, il percorso di PowerShell viene impostato come segue:  

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Esecuzione di PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>Per eseguire PowerShell da SQL Server Management Studio

1. Aprire **Esplora oggetti**.

2. Passare al nodo corrispondente all'oggetto desiderato.

3. Fare clic con il pulsante destro del mouse sull'oggetto e selezionare **Avvia PowerShell**.

## <a name="permissions"></a>Autorizzazioni

Quando viene aperto da [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], PowerShell non viene eseguito con privilegi di amministratore, che possono impedire determinate attività quali le chiamate a WMI.  
  
## <a name="see-also"></a>Vedere anche

- [SQL Server PowerShell](sql-server-powershell.md)