---
title: Contatori delle prestazioni in SqlClient
description: Usare il provider di dati Microsoft SqlClient per fare in modo che i contatori delle prestazioni di SQL Server verifichino lo stato dell'applicazione e le relative risorse di connessione usando Windows Performance Monitor o a livello di programmazione.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e9d8c2edb88a9ed50b47c761d3af8aec8016065a
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804294"
---
# <a name="performance-counters-in-sqlclient"></a>Contatori delle prestazioni in SqlClient

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

È possibile usare i contatori delle prestazioni <xref:Microsoft.Data.SqlClient> per monitorare lo stato dell'applicazione e le risorse di connessione usate. I contatori delle prestazioni possono essere monitorati tramite Performance Monitor di Windows. In alternativa, è possibile accedervi a livello di codice usando la classe <xref:System.Diagnostics.PerformanceCounter> nello spazio dei nomi <xref:System.Diagnostics>.

## <a name="available-performance-counters"></a>Contatori delle prestazioni disponibili

Attualmente sono disponibili 14 diversi contatori delle prestazioni per <xref:Microsoft.Data.SqlClient> come descritto nella tabella seguente.

|Contatore delle prestazioni|Descrizione|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|Numero di connessioni al secondo eseguite a un server database.|  
|`HardDisconnectsPerSecond`|Numero di disconnessioni al secondo eseguite a un server database.|  
|`NumberOfActiveConnectionPoolGroups`|Numero di gruppi univoci di pool di connessioni attivi. Questo contatore è controllato dal numero di stringhe di connessione univoche disponibili in AppDomain.|  
|`NumberOfActiveConnectionPools`|Numero complessivo di pool di connessioni.|  
|`NumberOfActiveConnections`|Numero di connessioni attive al momento in uso. **Nota:**  Per impostazione predefinita, questo contatore delle prestazioni non è abilitato. Per abilitarlo, consultare [Activate off-by-default counters](#ActivatingOffByDefault) (Attivare i contatori disattivati per impostazione predefinita).|  
|`NumberOfFreeConnections`|Numero di connessioni disponibili per l'uso nei pool di connessioni. **Nota:**  Per impostazione predefinita, questo contatore delle prestazioni non è abilitato. Per abilitarlo, consultare [Activate off-by-default counters](#ActivatingOffByDefault) (Attivare i contatori disattivati per impostazione predefinita).|  
|`NumberOfInactiveConnectionPoolGroups`|Numero di gruppi univoci di pool di connessioni attivi contrassegnati per l'eliminazione. Questo contatore è controllato dal numero di stringhe di connessione univoche disponibili in AppDomain.|  
|`NumberOfInactiveConnectionPools`|Numero di pool di connessioni inattivi in cui non si è verificata alcuna attività recente e che sono in attesa di essere eliminati.|  
|`NumberOfNonPooledConnections`|Numero di connessioni attive che non sono in pool.|  
|`NumberOfPooledConnections`|Numero di connessioni attive gestite dall'infrastruttura del pool di connessioni.|  
|`NumberOfReclaimedConnections`|Numero di connessioni che sono state recuperate tramite Garbage Collection laddove per l'applicazione non è stato chiamato `Close` o `Dispose`. **Nota** La chiusura o l'eliminazione non esplicita delle connessioni danneggia le prestazioni.|  
|`NumberOfStasisConnections`|Numero di connessioni attualmente in attesa del completamento di un'azione e che non sono pertanto disponibili per l'uso da parte dell'applicazione.|  
|`SoftConnectsPerSecond`|Numero di connessioni attive rimosse dal pool di connessioni. **Nota:**  Per impostazione predefinita, questo contatore delle prestazioni non è abilitato. Per abilitarlo, consultare [Activate off-by-default counters](#ActivatingOffByDefault) (Attivare i contatori disattivati per impostazione predefinita).|  
|`SoftDisconnectsPerSecond`|Numero di connessioni attive restituite al pool di connessioni. **Nota:**  Per impostazione predefinita, questo contatore delle prestazioni non è abilitato. Per abilitarlo, consultare [Activate off-by-default counters](#ActivatingOffByDefault) (Attivare i contatori disattivati per impostazione predefinita).|  

### <a name="connection-pool-groups-and-connection-pools"></a>Gruppi di pool di connessioni e pool di connessioni

Quando si usa l'autenticazione di Windows (sicurezza integrata), è necessario monitorare i contatori delle prestazioni `NumberOfActiveConnectionPoolGroups` e `NumberOfActiveConnectionPools`. Il motivo è che questi gruppi di pool di connessioni sono mappati a stringhe di connessione univoche. Quando si usa la sicurezza integrata, i pool di connessioni vengono mappati a stringhe di connessione e inoltre creano pool distinti per le singole identità di Windows. Ad esempio, se Fred e Julie, ognuno all'interno dello stesso AppDomain, usano la stessa stringa di connessione `"Data Source=MySqlServer;Integrated Security=true"`, viene creato un gruppo di pool di connessioni per la stringa di connessione e due pool aggiuntivi, uno per Fred e l'altro per Julie. Se John e Martha usano una stringa di connessione con un account di accesso di SQL Server identico, `"Data Source=MySqlServer;User Id=<myUserID>;Password=<myPassword>"`, verrà creato un singolo pool per l'identità **<myUserID>** .

<a name="ActivatingOffByDefault"></a>

### <a name="activate-off-by-default-counters"></a>Attivare i contatori disattivati per impostazione predefinita

I contatori delle prestazioni `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond` e `SoftConnectsPerSecond` sono disattivati per impostazione predefinita. Aggiungere le informazioni seguenti al file di configurazione dell'applicazione per abilitarli:

```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  

## <a name="retrieve-performance-counter-values"></a>Recuperare i valori del contatore delle prestazioni

Nell'applicazione console seguente viene illustrato come recuperare i valori dei contatori delle prestazioni nell'applicazione. Per la restituzione delle informazioni per tutti i provider di dati Microsoft SqlClient per i contatori delle prestazioni di SQL Server, è necessario che le connessioni siano aperte e attive.

> [!NOTE]
> In questo esempio viene usato il database campione [**AdventureWorks**](../../samples/adventureworks-install-configure.md). Le stringhe di connessione fornite nel codice di esempio presuppongono che il database sia installato e disponibile nel computer locale e che siano stati creati account di accesso corrispondenti a quelli specificati nelle stringhe di connessione. Può essere necessario abilitare gli account di accesso di SQL Server se il server è stato configurato con le impostazioni di sicurezza predefinite, che consentono solo l'autenticazione di Windows. Apportare le modifiche necessarie alle stringhe di connessione in base all'ambiente.

### <a name="example"></a>Esempio

[!code-csharp[SqlClient_PerformanceCounter#1](~/../sqlclient/doc/samples/SqlClient_PerformanceCounter.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Profilatura di runtime](/dotnet/framework/debug-trace-profile/runtime-profiling)
- [Introduzione al monitoraggio delle soglie delle prestazioni](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
