---
title: Identificatori di escape di SQL Server
description: Alcuni caratteri che possono essere presenti negli identificatori delimitati di SQL Server non sono supportati nei percorsi di Windows PowerShell. Ecco come usare il carattere di apice inverso come carattere di escape per questi elementi.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 105acf1e3e67d558ccbb0ae450afba9468de3a5f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838030"
---
# <a name="escape-sql-server-identifiers"></a>Identificatori di escape di SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile usare spesso l'apice inverso (`) come carattere di escape consentito negli identificatori delimitati di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Non è tuttavia possibile usare questo carattere per i nomi dei percorsi di PowerShell. Tuttavia, alcuni caratteri non supportano l'utilizzo dei caratteri di escape. Ad esempio, non è possibile usare i caratteri di escape per i due punti (:) in Windows PowerShell. Gli identificatori che contengono tale carattere devono essere codificati. La codifica è più affidabile dell'utilizzo dei caratteri di escape in quanto è supportata da tutti i caratteri.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

L'apice inverso (`) è posto generalmente sul tasto superiore sinistro della tastiera, sotto il tasto ESC.  

## <a name="examples"></a>Esempi

Di seguito è riportato un esempio di utilizzo dei caratteri di escape in un carattere #:  

```powershell
cd SQLSERVER:\SQL\MyComputer\MyInstance\MyDatabase\MySchema\`#MyTempTable  
```

Di seguito viene riportato un esempio dell'utilizzo di caratteri di escape parentesi in caso di specifica (impostazioni locali) come nome del computer:  

```powershell
Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
```

## <a name="see-also"></a>Vedere anche

- [Identificatori di SQL Server in PowerShell](sql-server-identifiers-in-powershell.md)
- [Provider PowerShell per SQL Server](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)