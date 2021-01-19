  Modifica una configurazione esistente del gruppo di carico di lavoro di Resource Governor e facoltativamente la assegna a un pool di risorse di Resource Governor.  
  
 ![Icona di collegamento a un argomento](../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER WORKLOAD GROUP { group_name | "default" }  
[ WITH  
    ([ IMPORTANCE = { LOW | MEDIUM | HIGH } ]  
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]  
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]  
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]   
      [ [ , ] MAX_DOP = value ]  
      [ [ , ] GROUP_MAX_REQUESTS = value ] )  
 ]  
[ USING { pool_name | "default" } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argomenti

 *group_name* | "**default**"       
 Nome di un gruppo di carico di lavoro esistente definito dall'utente o del gruppo di carico di lavoro predefinito di Resource Governor.  
  
> [!NOTE]  
> Resource Governor crea i gruppi "predefiniti" e interni all'installazione di [!INCLUDE[ssNoVersion](ssnoversion-md.md)].  
  
 Se utilizzata con ALTER WORKLOAD GROUP, l'opzione "default" deve essere delimitata da virgolette ("") o parentesi quadrate ([]) per evitare conflitti con DEFAULT, una parola riservata di sistema. Per altre informazioni, vedere [Identificatori del database](../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
> Per i gruppi del carico di lavoro e pool di risorse predefiniti vengono utilizzati sempre nomi scritti in lettere minuscole, ad esempio "default". Questo aspetto deve essere preso in considerazione per i server in cui vengono utilizzate regole di confronto con distinzione tra maiuscole e minuscole. In server con regole di confronto senza distinzione tra maiuscole e minuscole, ad esempio SQL_Latin1_General_CP1_CI_AS, le parole "default" e "Default" vengono considerate uguali.  
  
 IMPORTANCE = { LOW | **MEDIUM** | HIGH }       
 Specifica l'importanza relativa di una richiesta nel gruppo del carico di lavoro. I possibili valori di importanza sono i seguenti:  
  
-   LOW  
-   MEDIUM (valore predefinito)  
-   HIGH  
  
> [!NOTE]  
> Internamente, ogni impostazione dell'importanza viene memorizzata come un numero utilizzato per i calcoli.  
  
 IMPORTANCE è locale al pool di risorse. I gruppi di carico di lavoro con diversa importanza e interni allo stesso pool di risorse influiscono l'uno sull'altro, ma non sui gruppi di carico di lavoro in un altro pool di risorse.  
  
 REQUEST_MAX_MEMORY_GRANT_PERCENT = *value*     
 Specifica la quantità massima di memoria che una singola richiesta può accettare dal pool. *value* è una percentuale relativa alla dimensione del pool di risorse specificata da MAX_MEMORY_PERCENT.  

*value* è un integer fino a [!INCLUDE[ssSQL17](sssql17-md.md)] e un valore float a partire da [!INCLUDE[sql-server-2019](sssqlv15-md.md)]. Il valore predefinito è 25. L'intervallo consentito per *value* è compreso tra 1 e 100.
  
> [!NOTE]  
> La quantità specificata si riferisce solo alla memoria di concessione per l'esecuzione della query.  
  
> [!IMPORTANT]
> Impostando *value* su 0 si impedisce l'esecuzione delle query con operazioni SORT e HASH JOIN nei gruppi del carico di lavoro definiti dall'utente.     
>
> Non è consigliabile impostare *value* su un valore maggiore di 70 perché è possibile che il server non possa riservare una quantità sufficiente di memoria se sono in esecuzione altre query simultaneamente. È possibile che venga restituito l'errore di timeout query 8645.      
  
> [!NOTE]  
> Se i requisiti di memoria della query superano il limite specificato da questo parametro, il server effettua le operazioni seguenti:  
>   
> -  Per i gruppi di carico di lavoro definiti dall'utente, il server tenta di ridurre il grado di parallelismo delle query fino a quando i requisiti di memoria non rientrano nel limite o fino a quando il grado di parallelismo non è uguale a 1. Se i requisiti di memoria delle query sono ancora superiori al limite, si verifica l'errore 8657.  
>   
> -  Per i gruppi di carico di lavoro interni e predefiniti, il server permette alla query di ottenere la memoria necessaria.  
>   
> In entrambi i casi, è possibile che si verifichi l'errore di timeout 8645 se il server non dispone di memoria fisica sufficiente.  
  
 REQUEST_MAX_CPU_TIME_SEC = *value*       
 Viene specificato il tempo massimo della CPU, in secondi, utilizzabile da una richiesta. *value* deve essere 0 o un valore intero positivo. L'impostazione predefinita per *value* è 0, ovvero un valore illimitato.  
  
> [!NOTE]  
> Per impostazione predefinita, Resource Governor non impedirà la continuazione di una richiesta se viene superato il tempo massimo, ma verrà generato un evento. Per altre informazioni, vedere [Classe di evento CPU Threshold Exceeded](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md). 

> [!IMPORTANT]
> A partire da [!INCLUDE[ssSQL15](sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](sssql17-md.md)] CU3 e usando il [flag di traccia 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md), Resource Governor interrompe una richiesta se viene superato il tempo massimo.
  
 REQUEST_MEMORY_GRANT_TIMEOUT_SEC =*value*  
 Specifica il tempo massimo, in secondi, che una query può attendere prima che una concessione di memoria (memoria buffer di lavoro) diventi disponibile.  
  
> [!NOTE]  
> L'esecuzione della query può riuscire anche in caso di timeout relativo alla concessione di memoria. L'esito negativo di una query si verifica solo se sono in esecuzione più query simultaneamente. In caso contrario, la query può ottenere solo la minima concessione di memoria, con una conseguente riduzione delle prestazioni.  
  
 *value* deve essere un numero intero positivo. L'impostazione predefinita per *value*, 0, usa un calcolo interno basato sul costo della query per determinare il tempo massimo.  
  
 MAX_DOP =*value*       
 Viene specificato il grado massimo di parallelismo (DOP) per le richieste parallele. *value* deve essere 0 o un numero intero positivo, da 1 a 255. Quando *value* è 0, il server sceglie il grado massimo di parallelismo. Si tratta dell'impostazione predefinita e consigliata.  
  
> [!NOTE]  
> Il valore effettivo impostato da [!INCLUDE[ssDE](ssde-md.md)] per MAX_DOP potrebbe essere inferiore al valore specificato. Il valore finale è determinato dalla formula min(255, *numero di CPU)* .  
  
> [!CAUTION]  
> La modifica di MAX_DOP può influire negativamente sulle prestazioni di un server. Se è necessario modificare MAX_DOP, si consiglia di impostarlo a un valore minore o uguale al numero massimo di utilità di pianificazione dell'hardware che sono presenti in un singolo nodo NUMA. Si consiglia di non impostare MAX_DOP a un valore maggiore di 8.  
  
 MAX_DOP viene gestito nel modo seguente:  
  
-   MAX_DOP, inteso come hint per la query, viene rispettato fintanto che non supera il valore MAX_DOP del gruppo di carico di lavoro.  
  
-   MAX_DOP, inteso come hint per la query, ha sempre la precedenza sull'opzione "max degree of parallelism" di sp_configure.  
  
-   Il valore MAX_DOP del gruppo di carico di lavoro ha sempre la precedenza sull'opzione "max degree of parallelism" di sp_configure.  
  
-   Se la query viene contrassegnata come seriale (MAX_DOP = 1) in fase di compilazione, durante l'esecuzione non potrà ritornare a parallela, indipendentemente dal gruppo di carico di lavoro o dall'impostazione sp_configure.  
  
 Dopo la configurazione di DOP, è possibile diminuire solo la richiesta di memoria concessa. La riconfigurazione del gruppo di carico di lavoro non è visibile durante l'attesa nella coda della memoria concessa.  
  
 GROUP_MAX_REQUESTS = *value*      
 Viene specificato il numero massimo di richieste simultanee eseguibili nel gruppo del carico di lavoro. *value* deve essere 0 o un valore intero positivo. L'impostazione predefinita per *value*, vale a dire 0, consente un numero illimitato di richieste. Quando viene raggiunto il numero massimo di richieste simultanee, un utente in quel gruppo può effettuare l'accesso, ma viene posizionato in uno stato di attesa fino a quando le richieste simultanee non sono inferiori al valore specificato.  
  
 USING { *pool_name* | "**default**" }      
 Associa il gruppo del carico di lavoro al pool di risorse definito dall'utente identificato da *pool_name*, in modo da inserire il gruppo del carico di lavoro nel pool di risorse. Se *pool_name* non viene specificato o non si usa l'argomento USING, il gruppo del carico di lavoro viene inserito nel pool predefinito di Resource Governor.  
  
 Se utilizzata con ALTER WORKLOAD GROUP, l'opzione "default" deve essere delimitata da virgolette ("") o parentesi quadrate ([]) per evitare conflitti con DEFAULT, una parola riservata di sistema. Per altre informazioni, vedere [Identificatori del database](../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
> Per l'opzione "default" si applica la distinzione tra maiuscole e minuscole.  
  
## <a name="remarks"></a>Osservazioni  
 ALTER WORKLOAD GROUP è consentito nel gruppo predefinito.  
  
 Le modifiche alla configurazione del gruppo di carico di lavoro non hanno effetto fino all'esecuzione di ALTER RESOURCE GOVERNOR RECONFIGURE. Quando si modifica un'impostazione che influisce sul piano, la nuova impostazione diventerà effettiva nei piani memorizzati precedentemente nella cache solo dopo l'esecuzione di DBCC FREEPROCCACHE (*pool_name*), dove *pool_name* è il nome di un pool di risorse di Resource Governor in cui è associato il gruppo del carico di lavoro.  
  
-   Se si modifica MAX_DOP in 1, non è necessario eseguire DBCC FREEPROCCACHE poiché i piani paralleli possono essere eseguiti in modalità seriale. Potrebbe però non essere efficiente quanto un piano compilato come piano seriale.  
  
-   Se si modifica MAX_DOP da 1 in 0 o in un valore maggiore di 1, non è necessario eseguire DBCC FREEPROCCACHE. I piani seriali non possono però essere eseguiti in parallelo. È quindi necessario cancellare la cache corrispondente in modo che i nuovi piani possano essere potenzialmente compilati tramite il parallelismo.  
  
> [!CAUTION]  
> La cancellazione dei piani memorizzati nella cache da un pool di risorse associato a più di un gruppo del carico di lavoro avrà effetto su tutti i gruppi di carico di lavoro che hanno il pool di risorse definito dall'utente identificato da *pool_name*.  
  
 Per l'esecuzione di istruzioni DDL, è consigliabile avere familiarità con gli stati di Resource Governor. Per altre informazioni, vedere [Resource Governor](../relational-databases/resource-governor/resource-governor.md).  
  
 REQUEST_MEMORY_GRANT_PERCENT: in [!INCLUDE[ssVersion2005](ssversion2005-md.md)], per la creazione dell'indice è consentito l'uso di una maggiore quantità di memoria per l'area di lavoro di quella concessa inizialmente, al fine di migliorare le prestazioni. Questa speciale gestione è supportata da Resource Governor nelle versioni successive, tuttavia, la concessione iniziale ed eventuali concessioni di memoria aggiuntiva sono limitate dalle impostazioni di gruppo del carico di lavoro e dal pool di risorse.  
  
 **Creazione dell'indice in una tabella partizionata**  
  
 La quantità di memoria utilizzata per la creazione dell'indice in una tabella partizionata non allineata è proporzionale al numero di partizioni coinvolte.  Se la memoria totale necessaria supera il limite per query (REQUEST_MAX_MEMORY_GRANT_PERCENT) imposto dal gruppo di carico di lavoro di Resource Governor, la creazione dell'indice potrebbe non riuscire. Poiché il gruppo di carico di lavoro "default" consente a una query di superare il limite per query con la memoria minima necessaria per la compatibilità con [!INCLUDE[ssVersion2005](ssversion2005-md.md)], l'utente potrebbe essere in grado di eseguire la stessa creazione dell'indice in un gruppo di carico di lavoro "default", se nel pool di risorse "default" è configurata una quantità di memoria totale sufficiente per eseguire la query.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `CONTROL SERVER`.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene indicato come modificare l'importanza delle richieste nel gruppo predefinito da `MEDIUM` a `LOW`.  
  
```sql  
ALTER WORKLOAD GROUP "default"  
WITH (IMPORTANCE = LOW);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 L'esempio seguente mostra come spostare un gruppo di carico di lavoro dal pool in cui si trova al pool predefinito.  
  
```sql  
ALTER WORKLOAD GROUP adHoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Resource Governor](../relational-databases/resource-governor/resource-governor.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/drop-workload-group-transact-sql.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)  
