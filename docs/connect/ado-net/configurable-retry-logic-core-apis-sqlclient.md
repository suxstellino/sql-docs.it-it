---
title: API di base della logica di ripetizione dei tentativi configurabili in SqlClient
description: Informazioni su come usare le API di base per la logica di ripetizione dei tentativi configurabili per implementare la logica di ripetizione dei tentativi personalizzata nell'applicazione con Microsoft. Data. SqlClient.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: 83053651938adfb51640ee9bdb096c2b2c8b4708
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004119"
---
# <a name="configurable-retry-logic-core-apis-in-sqlclient"></a>API di base della logica di ripetizione dei tentativi configurabili in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Se i provider di logica di ripetizione dei tentativi incorporati non sono in grado di soddisfare le proprie esigenze, è possibile creare provider personalizzati. È quindi possibile assegnare tali provider a un `SqlConnection` `SqlCommand` oggetto o per applicare la logica personalizzata.

I provider predefiniti sono progettati intorno a tre interfacce che possono essere utilizzate per implementare provider personalizzati. I provider di tentativi personalizzati possono quindi essere usati in modo analogo ai provider di tentativi interni in un <xref:Microsoft.Data.SqlClient.SqlConnection> o <xref:Microsoft.Data.SqlClient.SqlCommand> :

1. `Microsoft.Data.SqlClient.SqlRetryIntervalBaseEnumerator`: Genera una sequenza di intervalli di tempo.
2. `Microsoft.Data.SqlClient.SqlRetryLogicBase`: Recupera l'intervallo di tempo successivo per un enumeratore specificato, se il numero di tentativi non è stato superato e viene soddisfatta una condizione temporanea.
3. `Microsoft.Data.SqlClient.SqlRetryLogicBaseProvider`: Applica la logica di ripetizione dei tentativi alle operazioni di connessione e comando.

> [!CAUTION]
> Implementando un provider di logica di ripetizione dei tentativi personalizzato, l'utente è responsabile di tutti gli aspetti, tra cui la concorrenza, le prestazioni e la gestione delle eccezioni.

## <a name="example"></a>Esempio

L'implementazione in questo esempio è il più semplice possibile per illustrare la personalizzazione dettagliata. Non include procedure avanzate come thread safety, Async e concorrenza. Per informazioni approfondite su un'implementazione reale, è possibile studiare la logica di ripetizione dei tentativi predefinita nel [repository GitHub Microsoft. Data. SqlClient](https://github.com/dotnet/SqlClient/).

1. Definire classi di logica di ripetizione dei tentativi configurabili personalizzati:

    - **Enumeratore**: definire una sequenza fissa di intervalli di tempo ed estendere l'intervallo di tempo accettabile da due minuti a quattro minuti.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#6](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#6)]

    - **Logica di ripetizione dei tentativi**: implementare la logica di ripetizione dei tentativi in qualsiasi comando che non fa parte di una transazione attiva. Abbassare il numero di tentativi da 60 a 20.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#7](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#7)]

    - **Provider**: implementa un provider di tentativi che esegue un nuovo tentativo sulle operazioni sincrone senza un `Retrying` evento. Aggiunge <xref:System.TimeoutException> ai numeri di <xref:Microsoft.Data.SqlClient.SqlException> errore di eccezione temporanei esistenti.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#8](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#8)]

2. Creare un'istanza del provider di tentativi costituita dai tipi personalizzati definiti:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#4](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#4)]

    - La funzione seguente valuterà un'eccezione usando l'elenco specificato di eccezioni che è possibile ritentare e l' <xref:System.TimeoutException> eccezione speciale per determinare se è reversibile:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#5](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#5)]

3. Usare la logica di ripetizione dei tentativi personalizzata:

    - Definire i parametri della logica di ripetizione dei tentativi:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#1](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#1)]

    - Creare un provider di tentativi personalizzato:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#2](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#2)]

    - Assegnare il provider di tentativi a `Microsoft.Data.SqlClient.SqlConnection.RetryLogicProvider` o `Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider` :

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#3](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#3)]

> [!NOTE]
> Non dimenticare di abilitare l'opzione logica di ripetizione dei tentativi configurabile prima di usarla. Per altre informazioni, vedere [abilitare la logica di ripetizione dei tentativi configurabile](appcontext-switches.md#enable-configurable-retry-logic).

## <a name="see-also"></a>Vedi anche

- [Repository GitHub Microsoft. Data. SqlClient](https://github.com/dotnet/SqlClient/)
- [Logica di ripetizione dei tentativi configurabile in SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
