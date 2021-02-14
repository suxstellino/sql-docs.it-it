---
title: sys.dm_db_tuning_recommendations (Transact-SQL) | Microsoft Docs
description: Informazioni su come individuare potenziali problemi di prestazioni e correzioni consigliate in SQL Server e nel database SQL di Azure
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_tuning_recommendations
- dm_db_tuning_recommendations
- sys.dm_db_tuning_recommendations_TSQL
- dm_db_tuning_recommendations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database tuning recommendations feature [SQL Server], sys.dm_db_tuning_recommendations dynamic management view
- sys.dm_db_tuning_recommendations dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 401636f25e34c3c76faed1cc73fc54b9e2b911bf
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489345"
---
# <a name="sysdm_db_tuning_recommendations-transact-sql"></a>\_indicazioni sull'ottimizzazione del database sys.dm \_ \_ (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Restituisce informazioni dettagliate sulle indicazioni di ottimizzazione.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.

| **Nome colonna** | **Tipo di dati** | **Descrizione** |
| --- | --- | --- |
| **nome** | **nvarchar(4000)** | Nome univoco dell'indicazione. |
| **type** | **nvarchar(4000)** | Nome dell'opzione di ottimizzazione automatica che ha prodotto la raccomandazione, ad esempio `FORCE_LAST_GOOD_PLAN` |
| **reason** | **nvarchar(4000)** | Motivo per cui è stata fornita questa raccomandazione. |
| **valido \_ da** | **datetime2** | La prima volta che questa raccomandazione è stata generata. |
| **ultimo \_ aggiornamento** | **datetime2** | Ultima volta in cui è stata generata questa raccomandazione. |
| **state** | **nvarchar(4000)** | Documento JSON che descrive lo stato dell'indicazione. Sono disponibili i campi seguenti:<br />-   `currentValue` -stato corrente dell'indicazione.<br />-   `reason` -costante che descrive il motivo per cui la raccomandazione si trova nello stato corrente.|
| **\_azione eseguibile \_** | **bit** | 1 = la raccomandazione può essere eseguita sul database tramite [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script.<br />0 = non è possibile eseguire la raccomandazione sul database (ad esempio: solo informazioni o raccomandazione ripristinata) |
| **\_azione ripristinabile \_** | **bit** | 1 = la raccomandazione può essere monitorata e ripristinata automaticamente dal motore di database.<br />0 = la raccomandazione non può essere monitorata e ripristinata automaticamente. La maggior parte delle &quot; azioni eseguibili &quot; sarà &quot; ripristinabile &quot; . |
| **\_ora di \_ inizio dell'azione di esecuzione \_** | **datetime2** | Data di applicazione della raccomandazione. |
| **\_durata dell'azione di esecuzione \_** | **time** | Durata dell'azione di esecuzione. |
| **Esegui \_ azione \_ avviata \_ da** | **nvarchar(4000)** | `User` = Piano forzato manualmente dall'utente nell'indicazione. <br /> `System` = Raccomandazione applicata automaticamente al sistema. |
| **tempo di esecuzione \_ avviato dall'azione \_ \_** | **datetime2** | Data di applicazione della raccomandazione. |
| **Annulla \_ \_ ora di inizio azione \_** | **datetime2** | Data di ripristino del suggerimento. |
| **Ripristina \_ \_ durata azione** | **time** | Durata dell'azione di ripristino. |
| **Annulla \_ azione \_ avviata \_ da** | **nvarchar(4000)** | `User` = Il piano consigliato non è stato applicato manualmente dall'utente. <br /> `System` = La raccomandazione del sistema è stata ripristinata automaticamente. |
| **Ripristina \_ \_ ora di avvio \_ azione** | **datetime2** | Data di ripristino del suggerimento. |
| **Punteggio** | **int** | Valore stimato/effetto per questa raccomandazione sulla scala 0-100 (maggiore è il migliore) |
| **Dettagli** | **nvarchar(max)** | Documento JSON che contiene ulteriori dettagli sull'indicazione. Sono disponibili i campi seguenti:<br /><br />`planForceDetails`<br />-    `queryId` : \_ ID query della query regressione.<br />-    `regressedPlanId` -plan_id del piano regressione.<br />-   `regressedPlanExecutionCount` : Numero di esecuzioni della query con piano regressione prima che venga rilevata la regressione.<br />-    `regressedPlanAbortedCount` -Numero di errori rilevati durante l'esecuzione del piano regressione.<br />-    `regressedPlanCpuTimeAverage` -Tempo medio CPU (in microsecondi) utilizzato dalla query regressione prima che venga rilevata la regressione.<br />-    `regressedPlanCpuTimeStddev` -Deviazione standard del tempo di CPU utilizzato dalla query regressione prima che venga rilevata la regressione.<br />-    `recommendedPlanId` -plan_id del piano da forzare.<br />-   `recommendedPlanExecutionCount`: Numero di esecuzioni della query con il piano da forzare prima che venga rilevata la regressione.<br />-    `recommendedPlanAbortedCount` : Numero di errori rilevati durante l'esecuzione del piano da forzare.<br />-    `recommendedPlanCpuTimeAverage` -Tempo medio CPU (in microsecondi) utilizzato dalla query eseguita con il piano che deve essere forzato (calcolato prima che la regressione venga rilevata).<br />-    `recommendedPlanCpuTimeStddev` Deviazione standard del tempo di CPU utilizzato dalla query regressione prima che venga rilevata la regressione.<br /><br />`implementationDetails`<br />-  `method` : Il metodo che deve essere usato per correggere la regressione. Il valore è sempre `TSql` .<br />-    `script` - [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script da eseguire per forzare il piano consigliato. |
  
## <a name="remarks"></a>Commenti  
 Le informazioni restituite da `sys.dm_db_tuning_recommendations` vengono aggiornate quando il motore di database identifica la possibile regressione delle prestazioni delle query e non è permanente. Le raccomandazioni vengono mantenute solo fino a quando non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene riavviato. Gli amministratori di database devono eseguire periodicamente copie di backup dell'indicazione di ottimizzazione se vogliono mantenerla dopo il riciclo del server. 

 `currentValue` il campo della `state` colonna può contenere i valori seguenti:
 
 | Stato | Descrizione |
 |--------|-------------|
 | `Active` | Il suggerimento è attivo e non è ancora applicato. L'utente può eseguire lo script di raccomandazione ed eseguirlo manualmente. |
 | `Verifying` | La raccomandazione viene applicata da [!INCLUDE[ssde_md](../../includes/ssde_md.md)] e il processo di verifica interno Confronta le prestazioni del piano forzato con il piano regressione. |
 | `Success` | La raccomandazione è stata applicata correttamente. |
 | `Reverted` | La raccomandazione viene ripristinata perché non sono presenti miglioramenti significativi delle prestazioni. |
 | `Expired` | Il suggerimento è scaduto e non può più essere applicato. |

Il documento JSON nella `state` colonna contiene il motivo per cui viene descritto il motivo per cui la raccomandazione si trova nello stato corrente. I valori nel campo Reason potrebbero essere: 

| Motivo | Descrizione |
|--------|-------------|
| `SchemaChanged` | La raccomandazione è scaduta perché lo schema di una tabella a cui si fa riferimento è stato modificato. Viene creata una nuova raccomandazione se viene rilevata una nuova regressione del piano di query nel nuovo schema. |
| `StatisticsChanged`| Il suggerimento è scaduto a causa della modifica delle statistiche in una tabella a cui si fa riferimento. Se viene rilevata una nuova regressione del piano di query basata su nuove statistiche, verrà creata una nuova raccomandazione. |
| `ForcingFailed` | Non è possibile forzare il piano consigliato su una query. Trovare `last_force_failure_reason` nella vista [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md) per individuare il motivo dell'errore. |
| `AutomaticTuningOptionDisabled` | `FORCE_LAST_GOOD_PLAN` l'opzione è disabilitata dall'utente durante il processo di verifica. Abilitare l' `FORCE_LAST_GOOD_PLAN` opzione utilizzando [ALTER DATABASE SET AUTOMATIC_TUNING &#40;istruzione Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) oppure forzare manualmente il piano utilizzando lo script nella `[details]` colonna. |
| `UnsupportedStatementType` | Non è possibile forzare il piano sulla query. Esempi di query non supportate sono cursori e `INSERT BULK` istruzioni. |
| `LastGoodPlanForced` | La raccomandazione è stata applicata correttamente. |
| `AutomaticTuningOptionNotEnabled`| [!INCLUDE[ssde_md](../../includes/ssde_md.md)] è stata identificata la potenziale regressione delle prestazioni, ma l' `FORCE_LAST_GOOD_PLAN` opzione non è abilitata. vedere [ALTER DATABASE SET AUTOMATIC_TUNING &#40;&#41;Transact-SQL ](../../t-sql/statements/alter-database-transact-sql-set-options.md). Applicare la raccomandazione manualmente o abilitare l' `FORCE_LAST_GOOD_PLAN` opzione. |
| `VerificationAborted`| Il processo di verifica è stato interrotto a causa del riavvio o della pulizia del Query Store. |
| `VerificationForcedQueryRecompile`| La query viene ricompilata perché non esiste un miglioramento significativo delle prestazioni. |
| `PlanForcedByUser`| L'utente ha forzato manualmente il piano utilizzando [sp_query_store_force_plan &#40;procedura&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) . Il motore di database non applica la raccomandazione se l'utente ha deciso in modo esplicito di forzare un piano. |
| `PlanUnforcedByUser` | L'utente non ha forzato manualmente il piano utilizzando [sp_query_store_unforce_plan &#40;procedura&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md) . Poiché l'utente ha ripristinato in modo esplicito il piano consigliato, il motore di database continuerà a utilizzare il piano corrente e genererà una nuova raccomandazione se si verifica una regressione del piano in futuro. |
| `UserForcedDifferentPlan` | L'utente ha forzato manualmente un piano diverso utilizzando [sp_query_store_force_plan &#40;procedura&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) . Il motore di database non applica la raccomandazione se l'utente ha deciso in modo esplicito di forzare un piano. |
| `TempTableChanged` | Una tabella temporanea utilizzata nel piano è stata modificata. |

 La statistica della colonna dettagli non Mostra le statistiche del piano di runtime (ad esempio, il tempo di CPU corrente). I dettagli della raccomandazione vengono presi al momento del rilevamento della regressione e descrivono il motivo per cui è stata [!INCLUDE[ssde_md](../../includes/ssde_md.md)] identificata la regressione delle prestazioni. Utilizzare `regressedPlanId` e `recommendedPlanId` per eseguire query [query Store viste del catalogo](../../relational-databases/performance/how-query-store-collects-data.md) per individuare le statistiche esatte del piano di Runtime.

## <a name="examples-of-using-tuning-recommendations-information"></a>Esempi di utilizzo delle informazioni sulle indicazioni di ottimizzazione  

### <a name="example-1"></a>Esempio 1
Di seguito è riportato lo [!INCLUDE[tsql](../../includes/tsql-md.md)] script generato che impone un piano valido per una determinata query:  
 
```sql
SELECT name, reason, score,
    JSON_VALUE(details, '$.implementationDetails.script') AS script,
    details.* 
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON(details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressed_plan_id int '$.regressedPlanId',
            last_good_plan_id int '$.recommendedPlanId') AS details
WHERE JSON_VALUE(state, '$.currentValue') = 'Active';
```
### <a name="example-2"></a>Esempio 2
Di seguito è riportato lo [!INCLUDE[tsql](../../includes/tsql-md.md)] script generato che impone un piano valido per una query specifica e informazioni aggiuntive sul guadagno stimato:

```sql
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  *(regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

### <a name="example-3"></a>Esempio 3
Di seguito è riportato lo [!INCLUDE[tsql](../../includes/tsql-md.md)] script generato che impone un piano appropriato per qualsiasi query specificata e informazioni aggiuntive che includono il testo della query e i piani di query archiviati in query Store:

```sql
WITH cte_db_tuning_recommendations
AS (SELECT reason,
        score,
        query_id,
        regressedPlanId,
        recommendedPlanId,
        current_state = JSON_VALUE(state, '$.currentValue'),
        current_state_reason = JSON_VALUE(state, '$.reason'),
        script = JSON_VALUE(details, '$.implementationDetails.script'),
        estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
        error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
    FROM sys.dm_db_tuning_recommendations
    CROSS APPLY OPENJSON(Details, '$.planForceDetails')
    WITH ([query_id] int '$.queryId',
        regressedPlanId int '$.regressedPlanId',
        recommendedPlanId int '$.recommendedPlanId',
        regressedPlanErrorCount int,    
        recommendedPlanErrorCount int,
        regressedPlanExecutionCount int,
        regressedPlanCpuTimeAverage float,
        recommendedPlanExecutionCount int,
        recommendedPlanCpuTimeAverage float
        )
    )
SELECT qsq.query_id,
    qsqt.query_sql_text,
    dtr.*,
    CAST(rp.query_plan AS XML) AS RegressedPlan,
    CAST(sp.query_plan AS XML) AS SuggestedPlan
FROM cte_db_tuning_recommendations AS dtr
INNER JOIN sys.query_store_plan AS rp ON rp.query_id = dtr.query_id
    AND rp.plan_id = dtr.regressedPlanId
INNER JOIN sys.query_store_plan AS sp ON sp.query_id = dtr.query_id
    AND sp.plan_id = dtr.recommendedPlanId
INNER JOIN sys.query_store_query AS qsq ON qsq.query_id = rp.query_id
INNER JOIN sys.query_store_query_text AS qsqt ON qsqt.query_text_id = qsq.query_text_id;
```

Per altre informazioni sulle funzioni JSON che possono essere usate per eseguire query sui valori nella visualizzazione raccomandazione, vedere [supporto JSON](../json/json-data-sql-server.md) in [!INCLUDE[ssde_md](../../includes/ssde_md.md)] .
  
## <a name="permissions"></a>Autorizzazioni  

È richiesta l' `VIEW SERVER STATE` autorizzazione in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .   
Richiede l' `VIEW DATABASE STATE` autorizzazione per il database in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .   

## <a name="see-also"></a>Vedi anche  
 [Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [sys.database_automatic_tuning_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)   
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [Supporto JSON](../json/json-data-sql-server.md)
