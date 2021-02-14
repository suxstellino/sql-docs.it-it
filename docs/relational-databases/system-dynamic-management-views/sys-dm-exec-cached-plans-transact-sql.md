---
description: sys.dm_exec_cached_plans (Transact-SQL)
title: sys.dm_exec_cached_plans (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_cached_plans
- dm_exec_cached_plans
- dm_exec_cached_plans_TSQL
- sys.dm_exec_cached_plans_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cached_plans dynamic management view
ms.assetid: 95b707d3-3a93-407f-8e88-4515d4f2039d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dc408790d3886bb516e2adda937faf565c31da79
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343017"
---
# <a name="sysdm_exec_cached_plans-transact-sql"></a>sys.dm_exec_cached_plans (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni piano di query memorizzato nella cache da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per velocizzare l'esecuzione di query. È possibile utilizzare questa vista a gestione dinamica per trovare i piani di query memorizzati nella cache, il testo delle query memorizzato nella cache, la quantità di memoria utilizzata dai piani memorizzati nella cache e il numero di riutilizzi dei piani nella cache.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata. Inoltre, i valori nelle colonne **memory_object_address** e **pool_id** sono filtrati; il valore della colonna è impostato su NULL.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_exec_cached_plans**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|bucketid|**int**|ID dell'hash bucket in cui la voce viene memorizzata nella cache. Il valore indica un intervallo compreso tra 0 e le dimensioni della tabella hash per il tipo di cache.<br /><br /> Per le cache di tipo Piani SQL e Piani per gli oggetti, le dimensioni massime della tabella hash sono 10007 nei sistemi a 32 bit e 40009 nei sistemi a 64 bit. Per la cache di tipo Alberi associati, le dimensioni massime della tabella hash sono 1009 nei sistemi a 32 bit e 4001 nei sistemi a 64 bit. Per la cache di tipo Stored procedure estese, le dimensioni massime della tabella cache sono 127 nei sistemi a 32 e a 64 bit.|  
|refcounts|**int**|Numero di oggetti della cache che fanno riferimento a questo oggetto della cache. **Refcounts** deve essere almeno 1 perché la cache contenga una voce.|  
|usecounts|**int**|Numeri di volte in cui l'oggetto della cache è stato ricercato. Non incrementato quando le query con parametri trovano un piano nella cache. Può essere incrementato più volte quando si utilizza il piano Showplan.|  
|size_in_bytes|**int**|Numero di byte utilizzati dall'oggetto della cache.|  
|memory_object_address|**varbinary (8)**|Indirizzo di memoria della voce memorizzata nella cache. È possibile utilizzare questo valore con [sys.dm_os_memory_objects](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md) per recuperare la suddivisione di memoria del piano memorizzato nella cache e con [sys.dm_os_memory_cache_entries](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql.md) per ottenere il costo della memorizzazione della voce nella cache.|  
|cacheobjtype|**nvarchar (34)**|Tipo di oggetto nella cache. I possibili valori sono i seguenti:<br /><br /> Compiled Plan<br /><br /> Compiled Plan Stub<br /><br /> Parse Tree<br /><br /> Extended Proc<br /><br /> CLR Compiled Func<br /><br /> CLR Compiled Proc|  
|objtype|**nvarchar (16)**|Tipo di oggetto. Di seguito sono riportati i valori possibili e le relative descrizioni.<br /><br /> Proc: stored procedure<br />Preparato: istruzione preparata<br />Adhoc: query ad hoc. Fa riferimento a [!INCLUDE[tsql](../../includes/tsql-md.md)] eventi inviati come eventi di linguaggio utilizzando **osql** o **SQLCMD** anziché come chiamate di procedure remote.<br />ReplProc: Replication-Filter-procedure<br />Trigger: trigger<br />Visualizzazione: visualizzazione<br />Impostazione predefinita: predefinita<br />UsrTab: tabella utente<br />SysTab: tabella di sistema<br />Check: vincolo CHECK<br />Regola: regola|  
|plan_handle|**varbinary(64)**|Identificatore del piano in memoria. Si tratta di un identificatore temporaneo, che rimane costante solo se il piano rimane nella cache. È possibile utilizzare questo valore con le funzioni a gestione dinamica seguenti:<br /><br /> [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)<br /><br /> [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)<br /><br /> [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)|  
|pool_id|**int**|ID del pool di risorse in base a cui viene rilevato l'utilizzo della memoria del piano.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
 <sup>1</sup>  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-batch-text-of-cached-entries-that-are-reused"></a>R. Restituzione del testo del batch per le voci memorizzate nella cache che vengono riutilizzate  
 Nell'esempio seguente viene restituito il testo SQL di tutte le voci memorizzate nella cache utilizzate più di una volta.  
  
```sql  
SELECT usecounts, cacheobjtype, objtype, text   
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle)   
WHERE usecounts > 1   
ORDER BY usecounts DESC;  
GO  
```  
  
### <a name="b-returning-query-plans-for-all-cached-triggers"></a>B. Restituzione dei piani di query per tutti i trigger memorizzati nella cache  
 Nell'esempio seguente vengono restituiti i piani di query di tutti i trigger memorizzati nella cache.  
  
```sql  
SELECT plan_handle, query_plan, objtype   
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_query_plan(plan_handle)   
WHERE objtype ='Trigger';  
GO  
```  
  
### <a name="c-returning-the-set-options-with-which-the-plan-was-compiled"></a>C. Restituzione delle opzioni SET con cui è stato compilato il piano  
 Nell'esempio seguente vengono restituite le opzioni SET con cui è stato compilato il piano. `sql_handle`Viene restituito anche l'oggetto per il piano. L'operatore PIVOT viene utilizzato per restituire gli `set_options` `sql_handle` attributi e come colonne anziché come righe. Per ulteriori informazioni sul valore restituito in `set_options` , vedere [sys.dm_exec_plan_attributes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).  
  
```sql  
SELECT plan_handle, pvt.set_options, pvt.sql_handle  
FROM (  
      SELECT plan_handle, epa.attribute, epa.value   
      FROM sys.dm_exec_cached_plans   
      OUTER APPLY sys.dm_exec_plan_attributes(plan_handle) AS epa  
      WHERE cacheobjtype = 'Compiled Plan'  
      ) AS ecpa   
PIVOT (MAX(ecpa.value) FOR ecpa.attribute IN ("set_options", "sql_handle")) AS pvt;  
GO  
```  
  
### <a name="d-returning-the-memory-breakdown-of-all-cached-compiled-plans"></a>D. Restituzione della suddivisione di memoria per tutti i piani compilati memorizzati nella cache  
 Nell'esempio seguente viene restituita una suddivisione della memoria utilizzata da tutti i piani compilati nella cache.  
  
```sql  
SELECT plan_handle, ecp.memory_object_address AS CompiledPlan_MemoryObject,   
    omo.memory_object_address, type, page_size_in_bytes   
FROM sys.dm_exec_cached_plans AS ecp   
JOIN sys.dm_os_memory_objects AS omo   
    ON ecp.memory_object_address = omo.memory_object_address   
    OR ecp.memory_object_address = omo.parent_address  
WHERE cacheobjtype = 'Compiled Plan';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_plan_attributes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_os_memory_objects &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md)   
 [sys.dm_os_memory_cache_entries &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  


