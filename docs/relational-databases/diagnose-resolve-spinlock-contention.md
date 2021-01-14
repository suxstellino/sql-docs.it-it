---
title: 'White paper: Diagnosticare e risolvere i conflitti di spinlock'
description: Questo articolo offre un'analisi dettagliata della diagnosi e della risoluzione dei conflitti di spinlock in SQL Server. Questo articolo è stato pubblicato originariamente dal team SQLCAT di Microsoft.
ms.date: 09/30/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: how-to
author: bluefooted
ms.author: pamela
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00b4856ab0c057b7f63aae44834884bc775d8e92
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102169"
---
# <a name="diagnose-and-resolve-spinlock-contention-on-sql-server"></a>Diagnosticare e risolvere i conflitti di spinlock in SQL Server

Questo articolo fornisce informazioni approfondite su come identificare e risolvere i problemi relativi ai conflitti di spinlock nelle applicazioni di SQL Server in sistemi a concorrenza elevata.

> [!NOTE]
> Le raccomandazioni e le procedure consigliate documentate in questo articolo si basano sull'esperienza reale di sviluppo e distribuzione di sistemi OLTP reali. Sono state pubblicate originariamente dal team Microsoft SQL Server Customer Advisory Team (SQLCAT).

## <a name="background"></a>Background

In passato, i computer Windows Server commerciali usavano solo uno o due chip di microprocessore/CPU e le CPU erano progettate con un solo processore o "core". Le capacità di elaborazione dei computer sono aumentate con l'uso di CPU più veloci, in larga misura grazie ai progressi della densità dei transistor. Secondo la "legge di Moore", la densità dei transistor o il numero di transistor che possono essere posizionati in un circuito integrato sono raddoppiati costantemente ogni due anni dopo lo sviluppo della prima CPU a chip singolo per utilizzo generico nel 1971. Negli ultimi anni, l'approccio tradizionale per l'aumento della capacità di elaborazione dei computer con CPU più veloci è stato ampliato tramite la creazione di computer con più CPU. Al momento della stesura di questo articolo, l'architettura della CPU Nehalem Intel è in grado di ospitare fino a otto core per CPU, che se usati in un sistema a otto socket possono essere quindi raddoppiati a 128 processori logici usando la tecnologia di hyper-threading. Con l'aumentare del numero di processori logici nei computer compatibili con x86, i problemi correlati alla concorrenza aumentano perché i processori logici sono in competizione per le risorse. Questa guida illustra come identificare e risolvere particolari problemi di contesa delle risorse osservati durante l'esecuzione di applicazioni SQL Server in sistemi a concorrenza elevata con alcuni carichi di lavoro.

In questa sezione verranno analizzate le lezioni apprese dal team SQLCAT durante la diagnosi e la risoluzione dei problemi relativi ai conflitti di spinlock. I conflitti di spinlock sono un tipo di problema di concorrenza osservato nei carichi di lavoro dei clienti reali in sistemi su larga scala.

## <a name="symptoms-and-causes-of-spinlock-contention"></a>Sintomi e cause dei conflitti di spinlock

In questa sezione viene descritto come diagnosticare i problemi di *conflitti di spinlock*, deleteri per le prestazioni delle applicazioni OLTP in SQL Server. La diagnosi e la risoluzione dei problemi di spinlock devono essere considerate un argomento avanzato, che richiede la conoscenza di strumenti di debug e dei meccanismi interni di Windows.

Gli spinlock sono primitive di sincronizzazione leggere usate per proteggere l'accesso alle strutture di dati. Gli spinlock non sono una peculiarità di SQL Server. Il sistema operativo li usa quando l'accesso a una determinata struttura di dati è necessario solo per un breve periodo di tempo. Quando un thread che tenta di acquisire uno spinlock non è in grado di ottenere l'accesso, viene eseguito in un ciclo di controllo periodico per determinare se la risorsa è disponibile anziché rinunciare immediatamente. Dopo un certo periodo di tempo, un thread in attesa di un spinlock rinuncerà prima di riuscire ad acquisire la risorsa. La rinuncia consente l'esecuzione di altri thread sulla stessa CPU. Questo comportamento è noto come backoff e verrà discusso in maggiore dettaglio più avanti in questo articolo.

SQL Server usa gli spinlock per proteggere l'accesso ad alcune delle strutture di dati interne. Gli spinlock vengono usati all'interno del motore per serializzare l'accesso a determinate strutture di dati in modo simile ai latch. La differenza principale tra un latch e uno spinlock è il fatto che gli spinlock eseguiranno rotazioni (ciclo) per un certo periodo di tempo per verificare la disponibilità di una struttura di dati, mentre un thread che tenta di acquisire l'accesso a una struttura protetta da un latch rinuncerà immediatamente se la risorsa non è disponibile. Per la rinuncia è richiesto il cambio del contesto di un thread fuori dalla CPU, in modo che possa essere eseguito un altro thread. Si tratta di un'operazione relativamente costosa e per le risorse mantenute per un breve periodo di tempo è più efficiente, in generale, consentire l'esecuzione di un thread in un ciclo con controllo periodico della disponibilità della risorsa.

## <a name="symptoms"></a>Sintomi

In qualsiasi sistema a concorrenza elevata occupato, è normale assistere alla contesa attiva per strutture ad accesso frequente protette da spinlock. Questo utilizzo è considerato problematico solo quando la contesa introduce un sovraccarico significativo della CPU. Le statistiche per gli spinlock sono esposte dalla vista a gestione dinamica (DMV) *sys.dm_os_spinlock_stats* in SQL Server. Ad esempio, questa query restituisce l'output seguente:

> [!NOTE]
> Altri dettagli sull'interpretazione delle informazioni restituite da questa DMV verranno presentati più avanti in questo articolo.

```sql
select * from sys.dm_os_spinlock_stats
order by spins desc
```

![sys.dm_os_spinlock_stats output](./media/diagnose-resolve-spinlock-contention/image4.png)

Le statistiche esposte da questa query sono descritte di seguito:

| Colonna | Descrizione |
|---|---|
| **Collisions** | Questo valore viene incrementato ogni volta che a un thread viene impedito l'accesso a una risorsa protetta da uno spinlock. |
| **Spins** | Questo valore viene incrementato ogni volta che un thread esegue un ciclo mentre è in attesa che uno spinlock diventi disponibile. Si tratta di una misura della quantità di lavoro eseguita da un thread durante il tentativo di acquisire una risorsa. |
| **Spins_per_collision** | Rapporto di rotazioni per conflitto. |
| **Sleep time** | Correlato agli eventi di backoff. Non rilevante, tuttavia, per le tecniche descritte in questo articolo. |
| **Backoffs** | Si verifica quando un thread "in rotazione" che tenta di accedere a una risorsa occupata determina che deve consentire l'esecuzione di altri thread sulla stessa CPU. |

Ai fini di questa discussione, le statistiche di particolare interesse sono il numero di conflitti, le rotazioni e gli eventi di backoff che si verificano in un periodo specifico quando il sistema è sottoposto a un carico elevato. Quando un thread tenta di accedere a una risorsa protetta da uno spinlock, si verifica un conflitto. Quando si verifica un conflitto, viene incrementato il numero di conflitti e il thread inizierà una rotazione in ciclo controllando periodicamente se la risorsa è disponibile. Per ogni rotazione (ciclo) del thread, il numero di rotazioni viene incrementato.

Il numero di rotazioni per conflitto è una misura della quantità di rotazioni che si verificano mentre uno spinlock viene mantenuto attivo da un thread e indica il numero di rotazioni che si verificano mentre i thread mantengono attivo lo spinlock. Ad esempio, un numero contenuto di rotazioni per conflitto e un numero elevato di conflitti indicano che viene eseguita una piccola quantità di rotazioni per lo spinlock e che ci sono molti thread che se lo contendono. Una grande quantità di rotazioni significa che il tempo dedicato alla rotazione nel codice dello spinlock è stato relativamente lungo (ovvero, il codice percorre un numero elevato di voci in un bucket di hash). Con l'aumento della contesta, con conseguente incremento del numero di conflitti, aumenta anche il numero di rotazioni.

I backoff possono essere spiegati in modo simile alle rotazioni. La progettazione prevede che, per evitare un sovraccarico eccessivo della CPU, gli spinlock non continuino a ruotare all'infinito fino a quando non riescono ad accedere a una risorsa occupata. Per evitare che uno spinlock usi eccessivamente le risorse della CPU, è previsto il backoff ossia che gli spinlock interrompano la rotazione e vadano in "sospensione". Gli spinlock eseguono il backoff indipendentemente dal fatto che ottengano la proprietà della risorsa di destinazione. Questa operazione viene eseguita per consentire la pianificazione di altri thread sulla CPU nella speranza che possa essere eseguito lavoro più produttivo. Il comportamento predefinito per il motore prevede la rotazione per un intervallo di tempo costante prima di eseguire un backoff. Il tentativo di ottenere uno spinlock richiede il mantenimento di stato di concorrenza della cache, ovvero un'operazione con utilizzo intensivo della CPU rispetto al costo della rotazione in termini di CPU. Pertanto, i tentativi di ottenere un spinlock vengono eseguiti con parsimonia e non vengono eseguiti per ogni rotazione di un thread. In SQL Server determinati tipi di spinlock (ad esempio: LOCK_HASH) sono stati migliorati usando un intervallo a incremento esponenziale tra i tentativi di acquisizione di spinlock (fino a un determinato limite), che spesso riduce l'impatto sulle prestazioni della CPU.

Il diagramma seguente offre una rappresentazione concettuale dell'algoritmo di spinlock:

![Rappresentazione concettuale dell'algoritmo di spinlock](./media/diagnose-resolve-spinlock-contention/image5.png)

## <a name="typical-scenarios"></a>Scenari tipici

I conflitti di spinlock possono verificarsi per diversi motivi che potrebbero non essere correlati alle decisioni di progettazione del database. Dato che gli spinlock controllano l'accesso alle strutture di dati interne, i conflitti di spinlock non si manifestano allo stesso modo della contesa di latch del buffer, influenzata direttamente dalle scelte di progettazione dello schema e dai modelli di accesso ai dati.

Il principale sintomo associato ai conflitti di spinlock è un utilizzo della CPU elevato a causa del numero elevato di rotazioni e di numerosi thread che tentano di acquisire lo stesso spinlock. In generale, questa situazione viene osservata nei sistemi con core CPU \>= 24 e più comunemente nei sistemi con core CPU \>= 32. Come affermato in precedenza, un certo livello di conflitti degli spinlock è normale per sistemi OLTP a concorrenza elevata con carico significativo e viene spesso segnalato un numero elevato di rotazioni (miliardi/trilioni) dalla DMV *sys.dm_os_spinlock_stats* nei sistemi in esecuzione da molto tempo. Anche in questo caso, l'osservazione di un numero elevato di rotazioni per un determinato tipo di spinlock non è sufficiente per stabilire che esiste un impatto negativo sulle prestazioni del carico di lavoro.

La presenza di conflitti di spinlock può essere indicata da una combinazione di molti dei sintomi seguenti:

* Viene osservato un numero elevato di rotazioni e di backoff per un particolare tipo di spinlock.

* Il sistema riscontra un utilizzo elevato della CPU o picchi di utilizzo della CPU. Negli scenari di utilizzo elevato della CPU, si notano attese di segnale elevate per SOS_SCHEDULER_YIELD (indicate dalla DMV *sys.dm_os_wait_stats*).

* Il sistema riscontra una concorrenza elevata.

* L'utilizzo della CPU e le rotazioni aumentano in modo sproporzionato rispetto alla velocità effettiva.

   > [!IMPORTANT]
   > Anche se ognuna delle condizioni precedenti è vera, è tuttavia possibile che la causa radice dei problemi di utilizzo elevato della CPU sia un'altra. In realtà, nella maggior parte dei casi l'aumento dell'utilizzo della CPU sarà dovuto a motivi diversi dai conflitti di spinlock. Di seguito sono riportate alcune delle cause più comuni per un maggiore utilizzo della CPU:

* Query che diventano più onerose nel tempo a causa della crescita dei dati sottostanti, determinando la necessità di eseguire letture logiche aggiuntive dei dati residenti in memoria.

* Modifiche nei piani di query con conseguente esecuzione non ottimale.

Se tutte queste condizioni sono vere, eseguire ulteriori indagini sui possibili problemi di conflitto di spinlock.

Un fenomeno comune facile da diagnosticare è una divergenza significativa tra la velocità effettiva e l'utilizzo della CPU. Molti carichi di lavoro OLTP hanno una relazione tra (velocità effettiva/numero di utenti nel sistema) e utilizzo della CPU. Un numero elevato di rotazioni osservato in combinazione con una divergenza significativa di utilizzo della CPU e velocità effettiva può essere un'indicazione di conflitti di spinlock che introducono un sovraccarico della CPU. Un aspetto importante da notare è che è anche comune osservare questo tipo di divergenza nei sistemi quando determinate query diventano più onerose nel tempo. Ad esempio, le query su set di dati che eseguono più letture logiche nel tempo possono causare sintomi simili.

Per la risoluzione di questi tipi di problemi, è fondamentale escludere altre cause più comuni di un utilizzo elevato della CPU.

## <a name="example"></a>Esempio

Nell'esempio seguente esiste una relazione quasi lineare tra l'utilizzo della CPU e la velocità effettiva misurata in termini di transazioni al secondo. È normale osservare un certo livello di divergenza perché il sovraccarico è dovuto a qualsiasi aumento del carico di lavoro. Come illustrato qui, questa divergenza diventa significativa. Quando l'utilizzo della CPU raggiunge il 100%, si verifica anche un calo brusco della velocità effettiva.

![Calo della CPU in Performance Monitor](./media/diagnose-resolve-spinlock-contention/image7.png)

Quando si misura il numero di rotazioni a intervalli di 3 minuti, è possibile osservare un aumento delle rotazioni più esponenziale che lineare e ciò indica che i conflitti di spinlock potrebbero risultare problematici.

![Grafico delle rotazioni a intervalli di 3 minuti](./media/diagnose-resolve-spinlock-contention/image8.png)

Come indicato in precedenza, gli spinlock sono più comuni nei sistemi a concorrenza elevata sottoposti a un carico elevato.

Alcuni degli scenari soggetti a questo problema sono i seguenti:

* Problemi di risoluzione dei nomi causati da un errore nei nomi completi degli oggetti. Per altre informazioni, vedere [Descrizione dei blocchi di SQL Server causati dai blocchi di compilazione](https://support.microsoft.com/help/263889/how-to-troubleshoot-blocking-caused-by-compile-locks). Questo problema specifico viene descritto più dettagliatamente in questo articolo.

* Contesa per i bucket di hash di blocco in Gestione blocchi per i carichi di lavoro che accedono spesso allo stesso blocco, ad esempio un blocco condiviso su una riga letta di frequente. Questo tipo di conflitto emerge come tipo di spinlock LOCK_HASH. In un caso particolare, è stato rilevato che questo problema è emerso come risultato di modelli di accesso definiti in modo non corretto in un ambiente di testing. In questo ambiente, un numero di thread superiore a quello previsto accedeva costantemente alla stessa riga a causa di parametri di test configurati in modo errato.

* Frequenza elevata di transazioni DTC in presenza di un elevato livello di latenza tra i coordinatori delle transazioni MSDTC. Questo problema specifico è documentato in dettaglio nel post di blog del team SQLCAT [Resolving DTC Related Waits and Tuning Scalability of DTC](https://techcommunity.microsoft.com/t5/datacat/resolving-dtc-related-waits-and-tuning-scalability-of-dtc/ba-p/305054) (Risoluzione delle attese correlate a DTC e ottimizzazione della scalabilità di DTC).

## <a name="diagnosing-spinlock-contention"></a>Diagnosi della contesa di spinlock

In questa sezione vengono fornite informazioni per la diagnosi dei conflitti di spinlock di SQL Server. Gli strumenti principali usati per diagnosticare i conflitti di spinlock sono:

| Strumento | Usa |
|---|---|
| **Monitoraggio delle prestazioni** | Cercare condizioni di utilizzo della CPU elevato o divergenze tra la velocità effettiva e l'utilizzo della CPU. |
| **sys.dm_os_spinlock stats DMV** | Cercare un numero elevato di rotazioni e di eventi di backoff in determinati periodi di tempo. |
| **Eventi estesi di SQL Server** | Usato per tenere traccia degli stack di chiamate per gli spinlock che riscontrano un numero elevato di rotazioni. |
| **Dump della memoria** | In alcuni casi, dump della memoria del processo di SQL Server e degli strumenti di debug di Windows. In generale, questo livello di analisi viene eseguito quando vengono coinvolti i team di supporto di Microsoft SQL Server. |

Il processo tecnico generale per la diagnosi dei conflitti di spinlock di SQL Server è il seguente:

1. **Passaggio 1**: determinare se è presente un conflitto che può essere correlato agli spinlock.

2. **Passaggio 2**: acquisire le statistiche da *sys.dm\_ os_spinlock_stats* per trovare il tipo di spinlock per cui si riscontra la maggior parte dei conflitti.

3. **Passaggio 3**: ottenere i simboli di debug per sqlservr.exe (sqlservr.pdb) e inserire i simboli nella stessa directory del file EXE del servizio SQL Server (sqlservr.exe) per l'istanza di SQL Server. Per visualizzare gli stack di chiamate per gli eventi di backoff, è necessario disporre dei simboli per la versione specifica di SQL Server che si sta eseguendo. I simboli per SQL Server sono disponibili nel server dei simboli Microsoft. Per altre informazioni su come scaricare i simboli dal server dei simboli Microsoft, vedere [Debug con i simboli](/windows/win32/dxtecharts/debugging-with-symbols).

4. **Passaggio 4**: usare gli eventi estesi di SQL Server per tenere traccia degli eventi di backoff per i tipi di spinlock di interesse.

Gli eventi estesi offrono la possibilità di tenere traccia dell'evento di \"backoff\" e di acquisire lo stack di chiamate per le operazioni che tentano prevalentemente di ottenere lo spinlock. Analizzando lo stack di chiamate è possibile determinare il tipo di operazione che contribuisce al conflitto per qualsiasi spinlock specifico.

## <a name="diagnostic-walkthrough"></a>Procedura dettagliata di diagnostica

Nella procedura dettagliata seguente viene illustrato come usare vari strumenti e tecniche per diagnosticare un problema di conflitti di spinlock in uno scenario reale. Questa procedura dettagliata si basa sullo scenario di un cliente che esegue un test di benchmark per simulare circa 6.500 utenti simultanei in un server da 8 socket e 64 core fisici con 1 TB di memoria.

### <a name="symptoms"></a>Sintomi

Sono stati osservati picchi periodici di utilizzo della CPU, che è arrivato fino a quasi il 100%. È stata osservata una divergenza tra la velocità effettiva e l'utilizzo della CPU che ha consentito di individuare il problema. Nel momento in cui si è verificato il picco di utilizzo elevato della CPU, è stato rilevato un modello con un numero elevato di rotazioni che si verificavano durante i periodi di utilizzo intensivo della CPU a intervalli specifici.

Questo è un caso estremo in cui i conflitti erano tali da creare una condizione di convoglio degli spinlock. L'effetto convoglio si verifica quando i thread non possono più progredire nella gestione del carico di lavoro, ma dedicano invece tutte le risorse di elaborazione ai tentativi di accedere al blocco. Il log di Performance Monitor illustra questa divergenza tra la velocità effettiva del log delle transazioni e l'utilizzo della CPU e, infine, il picco elevato nell'utilizzo della CPU.

![Picco di utilizzo della CPU in Performance Monitor](./media/diagnose-resolve-spinlock-contention/image9.png)

Dopo aver eseguito una query in *sys.dm_os_spinlock_stats* per determinare la presenza di un conflitto significativo per SOS_CACHESTORE, è stato usato uno script di eventi estesi per misurare il numero di eventi di backoff per i tipi spinlock di interesse.

| Nome | Conflitti | Rotazioni | Rotazioni per conflitto | Backoff |
|---|---|---|---|---|
| **SOS_CACHESTORE** |       14.752.117 |   942.869.471.526 |   63.914 |                67.900.620 |
| **SOS_SUSPEND_QUEUE** |   69.267.367 |   473.760.338.765 |   6\.840  |                2\.167.281 |
| **LOCK_HASH** |           5\.765.761 |    260.885.816.584 |   45.247 |                3\.739.208 |
| **MUTEX** |               2\.802.773 |    9\.767.503.682 |     3\.485  |                350.997 |
| **SOS_SCHEDULER** |       1\.207.007 |    3\.692.845.572 |     3\.060  |                109.746 |

Il modo più semplice per quantificare l'effetto delle rotazioni è controllare il numero degli eventi di backoff esposti da *sys.dm_os_spinlock_stats* nello stesso intervallo di 1 minuto per i tipi di spinlock con il maggior numero di rotazioni. Questo è il metodo migliore per rilevare conflitti significativi, perché indica quando i thread esauriscono il limite di rotazioni in attesa di acquisire lo spinlock. Lo script seguente illustra una tecnica avanzata che usa eventi estesi per misurare gli eventi di backoff correlati e identificare i percorsi di codice specifici in cui si verifica la contesa.

Per altre informazioni sugli eventi estesi in SQL Server, vedere [Introduzione agli eventi estesi di SQL Server](./extended-events/extended-events.md).

**Script**

```sql
/*
This Scriptis provided "AS IS" with no warranties, and confers no rights.

This script will monitor for backoff events over a given period of time and
capture the code paths (callstacks) for those.

--Find the spinlock types
select map_value, map_key, name from sys.dm_xe_map_values
where name = 'spinlock_types'
order by map_value asc

--Example: Get the type value for any given spinlock type
select map_value, map_key, name from sys.dm_xe_map_values
where map_value IN ('SOS_CACHESTORE', 'LOCK_HASH', 'MUTEX')

Examples:
61LOCK_HASH
144 SOS_CACHESTORE
08MUTEX

*/

--create the even session that will capture the callstacks to a bucketizer
--more information is available in this reference: http://msdn.microsoft.com/en-us/library/bb630354.aspx
create event session spin_lock_backoff on server
      add event sqlos.spinlock_backoff (action (package0.callstack)
where 
type = 61--LOCK_HASH
or type = 144--SOS_CACHESTORE
or type = 8--MUTEX
)
      add target package0.asynchronous_bucketizer (
            set filtering_event_name='sqlos.spinlock_backoff',
            source_type=1, source='package0.callstack')
      with (MAX_MEMORY=50MB, MEMORY_PARTITION_MODE = PER_NODE)

--Ensure the session was created
select * from sys.dm_xe_sessions
where name = 'spin_lock_backoff'

--Run this section to measure the contention 
alter event session spin_lock_backoff on server state=start

--wait to measure the number of backoffs over a 1 minute period
waitfor delay '00:01:00'

--To view the data
--1. Ensure the sqlservr.pdb is in the same directory as the sqlservr.exe
--2. Enable this trace flag to turn on symbol resolution 
DBCC traceon (3656, -1)

--Get the callstacks from the bucketize target
select event_session_address, target_name, execution_count, cast (target_data as XML)
from sys.dm_xe_session_targets xst
inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
where xs.name = 'spin_lock_backoff'

--clean up the session 
alter event session spin_lock_backoff on server state=stop
drop event session spin_lock_backoff on server
```

Analizzando l'output, è possibile visualizzare gli stack di chiamate per i percorsi di codice più comuni per le rotazioni SOS_CACHESTORE. Lo script è stato eseguito un paio di volte in momenti diversi in periodi in cui l'utilizzo della CPU era elevato per verificare la coerenza negli stack di chiamate restituiti. Si noti che gli stack di chiamate con il numero più alto di bucket di slot sono comuni tra i due output (35.668 e 8.506). Questi stack di chiamate hanno un "numero di slot" due volte maggiore rispetto alla voce successiva più alta. Questa condizione indica un percorso del codice di interesse.

> [!NOTE]
> Non è insolito che lo script precedente restituisca stack di chiamate. Quando lo script viene eseguito per 1 minuto, si è notato che gli stack di chiamate con un numero di slot > 1000 sono problematici, ma il numero di slot > 10.000 è più probabile che sia problematico perché ha un numero di slot superiore.

> [!NOTE]
> La formattazione dell'output seguente è stata pulita per migliorare la leggibilità.

**Output 1**

```xml
<BucketizerTarget truncated="0" buckets="256">
<Slot count="35668" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep 
      SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch 
      CMEDCatalogOwner::GetOwnerAliasIdFromSid 
      CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey
      CMEDCatalogOwner::GetProxyOwnerBySID
      CMEDProxyDatabase::GetOwnerBySID
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
      NTGroupInfo::`vector deleting destructor'
  </value> 
</Slot>
<Slot count="752" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep 
      SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey             CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
  </value>
  </Slot>
```

**Output 2**

```xml
<BucketizerTarget truncated="0" buckets="256">
<Slot count="8506" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep+c7 [ @ 0+0x0 SpinlockBase::Backoff Spinlock<144,1,0>::SpinToAcquireOptimistic
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get
      NTGroupInfo::`vector deleting destructor'
</value> 
 </Slot>
<Slot count="190" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep
       SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch 
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get
   </value> 
 </Slot>
```

Nell'esempio precedente, gli stack più interessanti hanno il numero di slot più elevato (35.668 e 8.506), che in effetti corrispondono a un numero di slot \> 1000.

A questo punto è lecito chiedersi cosa farne di queste informazioni. In generale, è richiesta una conoscenza approfondita del motore di SQL Server per usare le informazioni sugli stack di chiamate, quindi a questo punto il processo di risoluzione dei problemi entra in un'area grigia. In questo caso specifico, esaminando gli stack di chiamate è possibile osservare che il percorso del codice in cui si verifica il problema è correlato alle ricerche di metadati e sicurezza (come risulta evidente dagli stack frame seguenti **CMEDCatalogOwner::GetProxyOwnerBySID e CMEDProxyDatabase::GetOwnerBySID**).

In una situazione di isolamento è difficile usare queste informazioni per risolvere il problema, ma forniscono alcune idee su dove concentrare l'attenzione di interventi aggiuntivi di risoluzione dei problemi per isolare ulteriormente il problema.

Dato che questo problema è risultato correlato ai percorsi del codice che eseguono controlli correlati alla sicurezza, si è deciso di eseguire un test nel quale l'utente dell'applicazione che si connette al database ha ricevuto i privilegi di sysadmin. Sebbene questa tecnica non sia mai consigliata in un ambiente di produzione, nell'ambiente di test si è rivelata una procedura utile per la risoluzione dei problemi. Eseguendo le sessioni con privilegi elevati (sysadmin), i picchi della CPU correlati ai conflitti sono scomparsi.

## <a name="options-and-workarounds"></a>Opzioni e soluzioni alternative

La risoluzione dei problemi relativi ai conflitti di spinlock può essere palesemente un'attività non semplice. Non esiste un "unico approccio migliore comune". Il primo passaggio per la risoluzione dei problemi relativi alle prestazioni consiste nell'identificare la causa radice. L'uso delle tecniche e degli strumenti descritti in questo articolo è il primo passaggio nell'esecuzione dell'analisi necessaria per comprendere i punti di conflitto correlati agli spinlock.

Man mano che vengono sviluppate nuove versioni di SQL Server, il motore continua a migliorare la scalabilità implementando codice maggiormente ottimizzato per i sistemi a concorrenza elevata. SQL Server ha introdotto numerose ottimizzazioni per i sistemi a concorrenza elevata, una delle quali è il backoff esponenziale per i punti di contesa più comuni. Sono stati introdotti miglioramenti specifici a partire da SQL Server 2012 che migliorano in modo specifico questa particolare area sfruttando gli algoritmi di backoff esponenziali per tutti gli spinlock all'interno del motore.

Quando si progettano applicazioni di fascia alta con requisiti elevatissimi a livello di prestazioni e scalabilità, considerare come mantenere il più breve possibile il percorso del codice necessario in SQL Server. Un percorso del codice più breve significa che il motore di database esegue meno lavoro e si eviteranno naturalmente i punti di conflitto. Molte procedure consigliate hanno l'effetto collaterale di ridurre la quantità di lavoro richiesta del motore, con conseguente ottimizzazione delle prestazioni del carico di lavoro.

Si considerino un paio di procedure consigliate illustrate più indietro in questo articolo a titolo di esempio:

* **Nomi completi:** specificare nomi completi per tutti gli oggetti evita a SQL Server di dover eseguire i percorsi di codice necessari per la risoluzione dei nomi. Sono stati osservati punti di conflitto anche per il tipo di spinlock SOS_CACHESTORE, rilevati quando non si usano nomi completi nelle chiamate alle stored procedure. Se non si specificano nomi completi, SQL Server deve cercare lo schema predefinito per l'utente, con conseguente prolungamento del percorso del codice necessario per eseguire le istruzioni SQL.

* **Query con parametri:** un altro esempio è l'utilizzo di query con parametri e chiamate di stored procedure per ridurre il lavoro necessario per generare i piani di esecuzione. Ancora una volta il risultato è un percorso del codice più breve per l'esecuzione.

* **Contesa di LOCK_HASH:** in alcuni casi, la contesa per determinati conflitti nella struttura dei blocchi o nei bucket di hash è inevitabile. Sebbene il motore di SQL Server partizioni la maggior parte delle strutture di blocco, esistono comunque casi in cui l'acquisizione di un blocco comporta l'accesso allo stesso bucket di hash. Ad esempio, un'applicazione che accede alla stessa riga da molti thread contemporaneamente, ovvero i dati di riferimento. Questi tipi di problemi possono essere affrontati con tecniche di scalabilità orizzontale per questi dati di riferimento all'interno dello schema del database o che sfruttano gli hint NOLOCK quando possibile.

La prima linea di difesa per l'ottimizzazione dei carichi di lavoro SQL Server è sempre costituita dalle procedure di ottimizzazione standard, ad esempio indicizzazione, ottimizzazione delle query, ottimizzazione di I/O e così via. Tuttavia, oltre all'ottimizzazione standard, è importante adottare procedure che consentono di ridurre la quantità di codice necessaria per eseguire le operazioni. Anche quando si seguono le procedure consigliate, è comunque possibile che si verifichino conflitti di spinlock nei sistemi a concorrenza elevata. L'uso degli strumenti e delle tecniche presentati in questo articolo consente di isolare o escludere questi tipi di problemi e determinare quando è necessario coinvolgere le risorse Microsoft appropriate per ottenere assistenza.

Queste tecniche rappresentano sia una metodologia utile per questo tipo di risoluzione dei problemi che un'introduzione ad alcune delle tecniche più avanzate per la profilatura delle prestazioni disponibili con SQL Server.

## <a name="appendix-automate-memory-dump-capture"></a>Appendice: Automatizzare l'acquisizione dei dump della memoria

Lo script di eventi estesi seguente si è rivelato utile per automatizzare la raccolta di dump della memoria quando i conflitti di spinlock diventano significativi. In alcuni casi, i dump della memoria saranno necessari per eseguire una diagnosi completa del problema o verranno richiesti dai team di supporto tecnico Microsoft per eseguire analisi approfondite. In SQL Server 2008 esiste un limite di 16 frame negli stack di chiamate acquisiti dal bucketizer, che potrebbe non garantire una profondità sufficiente per determinare la posizione esatta di ingresso nello stack di chiamate dal motore. SQL Server 2012 ha introdotto miglioramenti aumentando a 32 il numero di frame acquisiti negli stack di chiamate dal bucketizer.

Lo script SQL seguente può essere usato per automatizzare il processo di acquisizione dei dump della memoria per consentire l'analisi dei conflitti di spinlock:

```sql
/*
This script is provided "AS IS" with no warranties, and confers no rights.

Use:    This procedure will monitor for spinlocks with a high number of backoff events
        over a defined time period which would indicate that there is likely significant
        spin lock contention.
        
        Modify the variables noted below before running.


Requires:
        xp_cmdshell to be enabled
            sp_configure 'xp_cmd', 1
            go 
            reconfigure 
            go
        
*********************************************************************************************************/
use tempdb
go 
if object_id('sp_xevent_dump_on_backoffs') is not null
    drop proc sp_xevent_dump_on_backoffs
go 
create proc sp_xevent_dump_on_backoffs
(
    @sqldumper_path                       nvarchar(max)      = '"c:\Program Files\Microsoft SQL Server\100\Shared\SqlDumper.exe"'
    ,@dump_threshold                      int                = 500           --capture mini dump when the slot count for the top bucket exceeds this
    ,@total_delay_time_seconds            int                = 60            --poll for 60 seconds
    ,@PID                                 int                = 0
    ,@output_path                         nvarchar(max)      = 'c:\'
    ,@dump_captured_flag                  int = 0 OUTPUT
    
)
as
/* 
    --Find the spinlock types
    select map_value, map_key, name from sys.dm_xe_map_values
    where name = 'spinlock_types'
    order by map_value asc

    --Example: Get the type value for any given spinlock type
    select map_value, map_key, name from sys.dm_xe_map_values
    where map_value IN ('SOS_CACHESTORE', 'LOCK_HASH', 'MUTEX')
*/
if exists (select * from sys.dm_xe_session_targets xst
                inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
                where xs.name = 'spinlock_backoff_with_dump')
    drop event session spinlock_backoff_with_dump on server

create event session spinlock_backoff_with_dump  on server
      add event sqlos.spinlock_backoff (action (package0.callstack)
            where
                type = 61                 --LOCK_HASH
                --or type = 144           --SOS_CACHESTORE
                --or type = 8             --MUTEX
                --or type = 53            --LOGCACHE_ACCESS
                --or type = 41            --LOGFLUSHQ
                --or type = 25            --SQL_MGR
                --or type = 39            --XDESMGR
                )
      add target package0.asynchronous_bucketizer (
            set filtering_event_name='sqlos.spinlock_backoff',
            source_type=1, source='package0.callstack')
      with (MAX_MEMORY=50MB, MEMORY_PARTITION_MODE = PER_NODE)

alter event session spinlock_backoff_with_dump  on server state=start


declare @instance_name            nvarchar(max) = @@SERVICENAME
declare @loop_count               int = 1
declare @xml_result               xml 
declare @slot_count               bigint 
declare @xp_cmdshell              nvarchar(max) = null

--start polling for the backoffs
print 'Polling for: ' + convert(varchar(32), @total_delay_time_seconds) + ' seconds'
while (@loop_count < CAST (@total_delay_time_seconds/1 as int))
begin 
    waitfor delay '00:00:01'

    --get the xml from the bucketizer for the session
    select @xml_result= CAST(target_data as xml)
    from sys.dm_xe_session_targets xst
        inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
    where xs.name = 'spinlock_backoff_with_dump'
    
    --get the highest slot count from the bucketizer
    select @slot_count = @xml_result.value(N'(//Slot/@count)[1]', 'int')

    --if the slot count is higher than the threshold in the one minute period
    --dump the process and clean up session
    if (@slot_count > @dump_threshold)
    begin 
        print 'exec xp_cmdshell ''' + @sqldumper_path + ' ' + convert(nvarchar(max), @PID) + ' 0 0x800 0 c:\ '''
        select @xp_cmdshell = 'exec xp_cmdshell ''' + @sqldumper_path + ' ' + convert(nvarchar(max), @PID) + ' 0 0x800 0 ' + @output_path + ' '''
        exec sp_executesql @xp_cmdshell
        print 'loop count: ' + convert (varchar(128), @loop_count)
        print 'slot count: ' + convert (varchar(128), @slot_count)
        set @dump_captured_flag = 1
        break
    end 

    --otherwise loop 
    set @loop_count = @loop_count + 1

end

--see what was collected then clean up
DBCC traceon (3656, -1)
select event_session_address, target_name, execution_count, cast (target_data as XML)
from sys.dm_xe_session_targets xst
    inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
where xs.name = 'spinlock_backoff_with_dump'

alter event session spinlock_backoff_with_dump  on server state=stop
drop event session spinlock_backoff_with_dump  on server
go

/* CAPTURE THE DUMPS 
******************************************************************/
--Example: This will run continuously until a dump is created. 
declare @sqldumper_path                nvarchar(max)        = '"c:\Program Files\Microsoft SQL Server\100\Shared\SqlDumper.exe"'
declare @dump_threshold                int                  = 300            --capture mini dump when the slot count for the top bucket exceeds this
declare @total_delay_time_seconds      int                  = 60             --poll for 60 seconds 
declare @PID                           int                  = 0
declare @flag                          tinyint              = 0
declare @dump_count                    tinyint              = 0
declare @max_dumps                     tinyint              = 3              --stop after collecting this many dumps
declare @output_path                   nvarchar(max)        = 'c:\'          --no spaces in the path please :)


--Get the process id for sql server 
declare @error_log table (LogDate datetime,
    ProcessInfo varchar(255),
    Text varchar(max)
    )
insert into @error_log
    exec ('xp_readerrorlog 0, 1, ''Server Process ID''')
select @PID = convert(int, (REPLACE(REPLACE(Text, 'Server Process ID is ', ''), '.', '')))
    from @error_log where Text like ('Server Process ID is%')
print 'SQL Server PID: ' + convert (varchar(6), @PID)

--Loop to monitor the spinlocks and capture dumps. while (@dump_count < @max_dumps)
begin 

    exec sp_xevent_dump_on_backoffs @sqldumper_path             = @sqldumper_path,
                                    @dump_threshold             = @dump_threshold,
                                    @total_delay_time_seconds   = @total_delay_time_seconds,
                                    @PID                        = @PID,
                                    @output_path                = @output_path,
                                    @dump_captured_flag         = @flag OUTPUT
    if (@flag > 0) 
        set @dump_count=@dump_count + 1
    print 'Dump Count: ' + convert(varchar(2), @dump_count)
    waitfor delay '00:00:02'

end
```

## <a name="appendix-capture-spinlock-statistics-over-time"></a>Appendice: Acquisire dati statistici sugli spinlock nel tempo

Lo script seguente può essere usato per esaminare le statistiche degli spinlock in un periodo di tempo specifico. Per ogni esecuzione restituirà le differenze tra i valori correnti e i valori precedenti raccolti.

```sql
/* Snapshot the current spinlock stats and store so that this can be compared over a time period
   Return the statistics between this point in time and the last collection point in time.

   **This data is maintained in tempdb so the connection must persist between each execution**
   **alternatively this could be modified to use a persisted table in tempdb. if that
   is changed code should be included to clean up the table at some point.**
*/

use tempdb
go

declare @current_snap_time    datetime
declare @previous_snap_time   datetime

set @current_snap_time = GETDATE()

if not exists(select name from tempdb.sys.sysobjects where name like '#_spin_waits%')
    create table #_spin_waits
    (
        lock_name    varchar(128)
        ,collisions  bigint
        ,spins       bigint
        ,sleep_time  bigint
        ,backoffs    bigint
        ,snap_time   datetime
    )

--capture the current stats
insert into #_spin_waits
    (
        lock_name
        ,collisions
        ,spins
        ,sleep_time
        ,backoffs
        ,snap_time
        )
        select  name
                ,collisions
                ,spins
                ,sleep_time
                ,backoffs
                ,@current_snap_time
        from sys.dm_os_spinlock_stats

select top 1 @previous_snap_time = snap_time from #_spin_waits
                where snap_time < (select max(snap_time) from #_spin_waits)
                order by snap_time desc

--get delta in the spin locks stats   
select top 10
        spins_current.lock_name
        , (spins_current.collisions - spins_previous.collisions) as collisions
        , (spins_current.spins - spins_previous.spins) as spins
        , (spins_current.sleep_time - spins_previous.sleep_time) as sleep_time
        , (spins_current.backoffs - spins_previous.backoffs) as backoffs
        , spins_previous.snap_time as [start_time]
        , spins_current.snap_time as [end_time]
        , DATEDIFF(ss, @previous_snap_time, @current_snap_time) as [seconds_in_sample]
    from #_spin_waits spins_current
    inner join (
        select * from #_spin_waits
          where snap_time = @previous_snap_time
        ) spins_previous on (spins_previous.lock_name = spins_current.lock_name)
    where
        spins_current.snap_time = @current_snap_time
        and spins_previous.snap_time = @previous_snap_time
        and spins_current.spins > 0
    order by (spins_current.spins - spins_previous.spins) desc

--clean up table
delete from #_spin_waits
where snap_time = @previous_snap_time
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sugli strumenti di monitoraggio delle prestazioni, vedere [Strumenti per il monitoraggio e l'ottimizzazione delle prestazioni](./performance/performance-monitoring-and-tuning-tools.md).
