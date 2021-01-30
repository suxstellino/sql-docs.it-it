---
description: ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
title: ALTER EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- ALTER_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL RESOURCE POOL statement
ms.assetid: 634c327d-971b-49ba-b8a2-e243a04040db
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 13cceefecf9d5c67cfde6cd10ec1b86fc564ebf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204216"
---
# <a name="alter-external-resource-pool-transact-sql"></a>ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Modifica un pool di risorse esterne di Resource Governor in cui sono specificate le risorse che possono essere usate da processi esterni. 

::: moniker range="=sql-server-2016"
Per [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] il pool esterno gestisce `rterm.exe`, `BxlServer.exe` e altri processi derivati.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Per [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)], il pool esterno gestisce `rterm.exe`, `python.exe`, `BxlServer.exe` e altri processi derivati.
::: moniker-end

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Sintassi
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```syntaxsql
ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] MAX_MEMORY_PERCENT = value ]
    [ [ , ] MAX_PROCESSES = value ]
    )
]
[ ; ]
  
<CPU_range_spec> ::=
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]
```  
::: moniker-end
::: moniker range="=sql-server-2016||=sql-server-2017"
 ```syntaxsql

ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] AFFINITY CPU =
            {
                AUTO
              | ( <cpu_range_spec> )
              | NUMANODE = (( <NUMA_node_id> )
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]
    [ [ , ] MAX_PROCESSES = value ]
    )
]
[ ; ]
  
<CPU_range_spec> ::=
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]
```  
::: moniker-end 

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

{ *pool_name* | "default" }  
Nome di un pool di risorse esterne esistente definito dall'utente o del pool di risorse esterne predefinito creato all'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Se usato con `ALTER EXTERNAL RESOURCE POOL`, "default" deve essere racchiuso tra virgolette ("") o parentesi quadre ([]) per evitare conflitti con `DEFAULT`, che è una parola riservata al sistema.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =*value*  
Specifica la larghezza di banda media massima della CPU ricevibile da tutte le richieste nel pool di risorse esterne in caso di contesa di CPU. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

MAX_MEMORY_PERCENT =*value*  
Specifica la memoria totale del server usabile dalle richieste in questo pool di risorse esterne. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

MAX_PROCESSES =*value*  
Specifica il numero massimo di processi consentiti per il pool di risorse esterne. Specificare 0 per impostare una soglia illimitata per il pool, che di conseguenza sarà limitato solo dalle risorse di computer.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =*value*  
Specifica la larghezza di banda media massima della CPU ricevibile da tutte le richieste nel pool di risorse esterne in caso di contesa di CPU. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

AFFINITY {CPU = AUTO | ( \<CPU_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}  
Associa il pool di risorse esterne a una CPU specifica.

AFFINITY CPU = **(** \<CPU_range_spec> **)** esegue il mapping del pool di risorse esterne alle CPU [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificate dai valori CPU_ID specificati. Quando si usa AFFINITY NUMANODE = **(** \<NUMA_node_range_spec> **)** , viene creata un'affinità tra il pool di risorse e le CPU fisiche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrispondenti al nodo o all'intervallo di nodi NUMA specificato.

MAX_MEMORY_PERCENT =*value*  
Specifica la memoria totale del server usabile dalle richieste in questo pool di risorse esterne. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

MAX_PROCESSES =*value*  
Specifica il numero massimo di processi consentiti per il pool di risorse esterne. Specificare 0 per impostare una soglia illimitata per il pool, che di conseguenza sarà limitato solo dalle risorse di computer.
::: moniker-end
## <a name="remarks"></a>Osservazioni

[!INCLUDE[ssDE](../../includes/ssde-md.md)] implementa il pool di risorse quando si esegue l'istruzione [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md).

Per informazioni generali sui pool di risorse, vedere [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) e [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).  

Per informazioni specifiche per la gestione di pool di risorse esterne usati per il machine learning, vedere [Governance delle risorse per Machine Learning in SQL Server](../../machine-learning/administration/resource-governor.md)...
## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL SERVER`.

## <a name="examples"></a>Esempi

L'istruzione seguente modifica un pool esterno, limitando l'uso della CPU al 50% e la memoria massima al 25% della memoria disponibile nel computer.
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>Vedere anche

+ [Governance delle risorse per Machine Learning in SQL Server](../../machine-learning/administration/resource-governor.md)
+ [Opzione di configurazione del server external scripts enabled](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md) 
