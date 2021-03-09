---
title: Ciclo di vita del supporto per il driver SqlClient
description: Pagina che contiene informazioni sul ciclo di vita del supporto tecnico.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 998fc5eecfa0e8840111b1ee9bf1d9e653ac5687
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464733"
---
# <a name="sqlclient-driver-support-lifecycle"></a>Ciclo di vita del supporto per il driver SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La libreria Microsoft.Data.SqlClient segue i criteri di supporto di .NET Core più recenti per tutte le versioni.

[Visualizzare i criteri di supporto di .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

## <a name="microsoftdatasqlclient-release-cadence"></a>Cadenza delle versioni di Microsoft.Data.SqlClient

Le nuove versioni stabili (GA) vengono pubblicate ogni sei mesi con cadenza regolare a partire dalla versione 1.2, oltre a 2-3 versioni di anteprima nel frattempo. Le versioni con supporto a lungo termine (LTS) verranno scelte dagli stakeholder e dai responsabili della manutenzione in base a un numero ridotto di qualifiche e alle risposte dei clienti.

### <a name="actively-supported-releases"></a>Versioni attualmente supportate

| Versione | Data di rilascio ufficiale | Ultima versione patch | Data di rilascio patch | Livello di supporto  | Fine del supporto |
| -- | -- | -- | -- | -- | -- |
| 2.1 | 19 novembre 2020 | 2.1.2 | 3 marzo 2021 | LTS | 20 novembre 2023 |
| 1.1 | 20 novembre 2019 | 1.1.3 | 15 maggio 2020 | LTS | 21 novembre 2022 |

### <a name="out-of-support-releases"></a>Versioni non più supportate

| Versione | Data ultimo rilascio patch | Ultima versione patch | Supporto terminato |
| -- | -- | -- | -- |
| 2.0 | 16 giugno 2020 | 2.0.1 | 25 agosto 2020 |
| 1.0 | 26 settembre 2019 | 1.0.19269.1 | 20 febbraio 2020 |


## <a name="azure-key-vault-provider-release-cadence"></a>Cadenza della versione del provider Azure Key Vault

Le nuove versioni stabili (GA) per `Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider` vengono pubblicate su richiesta quando vengono aggiunte nuove funzionalità. Le versioni con supporto a lungo termine (LTS) verranno scelte dagli stakeholder e dai responsabili della manutenzione in base a un numero ridotto di qualifiche e alle risposte dei clienti.

### <a name="actively-supported-releases"></a>Versioni attualmente supportate

| Versione | Data di rilascio ufficiale | Ultima versione patch | Data di rilascio patch | Livello di supporto  | Fine del supporto |
| -- | -- | -- | -- | -- | -- |
| 2.x | 3 marzo 2021 | 2.0.0 | 3 marzo 2021 | LTS | 4 marzo 2024 |
| 1.x | 19 novembre 2019 | 1.2.0 | 01 dicembre 2020 | LTS | 21 novembre 2022 |


## <a name="long-term-support-lts-releases"></a>Versioni con supporto a lungo termine (LTS)

Le versioni LTS sono supportate per tre anni dopo la versione iniziale.

## <a name="current-releases"></a>Versioni correnti

Le versioni correnti sono supportate per tre mesi dopo una versione corrente o LTS successiva.


## <a name="sql-version-compatibility-with-microsoftdatasqlclient"></a>Compatibilità delle versioni di SQL con Microsoft.Data.SqlClient

|Versione database&nbsp;&#8594;<br />&#8595; Versione del driver|database SQL di Azure|Azure Synapse Analytics|Istanza gestita di SQL di Azure|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|
|---|---|---|---|---|---|---|---|---|
|2.1|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|2.0|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|1.1|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|1.0|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|

## <a name="supported-os-versions"></a>Versioni dei sistemi operativi supportate

### <a name="support-for-net-framework-applications"></a>Supporto per applicazioni .NET Framework

Microsoft.Data.SqlClient supporta tutti i sistemi operativi supportati da .NET Framework 4.6 e versioni successive.

[Requisiti di sistema di .NET Framework](/dotnet/framework/get-started/system-requirements).

### <a name="support-for-net-core-applications"></a>Supporto per le applicazioni .NET Core

Microsoft.Data.SqlClient supporta tutti i sistemi operativi supportati da .NET Core 2.1 e versioni successive.

[Criteri del ciclo di vita del sistema operativo supportati da .NET Core](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md).

> [!NOTE]
> La modalità indipendente dalla globalizzazione non è attualmente supportata.
