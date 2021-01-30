---
description: CREATE RESOURCE POOL (Transact-SQL)
title: CREATE RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE RESOURCE POOL
- RESOURCE POOL
- CREATE_RESOURCE_POOL_TSQL
- RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE RESOURCE POOL
ms.assetid: 82712505-c6f9-4a65-a469-f029b5a2d6cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a55efe97292550be8d0150514c56c3eb478f39ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188512"
---
# <a name="create-resource-pool-transact-sql"></a>CREATE RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Crea un pool di risorse di Resource Governor in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un pool di risorse rappresenta un subset delle risorse fisiche (memoria, CPU e IO) di un'istanza del motore di database. Resource Governor consente a un amministratore di database di distribuire le risorse del server tra pool di risorse, fino a un massimo di 64 pool. Resource Governor non è disponibile in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
```syntaxsql
CREATE RESOURCE POOL pool_name  
[ WITH  
    (  
        [ MIN_CPU_PERCENT = value ]  
        [ [ , ] MAX_CPU_PERCENT = value ]   
        [ [ , ] CAP_CPU_PERCENT = value ]   
        [ [ , ] AFFINITY {SCHEDULER =  
                  AUTO 
                | ( <scheduler_range_spec> )   
                | NUMANODE = ( <NUMA_node_range_spec> )
                } ]   
        [ [ , ] MIN_MEMORY_PERCENT = value ]  
        [ [ , ] MAX_MEMORY_PERCENT = value ]  
        [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
        [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
    )   
]  
[;]  
  
<scheduler_range_spec> ::=  
{ SCHED_ID | SCHED_ID TO SCHED_ID }[,...n]  
  
<NUMA_node_range_spec> ::=  
{ NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID }[,...n]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*pool_name*  
Nome definito dall'utente per il pool di risorse. *pool_name* è un valore alfanumerico, può essere composto da un massimo di 128 caratteri, deve essere univoco all'interno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e deve essere conforme alle regole relative agli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
MIN_CPU_PERCENT =*value*  
Specifica la larghezza di banda media garantita della CPU concessa per tutte le richieste nel pool di risorse, in caso di contesa di CPU. *value* è un intero con impostazione predefinita 0. L'intervallo consentito per *value* è compreso tra 0 e 100.  
  
MAX_CPU_PERCENT =*value*  
Specifica la larghezza di banda media massima della CPU concessa per tutte le richieste nel pool di risorse in caso di contesa di CPU. *value* è un intero con impostazione predefinita 100. L'intervallo consentito per *value* è compreso tra 1 e 100.  
  
CAP_CPU_PERCENT =*value*   
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
Specifica il limite di utilizzo massimo della larghezza di banda della CPU concesso per tutte le richieste nel pool di risorse. Limita il livello massimo della larghezza di banda della CPU al valore specificato. *value* è un intero con impostazione predefinita 100. L'intervallo consentito per *value* è compreso tra 1 e 100.  
  
AFFINITY {SCHEDULER = AUTO | ( \<scheduler_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}      
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
Associa il pool di risorse a utilità di pianificazione specifiche. Il valore predefinito è AUTO.  
  
AFFINITY SCHEDULER = **(** \<scheduler_range_spec> **)** esegue il mapping del pool di risorse alle pianificazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificate dagli ID specificati. Questi ID eseguono il mapping ai valori nella colonna scheduler_id di [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 
  
Se si usa AFFINITY NUMANODE = **(** \<NUMA_node_range_spec> **)** , viene creata un'affinità tra il pool di risorse e le utilità di pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che eseguono il mapping alle CPU fisiche corrispondenti al nodo o all'intervallo di nodi NUMA specificato. È possibile utilizzare la seguente query [!INCLUDE[tsql](../../includes/tsql-md.md)] per individuare il mapping tra la configurazione NUMA fisica e gli ID delle utilità di pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc   
    ON osn.node_id = sc.parent_node_id   
    AND sc.scheduler_id < 1048576;  
```  
  
MIN_MEMORY_PERCENT =*value*    
Specifica la quantità minima di memoria riservata al pool di risorse non condivisibile con altri pool di risorse. *value* è di tipo Integer e il valore predefinito è 0. L'intervallo consentito per *value* è compreso tra 0 e 100.  
  
MAX_MEMORY_PERCENT =*value*    
Specifica la memoria totale del server utilizzabile dalle richieste in questo pool di risorse. *value* è un intero con impostazione predefinita 100. L'intervallo consentito per *value* è compreso tra 1 e 100.  
  
MIN_IOPS_PER_VOLUME =*value*    
**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
Specifica il numero minimo di operazioni di I/O al secondo per volume di disco da riservare per il pool di risorse. L'intervallo consentito per *value* è compreso tra 0 e 2^31-1 (2,147,483,647). Specificare 0 per indicare che non è impostata alcuna soglia minima per il pool. Il valore predefinito è 0.  
  
MAX_IOPS_PER_VOLUME =*value*    
**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
Specifica il numero massimo di operazioni di I/O al secondo per volume di disco da riservare per il pool di risorse. L'intervallo consentito per *value* è compreso tra 0 e 2^31-1 (2,147,483,647). Specificare 0 per impostare una soglia illimitata per il pool. Il valore predefinito è 0.  
  
Se il valore `MAX_IOPS_PER_VOLUME` per un pool è impostato su 0, il pool non è governato e può accettare tutti gli IOPS nel sistema anche se in altri pool è impostato il valore MIN_IOPS_PER_VOLUME. In questo caso, è consigliabile impostare il valore `MAX_IOPS_PER_VOLUME` per questo pool su un numero elevato, ad esempio il valore massimo 2^31-1, se si vuole che questo pool sia governato per l'I/O.  
  
## <a name="remarks"></a>Osservazioni  
`MIN_IOPS_PER_VOLUME` e `MAX_IOPS_PER_VOLUME` specificano il numero minimo e massimo di letture o scritture al secondo. Queste letture o scritture possono essere di qualsiasi dimensione e non indicano una velocità effettiva minima e massima.  
  
I valori per `MAX_CPU_PERCENT` e `MAX_MEMORY_PERCENT` devono essere maggiori o uguali ai valori per `MIN_CPU_PERCENT` e `MIN_MEMORY_PERCENT`, rispettivamente.  
  
`CAP_CPU_PERCENT` è diverso da `MAX_CPU_PERCENT`, in quanto i carichi di lavoro associati al pool possono usare la capacità della CPU oltre il valore di `MAX_CPU_PERCENT` se è disponibile, ma non oltre il valore di `CAP_CPU_PERCENT`.  
  
La percentuale totale di CPU per ogni componente per il quale è impostata l'affinità (utilità di pianificazione o nodi NUMA) non deve superare il 100%.  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'autorizzazione `CONTROL SERVER`.  
  
## <a name="examples"></a>Esempi  
### <a name="1-shows-how-to-create-a-resource-pool"></a>1. Mostra come creare un pool di risorse

In questo esempio viene creato un pool di risorse denominato "bigPool". Il pool utilizza le impostazioni predefinite di Resource Governor.  
  
```sql  
CREATE RESOURCE POOL bigPool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="2-set-the-cap_cpu_percent-to-a-hard-cap-and-set-affinity-scheduler"></a>2. Impostare CAP_CPU_PERCENT su un limite rigido e impostare AFFINITY SCHEDULER

Impostare CAP_CPU_PERCENT su un limite rigido del 30% e impostare AFFINITY SCHEDULER su un intervallo da 0 a 63 e da 128 a 191. 
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
     MIN_CPU_PERCENT = 10,  
     MAX_CPU_PERCENT = 20,  
     CAP_CPU_PERCENT = 30,  
     AFFINITY SCHEDULER = (0 TO 63, 128 TO 191),  
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
      );  
```  
  
### <a name="3-set-min_iops_per_volume-and-max_iops_per_volume"></a>3. Impostare MIN_IOPS_PER_VOLUME e MAX_IOPS_PER_VOLUME   

Impostare MIN_IOPS_PER_VOLUME su 20 e MAX_IOPS_PER_VOLUME su 100. Questi valori determinano le operazioni di lettura e scrittura di I/O fisico disponibili per il pool di risorse.  
  
**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
    MIN_IOPS_PER_VOLUME = 20,  
    MAX_IOPS_PER_VOLUME = 100  
      );  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)     
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)     
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)     
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)     
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)     
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)     
 [Pool di risorse di Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)     
 [Creare un pool di risorse](../../relational-databases/resource-governor/create-a-resource-pool.md)    
  
