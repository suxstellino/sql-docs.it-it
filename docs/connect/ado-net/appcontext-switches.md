---
title: Opzioni di AppContext in SqlClient
description: Informazioni sulle opzioni AppContext disponibili in SqlClient e su come usarle per modificare alcuni comportamenti predefiniti.
ms.date: 03/24/2021
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: 6ff171065f69ded313d96fd5156cc42daef37a35
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003767"
---
# <a name="appcontext-switches-in-sqlclient"></a>Opzioni di AppContext in SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La classe AppContext consente a SqlClient di fornire nuove funzionalità continuando a supportare i chiamanti che dipendono dal comportamento precedente. Gli utenti possono rifiutare esplicitamente una modifica di comportamento impostando opzioni di AppContext specifiche.

## <a name="enabling-decimal-truncation-behavior"></a>Abilitazione del comportamento di troncamento decimale

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

A partire da Microsoft.Data.SqlClient 2.0, i dati decimali vengono arrotondati per impostazione predefinita, come avviene in SQL Server. Per abilitare il comportamento di troncamento precedente, è possibile impostare l'opzione di AppContext **"Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal"** su `true` all'avvio dell'applicazione:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

## <a name="enabling-managed-networking-on-windows"></a>Abilitazione di reti gestite in Windows

[!INCLUDE [appliesto-xxxx-netcore-netst-md](../../includes/appliesto-xxxx-netcore-netst-md.md)]

In Windows SqlClient usa un'implementazione nativa dell'interfaccia di rete SNI per impostazione predefinita. Per abilitare l'uso dell'implementazione di SNI gestita, è possibile impostare l'opzione di AppContext **"Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows"** su `true` all'avvio dell'applicazione:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Questa opzione attiva o disattiva il comportamento del driver in modo da usare un'implementazione di rete gestita in progetti .NET Core 2.1+ e .NET Standard 2.0+ in Windows, eliminando tutte le dipendenze dalle librerie native per la libreria Microsoft.Data.SqlClient. Questa opzione viene usata solo per scopi di test e debug.

> [!NOTE]
> Esistono alcune differenze note rispetto all'implementazione nativa. Ad esempio, l'implementazione gestita non supporta l'autenticazione di Windows non di dominio.

## <a name="disabling-transparent-network-ip-resolution"></a>Disabilitazione della risoluzione dell'IP di rete trasparente

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

La risoluzione dell'IP di rete trasparente è una revisione della funzionalità MultiSubnetFailover esistente. Influisce sulla sequenza di connessione del driver quando il primo IP risolto del nome host non risponde e al nome host sono associati più IP. La risoluzione dell'IP di rete trasparente interagisce con MultiSubnetFailover per fornire le tre sequenze di connessione seguenti:

* 0: Viene eseguito un tentativo con un indirizzo IP, seguito da tutti gli indirizzi IP in parallelo
* 1: Viene eseguito un tentativo con tutti gli indirizzi IP in parallelo
* 2: Viene eseguito un tentativo con tutti gli indirizzi IP uno dopo l'altro

|TransparentNetworkIPResolution|MultiSubnetFailover|Comportamento|
|--------|--------|--------|
|True|True|1|
|Vero|Falso|0|
|False|True|1|
|False|False|2|

TransparentNetworkIPResolution è abilitata per impostazione predefinita. MultiSubnetFailover è disabilitata per impostazione predefinita. Per disabilitare la risoluzione dell'IP di rete trasparente, è possibile impostare l'opzione di AppContext **"Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString"** su `true` all'avvio dell'applicazione:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString", true);
```

Per altre informazioni sull'impostazione di queste proprietà, vedere la documentazione per la [proprietà SqlConnection.ConnectionString](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring).

## <a name="enable-a-minimum-timeout-during-login"></a>Abilitazione di un timeout minimo durante l'accesso

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Per impedire a un tentativo di accesso di attendere indefinitamente, è possibile impostare l'opzione di AppContext **"Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin"** su `true` all'avvio dell'applicazione:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin", false);
```

## <a name="disable-blocking-behavior-of-readasync"></a>Disabilitazione del comportamento di blocco di ReadAsync

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Per impostazione predefinita, ReadAsync viene eseguito in modo sincrono e blocca il thread chiamante in .NET Framework. Per disabilitare questo comportamento di blocco, è possibile impostare l'opzione di AppContext **"Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking"** su `false` all'avvio dell'applicazione:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking", false);
```

## <a name="enable-configurable-retry-logic"></a>Abilita logica di ripetizione dei tentativi configurabile

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

Per impostazione predefinita, la logica di ripetizione dei tentativi configurabile è disabilitata. Per abilitare questa funzionalità, impostare l'opzione AppContext Switch **. Microsoft. Data. SqlClient. EnableRetryLogic** su `true` all'avvio dell'applicazione. Questa opzione è obbligatoria, anche se un provider di tentativi viene assegnato a una connessione o a un comando.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.EnableRetryLogic", false);
```

* Per informazioni su come abilitare l'opzione usando un file di configurazione, vedere [Enable Safety switch](configurable-retry-logic-config-file-sqlclient.md#enable-safety-switch).

## <a name="see-also"></a>Vedere anche

[Classe AppContext](/dotnet/api/system.appcontext?view=netcore-3.1&preserve-view=true)
