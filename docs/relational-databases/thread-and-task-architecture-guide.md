---
title: Guida sull'architettura dei thread e delle attività | Microsoft Docs
description: Informazioni sull'architettura di thread e attività in SQL Server, incluse la pianificazione delle attività, l'aggiunta di CPU a caldo e le procedure consigliate per l'utilizzo di computer con più di 64 CPU.
ms.custom: ''
ms.date: 09/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
- task scheduling
- working threads
- Large Deficit First scheduling
- LDF scheduling
- scheduling, SQL Server
- tasks, SQL Server
- threads, SQL Server
- quantum, SQL Server
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
author: pmasl
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec3a20e10fd625b588c4bae79f73265a8c292324
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793121"
---
# <a name="thread-and-task-architecture-guide"></a>guida sull'architettura dei thread e delle attività
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

## <a name="operating-system-task-scheduling"></a>Pianificazione delle attività del sistema operativo
I thread sono le unità di elaborazione più piccole che possono essere eseguite da un sistema operativo e consentono la separazione della logica dell'applicazione in diversi percorsi di esecuzione simultanei. I thread sono utili quando in applicazioni complesse è necessario eseguire numerose attività simultaneamente. 

Quando esegue un'istanza di un'applicazione, il sistema operativo crea un'unità denominata processo per gestire l'istanza. Il processo dispone di un thread di esecuzione. Si tratta di una serie di istruzioni di programmazione eseguite dal codice dell'applicazione. In un'applicazione semplice con un singolo set di istruzioni che è possibile eseguire in serie, ad esempio, tale set di istruzioni viene gestito come unica **attività** ed è disponibile solo un percorso di esecuzione, o **thread**, per tutta l'applicazione. Nelle applicazioni più complesse è possibile eseguire più **attività** simultaneamente anziché in serie. Un'applicazione può eseguire questa operazione avviando processi separati per ogni attività, operazione che richiede un uso intensivo delle risorse, o avviare thread distinti, caratterizzati da un uso relativamente minore delle risorse. Ogni thread, inoltre, può essere pianificato per l'esecuzione indipendentemente dagli altri thread associati al processo.

I thread consentono alle applicazioni complesse di ottimizzare l'utilizzo di un processore (CPU) anche nel caso di computer con una singola CPU che possono eseguire un solo thread per volta. Se un thread esegue un'operazione che richiede tempi prolungati e non utilizza la CPU, ad esempio un'operazione di lettura o scrittura su disco, può essere eseguito un altro thread fino al completamento della prima operazione. La possibilità di eseguire thread mentre altri thread sono in attesa del completamento di un'operazione consente all'applicazione di ottimizzare l'utilizzo della CPU. Questo vale in particolare per le applicazioni multiutente che eseguono una grande quantità di I/O su disco, ad esempio i server di database. I computer con più CPU possono eseguire contemporaneamente un thread per ogni CPU. Se un computer dispone di otto CPU, ad esempio, può eseguire otto thread simultaneamente.

## <a name="sql-server-task-scheduling"></a>Pianificazione delle attività di SQL Server
Nell'ambito di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], una **richiesta** è la rappresentazione logica di una query o un batch. Una richiesta rappresenta anche le operazioni richieste dai thread di sistema, ad esempio checkpoint o writer di log. Le richieste sono presenti in vari stati nel corso della loro durata e possono accumulare attese quando le risorse necessarie per eseguire la richiesta non sono disponibili, ad esempio [blocchi](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md#locks) o [latch](../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md#latches). Per altre informazioni sugli stati delle richieste, vedere [sys.dm_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).

Un'**attività** rappresenta l'unità di lavoro che deve essere completata per soddisfare la richiesta. È possibile assegnare una o più attività a una singola richiesta. 
-  Le richieste parallele avranno diverse attività attive che vengono eseguite simultaneamente anziché in sequenza, con una **attività padre** (o attività di coordinamento) e più **attività figlio**. Un piano di esecuzione per una richiesta parallela può includere rami seriali, ovvero aree del piano con operatori che non vengono eseguiti in parallelo. L'attività padre è anche responsabile dell'esecuzione degli operatori seriali.
-  Le richieste in serie avranno una sola attività attiva in un determinato momento durante l'esecuzione.     
Le attività esistono in vari stati per tutta la loro durata. Per altre informazioni sugli stati delle attività, vedere [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Le attività in stato SOSPESO sono in attesa delle risorse necessarie per eseguire l'attività e diventare disponibili. Per altre informazioni sulle attività in attesa, vedere [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md).

Un **thread di lavoro** di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], definito anche ruolo di lavoro o semplicemente thread, è una rappresentazione logica di un thread del sistema operativo. Quando si eseguono le **richieste in serie**, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] genera un ruolo di lavoro per eseguire l'attività attiva (1:1). Se si eseguono **richieste parallele** in [modalità riga](../relational-databases/query-processing-architecture-guide.md#execution-modes), [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] assegna un ruolo di lavoro chiamato **thread padre** (o thread di coordinamento) che coordini i ruoli di lavoro figlio responsabili del completamento delle attività ad essi assegnate (1:1). Al thread padre è associata un'attività padre. Il thread padre è il punto di ingresso della richiesta ed esiste anche prima che il motore analizzi una query. Le responsabilità principali del thread padre sono: 
-  Coordinare un'analisi parallela.
-  Avviare i ruoli di lavoro figlio paralleli.
-  Raccogliere le righe dai thread paralleli e inviarle al client.
-  Eseguire aggregazioni locali e globali.    

> [!NOTE]
> Se un piano di query include rami seriali e paralleli, una delle attività parallele sarà responsabile dell'esecuzione del ramo seriale. 

Il numero di thread di lavoro generati per ogni attività dipende da:
-   Se la richiesta era idonea per il parallelismo secondo quanto determinato da Query Optimizer.
-   Qual è [grado di parallelismo (DOP)](../relational-databases/query-processing-architecture-guide.md#DOP) effettivamente disponibile nel sistema in base al carico corrente. Il valore può essere diverso dal DOP stimato, che si basa sulla configurazione del server per il massimo grado di parallelismo (MAXDOP). Ad esempio, la configurazione del server per MAXDOP può essere 8 ma il DOP disponibile al momento dell'esecuzione può essere solo 2 e questo influisce negativamente sulle prestazioni delle query. 

> [!NOTE]
> Il limite **MAXDOP (max degree of parallelism)** viene impostato per attività, non per richiesta. Ciò significa che durante l'esecuzione di una query parallela, una singola richiesta può generare più attività fino al limite MAXDOP e ogni attività userà un solo ruolo di lavoro. Per altre informazioni su MAXDOP, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Un'**utilità di pianificazione**, nota anche come utilità di pianificazione SOS, gestisce i thread di lavoro che richiedono tempo di elaborazione per svolgere il lavoro per conto delle attività. Ogni utilità di pianificazione viene mappata a un singolo processore (CPU). Il tempo in cui un ruolo di lavoro può rimanere attivo in un'utilità di pianificazione è denominato quantum del sistema operativo, con un massimo di 4 ms. Scaduto il tempo del quantum, un ruolo di lavoro cede il proprio tempo ad altri ruoli di lavoro che devono accedere alle risorse della CPU e modifica il proprio stato. Questa cooperazione tra ruoli di lavoro per ottimizzare l'accesso alle risorse della CPU è denominata **pianificazione cooperativa**, nota anche come pianificazione non preemptive. A sua volta, la modifica dello stato del ruolo di lavoro viene propagata all'attività associata a tale ruolo e alla richiesta associata all'attività. Per altre informazioni sugli stati dei ruoli di lavoro, vedere [sys.dm_os_workers](../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md). Per altre informazioni sulle utilità di pianificazione, vedere [sys.dm_os_schedulers](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 

In breve, una **richiesta** può generare una o più **attività** per eseguire unità di lavoro. Ogni attività viene assegnata a un **thread di lavoro** responsabile del completamento dell'attività. Ogni thread di lavoro deve essere pianificato (inserito in un'**utilità di pianificazione**) per l'esecuzione attiva dell'attività. 

> [!NOTE]
> Si consideri lo scenario seguente:   
> -  Worker 1 è un'attività a esecuzione prolungata, ad esempio una query di lettura che utilizza read-ahead su tabelle basate su disco. ThreadDiLavoro 1 scopre che le pagine di dati richieste sono già disponibili nel pool di buffer, quindi non deve cedere il proprio tempo per attendere le operazioni di I/O e può utilizzare il quantum completo prima di cedere.   
> -  ThreadDiLavoro 2 esegue attività di minore durata inferiori al millisecondo e pertanto deve cedere il tempo prima dell'esaurimento del quantum completo.     
>
> In questo scenario e fino a [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], ThreadDiLavoro 1 può fondamentalmente monopolizzare l'utilità di pianificazione avendo più tempo del quantum in generale.   
> A partire da [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], la pianificazione cooperativa include la pianificazione LDF (Large Deficit First). Con la pianificazione LDF, i modelli di utilizzo del quantum sono monitorati e un solo thread di lavoro non monopolizza un'utilità di pianificazione. Nello stesso scenario, ThreadDiLavoro 2 può utilizzare quantum ripetuti prima che a ThreadDiLavoro 1 vengano concessi ulteriori quantum, impedendo quindi a ThreadDiLavoro 1 di monopolizzare l'utilità di pianificazione con un modello non appropriato.

### <a name="scheduling-parallel-tasks"></a>Pianificazione delle attività in parallelo
Si supponga che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sia configurato con MaxDOP 8 e l'affinità di CPU sia configurata per 24 CPU (utilità di pianificazione) tra i nodi NUMA 0 e 1. Le utilità di pianificazione da 0 a 11 appartengono al nodo NUMA 0, le utilità di pianificazione da 12 a 23 appartengono al nodo NUMA 1. Un'applicazione invia la query seguente (richiesta) al [!INCLUDE[ssde_md](../includes/ssde_md.md)]:

```sql
SELECT h.SalesOrderID, h.OrderDate, h.DueDate, h.ShipDate
FROM Sales.SalesOrderHeaderBulk AS h 
INNER JOIN Sales.SalesOrderDetailBulk AS d ON h.SalesOrderID = d.SalesOrderID 
WHERE (h.OrderDate >= '2014-3-28 00:00:00');
```

> [!TIP]
> La query di esempio può essere eseguita usando il [database di esempio AdventureWorks2016_EXT](../samples/adventureworks-install-configure.md). Le tabelle `Sales.SalesOrderHeader` e `Sales.SalesOrderDetail` sono state ingrandite 50 volte e rinominate `Sales.SalesOrderHeaderBulk` e `Sales.SalesOrderDetailBulk`.

Il piano di esecuzione mostra un [hash join](../relational-databases/performance/joins.md#hash) tra due tabelle e ognuno degli operatori eseguiti in parallelo, come indicato dal cerchio giallo con due frecce. Ogni operatore Parallelism è un ramo diverso del piano. Di conseguenza, nel piano di esecuzione riportato di seguito sono presenti tre rami. 

![Piano di query parallela](../relational-databases/media/schedule-parallel-query-plan.png)

> [!NOTE]
> Se si pensa a un piano di esecuzione come a un albero, un **ramo** è un'area del piano che raggruppa uno o più operatori tra gli operatori Parallelism, chiamati anche iteratori di Exchange. Per altre informazioni sugli operatori del piano, vedere [Guida di riferimento a operatori Showplan logici e fisici](../relational-databases/showplan-logical-and-physical-operators-reference.md). 

Sebbene esistano tre rami nel piano di esecuzione, in qualsiasi momento durante l'esecuzione è possibile eseguire contemporaneamente solo due rami nel piano di esecuzione:
1.  Il ramo in cui viene usato un operatore *Clustered Index Scan* in `Sales.SalesOrderHeaderBulk` (input di compilazione del join) viene eseguito da solo.
2.  Quindi, il ramo in cui viene usato un operatore *Clustered Index Scan* in `Sales.SalesOrderDetailBulk` (input probe del join) viene eseguito contemporaneamente al ramo in cui è stato creato *Bitmap* e in cui è attualmente in esecuzione *Hash Match*.

Showplan XML mostra che 16 thread di lavoro sono stati prenotati e usati nel nodo NUMA 0:

```xml
<ThreadStat Branches="2" UsedThreads="16">
  <ThreadReservation NodeId="0" ReservedThreads="16" />
</ThreadStat>
```

La prenotazione dei thread garantisce che [!INCLUDE[ssde_md](../includes/ssde_md.md)] disponga di un numero sufficiente di thread di lavoro per eseguire tutte le attività necessarie per la richiesta. I thread possono essere prenotati in diversi nodi NUMA oppure in un solo nodo NUMA. La prenotazione del thread viene eseguita durante il runtime prima dell'avvio dell'esecuzione e dipende dal carico dell'utilità di pianificazione. Il numero di thread di lavoro prenotati viene derivato genericamente dalla formula ***rami simultanei* * *DOP del runtime*** ed esclude il thread di lavoro padre. Ogni ramo è limitato a un numero di thread di lavoro uguale a MaxDOP. In questo esempio sono presenti due rami simultanei e MaxDOP è impostato su 8, ovvero **2 * 8 = 16**.

Per riferimento, osservare il piano di esecuzione in tempo reale da [Statistiche query dinamiche](../relational-databases/performance/live-query-statistics.md) dove un ramo è stato completato e due rami sono in esecuzione simultaneamente.

![Piano di query parallela dinamica](../relational-databases/media/schedule-parallel-query-live-plan.png)

Il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] assegnerà un thread di lavoro per eseguire un'attività attiva (1:1) che può essere osservata durante l'esecuzione di query eseguendo una query nella DMV [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md), come illustrato nell'esempio seguente:

```sql
SELECT parent_task_address, task_address, 
       task_state, scheduler_id, worker_address
FROM sys.dm_os_tasks
WHERE session_id = <insert_session_id>
ORDER BY parent_task_address, scheduler_id;
```

> [!TIP]
> La colonna `parent_task_address` è sempre NULL per l'attività padre. 

> [!TIP]
> In un [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] occupato è possibile osservare un numero di attività attive che supera il limite impostato dai thread prenotati. Queste attività possono appartenere a un ramo che non viene più usato e si trovano in uno stato temporaneo in attesa della pulizia. 

[!INCLUDE[ssResult](../includes/ssresult-md.md)] Si noti che sono presenti 17 attività attive per i rami attualmente in esecuzione: 16 attività figlio corrispondenti ai thread prenotati, a cui si aggiunge l'attività padre o l'attività di coordinamento.

|parent_task_address|task_address|task_state|scheduler_id|worker_address|
|--------|--------|--------|--------|--------|
|NULL|**0x000001EF4758ACA8**|SUSPENDED|3|0x000001EFE6CB6160|
|0x000001EF4758ACA8|0x000001EFE43F3468|SUSPENDED|0|0x000001EF6DB70160|
|0x000001EF4758ACA8|0x000001EEB243A4E8|SUSPENDED|0|0x000001EF6DB7A160|
|0x000001EF4758ACA8|0x000001EC86251468|SUSPENDED|5|0x000001EEC05E8160|
|0x000001EF4758ACA8|0x000001EFE3023468|SUSPENDED|5|0x000001EF6B46A160|
|0x000001EF4758ACA8|0x000001EFE3AF1468|SUSPENDED|6|0x000001EF6BD38160|
|0x000001EF4758ACA8|0x000001EFE4AFCCA8|SUSPENDED|6|0x000001EF6ACB4160|
|0x000001EF4758ACA8|0x000001EFDE043848|SUSPENDED|7|0x000001EEA18C2160|
|0x000001EF4758ACA8|0x000001EF69038108|SUSPENDED|7|0x000001EF6AEBA160|
|0x000001EF4758ACA8|0x000001EFCFDD8CA8|SUSPENDED|8|0x000001EFCB6F0160|
|0x000001EF4758ACA8|0x000001EFCFDD88C8|SUSPENDED|8|0x000001EF6DC46160|
|0x000001EF4758ACA8|0x000001EFBCC54108|SUSPENDED|9|0x000001EFCB886160|
|0x000001EF4758ACA8|0x000001EC86279468|SUSPENDED|9|0x000001EF6DE08160|
|0x000001EF4758ACA8|0x000001EFDE901848|SUSPENDED|10|0x000001EFF56E0160|
|0x000001EF4758ACA8|0x000001EF6DB32108|SUSPENDED|10|0x000001EFCC3D0160|
|0x000001EF4758ACA8|0x000001EC8628D468|SUSPENDED|11|0x000001EFBFA4A160|
|0x000001EF4758ACA8|0x000001EFBD3A1C28|SUSPENDED|11|0x000001EF6BD72160|

Osservare che a ognuna delle 16 attività figlio è stato assegnato un thread di lavoro diverso (visibile nella colonna `worker_address`), ma tutti i ruoli di lavoro sono assegnati allo stesso pool di otto utilità di pianificazione (0, 5, 6, 7, 8, 9, 10, 11) e l'attività padre è assegnata a un'utilità di pianificazione all'esterno del pool (3).

> [!IMPORTANT]
> Dopo aver pianificato il primo set di attività parallele in un determinato ramo, il [!INCLUDE[ssde_md](../includes/ssde_md.md)] userà lo stesso pool di utilità di pianificazione per eventuali attività aggiuntive in altri rami. Ciò significa che lo stesso set di utilità di pianificazione verrà usato per tutte le attività parallele nell'intero piano di esecuzione, limitato solo da MaxDOP.  
> [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tenterà sempre di assegnare le utilità di pianificazione dallo stesso nodo NUMA per l'esecuzione delle attività e di assegnarle in sequenza (in modalità round robin), se le utilità di pianificazione sono disponibili. Tuttavia, il thread di lavoro assegnato all'attività padre può trovarsi in un nodo NUMA diverso rispetto alle altre attività.

Un thread di lavoro può rimanere attivo nell'utilità di pianificazione solo per la durata del quantum (4 ms) e deve restituire l'utilità di pianificazione dopo che il quantum è trascorso, in modo che un thread di lavoro assegnato a un'altra attività possa diventare attivo. Quando il quantum del processo di lavoro scade e non è più attivo, l'attività corrispondente viene inserita in una coda FIFO con stato RUNNABLE fino a quando non torna allo stato RUNNING, a condizione che l'attività non richieda l'accesso a risorse non disponibili, ad esempio un latch o un blocco. In tal caso l'attività passa allo stato SUSPENDED anziché allo stato RUNNABLE fino a quando le risorse non diventano disponibili.  

> [!TIP] 
> Per l'output della DMV descritto in precedenza, tutte le attività attive hanno lo stato SUSPENDED. Per informazioni dettagliate sulle attività in attesa, è possibile eseguire una query nella DMV [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md). 

In breve, una richiesta parallela genererà più attività. Ogni attività deve essere assegnata a un unico thread di lavoro. Ogni thread di lavoro deve essere assegnato a un' unica utilità di pianificazione. Il numero di utilità di pianificazione in uso non può quindi superare il numero di attività parallele per ramo, impostato dall'hint per la query o dalla configurazione di MaxDOP. Il thread di coordinamento non contribuisce al limite MaxDOP. 

### <a name="allocating-threads-to-a-cpu"></a>Allocazione di thread a una CPU
Per impostazione predefinita, ogni istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avvia un singolo thread e il sistema operativo distribuisce i thread delle istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tra i processori (CPU) di un computer in base al carico. Se è stata abilitata l'affinità del processo a livello di sistema operativo, il sistema operativo assegna ogni thread a una CPU specifica. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] assegna invece **thread di lavoro** di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] alle **utilità di pianificazione** che distribuiscono uniformemente i thread fra le CPU, in modalità round robin.
    
Per eseguire il multitasking, ad esempio quando più applicazioni accedono allo stesso gruppo di CPU, in certi casi il sistema operativo distribuisce i thread di lavoro tra CPU diverse. Sebbene in questo modo venga garantita una maggiore efficienza del sistema operativo, questa attività può comportare una riduzione delle prestazioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nel caso di carichi di lavoro elevati, poiché la cache di ogni processore viene ricaricata più volte con dati. L'assegnazione di CPU a thread specifici consente di migliorare le prestazioni, poiché le operazioni di ricaricamento dei processori vengono eliminate e si riduce la migrazione dei thread tra CPU, limitando lo scambio di contesto. Questo tipo di associazione tra un thread e un processore è definito "affinità processori". Se è stata abilitata l'affinità, il sistema operativo assegna ogni thread a una CPU specifica. 

L'[opzione affinity mask](../database-engine/configure-windows/affinity-mask-server-configuration-option.md) viene impostata tramite [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md). Quando la maschera di affinità non è impostata, l'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] alloca uniformemente i thread di lavoro fra le utilità di pianificazione che non sono state escluse.

> [!CAUTION]
> Evitare di configurare l'affinità CPU nel sistema operativo e contemporaneamente l'opzione affinity mask in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le due impostazioni mirano a ottenere lo stesso risultato e, se le configurazioni sono incoerenti, potrebbero causare risultati imprevisti. Per altre informazioni, vedere [Opzione di configurazione del server affinity mask](../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

La creazione di un pool di thread consente di ottimizzare le prestazioni quando al server è connesso un numero elevato di client. In genere viene creato un thread del sistema operativo distinto per ogni richiesta di query. In presenza, tuttavia, di centinaia di connessioni al server, l'utilizzo di un thread per ogni richiesta di query può occupare quantità elevate di risorse di sistema. L'[opzione max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) consente a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di creare di un pool di thread di lavoro per soddisfare una maggior quantità di richieste di query e quindi migliorare le prestazioni. 

### <a name="using-the-lightweight-pooling-option"></a>Utilizzo dell'opzione lightweight pooling
Lo scambio del contesto dei thread non produce in genere un overhead molto elevato. Per la maggior parte delle istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non si verificano differenze di prestazioni tra l'impostazione dell'opzione lightweight pooling su 0 o su 1. Le uniche istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che possono trarre vantaggio dall'opzione [lightweight pooling](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) sono quelle in esecuzione in un computer con le caratteristiche seguenti:    
* Server di grandi dimensioni con più CPU
* Tutte le CPU in esecuzione a una capacità prossima al massimo
* Viene eseguita un'intensa attività di cambio di contesto

Le prestazioni di questi sistemi potrebbero migliorare impostando il valore dell'opzione lightweight pooling su 1.

> [!IMPORTANT]
> Evitare di usare la modalità fiber per operazioni di routine. Questo può causare una riduzione delle prestazioni, limitando i vantaggi normali del cambio di contesto, per il fatto che alcuni componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non funzionano correttamente in modalità fiber. Per altre informazioni, vedere [lightweight pooling](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

## <a name="thread-and-fiber-execution"></a>Thread ed esecuzione in modalità fiber
Microsoft Windows utilizza un sistema a priorità numerica che utilizza intervalli compresi tra 1 e 31 per la pianificazione dei thread per l'esecuzione. Lo zero è riservato all'utilizzo da parte del sistema operativo. Quando più thread sono in attesa di esecuzione, Windows esegue il dispatch del thread con la priorità più alta.

Per impostazione predefinita, ogni istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ha priorità 7, che è considerata la priorità normale. In questo modo ai thread di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene assegnata una priorità sufficientemente alta per ottenere le risorse della CPU necessarie senza produrre effetti negativi sulle altre applicazioni. 

> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../includes/ssnotedepfuturedontuse-md.md)]  

È possibile usare l'opzione di configurazione [priority boost](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) per aumentare il livello di priorità dei thread di un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fino a 13 (alta priorità). Questa impostazione consente di assegnare ai thread di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] una priorità maggiore di quella della maggior parte delle altre applicazioni. Di conseguenza, l'invio dei thread di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene eseguito non appena è possibile eseguire i thread e non sarà preceduto dai thread di altre applicazioni. Le prestazioni vengono migliorate se nel server sono in esecuzione solo istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e non altre applicazioni. Tuttavia, se in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene eseguita un'operazione che impegna notevolmente la memoria, è probabile che la priorità delle altre applicazioni non sia maggiore di quella del thread di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

Se vengono eseguite più istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nello stesso computer e solo per alcune è impostata l'opzione priority boost, le prestazioni delle istanze in esecuzione con priorità normale potrebbero risentirne. Le prestazioni delle altre applicazioni e degli altri componenti sul server possono ridursi se priority boost è abilitato. Pertanto, è consigliabile utilizzarlo solo in condizioni assolutamente controllate.


## <a name="hot-add-cpu"></a>Aggiunta di CPU a caldo
Per aggiunta di CPU a caldo si intende la possibilità di aggiungere CPU a un sistema in esecuzione in modo dinamico. L'aggiunta di CPU può verificarsi fisicamente tramite l'aggiunta di nuovi componenti hardware, in modo logico tramite il partizionamento hardware online o virtualmente tramite un livello di virtualizzazione. A partire da [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)], in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è supportata l'aggiunta di CPU a caldo.

Requisiti per l'aggiunta di CPU a caldo:  
* È necessario disporre di hardware che supporta l'aggiunta di CPU a caldo.
* È necessario disporre della versione a 64 bit di Windows Server 2008 Datacenter o del sistema operativo Windows Server 2008 Enterprise Edition per sistemi con processore Itanium.
* Richiede [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition.
* Non è possibile configurare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per l'utilizzo di Soft-NUMA. Per altre informazioni su Soft-NUMA, vedere [Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md).

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non inizia automaticamente a utilizzare le CPU aggiunte. In tal modo si evita che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizzi CPU che potrebbero essere state aggiunte per altri scopi. Dopo aver aggiunto le CPU eseguire l'istruzione [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md) per consentire a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di riconoscere le nuove CPU come risorse disponibili.

> [!NOTE]
> Se è configurata l'opzione [affinity64 mask](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) , sarà necessario modificarla per consentire l'uso delle nuove CPU.
 
## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>Procedure consigliate per l'esecuzione di SQL Server in computer che hanno più di 64 CPU

### <a name="assigning-hardware-threads-with-cpus"></a>Assegnazione di thread di hardware alle CPU
Non usare le opzioni di configurazione del server affinity mask e affinity64 mask per associare processori a thread specifici. Queste opzioni sono limitate a 64 CPU. Usare invece l'opzione `SET PROCESS AFFINITY` di [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md).

### <a name="managing-the-transaction-log-file-size"></a>Gestione delle dimensioni del file del log delle transazioni
Non utilizzare l'aumento automatico per aumentare le dimensioni del file del log delle transazioni. L'aumento del log delle transazioni deve essere eseguito tramite un processo seriale. L'estensione del log può impedire il proseguimento delle operazioni di scrittura della transazioni fino al suo completamento. Preallocare invece lo spazio per i file di log impostando le dimensioni dei file su un valore sufficientemente elevato per supportare il tipico carico di lavoro nell'ambiente.

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>Impostazione dell'opzione max degree of parallelism per le operazioni sugli indici
Le prestazioni delle operazioni sugli indici, quali la creazione o la ricompilazione degli indici, possono essere ottimizzate nei computer dotati di molte CPU impostando temporaneamente il modello di recupero del database sul modello con registrazione minima delle operazioni bulk o sul modello con registrazione minima. Queste operazioni sugli indici possono generare attività del log significative e le contese relative al log possono influire sul grado di parallelismo selezionato in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Oltre a modificare l'opzione di configurazione del server **max degree of parallelism (MAXDOP)** , valutare la possibilità di adeguare il parallelismo per le operazioni sugli indici usando l'[opzione MAXDOP](../t-sql/statements/alter-index-transact-sql.md). Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../relational-databases/indexes/configure-parallel-index-operations.md). Per altre informazioni e istruzioni relative alla modifica dell'opzione di configurazione del server max degree of parallelism, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

### <a name="setting-the-maximum-number-of-worker-threads"></a>Impostazione del numero massimo di thread di lavoro
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] configurerà in modo dinamico l'opzione di configurazione del server **max worker threads** all'avvio. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa il numero di CPU disponibili e l'architettura di sistema per determinare questa configurazione del server nella fase di avvio, usando una [formula](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md#Recommendations) documentata.

Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti con certificazione per SQL Server. Se si sospetta la presenza di un problema di prestazioni, è poco probabile che si tratti della disponibilità dei thread di lavoro. È più probabile che la causa sia legata a I/O che determina l'attesa dei thread di lavoro. È consigliabile individuare la causa radice di un problema di prestazioni prima di modificare l'impostazione max worker threads. Tuttavia, se è necessario impostare manualmente il numero massimo di thread di lavoro, questo valore di configurazione deve essere sempre impostato su un valore che sia almeno sette volte il numero di CPU presenti nel sistema. Per altre informazioni, vedere [Configurare l'opzione max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

### <a name="using-sql-trace-and-sql-server-profiler"></a>Utilizzo di Traccia SQL e SQL Server Profiler
L'uso di Traccia SQL e SQL Server Profiler in un ambiente di produzione è sconsigliato. L'overhead per l'esecuzione di questi strumenti, inoltre, aumenta in funzione del numero di CPU. Se è necessario utilizzare Traccia SQL in un ambiente di produzione, ridurre al minimo il numero di eventi di traccia. Profilare e testare accuratamente ogni evento di traccia soggetto a carico ed evitare di utilizzare combinazioni di eventi che influiscono notevolmente sulle prestazioni.

> [!IMPORTANT]
> Traccia SQL e [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] sono deprecati. Anche lo spazio dei nomi *Microsoft.SqlServer.Management.Trace* che contiene gli oggetti Trace e Replay di Microsoft SQL Server è deprecato. 
> [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] 
> In alternativa, usare Eventi estesi. Per altre informazioni sugli [Eventi estesi](../relational-databases/extended-events/extended-events.md), vedere [Avvio rapido: Eventi estesi in SQL Server](../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) e [Profiler XEvent di SQL Server Management Studio](../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] per i carichi di lavoro Analysis Services NON è deprecato e continuerà a essere supportato.

### <a name="setting-the-number-of-tempdb-data-files"></a>Impostazione del numero di file di dati tempdb
Il numero di file dipende dal numero di processori (logici) del computer. In generale, se il numero di processori logici è minore o uguale a otto, usare un numero di file di dati pari al numero dei processori logici. Se il numero di processori logici è maggiore di otto, usare otto file di dati e, se la contesa persiste, aumentare il numero di file di dati per multipli di 4 fino a quando la contesa si riduce a livelli accettabili o modificare il carico di lavoro o il codice. Tenere anche presenti altre raccomandazioni per tempdb, disponibili in [Ottimizzazione delle prestazioni di tempdb in SQL Server](../relational-databases/databases/tempdb-database.md#optimizing-tempdb-performance-in-sql-server). 

Tuttavia, considerando attentamente le esigenze di concorrenza dei file tempdb, è possibile ridurre il sovraccarico della gestione del database. Ad esempio, se un sistema dispone di 64 CPU e solo 32 query utilizzano in genere file tempdb, aumentando il numero di file tempdb a 64 le prestazioni non miglioreranno.

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>Componenti di SQL Server che possono usare più di 64 CPU
La tabella seguente elenca i componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] specifica se possono usare più di 64 CPU.

|Nome del processo   |Programma eseguibile |Utilizza più di 64 CPU |  
|----------|----------|----------|  
|Motore di database di SQL Server |Sqlserver.exe  |Sì |  
|Reporting Services |Rs.exe |No |  
|Analysis Services  |As.exe |No |  
|Integration Services   |Is.exe |No |  
|Broker di servizio |Sb.exe |No |  
|Ricerca full-text   |Fts.exe    |No |  
|SQL Server Agent   |Sqlagent.exe   |No |  
|SQL Server Management Studio   |Ssms.exe   |No |  
|Installazione di SQL Server   |Setup.exe  |No |  
