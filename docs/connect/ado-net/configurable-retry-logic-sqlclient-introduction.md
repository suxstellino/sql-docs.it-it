---
title: Logica di ripetizione dei tentativi configurabile in SqlClient introduttiva
description: Informazioni sui diversi aspetti della logica di ripetizione dei tentativi configurabili in Microsoft. Data. SqlClient e su come rendere l'applicazione resiliente agli errori temporanei.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: 6018dc69c0fa6733b1482a8bdaa75d7cfaa4cb71
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004127"
---
# <a name="configurable-retry-logic-in-sqlclient-introduction"></a>Logica di ripetizione dei tentativi configurabile in SqlClient introduttiva

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La logica di ripetizione dei tentativi configurabile consente agli sviluppatori e agli amministratori di gestire il comportamento dell'applicazione quando si verificano errori La funzionalità consente di aggiungere controlli durante la connessione o l'esecuzione di un comando. I controlli possono essere definiti tramite codice o un file di configurazione dell'applicazione. È possibile definire i numeri di errore temporanei e le proprietà dei tentativi per controllare il comportamento dei tentativi. Inoltre, è possibile utilizzare le espressioni regolari per filtrare istruzioni SQL specifiche.

## <a name="feature-components"></a>Componenti delle funzionalità

Questa funzionalità è costituita da tre componenti principali:

1. **API di base**: gli sviluppatori possono usare queste interfacce per implementare la logica di ripetizione dei tentativi sugli <xref:Microsoft.Data.SqlClient.SqlConnection> <xref:Microsoft.Data.SqlClient.SqlCommand> oggetti e. Per altre informazioni, vedere [API di base per la logica di ripetizione dei tentativi configurabili in SqlClient](configurable-retry-logic-core-apis-sqlclient.md).
2. **Logica di ripetizione dei tentativi configurabile** predefiniti: i metodi di logica di ripetizione dei tentativi incorporati che usano le API di base sono accessibili dalla `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory` classe. Per altre informazioni, vedere [provider di logica di ripetizione dei tentativi interni in SqlClient](internal-retry-logic-providers-sqlclient.md).
3. **Schema del file di configurazione**: per specificare la logica di ripetizione dei tentativi predefinita per <xref:Microsoft.Data.SqlClient.SqlConnection> e <xref:Microsoft.Data.SqlClient.SqlCommand> in un'applicazione. Per altre informazioni, vedere [file di configurazione della logica di ripetizione dei tentativi configurabile con SqlClient](configurable-retry-logic-config-file-sqlclient.md).

## <a name="quick-start"></a>Avvio rapido

Per usare questa funzionalità, seguire questi quattro passaggi:

1. Abilitare l'opzione Safety nella versione di anteprima. Per informazioni su come abilitare l'opzione di sicurezza AppContext, vedere [abilitare la logica di ripetizione dei tentativi configurabile](appcontext-switches.md#enable-configurable-retry-logic).

2. Definire le opzioni per la logica di ripetizione dei tentativi utilizzando `Microsoft.Data.SqlClient.SqlRetryLogicOption` .  
In questo esempio, alcuni parametri di ripetizione dei tentativi vengono impostati e il resto utilizzerà i valori predefiniti.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_OpenConnection#1](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_OpenConnection.cs#1)]

3. Creare un provider di logica di ripetizione dei tentativi utilizzando l' `Microsoft.Data.SqlClient.SqlRetryLogicOption` oggetto.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_OpenConnection#2](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_OpenConnection.cs#2)]

4. Assegnare l' `Microsoft.Data.SqlClient.SqlRetryLogicBaseProvider` istanza a `Microsoft.Data.SqlClient.SqlConnection.RetryLogicProvider` o `Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider` .  
In questo esempio, il comando di apertura della connessione tenterà di nuovo se raggiunge uno degli errori temporanei nell' `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory` elenco interno per un massimo di cinque volte.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_OpenConnection#3](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_OpenConnection.cs#3)]

> [!NOTE]
> Questi passaggi sono gli stessi per l'esecuzione di un comando, ad eccezione del fatto che il provider di tentativi viene assegnato alla `Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider` proprietà prima dell'esecuzione del comando.

## <a name="see-also"></a>Vedi anche

- [API di base della logica di ripetizione dei tentativi configurabili in SqlClient](configurable-retry-logic-core-apis-sqlclient.md)
- [Provider di logica di ripetizione dei tentativi interni in SqlClient](internal-retry-logic-providers-sqlclient.md)
- [File di configurazione della logica di ripetizione dei tentativi configurabile con SqlClient](configurable-retry-logic-config-file-sqlclient.md)
- [Abilita logica di ripetizione dei tentativi configurabile](appcontext-switches.md#enable-configurable-retry-logic)
- [Logica di ripetizione dei tentativi configurabile in SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
