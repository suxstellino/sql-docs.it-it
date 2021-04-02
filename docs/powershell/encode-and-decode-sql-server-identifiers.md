---
title: Codificare e decodificare identificatori di SLQ Server
description: Alcuni caratteri che possono essere presenti negli identificatori delimitati di SQL Server non sono supportati nei percorsi di Windows PowerShell. Ecco come includerli rappresentandoli con i relativi valori esadecimali.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 46eb3f29ce21aada716458d554dd329b1bd10dff
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981187"
---
# <a name="encode-and-decode-sql-server-identifiers"></a>Codificare e decodificare identificatori di SLQ Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gli identificatori delimitati di SQL Server possono contenere caratteri non supportati nei percorsi di Windows PowerShell. È possibile specificare questi caratteri codificando i valori esadecimali.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

I caratteri non supportati nei nomi dei percorsi di Windows PowerShell possono essere rappresentati o codificati come il carattere "%" seguito dal valore esadecimale del modello di bit che rappresenta il carattere, come in " **%** xx". La codifica può sempre essere utilizzata per gestire i caratteri non supportati nei percorsi di Windows PowerShell.

Il cmdlet **Encode-SqlName** accetta come input un identificatore di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Viene restituita una stringa con tutti i caratteri non supportati dal linguaggio di Windows PowerShell codificati con "% xx". Il cmdlet **Decode-SqlName** accetta come input un identificatore di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] codificato e restituisce l'identificatore originale.  

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

I cmdlet **Encode-Sqlname** e **Decode-Sqlname** codificano o decodificano solo i caratteri consentiti negli identificatori delimitati di SQL Server, ma non sono supportati nei percorsi di PowerShell. Di seguito sono elencati i caratteri codificati da **Encode-SqlName** e decodificati da **Decode-SqlName**:

|**Carattere**|\ |/|:|%|\<|>|*|?|[|]|&#124;|  
|-|-|-|-|-|-|-|-|-|-|-|-|
|**Codifica esadecimale**|%5C|%2F|%3A|%25|%3C|%3E|%2A|%3F|%5B|%5D|%7C|

## <a name="encoding-an-identifier"></a>Codifica di un identificatore  

### <a name="to-encode-a-sql-server-identifier-in-a-powershell-path"></a>Per codificare un identificatore di SQL Server in un percorso di PowerShell

- Utilizzare uno dei due metodi per codificare un identificatore di SQL Server:
    - Specificare il codice esadecimale per il carattere non supportato utilizzando la sintassi% XX, dove XX rappresenta il codice esadecimale.
    - Passare l'identificatore come stringa tra virgolette al cmdlet **Encode-Sqlname**

### <a name="examples-encoding"></a>Esempi (codifica)

In questo esempio viene specificata la versione codificata del carattere ":" (% 3A):

```powershell
Set-Location Table%3ATest
```

In alternativa, è possibile usare **Encode-SqlName** per compilare un nome supportato da Windows PowerShell:

```powershell
Set-Location (Encode-SqlName "Table:Test")
```

## <a name="decoding-an-identifier"></a>Decodifica di un identificatore

### <a name="to-decode-a-sql-server-identifier-from-a-powershell-path"></a>Per decodificare un identificatore di SQL Server da un percorso di PowerShell

Usare il cmdlet **Decode-Sqlname** per sostituire le codifiche esadecimali con i caratteri rappresentati dalla codifica.

### <a name="examples-decoding"></a>Esempi (decodifica)

In questo esempio viene restituito "Table:Test":

```powershell
Decode-SqlName "Table%3ATest"
```

## <a name="see-also"></a>Vedere anche

- [Identificatori di SQL Server in PowerShell](sql-server-identifiers-in-powershell.md)
- [Provider PowerShell per SQL Server](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)  
