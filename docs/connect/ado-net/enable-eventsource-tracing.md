---
title: Abilitare la traccia eventi in SqlClient
description: Descrive come abilitare la traccia eventi in SqlClient implementando un listener di eventi e come accedere ai dati dell'evento.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: c13942dc5633deec4759ebd4c8a5661654c0952b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837088"
---
# <a name="enable-event-tracing-in-sqlclient"></a>Abilitare la traccia eventi in SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

[Event Tracing for Windows (ETW)](/windows/win32/etw/event-tracing-portal) è un'efficiente funzionalità di traccia a livello di kernel che consente di registrare gli eventi definiti dal driver per scopi di debug e test. SqlClient supporta l'acquisizione di eventi ETW a diversi livelli informativi. Per iniziare ad acquisire questi eventi, le applicazioni client devono restare in ascolto degli eventi dall'implementazione EventSource di SqlClient:

```
Microsoft.Data.SqlClient.EventSource
```

L'implementazione corrente supporta le parole chiave degli eventi seguenti:

| Nome parola chiave | Valore | Description |
| ------------ | ----- | ----------- |
| ExecutionTrace | 1 | Attiva l'acquisizione di eventi di avvio/arresto prima e dopo l'esecuzione del comando. |
| Trace | 2 | Attiva l'acquisizione di eventi di traccia del flusso applicazione di base. |
| Scope | 4 | Attiva l'acquisizione di eventi di ingresso e uscita |
| NotificationTrace | 8 | Attiva l'acquisizione di eventi di traccia di `SqlNotification` |
| NotificationScope | 16 | Attiva l'acquisizione di eventi di ingresso e uscita dell'ambito di `SqlNotification` |
| PoolerTrace | 32 | Attiva l'acquisizione di eventi di traccia del flusso del pool di connessioni |
| PoolerScope | 64 | Attiva l'acquisizione di eventi di traccia dell'ambito del pool di connessioni |
| AdvancedTrace | 128 | Attiva l'acquisizione di eventi di traccia del flusso avanzati |
| AdvancedTraceBin  | 256 | Attiva l'acquisizione di eventi di traccia del flusso avanzati con informazioni aggiuntive |
| CorrelationTrace | 512 | Attiva l'acquisizione di eventi di traccia del flusso di correlazione. |
| StateDump | 1024 | Attiva l'acquisizione del backup dello stato completo di `SqlConnection` |
| SNITrace | 2048 | Attiva l'acquisizione di eventi di traccia del flusso dall'implementazione di rete gestita (applicabile solo in .NET Core) |
| SNIScope | 4096 | Attiva l'acquisizione di eventi dell'ambito dall'implementazione di rete gestita (applicabile solo in .NET Core) |
|||

## <a name="example"></a>Esempio

L'esempio seguente abilita la traccia eventi per un'operazione sui dati nel database di esempio **AdventureWorks** e visualizza gli eventi nella finestra della console.

[!code-csharp [SqlClientEventSource#1](~/../sqlclient/doc/samples/SqlClientEventSource.cs#1)]

## <a name="event-tracing-support-in-native-sni"></a>Supporto della traccia eventi in SNI nativo

**Microsoft.Data.SqlClient** v2.1.0 estende il supporto per la traccia eventi in **Microsoft.Data.SqlClient.SNI** e **Microsoft.Data.SqlClient.SNI.runtime**. Inviando un EventCommand a `SqlClientEventSource`, è possibile raccogliere gli eventi in SNI.dll nativo tramite gli strumenti [Xperf](/windows-hardware/test/wpt/) e [PerfView](https://github.com/microsoft/perfview). I valori di EventCommand validi sono elencati di seguito:

```cs
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

L'esempio seguente abilita la traccia eventi in SNI.dll nativo quando l'applicazione è destinata a .NET Framework. 

```cs
// Native SNI tracing example
// .NET Framework application
using System;
using System.Diagnostics.Tracing;
using Microsoft.Data.SqlClient;

public class SqlClientListener : EventListener
{
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        if (eventSource.Name.Equals("Microsoft.Data.SqlClient.EventSource"))
        {
            // Enables both trace and flow events
            EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
        }
    }
}

class Program
{
    static string connectionString = @"Data Source = localhost; Initial Catalog = AdventureWorks;Integrated Security=true;";

    static void Main(string[] args)
    {
        using (SqlClientListener listener = new SqlClientListener())
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
        }        
    }
}
```

### <a name="use-xperf-to-collect-trace-log"></a>Usare Xperf per raccogliere il log di traccia

1. Avviare la traccia usando la riga di comando seguente.

   ```
   xperf -start trace -f myTrace.etl -on *Microsoft.Data.SqlClient.EventSource
   ```

2. Eseguire l'esempio di traccia SNI nativo per connettersi a SQL Server.

3. Arrestare la traccia usando la riga di comando seguente.

   ```
   xperf -stop trace
   ```

4. Usare PerfView per aprire il file myTrace.etl specificato nel passaggio 1. Il log di traccia SNI è reperibile con i nomi degli eventi `Microsoft.Data.SqlClient.EventSource/SNIScope` e `Microsoft.Data.SqlClient.EventSource/SNITrace`.

   ![Usare PerfView per visualizzare il file di traccia SNI](media/view-event-trace-native-sni.png)


### <a name="use-perfview-to-collect-trace-log"></a>Usare PerfView per raccogliere il log di traccia

1. Avviare PerfView ed eseguire `Collect > Collect` dalla barra dei menu.

2. Configurare il nome del file di traccia, il percorso di output e il nome del provider.

   ![Configurare Prefview prima della raccolta](media/collect-event-trace-native-sni.png)

3. Avviare la raccolta.

4. Eseguire l'esempio di traccia SNI nativo per connettersi a SQL Server.

5. Arrestare la raccolta da PerfView. La generazione del file PerfViewData.etl in base alla configurazione nel passaggio 2 potrebbe richiedere tempo.

6. Aprire il `etl` file in PerfView. Il log di traccia SNI è reperibile con i nomi degli eventi `Microsoft.Data.SqlClient.EventSource/SNIScope` e `Microsoft.Data.SqlClient.EventSource/SNITrace`.

## <a name="external-resources"></a>Risorse esterne  

Per ulteriori informazioni, vedere le risorse seguenti.  
  
|Risorsa|Descrizione|  
|--------------|-----------------|  
|[classe EventSource](/dotnet/api/system.diagnostics.tracing.eventsource)|Utilizzato per creare eventi ETW.|
|[Classe EventListener](/dotnet/api/system.diagnostics.tracing.eventlistener)|Fornisce metodi per abilitare e disabilitare eventi da origini evento.|
