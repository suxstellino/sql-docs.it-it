---
title: Configurazione di common criteria compliance enabled
description: Informazioni sui criteri abilitati dall'opzione common criteria compliance in SQL Server. Vedere come rispettare il livello di garanzia di valutazione dei criteri comuni. Per l'approvazione della certificazione EUCC. Obblighi di conformità A livello globale in settori e autorità regolamentate.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- common criteria compliance
helpviewer_keywords:
- CC (common criteria) [Database Engine]
- common criteria compliance [Database Engine]
- Risidual Information Protection [Database Engine]
- RIP (Residual Information Protection)
author: markingmyname
ms.author: maghan
ms.reviewer: wopeter
ms.custom: ''
ms.date: 04/07/2021
ms.openlocfilehash: 8d601d227ebfc63007cb0af9cd859316d5c93357
ms.sourcegitcommit: 09122d02fc3d86c6028366653337c083da8a3f4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107072423"
---
# <a name="common-criteria-compliance-enabled-server-configuration"></a>Opzione di configurazione del server Common Criteria Compliance Enabled

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'opzione Common Criteria Compliance Enabled attiva gli elementi seguenti, necessari per [Common Criteria for Information Technology Security Evaluation](https://www.commoncriteriaportal.org) (Criteri comuni per la valutazione della sicurezza IT). Requisito per un obbligo di conformità globale in settori e autorità regolamentate.

| Criteri | Descrizione |
|----------|-------------|
| Protezione delle informazioni residuali (RIP, Residual Information Protection) | RIP richiede la sovrascrittura di una allocazione di memoria con uno schema di bit noto prima che la memoria venga riallocata a una nuova risorsa. La conformità allo standard RIP consente una maggior sicurezza, ma la sovrascrittura dell'allocazione di memoria può rallentare le prestazioni. La sovrascrittura viene eseguita dopo l'attivazione dell'opzione common criteria compliance enabled. |
|Possibilità di visualizzare le statistiche relative agli accessi | Il controllo degli accessi è abilitato dopo l'abilitazione dell'opzione common criteria compliance. </br></br></br> Orari di accesso resi disponibili in base a ogni sessione ogni volta che un utente accede correttamente a SQL Server: </br> -Informazioni sull'ora dell'ultimo accesso riuscito </br> -Ora dell'ultimo accesso non riuscito </br> : Numero di tentativi tra l'ultimo accesso riuscito e l'account di accesso corrente. </br></br></br> Queste statistiche di accesso possono essere visualizzate eseguendo una query sulla vista a gestione dinamica [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) . |
|La colonna `GRANT` non deve eseguire l'override della tabella `DENY` | Dopo l'attivazione dell'opzione common criteria compliance enabled, un'istruzione `DENY` a livello di tabella ha la precedenza su un'istruzione `GRANT` a livello di colonna. Quando l'opzione non è abilitata, un livello di colonna ha la `GRANT` precedenza su un livello di tabella `DENY` . |

L'opzione Common Criteria Compliance Enabled è un'opzione avanzata. I criteri comuni vengono valutati e certificati solo per l'edizione Enterprise e l'edizione Datacenter. Per lo stato più recente della certificazione dei criteri comuni, vedere il Microsoft SQL Server sito dei [criteri comuni](https://go.microsoft.com/fwlink/?LinkId=616319) .

> [!IMPORTANT]
> Oltre ad attivare l'opzione common criteria compliance enabled, è necessario scaricare ed eseguire uno script che completa la configurazione di SQL Server per la conformità al livello di garanzia di valutazione 4 + (EAL4 +) dei criteri comuni. È possibile scaricare questo script dal Microsoft SQL Server sito dei [criteri comuni](https://go.microsoft.com/fwlink/?LinkId=616319) .

Se si usa il `sp_configure` stored procedure di sistema per modificare l'impostazione, è possibile modificare common criteria compliance enabled solo quando il valore di Show Advanced Options è impostato su 1. L'impostazione diventa effettiva dopo il riavvio del server. I possibili valori sono 0 e 1:

- 0 indica che la conformità ai criteri comuni non è abilitata (impostazione predefinita).

- 1 indica che la conformità ai criteri comuni è abilitata.

## <a name="examples"></a>Esempi

Nell'esempio seguente viene abilitata la conformità ai criteri comuni.

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'common criteria compliance enabled', 1;
GO
RECONFIGURE WITH OVERRIDE;
GO
```

Riavviare SQL Server.

## <a name="next-steps"></a>Passaggi successivi

- [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
