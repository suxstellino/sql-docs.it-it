Crea un gruppo del carico di lavoro di Resource Governor e lo associa al pool di risorse di Resource Governor. Resource Governor non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](msconame-md.md)][!INCLUDE[ssNoVersion](ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).

:::image type="icon" source="../database-engine/configure-windows/media/topic-link.gif"::: [Convenzioni della sintassi Transact-SQL](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE WORKLOAD GROUP group_name
[ WITH
    ( [ IMPORTANCE = { LOW | MEDIUM | HIGH } ]
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]
      [ [ , ] MAX_DOP = value ]
      [ [ , ] GROUP_MAX_REQUESTS = value ] )
 ]
[ USING {
    [ pool_name | "default" ]
    [ [ , ] EXTERNAL external_pool_name | "default" ] ]
    } ]
[ ; ]
```

## <a name="arguments"></a>Argomenti

*group_name*</br>
Nome definito dall'utente per il gruppo di carico di lavoro. *group_name* è un valore alfanumerico, può essere composto da un massimo di 128 caratteri, deve essere univoco all'interno di un'istanza di [!INCLUDE[ssNoVersion](ssnoversion-md.md)] e deve essere conforme alle regole relative agli [identificatori](../relational-databases/databases/database-identifiers.md).

IMPORTANCE = { LOW | **MEDIUM** | HIGH }</br>
Specifica l'importanza relativa di una richiesta nel gruppo del carico di lavoro. L'importanza è compresa fra le seguenti, MEDIUM è l'impostazione predefinita:

- LOW
- MEDIUM (valore predefinito)
- HIGH

> [!NOTE]
> Internamente, ogni impostazione dell'importanza viene memorizzata come un numero utilizzato per i calcoli.

IMPORTANCE è locale al pool di risorse. I gruppi di carico di lavoro con diversa importanza e interni allo stesso pool di risorse influiscono l'uno sull'altro, ma non sui gruppi di carico di lavoro in un altro pool di risorse.

REQUEST_MAX_MEMORY_GRANT_PERCENT = *value*</br>
Specifica la quantità massima di memoria che una singola richiesta può accettare dal pool. *value* è una percentuale relativa alla dimensione del pool di risorse specificata da MAX_MEMORY_PERCENT.

*value* è un intero fino a [!INCLUDE[ssSQL17](sssql17-md.md)] e un valore float a partire da [!INCLUDE[sql-server-2019](sssqlv15-md.md)] e in Istanza gestita di SQL di Azure. Il valore predefinito è 25. L'intervallo consentito per *value* è compreso tra 1 e 100.

> [!IMPORTANT]  
> La quantità specificata si riferisce solo alla memoria di concessione per l'esecuzione della query.
>
> Impostando *value* su 0 si impedisce l'esecuzione delle query con operazioni SORT e HASH JOIN nei gruppi del carico di lavoro definiti dall'utente.
>
> Non è consigliabile impostare *value* su un valore maggiore di 70 perché è possibile che il server non possa riservare una quantità sufficiente di memoria se sono in esecuzione altre query simultaneamente. È possibile che venga restituito l'errore di timeout query 8645.
>
> Se i requisiti di memoria della query superano il limite specificato da questo parametro, il server effettua le operazioni seguenti:
>
> - Per i gruppi di carico di lavoro definiti dall'utente, il server tenta di ridurre il grado di parallelismo delle query fino a quando i requisiti di memoria non rientrano nel limite o fino a quando il grado di parallelismo non è uguale a 1. Se i requisiti di memoria delle query sono ancora superiori al limite, si verifica l'errore 8657.
>
> - Per i gruppi di carico di lavoro interni e predefiniti, il server permette alla query di ottenere la memoria necessaria.
>
> In entrambi i casi, è possibile che si verifichi l'errore di timeout 8645 se il server non dispone di memoria fisica sufficiente.

REQUEST_MAX_CPU_TIME_SEC = *value*</br>
Viene specificato il tempo massimo della CPU, in secondi, utilizzabile da una richiesta. *value* deve essere 0 o un valore intero positivo. L'impostazione predefinita per *value* è 0, ovvero un valore illimitato.

> [!NOTE]
> Per impostazione predefinita, Resource Governor non impedirà la continuazione di una richiesta se viene superato il tempo massimo, ma verrà generato un evento. Per altre informazioni, vedere [Classe di evento CPU Threshold Exceeded](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).
> [!IMPORTANT]
> A partire da [!INCLUDE[ssSQL15](sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](sssql17-md.md)] CU3 e usando il [flag di traccia 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md), Resource Governor interrompe una richiesta se viene superato il tempo massimo.

REQUEST_MEMORY_GRANT_TIMEOUT_SEC = *value*</br>
Specifica il tempo massimo, in secondi, che una query può attendere prima che una concessione di memoria (memoria buffer di lavoro) diventi disponibile. *value* deve essere 0 o un valore intero positivo. L'impostazione predefinita per *value*, 0, usa un calcolo interno basato sul costo della query per determinare il tempo massimo.

> [!NOTE]
> L'esecuzione della query può riuscire anche in caso di timeout relativo alla concessione di memoria. L'esito negativo di una query si verifica solo se sono in esecuzione più query simultaneamente. In caso contrario, la query può ottenere solo la minima concessione di memoria, con una conseguente riduzione delle prestazioni.

MAX_DOP = *value*</br>
Specifica il **massimo grado di parallelismo (MAXDOP)** per l'esecuzione di richieste parallele. *value* deve essere 0 o un valore intero positivo. L'intervallo consentito per *value* è compreso tra 0 e 64. L'impostazione predefinita per *value*, 0, usa l'impostazione globale. MAX_DOP viene gestito nel modo seguente:

> [!NOTE]
> MAX_DOP del gruppo di carico di lavoro esegue l'override della [configurazione server del massimo grado di parallelismo](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) e della [configurazione con ambito database](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) di **MAXDOP**.

> [!TIP]
> Per eseguire questa operazione a livello di query, usare l'[hint per la query](../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**. L'impostazione del massimo grado di parallelismo come hint per la query è efficace finché non supera il valore MAX_DOP del gruppo di carico di lavoro. Se il valore dell'hint per la query MAXDOP supera il valore configurato con Resource Governor, il [!INCLUDE[ssDEnoversion](ssdenoversion-md.md)] usa il valore `MAX_DOP` di Resource Governor. L'[hint per la query](../t-sql/queries/hints-transact-sql-query.md) MAXDOP esegue sempre l'override della [configurazione server per il massimo grado di parallelismo ](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
>
> Per eseguire questa operazione a livello di database, usare la [configurazione con ambito database](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) **MAXDOP**.
>
> Per eseguire questa operazione a livello di server, usare l'[opzione di configurazione server](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) relativa al **massimo grado di parallelismo (MAXDOP)** .

GROUP_MAX_REQUESTS = *value*</br>
Viene specificato il numero massimo di richieste simultanee eseguibili nel gruppo del carico di lavoro. *value* deve essere 0 o un numero intero positivo. L'impostazione predefinita per *value* è 0 e consente un numero illimitato di richieste. Quando viene raggiunto il numero massimo di richieste simultanee, un utente in quel gruppo può effettuare l'accesso, ma viene posizionato in uno stato di attesa fino a quando le richieste simultanee non sono inferiori al valore specificato.

USING { *pool_name* |  **"default"** }</br>
Associa il gruppo del carico di lavoro al pool di risorse definito dall'utente identificato da *pool_name*. In questo modo, il gruppo del carico di lavoro viene inserito nel pool di risorse. Se *pool_name* non viene specificato o non si usa l'argomento USING, il gruppo del carico di lavoro viene inserito nel pool predefinito di Resource Governor.

"default" è una parola riservata e, se utilizzata con USING, deve essere delimitata da virgolette ("") o parentesi quadre ([]).

> [!NOTE]
> Per i gruppi di carico di lavoro e i pool di risorse predefiniti vengono utilizzati sempre nomi scritti in lettere minuscole, ad esempio "default". Questo aspetto deve essere preso in considerazione per i server in cui vengono utilizzate regole di confronto con distinzione tra maiuscole e minuscole. In server con regole di confronto senza distinzione tra maiuscole e minuscole, ad esempio SQL_Latin1_General_CP1_CI_AS, le parole "default" e "Default" vengono considerate uguali.

EXTERNAL external_pool_name | "default"</br>
**Si applica a**: [!INCLUDE[ssNoVersion](ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](sssql16-md.md)]).

Il gruppo del carico di lavoro può specificare un pool di risorse esterne. È possibile definire un gruppo di carico del lavoro e associarlo a due pool:

- Un pool di risorse per i carichi di lavoro [!INCLUDE[ssNoVersion](ssnoversion-md.md)] e le query
- Un pool di risorse esterne per i processi esterni. Per altre informazioni, vedere [sp_execute_external_script &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="remarks"></a>Osservazioni

Quando si usa `REQUEST_MEMORY_GRANT_PERCENT`, per la creazione dell'indice è possibile usare più memoria dell'area di lavoro di quella concessa inizialmente, al fine di migliorare le prestazioni. Tale gestione particolare è supportata da Resource Governor in [!INCLUDE[ssCurrent](sscurrent-md.md)]. La concessione iniziale, così come qualsiasi concessione supplementare, è tuttavia limitata dalle impostazioni del pool di risorse e del gruppo di carico di lavoro.

Il limite `MAX_DOP` viene impostato per [attività](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Non è un limite per [richiesta](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) o per query. Ciò significa che durante l'esecuzione di query parallele una singola richiesta può generare più attività che vengono assegnate a un'[utilità di pianificazione](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Per altre informazioni, vedere [Guida sull'architettura dei thread e delle attività](../relational-databases/thread-and-task-architecture-guide.md).

Se si usa `MAX_DOP` e una query viene contrassegnata come seriale in fase di compilazione, in fase di esecuzione non può tornare parallela, indipendentemente dal gruppo di carico di lavoro o dall'impostazione della configurazione server. Dopo che è stato configurato, il valore `MAX_DOP` può essere diminuito solo in caso di utilizzo elevato della memoria. La riconfigurazione del gruppo di carico di lavoro non è visibile durante l'attesa nella coda della memoria concessa.

### <a name="index-creation-on-a-partitioned-table"></a>Creazione dell'indice in una tabella partizionata

La quantità di memoria utilizzata per la creazione dell'indice in una tabella partizionata non allineata è proporzionale al numero di partizioni coinvolte. Se la memoria totale necessaria supera il limite per query `REQUEST_MAX_MEMORY_GRANT_PERCENT` imposto dal gruppo di carico di lavoro di Resource Governor, la creazione dell'indice potrebbe non riuscire. Poiché il gruppo di carico di lavoro *"default"* consente a una query di superare il limite per query con la memoria minima necessaria, l'utente potrebbe essere in grado di eseguire la stessa creazione dell'indice nel gruppo di carico di lavoro *"default"* , se nel pool di risorse *"default"* è configurata una quantità di memoria totale sufficiente per eseguire la query.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL SERVER`.

## <a name="example"></a>Esempio

Creare un gruppo di carico di lavoro denominato `newReports` che usa le impostazioni predefinite di Resource Governor ed è contenuto nel pool predefinito di Resource Governor. Nell'esempio viene specificato il pool `default`, ma questo non è necessario.

```sql
CREATE WORKLOAD GROUP newReports
WITH
    (REQUEST_MAX_MEMORY_GRANT_PERCENT = 2.5
      , REQUEST_MAX_CPU_TIME_SEC = 100
      , MAX_DOP = 4)    
USING "default" ;
GO
```

## <a name="see-also"></a>Vedere anche

- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/drop-workload-group-transact-sql.md)
- [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)