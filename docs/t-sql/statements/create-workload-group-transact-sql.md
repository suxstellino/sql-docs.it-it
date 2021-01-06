---
description: CREATE WORKLOAD GROUP (Transact-SQL)
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/27/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 1ad781bd554d7bcf506371ab71e3441e38f91f8c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643979"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server e Istanza gestita di SQL

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita di SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server e Istanza gestita di SQL

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Crea un gruppo di carico di lavoro. I gruppi di carico di lavoro sono contenitori per un set di richieste e su di essi si basa la configurazione della gestione del carico di lavoro in un sistema. I gruppi di carico di lavoro consentono di riservare risorse per l'isolamento del carico di lavoro, contengono risorse, definiscono le risorse per ogni richiesta e rispettano le regole di esecuzione. Al termine dell'esecuzione dell'istruzione, le impostazioni sono attive.

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

```syntaxsql
CREATE WORKLOAD GROUP group_name
 WITH
 (   MIN_PERCENTAGE_RESOURCE = value 
   , CAP_PERCENTAGE_RESOURCE = value 
   , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH } ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]

```

*group_name*</br>
Specifica il nome con cui viene identificato il gruppo del carico di lavoro. *group_name* è un sysname. Può essere composto da un massimo di 128 caratteri e deve essere univoco all'interno dell'istanza.

*MIN_PERCENTAGE_RESOURCE* = valore</br>
Specifica un'allocazione delle risorse minima garantita per questo gruppo di carico di lavoro non condiviso con altri gruppi di carico di lavoro. Il *valore* è un numero intero compreso tra 0 e 100. La somma di min_percentage_resource in tutti i gruppi di carico di lavoro non può essere superiore a 100. Il valore di min_percentage_resource non può essere maggiore di cap_percentage_resource. Sono consentiti valori minimi effettivi per ogni livello di servizio. Per altre informazioni dettagliate, vedere [Valori effettivi](#effective-values).

*CAP_PERCENTAGE_RESOURCE* = valore</br>
Specifica l'utilizzo massimo delle risorse per tutte le richieste in un gruppo di carico di lavoro. L'intervallo di numeri interi consentito per il valore è compreso tra 1 e 100. Il valore di cap_percentage_resource deve essere maggiore di min_percentage_resource. Il valore effettivo per cap_percentage_resource può essere ridotto se min_percentage_resource è impostato su un valore maggiore di zero in altri gruppi di carico di lavoro.

*REQUEST_MIN_RESOURCE_GRANT_PERCENT* = valore</br>
Imposta la quantità minima di risorse allocate per ogni richiesta. Il *valore* è un parametro obbligatorio con un intervallo decimale compreso tra 0,75 e 100,00. Il valore di request_min_resource_grant_percent deve essere un multiplo di 0,25, deve essere un fattore di min_percentage_resource ed essere minore di cap_percentage_resource. Sono consentiti valori minimi effettivi per ogni livello di servizio. Per altre informazioni dettagliate, vedere [Valori effettivi](#effective-values).

Ad esempio:

```sql
CREATE WORKLOAD GROUP wgSample 
WITH
  ( MIN_PERCENTAGE_RESOURCE = 26                -- integer value
    , REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed a minimum of 8 concurrency)
    , CAP_PERCENTAGE_RESOURCE = 100 )
```

Si considerino i valori usati per le classi di risorse come linee guida per request_min_resource_grant_percent.  La tabella seguente contiene le allocazioni delle risorse per la seconda generazione.

|Classe di risorse|Percentuale di risorse|
|---|---|
|Smallrc|3%|
|Mediumrc|10%|
|Largerc|22%|
|Xlargerc|70%|
|||

*REQUEST_MAX_RESOURCE_GRANT_PERCENT* = valore</br>         
Imposta la quantità massima di risorse allocate per ogni richiesta. Il *valore* è un parametro decimale facoltativo con un valore predefinito uguale a request_min_resource_grant_percent. Il *valore* deve essere maggiore o uguale a request_min_resource_grant_percent. Quando il valore di request_max_resource_grant_percent è maggiore di request_min_resource_grant_percent e sono disponibili risorse di sistema, vengono allocate risorse aggiuntive a una richiesta.

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }</br>        
Specifica l'importanza predefinita di una richiesta per il gruppo del carico di lavoro. L'importanza è compresa fra le seguenti, NORMAL è l'impostazione predefinita:

- LOW
- BELOW_NORMAL
- NORMAL (valore predefinito)
- ABOVE_NORMAL
- HIGH

L'importanza impostata nel gruppo di carico di lavoro è un'importanza predefinita per tutte le richieste nel gruppo di carico di lavoro. Un utente può anche impostare l'importanza a livello di classificatore, eseguendo in questo modo l'override dell'impostazione dell'importanza del gruppo di carico di lavoro, che consente di differenziare l'importanza permettendo alle richieste di accedere più rapidamente alle risorse non riservate. Quando la somma di min_percentage_resource tra gruppi di carico di lavoro è inferiore a 100, sono presenti risorse non riservate che vengono assegnate in base all'importanza.

*QUERY_EXECUTION_TIMEOUT_SEC* = valore</br>     
Specifica il tempo massimo di esecuzione di una query, in secondi, prima che venga annullata. *value* deve essere 0 o un valore intero positivo. L'impostazione predefinita per il valore è 0, indicante che la query non raggiunge mai il timeout. QUERY_EXECUTION_TIMEOUT_SEC inizia il conteggio una volta che la query è in esecuzione, non quando la query viene accodata.

## <a name="remarks"></a>Osservazioni

I gruppi di carico di lavoro corrispondenti alle classi di risorse vengono creati automaticamente per la compatibilità con le versioni precedenti. Non è possibile eliminare questi gruppi di carico di lavoro definiti dal sistema. È possibile creare altri 8 gruppi di carico di lavoro definiti dall'utente.

Se viene creato un gruppo di carico di lavoro con `min_percentage_resource` maggiore di zero, l'istruzione `CREATE WORKLOAD GROUP` verrà accodata finché non sono disponibili risorse sufficienti per creare il gruppo di carico di lavoro.

## <a name="effective-values"></a>Valori effettivi

I parametri `min_percentage_resource`, `cap_percentage_resource`, `request_min_resource_grant_percent` e `request_max_resource_grant_percent` hanno valori effettivi che vengono modificati nel contesto del livello di servizio corrente e della configurazione di altri gruppi del carico di lavoro.

Il parametro `request_min_resource_grant_percent` ha un valore effettivo poiché sono necessarie risorse minime per ogni query, a seconda del livello di servizio.  Ad esempio, al livello di servizio più basso, DW100c, è necessario un minimo di 25% di risorse per ogni richiesta.  Se il gruppo di carico di lavoro è configurato con il 3% `request_min_resource_grant_percent` e `request_max_resource_grant_percent`, i valori effettivi per entrambi i parametri si adattano al 25% all'avvio dell'istanza.  Se l'istanza viene aumentata a DW1000c, i valori configurati ed effettivi per entrambi i parametri sono al 3% poiché il 3% è il valore minimo supportato al livello di servizio.  Se l'istanza viene aumentata a un valore maggiore di DW1000c, i valori configurati ed effettivi per entrambi i parametri rimangono al 3%.  Per altri dettagli sui valori effettivi ai diversi livelli di servizio, vedere la tabella seguente.

|Livello di servizio|Valore effettivo mio per REQUEST_MIN_RESOURCE_GRANT_PERCENT|Numero massimo di query simultanee|
|---|---|---|
|DW100c|25%|4|
|DW200c|12,5%|8|
|DW300c|8%|12|
|DW400c|6,25%|16|
|DW500c|5%|20|
|DW1000c|3%|32|
|DW1500c|3%|32|
|DW2000c|2%|48|
|DW2500c|2%|48|
|DW3000c|1,5%|64|
|DW5000c|1,5%|64|
|DW6000c|0,75%|128|
|DW7500c|0,75%|128|
|DW10000c|0,75%|128|
|DW15000c|0,75%|128|
|DW30000c|0,75%|128|
||||

Il parametro `min_percentage_resource` deve essere maggiore o uguale al valore `request_min_resource_grant_percent` effettivo. Il valore di un gruppo di carico di lavoro con `min_percentage_resource` inferiore al valore `min_percentage_resource` effettivo ha il valore adattato a zero in fase di esecuzione. In questo caso, le risorse configurate per `min_percentage_resource` sono condivisibili in tutti i gruppi di carico di lavoro. Ad esempio, il gruppo di carico di lavoro `wgAdHoc` con `min_percentage_resource` del 10% in esecuzione in DW1000c avrà un valore `min_percentage_resource` effettivo del 10% (3% è il valore minimo supportato in DW1000c). `wgAdhoc` in DW100c avrà un valore min_percentage_resource effettivo dello 0%. Il 10% configurato per `wgAdhoc` verrà condiviso tra tutti i gruppi di carico di lavoro.

Il parametro `cap_percentage_resource` ha anche un valore effettivo. Se un gruppo di carico di lavoro `wgAdhoc` è configurato con una `cap_percentage_resource` del 100% e viene creato un altro gruppo di carico di lavoro `wgDashboards` con `min_percentage_resource` del 25%, il valore `cap_percentage_resource` effettivo per `wgAdhoc` diventa 75%.

Il modo più semplice per conoscere i valori di runtime per i gruppi di carico di lavoro consiste nell'eseguire una query sulla vista di sistema [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md).

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL DATABASE`

## <a name="see-also"></a>Vedere anche

- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Avvio rapido: Configurare l'isolamento del carico di lavoro tramite T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
