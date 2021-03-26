---
title: Ottimizzazione automatica
description: Informazioni sull'ottimizzazione automatica in SQL Server e nel database SQL di Azure
ms.custom: fasttrack-edit
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- performance tuning [SQL Server]
- automatic tuning
- aprc
- automatic plan regression correction
- last known good plan
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ea61597a7f9874b7438392167380c5f01b11527
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551656"
---
# <a name="automatic-tuning"></a>Ottimizzazione automatica
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

L'ottimizzazione automatica è una funzionalità di database che offre informazioni su potenziali problemi di prestazioni delle query, suggerisce soluzioni e corregge automaticamente i problemi rilevati.

L'ottimizzazione automatica, introdotta in [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , informa l'utente ogni volta che viene rilevato un potenziale problema di prestazioni e consente di applicare azioni correttive oppure consente di [!INCLUDE[ssde_md](../../includes/ssde_md.md)] correggere automaticamente i problemi di prestazioni. L'ottimizzazione automatica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di identificare e correggere i problemi di prestazioni causati dalle **regressioni della scelta del piano di esecuzione delle query**. L'ottimizzazione automatica in [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] crea anche gli indici necessari ed Elimina gli indici inutilizzati. Per ulteriori informazioni sui piani di esecuzione delle query, vedere [piani di esecuzione](../../relational-databases/performance/execution-plans.md).

Esegue il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] monitoraggio delle query eseguite sul database e migliora automaticamente le prestazioni del carico di lavoro. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] dispone di un meccanismo di Intelligence incorporato che può ottimizzare automaticamente e migliorare le prestazioni delle query, adattando in modo dinamico il database al carico di lavoro. Sono disponibili due funzionalità di ottimizzazione automatica:

-    La **correzione automatica del piano** identifica i piani di esecuzione di query problematici, ad esempio i problemi di analisi dei parametri o di analisi dei [parametri](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) , e corregge i problemi di prestazioni relativi al piano di esecuzione delle query forzando l'ultimo piano valido noto prima che si verifichi la regressione. **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

-    **Gestione automatica** degli indici identifica gli indici che devono essere aggiunti nel database e gli indici che devono essere rimossi. **Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

## <a name="why-automatic-tuning"></a>Perché optare per l'ottimizzazione automatica?

Tre delle attività principali dell'amministrazione del database classica sono il monitoraggio del carico di lavoro, l'identificazione [!INCLUDE[tsql_md](../../includes/tsql-md.md)] di query critiche e l'identificazione degli indici che devono essere aggiunti per migliorare le prestazioni o gli indici usati raramente e che potrebbero essere rimossi per migliorare le prestazioni. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Fornisce informazioni dettagliate sulle query e sugli indici che è necessario monitorare. Tuttavia, il monitoraggio costante di un database è un'attività complessa e noiosa, soprattutto quando si gestiscono molti database. La gestione di un numero elevato di database potrebbe non essere possibile in modo efficiente. Anziché eseguire manualmente il monitoraggio e l'ottimizzazione del database, è consigliabile delegare alcune delle azioni di monitoraggio e ottimizzazione alla [!INCLUDE[ssde_md](../../includes/ssde_md.md)] funzionalità di ottimizzazione automatica.

### <a name="how-does-automatic-tuning-work"></a>Come funziona l'ottimizzazione automatica

L'ottimizzazione automatica è un processo di monitoraggio e analisi continuo che apprende costantemente le caratteristiche del carico di lavoro e identifica i potenziali problemi e miglioramenti.

![Processo di ottimizzazione automatica](./media/tuning-process.png)

Questo processo consente al database di adattarsi in modo dinamico al carico di lavoro individuando gli indici e i piani che potrebbero migliorare le prestazioni dei carichi di lavoro e gli indici che influiscono sui carichi di lavoro. In base a questi risultati, l'ottimizzazione automatica applica le azioni di ottimizzazione che migliorano le prestazioni del carico di lavoro. Inoltre, l'ottimizzazione automatica monitora continuamente le prestazioni del database dopo l'implementazione delle modifiche per garantire un miglioramento delle prestazioni del carico di lavoro. Qualsiasi azione che non migliora le prestazioni viene ripristinata automaticamente. Questo processo di verifica è una funzionalità chiave che garantisce che le modifiche apportate dall'ottimizzazione automatica non riducano le prestazioni complessive del carico di lavoro.

## <a name="automatic-plan-correction"></a>Correzione automatica dei piani

La correzione automatica dei piani è una funzionalità di ottimizzazione automatica che identifica la **regressione scelta del piano di esecuzione** e corregge automaticamente il problema forzando l'ultimo piano valido noto. Per ulteriori informazioni sui piani di esecuzione delle query e su Query Optimizer, vedere la [Guida sull'architettura di elaborazione](../../relational-databases/query-processing-architecture-guide.md)delle query.

> [!IMPORTANT]
> La correzione automatica dei piani dipende dalla Query Store abilitata nel database per il rilevamento del carico di lavoro. 

### <a name="what-is-execution-plan-choice-regression"></a>Informazioni sulla regressione scelta del piano di esecuzione

[!INCLUDE[ssdenoversion_md](../../includes/ssdenoversion_md.md)]Per eseguire le query, può utilizzare piani di esecuzione diversi [!INCLUDE[tsql_md](../../includes/tsql-md.md)] . I piani di query dipendono dalle statistiche, dagli indici e da altri fattori. Il piano ottimale che deve essere utilizzato per eseguire una [!INCLUDE[tsql_md](../../includes/tsql-md.md)] query può variare nel tempo a seconda delle modifiche apportate a tali fattori. In alcuni casi, il nuovo piano potrebbe non essere migliore rispetto a quello precedente e il nuovo piano potrebbe causare una regressione delle prestazioni, ad esempio una [sensibilità del parametro o](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) un problema correlato all'analisi dei parametri. 

 ![Regressione scelta del piano di esecuzione della query](media/plan-choice-regression.png "Regressione scelta del piano di esecuzione della query") 

Ogni volta che si nota una regressione scelta del piano, si dovrebbe trovare un piano valido precedente e forzarlo per l'utilizzo al posto di quello corrente. Questa operazione può essere eseguita tramite la `sp_query_store_force_plan` procedura. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]In [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] vengono fornite informazioni sui piani con regressione e sulle azioni correttive consigliate. Inoltre, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] consente di automatizzare completamente questo processo e di [!INCLUDE[ssde_md](../../includes/ssde_md.md)] risolvere eventuali problemi rilevati in relazione alla modifica del piano.

> [!IMPORTANT]
> La correzione automatica del piano deve essere utilizzata nell'ambito di un aggiornamento del livello di compatibilità del database, dopo l'acquisizione di una baseline, per ridurre automaticamente i rischi di aggiornamento del carico di lavoro. Per altre informazioni su questo caso d'uso, vedere [garantire la stabilità delle prestazioni durante l'aggiornamento a SQL Server più recenti](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). 

### <a name="automatic-plan-choice-correction"></a>Correzione automatica del piano

Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] può passare automaticamente all'ultimo piano positivo noto ogni volta che viene rilevata una regressione della scelta del piano.

![Correzione scelta del piano di esecuzione della query](media/force-last-good-plan.png "Correzione scelta del piano di esecuzione della query") 

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]Rileva automaticamente qualsiasi possibile regressione scelta del piano, incluso il piano da utilizzare al posto del piano errato. Quando [!INCLUDE[ssde_md](../../includes/ssde_md.md)] applica l'ultimo piano valido noto prima che venga eseguita la regressione, monitora automaticamente le prestazioni del piano forzato. Se il piano forzato non è migliore del piano regressione, il nuovo piano verrà imposto e [!INCLUDE[ssde_md](../../includes/ssde_md.md)] verrà compilato un nuovo piano. Se [!INCLUDE[ssde_md](../../includes/ssde_md.md)] Verifica che il piano forzato è migliore del piano regressione, il piano forzato verrà mantenuto. Verrà mantenuto fino a quando non si verifica una ricompilazione (ad esempio, per l'aggiornamento delle statistiche successivo o per la modifica dello schema). Per ulteriori informazioni sull'utilizzo forzato del piano e sui tipi di piani che possono essere forzati, vedere [limitazioni di forzatura del piano](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md#plan-forcing-limitations).

> [!NOTE]
> Se l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza viene riavviata prima che venga verificata un'azione di forzatura del piano, il piano verrà automaticamente applicato. In caso contrario, l'utilizzo forzato del piano viene mantenuto al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riavvio.

### <a name="enabling-automatic-plan-choice-correction"></a>Abilitazione della correzione della scelta del piano automatica

È possibile abilitare l'ottimizzazione automatica per ogni database e specificare che l'ultimo piano valido deve essere forzato ogni volta che viene rilevata una regressione della modifica del piano. L'ottimizzazione automatica viene abilitata con il comando seguente:

```sql   
ALTER DATABASE <yourDatabase>
SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON ); 
```

Una volta abilitata questa opzione, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] forza automaticamente eventuali consigli in cui il guadagno stimato della CPU è superiore a 10 secondi oppure il numero di errori nel nuovo piano è superiore al numero di errori nel piano consigliato e verifica che il piano forzato sia migliore rispetto a quello corrente.

### <a name="alternative---manual-plan-choice-correction"></a>Opzione alternativa: correzione manuale della scelta del piano

Senza l'ottimizzazione automatica, gli utenti devono monitorare periodicamente il sistema e cercare le query che hanno regressione. Se un piano ha regresso, l'utente deve trovare un piano valido precedente e forzarlo invece di quello corrente usando la `sp_query_store_force_plan` procedura. La procedura consigliata è quella di forzare l'ultimo piano valido noto perché i piani precedenti potrebbero non essere validi a causa di modifiche di statistica o di indice. L'utente che impone l'ultimo piano valido noto deve monitorare le prestazioni della query eseguita usando il piano forzato e verificare che il piano forzato funzioni come previsto. A seconda dei risultati del monitoraggio e dell'analisi, il piano deve essere forzato o l'utente deve trovare un altro modo per ottimizzare la query, ad esempio la riscrittura. I piani forzati manualmente non devono essere forzati per sempre, perché [!INCLUDE[ssde_md](../../includes/ssde_md.md)] deve essere in grado di applicare piani ottimali. L'utente o l'amministratore di database deve infine disforzare il piano usando la `sp_query_store_unforce_plan` procedura e consentire al di [!INCLUDE[ssde_md](../../includes/ssde_md.md)] trovare il piano ottimale. 

> [!TIP]
> In alternativa, utilizzare le **query con piani forzati** query Store visualizzazione per individuare e disforzare i piani.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce tutte le viste e le procedure necessarie per monitorare le prestazioni e risolvere i problemi in Query Store.

In [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] è possibile trovare regressioni della scelta del piano usando le viste di sistema query Store. A partire da [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] rileva e Mostra le potenziali regressioni della scelta del piano e le azioni consigliate da applicare nel [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) DMV. La DMV Mostra le informazioni sul problema, l'importanza del problema e i dettagli, ad esempio la query identificata, l'ID del piano regressione, l'ID del piano usato come baseline per il confronto e l' [!INCLUDE[tsql_md](../../includes/tsql-md.md)] istruzione che può essere eseguita per risolvere il problema.

| tipo | description | Datetime | score | dettagli | ... |
| --- | --- | --- | --- | --- | --- |
| `FORCE_LAST_GOOD_PLAN` | Tempo CPU modificato da 4 ms a 14 ms | 3/17/2017 | 83 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |
| `FORCE_LAST_GOOD_PLAN` | Tempo CPU modificato da 37 MS a 84 ms | 16/03/2017 | 26 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |

Alcune colonne di questa vista sono descritte nell'elenco seguente:
 - Tipo di azione consigliata `FORCE_LAST_GOOD_PLAN` .
 - Descrizione che contiene informazioni sul motivo per [!INCLUDE[ssde_md](../../includes/ssde_md.md)] cui il ritiene che la modifica del piano rappresenti una potenziale regressione delle prestazioni.
 - DateTime quando viene rilevata la regressione potenziale.
 - Punteggio di questa raccomandazione.
 - Dettagli sui problemi, ad esempio ID del piano rilevato, ID del piano regressione, ID del piano da forzare per risolvere il problema, [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script che potrebbe essere applicato per risolvere il problema e così via. I dettagli vengono archiviati in [formato JSON](../json/json-data-sql-server.md).

Usare la query seguente per ottenere uno script che corregge il problema e informazioni aggiuntive sul guadagno stimato:

```sql   
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
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

[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]     

| reason | score | script | \_ID query | ID piano corrente \_ | ID piano consigliato \_ | guadagno stimato \_ | \_soggetta a errori
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Tempo CPU modificato da 3 ms a 46 ms | 36 | Il \_ \_ \_ piano di forzatura dell'archivio query di exec SP \_ 12, 17; | 12 | 28 | 17 | 11,59 | 0

La colonna `estimated_gain` rappresenta il numero stimato di secondi che verrebbero salvati se il piano consigliato venisse utilizzato per l'esecuzione della query anziché per il piano corrente. Se il guadagno è maggiore di 10 secondi, è necessario forzare il piano consigliato anziché il piano corrente. Se sono presenti più errori (ad esempio, timeout o esecuzioni interrotte) nel piano corrente rispetto al piano consigliato, la colonna `error_prone` verrebbe impostata sul valore `YES` . Un piano a rischio di errori è un altro motivo per cui è necessario forzare il piano consigliato anziché quello corrente.

Sebbene [!INCLUDE[ssde_md](../../includes/ssde_md.md)] fornisca tutte le informazioni necessarie per identificare le regressioni della scelta del piano, il monitoraggio continuo e la correzione dei problemi di prestazioni potrebbero diventare un processo noioso. L'ottimizzazione automatica rende molto più semplice questo processo.

> [!NOTE]
> I dati nella `sys.dm_db_tuning_recommendations` DMV non vengono mantenuti dopo il riavvio del motore di database. Utilizzare la `sqlserver_start_time` colonna [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) per individuare l'ultima ora di avvio del motore di database.   

## <a name="automatic-index-management"></a>Gestione automatica degli indici

In [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] la gestione degli indici è semplice perché [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] apprende il carico di lavoro e garantisce che i dati siano sempre indicizzati in modo ottimale. La progettazione appropriata degli indici è fondamentale per ottenere prestazioni ottimali del carico di lavoro e la gestione automatica degli indici risulta utile per ottimizzare gli indici. La gestione automatica degli indici consente di correggere i problemi di prestazioni nei database indicizzati in modo non corretto oppure di gestire e migliorare gli indici nello schema del database esistente. L'ottimizzazione automatica in [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] esegue le azioni seguenti:

 - Identifica gli indici che possono migliorare le prestazioni delle [!INCLUDE[tsql_md](../../includes/tsql-md.md)] query che leggono i dati dalle tabelle.
 - Identifica indici ridondanti o indici che non sono stati utilizzati in un periodo di tempo più lungo che potrebbero essere rimossi. La rimozione di indici non necessari migliora le prestazioni delle query che aggiornano i dati nelle tabelle.

### <a name="why-do-you-need-index-management"></a>Utilità della gestione degli indici

Gli indici velocizzano alcune query che leggono i dati dalle tabelle, ma possono rallentare le query che aggiornano i dati. È necessario valutare con attenzione quando creare un indice e quali colonne includervi. Alcuni indici potrebbero non essere più necessari dopo un certo periodo di tempo. Pertanto, è necessario identificare periodicamente ed eliminare questi indici che non offrono alcun vantaggio. Se si ignorano gli indici inutilizzati, le prestazioni delle query che aggiornano i dati verrebbero diminuite senza alcun vantaggio per le query che leggono i dati. Gli indici inutilizzati influiscono anche sulle prestazioni complessive del sistema perché risultano necessarie operazioni di registrazione superflue per aggiornamenti aggiuntivi.

Per individuare il set ottimale di indici che consentono di migliorare le prestazioni delle query di lettura dei dati dalle tabelle con un impatto minimo per gli aggiornamenti, possono essere necessarie operazioni di analisi continue e complesse.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] Usa l'Intelligence incorporata e le regole avanzate che analizzano le query, identificano gli indici ottimali per i carichi di lavoro correnti e identificano gli indici che potrebbero dover essere rimossi. Il database SQL di Azure garantisce un set minimo necessario di indici che ottimizzano le query che leggono i dati, con un effetto ridotto sulle altre query.

### <a name="automatic-index-management"></a>Gestione automatica degli indici

Oltre al rilevamento, [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] può applicare automaticamente le raccomandazioni identificate. Se si ritiene che le regole predefinite migliorino le prestazioni del database, è possibile consentire la [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] gestione automatica degli indici.

Per abilitare l'ottimizzazione automatica in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e consentire la funzionalità di ottimizzazione automatica per la gestione completa del carico di lavoro, vedere [abilitare l'ottimizzazione automatica nel database SQL di Azure con portale di Azure](/azure/sql-database/sql-database-automatic-tuning-enable).

Quando [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] applica una raccomandazione create index o drop index, monitora automaticamente le prestazioni delle query interessate dall'indice. Il nuovo indice verrà mantenuto solo se le prestazioni delle query interessate sono migliorate. L'indice eliminato verrà ricreato automaticamente se sono presenti query che vengono eseguite più lentamente a causa dell'assenza dell'indice.

### <a name="automatic-index-management-considerations"></a>Considerazioni sulla gestione automatica degli indici

Le azioni necessarie per creare gli indici necessari in [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] potrebbero utilizzare risorse e influire temporaneamente sulle prestazioni del carico di lavoro. Per ridurre al minimo l'effetto della creazione dell'indice sulle prestazioni del carico di lavoro, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  trova un intervallo di tempo appropriato per qualsiasi operazione di gestione degli indici. L'azione di ottimizzazione viene posticipata se il database necessita di risorse per l'esecuzione del carico di lavoro e viene riavviato quando il database dispone di risorse inutilizzate che possono essere usate per l'attività di manutenzione. Una funzionalità importante nella gestione automatica degli indici e la verifica delle azioni. Quando [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  Crea o Elimina un indice, un processo di monitoraggio analizza le prestazioni del carico di lavoro per verificare che l'azione abbia migliorato le prestazioni complessive. Se non ha portato a un miglioramento significativo, l'azione viene immediatamente ripristinata. In questo modo, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  garantisce che le azioni di ottimizzazione automatica non influiscano negativamente sulle prestazioni del carico di lavoro. Gli indici creati tramite l'ottimizzazione automatica sono trasparenti per l'operazione di manutenzione sullo schema sottostante. Le modifiche dello schema, ad esempio l'eliminazione o ridenominazione delle colonne, non vengono impedite dalla presenza di indici creati automaticamente. Gli indici creati automaticamente da [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  vengono eliminati immediatamente quando vengono eliminate la tabella o le colonne correlate.

### <a name="alternative---manual-index-management"></a>Alternativa-gestione degli indici manuale

Senza la gestione automatica degli indici, un utente o un amministratore di database deve eseguire manualmente una query sulla [sys.dm_db_missing_index_details &#40;visualizzazione&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) o utilizzare il report Dashboard prestazioni in [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] per trovare gli indici che potrebbero migliorare le prestazioni, creare indici utilizzando i dettagli forniti in questa visualizzazione e monitorare manualmente le prestazioni della query. Per individuare gli indici che devono essere eliminati, gli utenti devono monitorare le statistiche sull'utilizzo operativo degli indici per individuare gli indici usati raramente.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] semplifica questo processo. [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] analizza il carico di lavoro, identifica le query che possono essere eseguite più velocemente con un nuovo indice e identifica gli indici inutilizzati o duplicati. Per altre informazioni sull'identificazione degli indici che devono essere modificati, vedere [Find index recommendations in Azure portal](/azure/sql-database/sql-database-advisor-portal) (Trovare raccomandazioni per gli indici nel portale di Azure).

## <a name="see-also"></a>Vedi anche  
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_automatic_tuning_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)  
 [sys.dm_db_tuning_recommendations &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sp_query_store_force_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)     
 [sp_query_store_unforce_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)           
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_os_sys_info &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)    
 [Funzioni JSON](../json/json-data-sql-server.md)    
 [Piani di esecuzione](../../relational-databases/performance/execution-plans.md)    
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Strumenti per il monitoraggio e l'ottimizzazione delle prestazioni](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
 [Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)    