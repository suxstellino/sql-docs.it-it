---
description: CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
title: CREATE EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL RESOURCE POOL
- CREATE EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE POOL
- EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE
- EXTERNAL_RESOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL RESOURCE POOL statement
ms.assetid: 8cc798ad-c395-461c-b7ff-8c561c098808
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 7f57b0e829c240813d6f1a453d764e64bce6b279
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171603"
---
# <a name="create-external-resource-pool-transact-sql"></a>CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Crea un pool esterno per definire le risorse per i processi esterni. Un pool di risorse rappresenta un subset delle risorse fisiche (memoria e CPU) di un'istanza del motore di database. Resource Governor consente di distribuire le risorse del server tra pool di risorse, fino a un massimo di 64 pool.

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
CREATE EXTERNAL RESOURCE POOL pool_name  
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
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] AFFINITY CPU =    
            {  
                AUTO   
              | ( <cpu_range_spec> )   
              | NUMANODE = ( <NUMA_node_id> )   
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

*pool_name*  
Nome definito dall'utente per il pool di risorse esterne. *pool_name* è un valore alfanumerico e può contenere fino a 128 caratteri. Questo argomento deve essere univoco all'interno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e deve essere conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =*value*  
Larghezza di banda media massima della CPU che il pool di risorse esterne può ricevere in caso di contesa di CPU. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.


MAX_MEMORY_PERCENT =*value*  
Specifica la memoria totale del server usabile dalle richieste in questo pool di risorse esterne. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

MAX_PROCESSES =*value*  
Numero massimo di processi consentiti per il pool di risorse esterne. 0 = soglia illimitata per il pool, che di conseguenza sarà limitato solo dalle risorse di computer.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =*value*  
Larghezza di banda media massima della CPU che il pool di risorse esterne può ricevere in caso di contesa di CPU. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

AFFINITY {CPU = AUTO | ( <CPU_range_spec>) | NUMANODE = (\<NUMA_node_range_spec>)} Associa il pool di risorse esterne a CPU specifiche.

AFFINITY CPU = **(** <CPU_range_spec> **)** esegue il mapping del pool di risorse esterne alle CPU [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificate dai valori CPU_ID specificati.

Quando si usa AFFINITY NUMANODE = **(\<NUMA_node_range_spec> **)** , viene creata un'affinità tra il pool di risorse e le CPU fisiche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrispondenti al nodo o all'intervallo di nodi NUMA specificato. 

MAX_MEMORY_PERCENT =*value*  
Specifica la memoria totale del server usabile dalle richieste in questo pool di risorse esterne. *value* è un valore intero. L'intervallo consentito per *value* è compreso tra 1 e 100.

MAX_PROCESSES =*value*  
Numero massimo di processi consentiti per il pool di risorse esterne. 0 = soglia illimitata per il pool, che di conseguenza sarà limitato solo dalle risorse di computer.
::: moniker-end

## <a name="remarks"></a>Osservazioni

[!INCLUDE[ssDE](../../includes/ssde-md.md)] implementa il pool di risorse quando si esegue l'istruzione [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md).

Per informazioni generali sui pool di risorse, vedere [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) e [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).

Per informazioni specifiche per la gestione dei pool di risorse esterne usati per il Machine Learning, vedere [Governance delle risorse per Machine Learning in SQL Server](../../machine-learning/administration/resource-governor.md). 

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL SERVER`.

## <a name="examples"></a>Esempi

Il pool esterno ha limitato l'utilizzo della CPU al 75%. La memoria massima corrisponde al 30% della memoria disponibile nel computer.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>Vedere anche

+ [Opzione di configurazione del server external scripts enabled](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)
+ [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)
+ [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)
