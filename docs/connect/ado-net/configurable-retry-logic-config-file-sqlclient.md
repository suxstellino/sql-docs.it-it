---
title: File di configurazione della logica di ripetizione dei tentativi configurabile con SqlClient
description: Informazioni su come usare un file di configurazione per specificare i provider di logica di ripetizione dei tentativi predefiniti e personalizzare le opzioni di logica di ripetizione dei tentativi in Microsoft. Data. SqlClient.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: da47a26e992b91729853d5a81a5dff3660c5df0c
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004129"
---
# <a name="configurable-retry-logic-configuration-file-with-sqlclient"></a>File di configurazione della logica di ripetizione dei tentativi configurabile con SqlClient

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il metodo di ripetizione dei tentativi predefinito quando è abilitata l'opzione Safety è `Microsoft.Data.SqlClient.SqlConnection.SqlConfigurableRetryFactory.CreateNoneRetryProvider` per <xref:Microsoft.Data.SqlClient.SqlConnection> e <xref:Microsoft.Data.SqlClient.SqlCommand> . È possibile specificare un metodo di ripetizione dei tentativi diverso tramite un file di configurazione.

## <a name="configuration-sections"></a>Sezioni di configurazione

Le opzioni predefinite per la logica di ripetizione dei tentativi per un'applicazione possono essere modificate aggiungendo le sezioni seguenti all'interno della `configSections` sezione del file di configurazione:

- `SqlConfigurableRetryLogicConnection`: per specificare la logica di ripetizione dei tentativi predefinita per <xref:Microsoft.Data.SqlClient.SqlConnection> .

```csharp
<section name="SqlConfigurableRetryLogicConnection"
        type="Microsoft.Data.SqlClient.SqlConfigurableRetryConnectionSection, Microsoft.Data.SqlClient"/>
```

- `SqlConfigurableRetryLogicCommand`: per specificare la logica di ripetizione dei tentativi predefinita per <xref:Microsoft.Data.SqlClient.SqlCommand> .

```csharp
<section name="SqlConfigurableRetryLogicCommand"
        type="Microsoft.Data.SqlClient.SqlConfigurableRetryCommandSection, Microsoft.Data.SqlClient"/>
```

- `AppContextSwitchOverrides`: .NET Framework supporta le opzioni AppContext tramite una `AppContextSwitchOverrides` sezione, che non deve essere definita in modo esplicito. Per attivare un commutatore in .NET Core, è necessario specificare questa sezione.

```csharp
<section name="AppContextSwitchOverrides"
        type="Microsoft.Data.SqlClient.AppContextSwitchOverridesSection, Microsoft.Data.SqlClient"/>
```

> [!NOTE]
> All'interno della sezione è necessario specificare le configurazioni seguenti `configuration` . Dichiarare queste nuove sezioni per configurare la logica di ripetizione dei tentativi predefinita tramite un file di configurazione dell'applicazione.

### <a name="enable-safety-switch"></a>Abilita opzione di sicurezza

È possibile abilitare l'opzione di sicurezza tramite un file di configurazione. Per informazioni su come abilitarla tramite il codice dell'applicazione, vedere [abilitare la logica di ripetizione dei tentativi configurabile](appcontext-switches.md#enable-configurable-retry-logic).

- **.NET Framework**: per altre informazioni, vedere [elemento AppContextSwitchOverrides](/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element).

```csharp
<runtime>
    <AppContextSwitchOverrides value="Switch.Microsoft.Data.SqlClient.EnableRetryLogic=true"/>
</runtime>
```

- **.NET Core**: supporta più punti e virgola (;) commutatori delimitati come .NET Framework.

```csharp
<AppContextSwitchOverrides value="Switch.Microsoft.Data.SqlClient.EnableRetryLogic=true"/>
```

### <a name="connection-section"></a>Sezione connessione

Per specificare la logica di ripetizione dei tentativi predefinita per tutte le `Microsoft.Data.SqlClient.SqlConnection` istanze di un'applicazione, è possibile usare gli attributi seguenti:

- **numberOfTries**: imposta il numero di tentativi.

- **deltaTime**: imposta l'intervallo di tempo di Gap come un `System.TimeSpan` oggetto.

- **MinTime**: imposta l'intervallo di tempo minimo di Gap consentito come un `System.TimeSpan` oggetto.

- **MaxTime**: imposta l'intervallo di tempo massimo di Gap consentito come un `System.TimeSpan` oggetto.

- **transientErrors**: imposta l'elenco dei numeri di errore temporanei per i quali eseguire un nuovo tentativo.

- **retryMethod**: specifica un creatore del metodo di ripetizione dei tentativi che riceve la configurazione di ripetizione dei tentativi tramite un `Microsoft.Data.SqlClient.SqlRetryLogicOption` parametro e restituisce un `Microsoft.Data.SqlClient.SqlRetryLogicBaseProvider` oggetto.

- **retryLogicType**: imposta un provider di logica di ripetizione dei tentativi personalizzato che contiene gli autori del metodo di ripetizione dei tentativi che forniscono `retryMethod` . Questi metodi devono soddisfare i criteri per `retryMethod` . Usare il nome completo del tipo del provider. Per ulteriori informazioni, vedere [specifica di nomi di tipo completi](/dotnet/framework/reflection-and-codedom/specifying-fully-qualified-type-names).

> [!NOTE]
> Non è necessario specificare `retryLogicType` se si usano i provider di tentativi predefiniti. Per trovare i provider di tentativi incorporati, vedere [provider di logica di ripetizione dei tentativi interni in SqlClient](internal-retry-logic-providers-sqlclient.md).

### <a name="command-section"></a>Sezione Command

Il seguente attributo può essere impostato anche per tutte le <xref:Microsoft.Data.SqlClient.SqlCommand> istanze in un'applicazione:

- **authorizedSqlCondition**: imposta un'espressione regolare di pre-ripetizione dei tentativi per <xref:Microsoft.Data.SqlClient.SqlCommand.CommandText> filtrare istruzioni SQL specifiche.

> [!NOTE]
> L'espressione regolare fa distinzione tra maiuscole e minuscole.

### <a name="examples"></a>Esempio

- Tenta di stabilire una connessione fino a tre volte con un ritardo approssimativo di 1 secondo tra i tentativi usando il `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateFixedRetryProvider` metodo e l'elenco di errori temporanei predefinito:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryMethod ="CreateFixedRetryProvider" 
                                            numberOfTries ="3" deltaTime ="00:00:01"/>
    ```

- Tenta di stabilire una connessione fino a cinque volte con un ritardo di 45 secondi tra i tentativi usando il `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateExponentialRetryProvider` metodo e l'elenco di errori temporanei predefinito:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryMethod ="CreateExponentialRetryProvider" 
                        numberOfTries ="5" deltaTime ="00:00:03" maxTime ="00:00:45"/>
    ```

- Tenta di eseguire un comando fino a quattro volte con un ritardo compreso tra 2 e 30 secondi utilizzando il `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateIncrementalRetryProvider` metodo e l'elenco degli errori temporanei predefinito:

    ```csharp
    <SqlConfigurableRetryLogicCommand retryMethod ="CreateIncrementalRetryProvider"
                        numberOfTries ="4" deltaTime ="00:00:02" maxTime ="00:00:30"/>
    ```

- Tenta di eseguire un comando fino a otto volte con un ritardo da un secondo a un minuto. È limitato ai comandi che `CommandText` contengono la parola `SELECT` e i numeri di eccezione 102 o 997. Usa il `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateIncrementalRetryProvider` metodo incorporato:

    ```csharp
    <SqlConfigurableRetryLogicCommand retryMethod ="CreateIncrementalRetryProvider" 
                            numberOfTries ="8" deltaTime ="00:00:01" maxTime ="00:01:00"
                            transientErrors="102, 997"
                            authorizedSqlCondition="\b(SELECT)\b"/>
    ```

> [!NOTE]
> Nei due esempi successivi è possibile trovare il codice sorgente della logica di ripetizione dei tentativi personalizzata dalle [API di base per la logica di ripetizione dei tentativi configurabili in SqlClient](configurable-retry-logic-core-apis-sqlclient.md#example). Si presuppone che il `CreateCustomProvider` metodo sia definito nella `CustomCRL_Doc.CustomRetry` classe nell' `CustomCRL_Doc.dll` assembly che si trova nella directory in esecuzione dell'applicazione.

- Tenta di stabilire una connessione fino a cinque volte, con un ritardo compreso tra 3 e 45 secondi, i numeri di errore 4060, 997 e 233 nell'elenco e usando il provider di tentativi personalizzato specificato:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryLogicType ="CustomCRL_Doc.CustomRetry, CustomCRL_Doc, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"
                        retryMethod ="CreateCustomProvider" 
                        numberOfTries ="5" deltaTime ="00:00:03" maxTime ="00:00:45"
                        transientErrors ="4060, 997, 233"/>
    ```

- Questo esempio si comporta come quello precedente:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryLogicType ="CustomCRL_Doc.CustomRetry, CustomCRL_Doc"
                        retryMethod ="CreateCustomProvider" 
                        numberOfTries ="5" deltaTime ="00:00:03" maxTime ="00:00:45"
                        transientErrors ="4060, 997, 233"/>
    ```

> [!NOTE]
> I provider di logica di ripetizione dei tentativi verranno memorizzati nella cache al primo utilizzo in una connessione o un comando per un uso futuro durante la durata di un'applicazione.

> [!NOTE]
> Eventuali errori durante la lettura di un file di configurazione dell'applicazione per le impostazioni della logica di ripetizione dei tentativi non causeranno errori nell'applicazione. `Microsoft.Data.SqlClient.SqlConnection.SqlConfigurableRetryFactory.CreateNoneRetryProvider`Verrà invece utilizzato il valore predefinito.
>
> È possibile utilizzare la traccia dell'origine evento per verificare o risolvere i problemi relativi alla configurazione della logica di ripetizione dei tentativi. Per altre informazioni, vedere [abilitare la traccia eventi in SqlClient](enable-eventsource-tracing.md).

## <a name="see-also"></a>Vedi anche

- [Abilita logica di ripetizione dei tentativi configurabile](appcontext-switches.md#enable-configurable-retry-logic)
- [Provider di logica di ripetizione dei tentativi interni in SqlClient](internal-retry-logic-providers-sqlclient.md)
- [Abilitare la traccia eventi in SqlClient](enable-eventsource-tracing.md)
- [Specifica di nomi di tipo completi](/dotnet/framework/reflection-and-codedom/specifying-fully-qualified-type-names)
- [Logica di ripetizione dei tentativi configurabile in SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
