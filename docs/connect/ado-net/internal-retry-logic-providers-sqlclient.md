---
title: Provider di logica di ripetizione dei tentativi interni in SqlClient
description: Informazioni su come usare i provider di logica di ripetizione dei tentativi configurabili incorporati nell'applicazione per gestire gli errori temporanei nel database.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: 98a601b0d61f79bed6a802b86caecfabd16f75a6
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004126"
---
# <a name="internal-retry-logic-providers-in-sqlclient"></a>Provider di logica di ripetizione dei tentativi interni in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

I provider di tentativi interni incorporati sono stati implementati per i modelli di ripetizione più comuni. È possibile usare i provider di tentativi usando i `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory` metodi statici seguenti:

- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateFixedRetryProvider`
- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateIncrementalRetryProvider`
- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateExponentialRetryProvider`
- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateNoneRetryProvider`

> [!NOTE]
> Tutti i provider di tentativi interni sono leggermente in modo casuale i tempi di Gap intervallo prima di ogni tentativo. Questa sequenza casuale evita di raggiungere il database nello stesso momento in cui più client tentano di connettersi o eseguono un comando con la stessa configurazione.

> [!WARNING]
> I provider di tentativi interni non supportano un nuovo tentativo su un comando che viene eseguito in una transazione aperta. Questa operazione verrà eseguita senza logica di ripetizione dei tentativi. È possibile eseguire l'override di questo comportamento usando la logica di ripetizione dei tentativi personalizzata. Per altre informazioni, vedere [API di base per la logica di ripetizione dei tentativi configurabili in SqlClient](configurable-retry-logic-core-apis-sqlclient.md).

<!-- These links won't be live until after the feature is released in a GA version.
## Example

You can find samples for `connection` and `command` retry logic at the following links:

- [Microsoft.Data.SqlClient.SqlConnection.RetryLogicProvider#example](/dotnet/api/microsoft.data.sqlclient.sqlconnection.RetryLogicProvider?view=sqlclient-dotnet-core-2.1&preserve-view=true#examples)
- [Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider#example](/dotnet/api/microsoft.data.sqlclient.sqlcommand.RetryLogicProvider?view=sqlclient-dotnet-core-2.1&preserve-view=true#examples)
-->

## <a name="see-also"></a>Vedi anche

- [API di base della logica di ripetizione dei tentativi configurabili in SqlClient](configurable-retry-logic-core-apis-sqlclient.md)
- [Logica di ripetizione dei tentativi configurabile in SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
