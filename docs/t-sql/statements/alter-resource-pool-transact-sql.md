---
description: ALTER RESOURCE POOL (Transact-SQL)
title: ALTER RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_RESOURCE_POOL_TSQL
- ALTER RESOURCE POOL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE POOL
ms.assetid: 9c1c4cfb-0e3b-4f01-bf57-3fce94c7d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0f33cb191bee4b8856983f946a768c5abdd38726
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204044"
---
# <a name="alter-resource-pool-transact-sql"></a>ALTER RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica una configurazione esistente del pool di risorse di Resource Governor in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER RESOURCE POOL { pool_name | "default" }  
[WITH  
    ( [ MIN_CPU_PERCENT = value ]  
    [ [ , ] MAX_CPU_PERCENT = value ]   
    [ [ , ] CAP_CPU_PERCENT = value ]   
    [ [ , ] AFFINITY {
                        SCHEDULER = AUTO 
                      | ( <scheduler_range_spec> ) 
                      | NUMANODE = ( <NUMA_node_range_spec> )
                      }]   
    [ [ , ] MIN_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]   
    [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
    [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
)]   
[;]  
  
<scheduler_range_spec> ::=  
{SCHED_ID | SCHED_ID TO SCHED_ID}[,...n]  
  
<NUMA_node_range_spec> ::=  
{NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID}[,...n]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 { *pool_name* |  **"default"** }  
 Nome di un pool di risorse esistente definito dall'utente o del pool di risorse predefinito creato all'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se usato con ALTER RESOURCE POOL, "default" deve essere racchiuso tra virgolette ("") o parentesi quadre ([]) per evitare conflitti con DEFAULT, che è una parola di sistema riservata. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  Per i gruppi del carico di lavoro e pool di risorse predefiniti vengono utilizzati sempre nomi scritti in lettere minuscole, ad esempio "default". Questo aspetto deve essere preso in considerazione per i server in cui vengono utilizzate regole di confronto con distinzione tra maiuscole e minuscole. In server con regole di confronto senza distinzione tra maiuscole e minuscole, ad esempio SQL_Latin1_General_CP1_CI_AS, le parole "default" e "Default" vengono considerate uguali.  
  
 MIN_CPU_PERCENT =*value*  
 Specifica la larghezza di banda media garantita della CPU concessa per tutte le richieste nel pool di risorse, in caso di contesa di CPU. *value* è un intero con impostazione predefinita 0. L'intervallo consentito per *value* è compreso tra 0 e 100.  
  
 MAX_CPU_PERCENT =*value*  
 Specifica la larghezza di banda media massima della CPU ricevuta da tutte le richieste nel pool di risorse in caso di contesa di CPU. *value* è un intero con impostazione predefinita 100. L'intervallo consentito per *value* è compreso tra 1 e 100.  
  
 CAP_CPU_PERCENT =*value*  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Specifica la capacità di CPU massima di destinazione per le richieste nel pool di risorse. *value* è un intero con impostazione predefinita 100. L'intervallo consentito per *value* è compreso tra 1 e 100.  
  
> [!NOTE]  
>  Data la natura statistica di governance della CPU, è possibile riscontrare occasionali picchi che superano il valore specificato in CAP_CPU_PERCENT.  
  
 AFFINITY {SCHEDULER = AUTO | (Scheduler_range_spec) | NUMANODE = (NUMA_node_range_spec)}  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Associa il pool di risorse a utilità di pianificazione specifiche. Il valore predefinito è AUTO.  
  
 AFFINITY SCHEDULER = (Scheduler_range_spec) esegue il mapping del pool di risorse alle pianificazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificate dagli ID specificati. Questi ID eseguono il mapping ai valori nella colonna scheduler_id di [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).  
  
 Se si utilizza AFFINITY NAMANODE = (NUMA_node_range_spec), viene creata un'affinità tra il pool di risorse e le utilità di pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che eseguono il mapping alle CPU fisiche corrispondenti al nodo o all'intervallo di nodi NUMA specificato. È possibile utilizzare la seguente query Transact-SQL per individuare il mapping tra la configurazione NUMA fisica e gli ID delle utilità di pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc 
   ON osn.node_id = sc.parent_node_id 
      AND sc.scheduler_id < 1048576;  
```  
  
 MIN_MEMORY_PERCENT =*value*  
 Specifica la quantità minima di memoria riservata al pool di risorse non condivisibile con altri pool di risorse. *value* è un intero con impostazione predefinita 0. L'intervallo consentito per *value* è compreso tra 0 e 100.  
  
 MAX_MEMORY_PERCENT =*value*  
 Specifica la memoria totale del server utilizzabile dalle richieste in questo pool di risorse. *value* è un intero con impostazione predefinita 100. L'intervallo consentito per *value* è compreso tra 1 e 100.  
  
 MIN_IOPS_PER_VOLUME =*value*  
 **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
 Specifica il numero minimo di operazioni di I/O al secondo per volume di disco da riservare per il pool di risorse. L'intervallo consentito per *value* è compreso tra 0 e 2^31-1 (2,147,483,647). Specificare 0 per indicare che non è impostata alcuna soglia minima per il pool.  
  
 MAX_IOPS_PER_VOLUME =*value*  
 **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
 Specifica il numero massimo di operazioni di I/O al secondo per volume di disco da riservare per il pool di risorse. L'intervallo consentito per *value* è compreso tra 0 e 2^31-1 (2,147,483,647). Specificare 0 per impostare una soglia illimitata per il pool. Il valore predefinito è 0.  
  
 Se il valore MAX_IOPS_PER_VOLUME per un pool è impostato su 0, il pool non è governato e può accettare tutti gli IOPS nel sistema anche se in altri pool è stato impostato il valore MIN_IOPS_PER_VOLUME. In questo caso, è consigliabile impostare il valore MAX_IOPS_PER_VOLUME per questo pool su un numero elevato, ad esempio il valore massimo 2^31-1, se si desidera che questo pool sia governato per l'I/O.  
  
## <a name="remarks"></a>Commenti  
 MAX_CPU_PERCENT e MAX_MEMORY_PERCENT devono essere maggiori o uguali rispettivamente a MIN_CPU_PERCENT e MIN_MEMORY_PERCENT.  
  
 MAX_CPU_PERCENT può usare la capacità di CPU superiore al valore di MAX_CPU_PERCENT se è disponibile. Sebbene si possano riscontrare occasionali picchi superiori al valore di CAP_CPU_PERCENT, i carichi di lavoro non devono superare CAP_CPU_PERCENT per periodi prolungati, anche nel caso in cui sia disponibile una capacità di CPU aggiuntiva.  
  
 La percentuale totale di CPU per ogni componente per il quale è impostata l'affinità (utilità di pianificazione o nodi NUMA) non deve superare il 100%.  
  
 Per l'esecuzione di istruzioni DDL, è consigliabile avere familiarità con gli stati di Resource Governor. Per altre informazioni, vedere [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
 Quando si modifica un'impostazione che influisce sul piano, la nuova impostazione diventerà effettiva nei piani memorizzati precedentemente nella cache solo dopo l'esecuzione di DBCC FREEPROCCACHE (*pool_name*), dove *pool_name* è il nome di un pool di risorse di Resource Governor.  
  
-   Se si modifica AFFINITY da utilità di pianificazione multiple a singola utilità di pianificazione, non è necessario eseguire DBCC FREEPROCCACHE poiché i piani paralleli possono essere eseguiti in modalità seriale. Potrebbe però non essere efficiente quanto un piano compilato come piano seriale.  
  
-   Se si modifica AFFINITY da utilità di pianificazione singola a utilità di pianificazione multiple, non è necessario eseguire DBCC FREEPROCCACHE. I piani seriali non possono però essere eseguiti in parallelo. È quindi necessario cancellare la cache corrispondente in modo che i nuovi piani possano essere potenzialmente compilati tramite il parallelismo.  
  
> [!CAUTION]  
>  La cancellazione dei piani memorizzati nella cache da un pool di risorse associato a più di un gruppo del carico di lavoro avrà effetto su tutti i gruppi di carico di lavoro che hanno il pool di risorse definito dall'utente identificato da *pool_name*.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL SERVER.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono mantenute tutte le impostazioni predefinite del pool di risorse per il pool `default`, ad eccezione del valore `MAX_CPU_PERCENT`, modificato in `25`.  
  
```sql  
ALTER RESOURCE POOL "default"  
WITH  
     ( MAX_CPU_PERCENT = 25);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 Nell'esempio seguente per `CAP_CPU_PERCENT` viene impostato un limite di utilizzo massimo pari all'80%, mentre per `AFFINITY SCHEDULER` vengono impostati un valore singolo pari a 8 e un intervallo compreso tra 12 e 16.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
```sql  
ALTER RESOURCE POOL Pool25  
WITH(   
     MIN_CPU_PERCENT = 5,  
     MAX_CPU_PERCENT = 10,       
     CAP_CPU_PERCENT = 80,  
     AFFINITY SCHEDULER = (8, 12 TO 16),   
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
);  
  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
