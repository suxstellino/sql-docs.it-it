---
title: Esecuzione di Windows PowerShell da SQL Server Management Studio
description: Informazioni su come avviare una sessione di Windows PowerShell da Esplora oggetti in SQL Server Management Studio, con il percorso preimpostato sulla posizione degli oggetti di propria scelta.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 9fd9b7039680b05515d1f70102408394b5443c9c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839462"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Esecuzione di Windows PowerShell da SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile avviare sessioni di Windows PowerShell da **Esplora oggetti** in SQL Server Management Studio (SSMS). SSMS avvia Windows PowerShell, carica il modulo **SqlServer** e imposta il contesto del percorso sul nodo associato nell'albero **Esplora oggetti** .

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Quando si specifica l'esecuzione di PowerShell per un oggetto in **Esplora oggetti**, SQL Server Management Studio avvia una sessione di Windows PowerShell in cui gli snap-in SQL Server PowerShell sono stati caricati e registrati. Il percorso della sessione è preimpostato sulla posizione dell'oggetto su cui si è fatto clic con il pulsante destro del mouse in Esplora oggetti.

Se ad esempio si fa clic con il pulsante destro del mouse sull'oggetto di database AdventureWorks in Esplora oggetti e si seleziona **Avvia PowerShell**, il percorso di Windows PowerShell viene impostato come illustrato di seguito:

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Esecuzione di PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>Per eseguire PowerShell da SQL Server Management Studio

1. Aprire **Esplora oggetti**.

2. Passare al nodo corrispondente all'oggetto desiderato.

3. Fare clic con il pulsante destro del mouse sull'oggetto e selezionare **Avvia PowerShell**.

## <a name="permissions"></a>Autorizzazioni

Quando viene aperto da SQL Server Management Studio, PowerShell non viene eseguito con privilegi di amministratore, che potrebbero impedire alcune attività quali chiamate a WMI.

## <a name="see-also"></a>Vedere anche

- [SQL Server PowerShell](sql-server-powershell.md)