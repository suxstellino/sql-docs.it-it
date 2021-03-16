---
title: Opzione di configurazione del server Common Criteria Compliance Enabled | Microsoft Docs
description: Informazioni sui criteri abilitati dall'opzione common criteria compliance in SQL Server e su come rispettare il livello di garanzia di valutazione 4+ dei criteri comuni.
ms.custom: ''
ms.date: 08/21/2018
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- common criteria compliance
helpviewer_keywords:
- CC (common criteria) [Database Engine]
- common criteria compliance [Database Engine]
- Risidual Information Protection [Database Engine]
- RIP (Residual Information Protection)
ms.assetid: 61766eea-c450-408d-af33-fbe7ef8c9ff2
author: rothja
ms.author: jroth
ms.openlocfilehash: af850d1c160e7df312e3aac704162a2e57066b70
ms.sourcegitcommit: 2cc2e4e17ce88ef47cda32a60a02d929e617738e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2021
ms.locfileid: "103473227"
---
# <a name="common-criteria-compliance-enabled-server-configuration"></a>Opzione di configurazione del server Common Criteria Compliance Enabled
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'opzione Common Criteria Compliance Enabled attiva gli elementi seguenti, necessari per [Common Criteria for Information Technology Security Evaluation](https://www.commoncriteriaportal.org/) (Criteri comuni per la valutazione della sicurezza IT).  
  
|Criteri|Descrizione|  
|--------------|-----------------|  
|Protezione delle informazioni residuali (RIP, Residual Information Protection)|RIP richiede la sovrascrittura di una allocazione di memoria con uno schema di bit noto prima che la memoria venga riallocata a una nuova risorsa. La conformità allo standard RIP consente una maggior sicurezza, ma la sovrascrittura dell'allocazione di memoria può rallentare le prestazioni. La sovrascrittura viene eseguita dopo l'attivazione dell'opzione common criteria compliance enabled.|  
|Possibilità di visualizzare le statistiche relative agli accessi|Dopo l'attivazione dell'opzione common criteria compliance enabled, viene attivato il controllo degli accessi. Ogni volta che un utente accede a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vengono rese disponibili le informazioni relative all'ora dell'ultimo accesso riuscito, alla data e all'ora dell'ultimo accesso non riuscito e al numero di tentativi compresi tra l'ultimo accesso riuscito e quello corrente per ogni singola sessione. Queste statistiche di accesso possono essere visualizzate eseguendo una query sulla vista a gestione dinamica [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) .|  
|La colonna `GRANT` non deve eseguire l'override della tabella `DENY`|Dopo l'attivazione dell'opzione common criteria compliance enabled, un'istruzione `DENY` a livello di tabella ha la precedenza su un'istruzione `GRANT` a livello di colonna. Se l'opzione non è abilitata, un'istruzione `GRANT` a livello di colonna ha la precedenza su un'istruzione `DENY` a livello di tabella.|  
  
 L'opzione Common Criteria Compliance Enabled è un'opzione avanzata. I criteri comuni vengono valutati e certificati solo per l'edizione Enterprise e l'edizione Datacenter. Per lo stato più aggiornato della certificazione con criteri comuni, vedere il sito Web [Criteri comuni per Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=616319) .  
  
> [!IMPORTANT]  
>  Oltre ad attivare l'opzione common criteria compliance enabled, è necessario scaricare ed eseguire uno script che completi la configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la conformità al livello di garanzia di valutazione 4+ (EAL4+) dei criteri comuni. È possibile scaricare questo script dal sito Web relativo ai [criteri comuni per Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=616319) .  
  
 Se si utilizza la stored procedure di sistema `sp_configure` per modificare l'impostazione, è possibile modificare common criteria compliance enabled solo quando il valore di show advanced options è impostato su 1. L'impostazione diventa effettiva dopo il riavvio del server. I possibili valori sono 0 e 1:  
  
-   0 indica che la conformità ai criteri comuni non è abilitata. Questa è la modalità predefinita.  
  
-   1 indica che la conformità ai criteri comuni è abilitata.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene abilitata la conformità ai criteri comuni.  
  
```  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'common criteria compliance enabled', 1;  
GO  
RECONFIGURE WITH OVERRIDE; 
GO  
```  

Riavviare [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
  
## <a name="see-also"></a>Vedere anche  
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
