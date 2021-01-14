---
title: Gestire le risorse con Resource Governor
description: Informazioni su come usare Resource Governor per gestire l'allocazione delle risorse di CPU, I/O fisico e memoria per carichi di lavoro Python ed R in Machine Learning Services per SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3e71b33eb08fd386232e992e9b47da7b5057aa32
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170723"
---
# <a name="manage-python-and-r-workloads-with-resource-governor-in-sql-server-machine-learning-services"></a>Gestire carichi di lavoro Python ed R con Resource Governor in Machine Learning Services per SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Informazioni su come usare [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) per gestire l'allocazione delle risorse di CPU, I/O fisico e memoria per carichi di lavoro Python ed R in Machine Learning Services per SQL Server.

Gli algoritmi di apprendimento automatico in Python ed R sono a elevato utilizzo di calcolo. A seconda delle priorità del carico di lavoro, può essere necessario aumentare o ridurre le risorse disponibili per Machine Learning Services.

Per altre informazioni, vedere [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

> [!NOTE] 
> Resource Governor è una funzionalità della versione Enterprise Edition.

## <a name="default-allocations"></a>Allocazioni predefinite

Per impostazione predefinita, i runtime degli script esterni per l'apprendimento automatico sono limitati a non oltre il 20% della memoria totale del computer. A seconda del sistema in uso, è possibile che questo limite non sia adeguato per attività di apprendimento automatico importanti, ad esempio per il training di un modello o per la stima di molte righe di dati. 

## <a name="manage-resources-with-resource-governor"></a>Gestire le risorse con Resource Governor
 
Per impostazione predefinita, i processi esterni usano fino al 20% della memoria totale dell'host nel server locale. È possibile modificare il pool di risorse predefinito per apportare modifiche a livello di server, con i processi R e Python che usano qualsiasi capacità resa disponibile ai processi esterni.

Se necessario, è possibile creare **pool di risorse esterne** personalizzati, con classificatori e gruppi di carico di lavoro associati, per determinare l'allocazione delle risorse per richieste provenienti da programmi o host specifici o altri criteri forniti. Un pool di risorse esterne è un nuovo tipo di pool di risorse introdotto in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] per supportare la gestione dei processi R e Python esterni al motore di database.

1. [Abilitare la governance delle risorse](../../relational-databases/resource-governor/enable-resource-governor.md) (disattivata per impostazione predefinita).

2. Eseguire [CREATE EXTERNAL RESOURCE POOL](../../t-sql/statements/create-external-resource-pool-transact-sql.md) per creare e configurare il pool di risorse, quindi [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md) per implementarlo.

3. Creare un gruppo di carico di lavoro per allocazioni granulari, ad esempio tra il training e l'assegnazione dei punteggi.

4. Creare un classificatore per intercettare le chiamate per l'elaborazione esterna.

5. Eseguire query e procedure usando gli oggetti creati.

Per istruzioni dettagliate, vedere [Creare un pool di risorse per Machine Learning Services per SQL Server](create-external-resource-pool.md).

Per un'introduzione ai concetti generali e alla terminologia, vedere [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md).

## <a name="processes-under-resource-governance"></a>Processi inclusi nella governance delle risorse
  
 È possibile usare un *pool di risorse esterne* per gestire le risorse usate dagli eseguibili seguenti in un'istanza del motore di database:

+ Rterm.exe per la chiamata in locale da SQL Server o per la chiamata in remoto con SQL Server come contesto di calcolo remoto
+ Python.exe per la chiamata in locale da SQL Server o per la chiamata in remoto con SQL Server come contesto di calcolo remoto
+ BxlServer.exe e processi satellite
+ Processi satellite avviati da Launchpad, ad esempio PythonLauncher.dll
  
> [!NOTE]
> La gestione diretta del servizio Launchpad tramite Resource Governor non è supportata. Launchpad è un servizio attendibile che può ospitare solo utilità di avvio fornite da Microsoft. Le utilità di avvio attendibili vengono configurate per evitare l'utilizzo eccessivo di risorse.
  
## <a name="next-steps"></a>Passaggi successivi

+ [Creare un pool di risorse per Machine Learning](create-external-resource-pool.md)
+ [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)