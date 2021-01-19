---
title: Guida sull'architettura di gestione della memoria | Microsoft Docs
description: Informazioni sull'architettura di gestione della memoria in SQL Server, incluse le modifiche apportate alla gestione della memoria nelle versioni precedenti.
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, memory management architecture
- memory management architecture guide
- PMO
- Partitioned Memory Objects
- cmemthread
- AWE
- SPA, Single Page Allocator
- MPA, Multi Page Allocator
- memory allocation, SQL Server
- memory pressure, SQL Server
- stack size, SQL Server
- buffer manager, SQL Server
- buffer pool, SQL Server
- resource monitor, SQL Server
ms.assetid: 7b0d0988-a3d8-4c25-a276-c1bdba80d6d5
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6af1077d46fa6378e0853eb570ba560d49e6eaec
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171343"
---
# <a name="memory-management-architecture-guide"></a>guida sull'architettura di gestione della memoria

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

## <a name="windows-virtual-memory-manager"></a>Virtual Memory Manager di Windows  
Viene eseguito il mapping delle aree di cui è stato eseguito il commit dello spazio degli indirizzi alla memoria fisica disponibile tramite Virtual Memory Manager (VMM) di Windows.  
  
Per altre informazioni sulla quantità di memoria fisica supportata dai diversi sistemi operativi, vedere la documentazione di Windows relativa ai [limiti di memoria per le versioni di Windows](/windows/desktop/Memory/memory-limits-for-windows-releases).  
  
I sistemi con memoria virtuale consentono il commit in eccesso della memoria fisica, per cui il rapporto tra memoria virtuale e memoria fisica può essere maggiore di 1:1. Di conseguenza, i computer con diverse configurazioni di memoria fisica consentono l'esecuzione di programmi di dimensioni elevate. Tuttavia l'utilizzo di una quantità di memoria virtuale di molto superiore alla combinazione dei set di lavoro medi per tutti i processi determina un peggioramento delle prestazioni. 

## <a name="sql-server-memory-architecture"></a>Architettura della memoria di SQL Server

In[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] la memoria viene acquisita e liberata in modo dinamico in base alle esigenze. In genere non è necessario che un amministratore specifichi la quantità di memoria da allocare a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L'opzione corrispondente, tuttavia, è ancora disponibile e in alcuni ambienti è necessario impostarla.

Uno dei principali obiettivi di progettazione di tutti i software di database è la riduzione del disco I/O dal momento che le letture e le scritture del disco sono le operazioni che consumano più risorse. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] crea un pool di buffer in memoria per contenere le pagine lette dal database. Gran parte del codice in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è dedicata alla riduzione del numero di letture e scritture fisiche tra il disco e il pool di buffer. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tenta di raggiungere un equilibrio tra i due obiettivi:

* Evitare che le dimensioni del pool di buffer aumentino fino a limitare la memoria dell'intero sistema.
* Ridurre al minimo l'I/O fisico sui file di database aumentando la dimensione del pool di buffer fino a raggiungere il valore massimo possibile.

> [!NOTE]
> In un sistema con carichi pesanti, alcune query di grandi dimensioni la cui esecuzione richiede una grande quantità di memoria non possono ottenere la quantità minima di memoria richiesta e ricevono un errore di timeout mentre sono in attesa delle risorse della memoria. Per risolvere il problema, aumentare il valore dell' [opzione query wait](../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Per una query parallela, provare a ridurre l' [opzione Massimo grado di parallelismo](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
 
> [!NOTE]
> In un sistema con carichi elevati e con un numero eccessivo di richieste di memoria, le query con merge join, sort e bitmap nel piano di query possono eliminare il bitmap quando non ottengono la memoria minima necessaria per il bitmap. Ciò può influire sulle prestazioni delle query e, se il processo di ordinamento non può essere contenuto nella memoria, è possibile aumentare l'uso delle tabelle di lavoro nel database tempdb, causando la crescita di tempdb. Per risolvere questo problema, aggiungere memoria fisica o ottimizzare le query per l'uso di un piano di query diverso e più rapido.
 
### <a name="providing-the-maximum-amount-of-memory-to-ssnoversion"></a>Assegnazione della quantità massima di memoria a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Usando AWE e il privilegio Blocco di pagine in memoria, è possibile fornire al motore di database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] le quantità di memoria seguenti. 

> [!NOTE]
> La tabella seguente include una colonna per le versioni a 32 bit non più disponibili.

|Criteri di memoria|32 bit <sup>1</sup> |64 bit|
|-------|-------|-------| 
|Memoria convenzionale |Tutte le edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Fino al limite dello spazio degli indirizzi virtuali di processo: <br>- 2 GB<br>- 3 GB con parametro di avvio /3gb <sup>2</sup> <br>- 4 GB su WOW64 <sup>3</sup> |Tutte le edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Fino al limite dello spazio degli indirizzi virtuali di processo: <br>- 7 TB con architettura IA64 (IA64 non è supportato in [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] e versioni successive)<br>- Valore massimo del sistema operativo con architettura x64 <sup>4</sup>
|Meccanismo AWE (consente a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di superare il limite dello spazio degli indirizzi virtuali di processo nelle piattaforme a 32 bit) |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Edizione Standard, Enterprise e Developer: Il pool di buffer può accedere a un massimo di 64 GB di memoria.|Non applicabile <sup>5</sup> |
|Privilegio Blocco di pagine in memoria del sistema operativo (consente di bloccare la memoria fisica, impedendo il paging della memoria bloccata da parte del sistema operativo). <sup>6</sup> |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Edizione Standard, Enterprise e Developer: Obbligatorio perché il processo [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possa usare il meccanismo AWE. La memoria allocata tramite il meccanismo AWE non può essere trasferita. <br> La concessione di questo privilegio senza l'attivazione di AWE non ha alcun effetto sul server. | Usato solo quando necessario, ovvero in presenza di segnali di page out del processo sqlservr. In questo caso, verrà segnalato l'errore 17890 nel log degli errori, simile a quello riportato nell'esempio seguente:`A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`|

<sup>1</sup> Le versioni a 32 bit non sono disponibili a partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)].  
<sup>2</sup> /3gb è un parametro di avvio del sistema operativo.  
<sup>3</sup> WOW64 (Windows on Windows 64) è una modalità che consente di eseguire la versione a 32 bit di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un sistema operativo a 64 bit.  
<sup>4</sup> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition supporta fino a 128 GB. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition supporta il valore massimo del sistema operativo.  
<sup>5</sup> Si noti che l'opzione sp_configure awe enabled è presente in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]a 64 bit, ma viene ignorata.    
<sup>6</sup> Se viene concesso il privilegio Blocco di pagine in memoria (LPIM), sia su supporto a 32 bit per AWE che a 64 bit indipendente, è consigliabile impostare anche la memoria massima del server. Per altre informazioni su Blocco di pagine in memoria, vedere [Opzioni di configurazione del server Server Memory](../database-engine/configure-windows/server-memory-server-configuration-options.md#lock-pages-in-memory-lpim).

> [!NOTE]
> Le versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] potrebbero essere eseguite in un sistema operativo a 32 bit. L'accesso a più di 4 gigabyte (GB) di memoria in un sistema operativo a 32 bit richiede estensioni AWE (Address Windowing Extensions) per la gestione della memoria. Queste estensioni non sono necessarie quando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è in esecuzione su sistemi operativi a 64 bit. Per altre informazioni su AWE, vedere [Spazio degli indirizzi di processo](/previous-versions/sql/sql-server-2008-r2/ms189334(v=sql.105)) e [Gestione della memoria per database di grandi dimensioni](/previous-versions/sql/sql-server-2008-r2/ms191481(v=sql.105)) nella documentazione di [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)].   

<a name="changes-to-memory-management-starting-2012-11x-gm"></a>

## <a name="changes-to-memory-management-starting-with-sssql11"></a>Modifiche apportate alla gestione della memoria a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]

Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]), l'allocazione di memoria viene gestita con cinque meccanismi diversi:
-  **Allocatore di pagine singole**, che include solo le allocazioni di memoria minori o uguali a 8 KB nel processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le opzioni di configurazione *max server memory (MB)* e *min server memory (MB)* che determinano i limiti di memoria fisica usata dall'allocatore di pagine singole. Il pool di buffer è contemporaneamente il meccanismo per l'allocazione di pagine singole e il maggiore consumer di allocazioni di pagine singole.
-  **Allocatore di più pagine**, per le allocazioni di memoria che richiedono più di 8 KB.
-  **Allocatore CLR**, che include gli heap CLR SQL e le relative allocazioni globali create durante l'inizializzazione di CLR.
-  Allocazioni di memoria per gli **[stack di thread](../relational-databases/memory-management-architecture-guide.md#stacksizes)** nel processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].
-  **Allocazioni di Windows dirette**, per le richieste di allocazione di memoria effettuate direttamente da Windows. Sono incluse le allocazioni per l'utilizzo dell'heap di Windows e le allocazioni virtuali dirette effettuate dai moduli caricati nel processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Alcuni esempi di queste richieste di allocazione di memoria sono le allocazioni da DLL di stored procedure estese, gli oggetti creati tramite procedure di automazione (chiamate sp_OA) e le allocazioni dai provider di server collegati.

A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], le allocazioni di pagine singole, le allocazioni di più pagine e le allocazioni CLR sono tutte consolidate in un **allocatore di pagine di "qualsiasi dimensione"** e sono incluse nei limiti di memoria controllati dalle opzioni di configurazione *max server memory (MB)* e *min server memory (MB)* . Questa modifica introduce capacità di ridimensionamento più accurate per tutti i requisiti di memoria che passano attraverso lo strumento di gestione della memoria di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

> [!IMPORTANT]
> Controllare con attenzione le configurazioni correnti di *max server memory (MB)* e *min server memory (MB)* dopo l'aggiornamento alle versioni da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] a [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]. Infatti, a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], queste configurazioni ora includono e tengono conto di più allocazioni di memoria rispetto alle versioni precedenti. Queste modifiche sono valide sia per le versioni a 32 bit che a 64 bit di [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], sia per le versioni da [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] a [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] a 64 bit.

La tabella seguente indica se un tipo specifico di allocazione di memoria è controllato dalle opzioni di configurazione *max server memory (MB)* e *min server memory (MB)* :

|Tipo di allocazione di memoria| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|
|-------|-------|-------|
|Allocazioni di singole pagine|Sì|Sì, consolidata in allocazioni di pagine "di qualsiasi dimensione"|
|Allocazioni di più pagine|No|Sì, consolidata in allocazioni di pagine "di qualsiasi dimensione"|
|Allocazioni CLR|No|Sì|
|Memoria stack di thread|No|No|
|Allocazioni dirette da Windows|No|No|

A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] potrebbe allocare altra memoria rispetto al valore specificato nell'impostazione max server memory. Questo comportamento può verificarsi quando il valore **_Memoria totale server (KB)_** ha già raggiunto il valore dell'impostazione **_Memoria prevista server (KB)_** (come specificato da max server memory). Se la memoria contigua disponibile è insufficiente per soddisfare le richieste di più pagine di memoria (più di 8 KB) a causa della frammentazione della memoria, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può eseguire l'overcommit anziché rifiutare la richiesta di memoria. 

Non appena viene eseguita questa allocazione, l'attività in background *Monitoraggio risorse* inizia a segnalare a tutti i consumer di memoria di rilasciare la memoria allocata e tenta di portare il valore *Memoria totale server (KB)* al di sotto del valore specificato per *Memoria prevista server (KB)* . Pertanto, l'utilizzo della memoria di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] potrebbe entro breve superare l'impostazione max server memory. In questo caso, la lettura del contatore delle prestazioni *Memoria totale server (KB)* risulterà superiore alle impostazioni max server memory e *Memoria prevista server (KB)* .

Questo comportamento viene in genere osservato durante le operazioni seguenti: 
-  Query su indici Columnstore di grandi dimensioni.
-  Compilazioni o ricompilazioni di indici ColumnStore che usano grandi quantità di memoria per eseguire operazioni di hash e ordinamento.
-  Operazioni di backup che richiedono buffer di memoria di grandi dimensioni.
-  Operazioni di traccia che devono archiviare parametri di input di grandi dimensioni.

<a name="#changes-to-memory-management-starting-with-includesssql11includessssql11-mdmd"></a>
## <a name="changes-to-memory_to_reserve-starting-with-sssql11"></a>Modifiche apportate a "memory_to_reserve" a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]
Nelle versioni precedenti di SQL Server ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]) lo strumento di gestione della memoria di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] riserva parte dello spazio indirizzi virtuali del processo per l'**allocatore di più pagine**, l'**allocatore CLR**, le allocazioni di memoria per gli **stack di thread** nel processo di SQL Server e le **allocazioni di Windows dirette**. Questa parte dello spazio indirizzi virtuali è nota anche come area MemToLeave o "pool non di buffer".

Lo spazio indirizzi virtuali riservato per queste allocazioni varia a seconda dell'opzione di configurazione _**memory\_to\_reserve**_. Il valore predefinito usato da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è 256 MB. Per sostituire il valore predefinito, usare il parametro di avvio [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] *-g*. Fare riferimento alla pagina della documentazione [Opzioni di avvio del servizio del motore di database](../database-engine/configure-windows/database-engine-service-startup-options.md) per informazioni sul parametro di avvio *-g*.

Dato che a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] il nuovo allocatore di pagine "di qualsiasi dimensione" gestisce anche le allocazioni di dimensioni superiori a 8 KB, il valore *memory_to_reserve* non include le allocazioni di più pagine. Ad eccezione di questa modifica, non vi sono altre novità per questa opzione di configurazione.

La tabella seguente indica se un tipo specifico di allocazione di memoria rientra nell'area *memory_to_reserve* dello spazio indirizzi virtuali per il processo [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]:

|Tipo di allocazione di memoria| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| A partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|
|-------|-------|-------|
|Allocazioni di singole pagine|No|No, consolidata in allocazioni di pagine "di qualsiasi dimensione"|
|Allocazioni di più pagine|Sì|No, consolidata in allocazioni di pagine "di qualsiasi dimensione"|
|Allocazioni CLR|Sì|Sì|
|Memoria stack di thread|Sì|Sì|
|Allocazioni dirette da Windows|Sì|Sì|

## <a name="dynamic-memory-management"></a><a name="dynamic-memory-management"></a> Gestione della memoria dinamica
Il comportamento predefinito per la gestione della memoria del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] consiste nell'acquisire la quantità di memoria necessaria senza causare insufficienza di memoria nel sistema. In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] questo comportamento è reso possibile tramite l'utilizzo delle API di notifica della memoria di Microsoft Windows.

Quando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizza la memoria in modo dinamico, esegue query periodiche sul sistema per determinare la quantità di memoria libera disponibile. Il mantenimento di tale memoria libera impedisce il paging del sistema operativo. Se è disponibile una quantità minore di memoria libera, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] rilascia memoria al sistema operativo. Se è disponibile una quantità maggiore di memoria libera, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può essere allocata più memoria. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] aggiunge memoria solo se richiesto dal relativo carico di lavoro. In un server non operativo non vengono aumentate le dimensioni del proprio spazio degli indirizzi virtuali.  
  
**[Max server memory](../database-engine/configure-windows/server-memory-server-configuration-options.md)** controlla l'allocazione di memoria di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la memoria per la compilazione, tutte le cache (incluso il pool di buffer), le [concessioni di memoria per l'esecuzione di query](#effects-of-min-memory-per-query), la [memoria per la gestione blocchi](#memory-used-by-sql-server-objects-specifications) e la memoria CLR <sup>1</sup> (in particolare i clerk di memoria presenti in **[sys.dm_os_memory_clerks](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)** ). 

<sup>1</sup> La memoria CLR viene gestita nelle allocazioni max_server_memory a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].

La query seguente restituisce le informazioni sulla memoria attualmente allocata:  
  
```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
    large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
    locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
    virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
    virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
    virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
    page_fault_count AS sql_page_fault_count,
    memory_utilization_percentage AS sql_memory_utilization_percentage, 
    process_physical_memory_low AS sql_process_physical_memory_low, 
    process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  
 
<a name="stacksizes"></a> La memoria per gli stack di thread <sup>1</sup>, CLR <sup>2</sup>, i file DLL di procedure estese, i provider OLE DB a cui fanno riferimento query distribuite, gli oggetti di automazione con riferimenti nelle istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] e qualsiasi allocazione di memoria eseguita da una DLL non [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]**non** sono controllate da max server memory.

<sup>1</sup> Fare riferimento alla pagina della documentazione [Configurare l'opzione di configurazione del server max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) per informazioni sui thread di lavoro predefiniti calcolati per un determinato numero di CPU per cui è stata impostata l'affinità nell'host corrente. Le dimensioni di stack di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sono le seguenti:

|Architettura di SQL Server|Architettura del sistema operativo|Dimensioni dello stack|  
|--------------------|----------------------|----------------------|
|x86 (32 bit)|x86 (32 bit)|512 KB|
|x86 (32 bit)|x64 (64 bit)|768 KB| 
|x64 (64 bit)|x64 (64 bit)|2048 KB|
|IA64 (Itanium)|IA64 (Itanium)|4096 KB|

<sup>2</sup> La memoria CLR viene gestita nelle allocazioni max_server_memory a partire da [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa l'API di notifica di memoria **QueryMemoryResourceNotification** per determinare i casi in cui è possibile allocare e rilasciare memoria con lo strumento di gestione della memoria di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

All'avvio di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , viene calcolata la dimensione dello spazio degli indirizzi virtuali per il pool di buffer in base a parametri come la quantità di memoria fisica nel sistema, il numero di thread del server e vari parametri di avvio. In[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene riservata la quantità calcolata di spazio degli indirizzi virtuali del processo per il pool di buffer, ma viene acquisita solo la quantità di memoria fisica necessaria (ovvero ne viene eseguito il commit) per il carico corrente.

L'istanza continua quindi ad acquisire la memoria necessaria per supportare il carico di lavoro. Man mano che altri utenti si connettono ed eseguono query, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene acquisita ulteriore memoria fisica su richiesta. La memoria fisica continua a essere acquisita da un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fino a quando viene raggiunto il limite di allocazione definito dall'opzione Memoria massima del server o fino a quando il sistema operativo indica che non è più disponibile memoria in eccesso. La memoria viene liberata quando viene superato il valore dell'impostazione Memoria minima del server e il sistema operativo indica un'insufficienza di memoria disponibile. 

L'avvio di altre applicazioni in un computer in cui viene eseguita un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]comporta l'utilizzo di ulteriore memoria e la quantità di memoria fisica disponibile scende al di sotto del valore di destinazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Nell'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] l'utilizzo della memoria viene regolato automaticamente. Se un'altra applicazione viene arrestata e viene resa disponibile altra memoria, l'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] aumenta le dimensioni della propria allocazione di memoria. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può liberare e acquisire diversi megabyte di memoria al secondo, in modo che possa adattarsi rapidamente alle modifiche dell'allocazione della memoria.

## <a name="effects-of-min-and-max-server-memory"></a>Effetti delle opzioni min server memory e max server memory
Le opzioni di configurazione *min server memory* e *max server memory* stabiliscono il limite massimo e minimo della quantità di memoria usata dal pool di buffer e in altre cache del motore di database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il pool di buffer non acquisisce immediatamente la quantità di memoria specificata in Memoria minima del server. ma solo la memoria necessaria per l'inizializzazione. Con l'aumentare del carico di lavoro da gestire, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] continua ad acquisire la memoria necessaria per supportare il carico di lavoro Il pool di buffer non libera la memoria acquisita fino a quando non viene raggiunta la quantità specificata in Memoria minima del server. Quando viene raggiunto il limite impostato in Memoria minima del server, il pool di buffer usa quindi l'algoritmo standard per acquisire e liberare memoria nella misura necessaria. L'unica differenza è rappresentata dal fatto che nel pool di buffer la quantità di memoria allocata non scende mai al di sotto del livello specificato in Memoria minima del server, né viene mai acquisita una quantità di memoria superiore al livello specificato Memoria massima del server.

> [!NOTE]
> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] come processo acquisisce più memoria rispetto a quella specificata dall'opzione Memoria massima del server. I componenti interni ed esterni possono allocare memoria al di fuori del pool di buffer, richiedendo ulteriore memoria, ma la memoria allocata al pool di buffer in genere rappresenta ancora la parte di memoria maggiore usata da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

La quantità di memoria acquisita da [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] dipende interamente dal carico di lavoro assegnato all'istanza. È possibile che per un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che elabora un numero ridotto di richieste, la quantità di memoria allocata rimanga sempre inferiore al valore di min server memory.

Se si specifica lo stesso valore per min server memory e max server memory, quando la memoria allocata al [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] raggiunge tale valore, il [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] smette di liberare e acquisire dinamicamente memoria per il pool di buffer.

Se un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è in esecuzione in un computer in cui viene spesso avviata o arrestata l'esecuzione di altre applicazioni, è possibile che l'allocazione e la deallocazione della memoria da parte dell'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] rallenti l'avvio delle altre applicazioni. Se [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è solo una delle numerose applicazioni server in esecuzione in un singolo computer, può inoltre essere necessario che l'amministratore di sistema controlli la quantità di memoria allocata a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. In questi casi, è possibile usare le opzioni Memoria minima del server e Memoria massima del server per controllare la quantità di memoria che può essere impiegata da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . I valori delle opzioni **min server memory** e **max server memory** vengono specificati in megabyte. Per altre informazioni, vedere [Opzioni di configurazione del server Server Memory](../database-engine/configure-windows/server-memory-server-configuration-options.md).

## <a name="memory-used-by-sql-server-objects-specifications"></a>Memoria usata dalle specifiche degli oggetti di SQL Server
Nella seguente tabella sono indicate le quantità di memoria approssimative usate da diversi oggetti in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. I valori riportati sono stimati e possono variare a seconda dell'ambiente e delle modalità di creazione degli oggetti:

* Blocco (gestito da Gestione blocchi): 64 byte + 32 byte per proprietario   
* Connessione utente: circa (3 \*network_packet_size + 94 kb)    

**dimensioni_pacchetto_rete** indica le dimensioni dei pacchetti TDS (Tabular Data Scheme) usati per le comunicazioni tra le applicazioni e il motore di database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La dimensione predefinita del pacchetto è 4 KB e viene controllata dall'opzione di configurazione delle dimensioni del pacchetto di rete.

Quando è abilitato MARS (Multiple Active Result Set), la connessione utente corrisponde a circa (3 + 3 \*num_logical_connections)\* network_packet_size + 94 KB.

## <a name="effects-of-min-memory-per-query"></a>Effetti dell'opzione min memory per query
L'opzione di configurazione *min memory per query* consente di specificare la quantità minima di memoria, in kilobyte, che verrà allocata per l'esecuzione di una query. Questa operazione è nota anche come concessione di memoria minima. Tutte le query devono attendere che la memoria minima richiesta venga assegnata, prima che possa iniziare l'esecuzione, o fino a quando non viene superato il valore specificato nell'opzione di configurazione query wait server. Il tipo di attesa accumulato in questo scenario è RESOURCE_SEMAPHORE.

> [!IMPORTANT]
> Non impostare l'opzione di configurazione del server min memory per query su un valore troppo elevato, in particolare nei sistemi molto occupati, perché ciò potrebbe causare:         
> - Una maggiore contesa per le risorse di memoria.         
> - Una riduzione della concorrenza aumentando la quantità di memoria per ogni singola query, anche se la memoria necessaria in fase di esecuzione è inferiore rispetto a questa configurazione.    
>    
> Per consigli sull'uso di questa configurazione, vedere [Configurare l'opzione di configurazione del server min memory per query](../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md#Recommendations).

### <a name="memory-grant-considerations"></a><a name="memory-grant-considerations"></a>Considerazioni sulla concessione di memoria
Per l'**esecuzione in modalità riga** la concessione di memoria iniziale non può essere superata in qualsiasi condizione. Se è necessaria più memoria della concessione iniziale per l'esecuzione di operazioni di **hash** o **ordinamento**, verrà eseguito lo spill su disco. Un'operazione di hash con spill è supportata da un file di lavoro in TempDB, mentre un'operazione di ordinamento con spill è supportata da una [tabella di lavoro](../relational-databases/query-processing-architecture-guide.md#worktables).   

Un evento spill che si verifica durante un'operazione di ordinamento è noto come [avviso di ordinamento](../relational-databases/event-classes/sort-warnings-event-class.md). Gli avvisi di ordinamento indicano operazioni di ordinamento per cui la memoria disponibile risulta insufficiente. Ciò vale soltanto per le operazioni di ordinamento eseguite in una query, ad esempio una clausola `ORDER BY` in un'istruzione `SELECT`, e non per le operazioni di ordinamento che implicano la creazione di indici.

Un evento spill che si verifica durante un'operazione di hash è noto come [avviso di hash](../relational-databases/event-classes/hash-warning-event-class.md). Questo tipo di avviso viene generato quando si verifica una ricorsione di hash o l'interruzione dell'hashing (bailout hash) durante un'operazione di hash.
-  La ricorsione di hash si verifica quando la memoria disponibile non è sufficiente per l'input di compilazione, che viene quindi suddiviso in più partizioni elaborate separatamente. Se la memoria disponibile non è sufficiente per una delle partizioni, la partizione viene suddivisa ulteriormente in sottopartizioni che vengono elaborate separatamente. Il processo di suddivisione continua fino a quando non vengono inserite nella memoria disponibile tutte le partizioni o non viene raggiunto il livello massimo di ricorsione.
-  L'evento hash bailout si verifica quando un'operazione di hashing raggiunge il livello massimo di ricorsione e ricorre a un piano alternativo per l'elaborazione dei dati partizionati rimanenti. Questi eventi possono causare una riduzione delle prestazioni nel server.

Per l'**esecuzione in modalità batch**, la concessione di memoria iniziale può aumentare in modo dinamico fino a una determinata soglia interna per impostazione predefinita. Questo meccanismo di concessione di memoria dinamica è progettato per consentire l'esecuzione residente in memoria di operazioni di **hash** oppure **ordinamento** in modalità batch. Se la memoria risulta ancora insufficiente per queste operazioni, ne verrà eseguito lo spill su disco.

Per altre informazioni sulle modalità di esecuzione, vedere la [Guida sull'architettura di elaborazione delle query](../relational-databases/query-processing-architecture-guide.md#execution-modes).

## <a name="buffer-management"></a>Gestione del buffer
Lo scopo principale di un database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è l'archiviazione e il recupero dei dati. L'esecuzione di una quantità elevata di operazioni di I/O su disco è pertanto una caratteristica fondamentale del motore di database. Poiché le operazioni di I/O nel disco possono utilizzare molte risorse e richiedere un tempo relativamente lungo per il completamento, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene data grande importanza all'efficienza dell'I/O. La gestione del buffer è un elemento chiave per il raggiungimento di tale efficienza. Il componente di gestione del buffer è costituito da due meccanismi, ovvero **Gestione buffer** che consente di accedere alle pagine del database e aggiornarle, e la **cache del buffer**, detta anche **pool di buffer**, che consente di ridurre le operazioni di I/O del file di database. 

### <a name="how-buffer-management-works"></a>Funzionamento di Gestione buffer
Un buffer è una pagina da 8 KB in memoria, ovvero delle stesse dimensioni di una pagina di dati o di indice. La cache del buffer è quindi suddivisa in pagine da 8 KB. Gestione buffer consente di gestire le funzioni per la lettura delle pagine di dati o di indice dai file su disco del database nella cache buffer e la riscrittura sul disco delle pagine modificate. Una pagina rimane nella cache del buffer fino a quando per Gestione buffer non è necessaria l'area del buffer per leggere un maggior numero di dati. I dati vengono riscritti sul disco solo se vengono modificati. I dati nella cache del buffer possono essere modificati più volte prima di venire riscritti sul disco. Per altre informazioni, vedere [Lettura di pagine](../relational-databases/reading-pages.md) e [Scrittura di pagine](../relational-databases/writing-pages.md).

All'avvio di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , viene calcolata la dimensione dello spazio degli indirizzi virtuali per la cache del buffer in base a parametri come la quantità di memoria fisica nel sistema, il numero massimo di thread del server configurati e vari parametri di avvio. In[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene riservata la quantità calcolata di spazio degli indirizzi virtuali del processo per la cache del buffer (chiamata destinazione di memoria), ma viene acquisita solo la quantità di memoria fisica necessaria (ovvero ne viene eseguito il commit) per il carico corrente. È possibile eseguire query sulle colonne **bpool_commit_target** e **bpool_committed columns** nella vista del catalogo [sys.dm_os_sys_info](../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) per restituire il numero di pagine riservate come destinazione di memoria e il numero di pagine su cui attualmente viene eseguito il commit nella cache del buffer, rispettivamente.

L'intervallo tra l'avvio di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e il momento in cui alla cache del buffer viene allocata la memoria massima è detto processo di avvio. Durante questo intervallo le richieste di lettura riempiono i buffer in base alle esigenze. Ad esempio, una richiesta di lettura di una singola pagina da 8 KB riempie una singola pagina del buffer. Questo significa che il processo di avvio dipende dal numero e dal tipo di richieste del client. Il processo di avvio viene reso più rapido mediante la trasformazione delle richieste di lettura di pagina singola in richieste di otto pagine allineate, ovvero un extent. Ciò consente un completamento del processo di avvio molto più rapido, in particolare nei computer con molta memoria. Per altre informazioni su pagine ed extent, vedere [Guida all'architettura di pagine ed extent](../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

Gestione buffer utilizza la maggior parte della memoria nel processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e coopera pertanto con Gestione memoria per consentire agli altri componenti di utilizzare i relativi buffer. Gestione buffer interagisce essenzialmente con i componenti seguenti:

* Strumento di gestione delle risorse, per controllare l'utilizzo globale della memoria e, nelle piattaforme a 32 bit, l'utilizzo dello spazio degli indirizzi.  
* Gestione database e il sistema operativo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (SQLOS) per operazioni di I/O di file di basso livello.  
* Strumento di gestione dei log per la registrazione write-ahead.  

### <a name="supported-features"></a>Funzionalità supportate
Gestione buffer supporta le caratteristiche seguenti:

* Gestione buffer supporta **NUMA (Non-Uniform Memory Access)** . Le pagine della cache buffer vengono distribuite tra i nodi hardware NUMA, consentendo a un thread di accedere a una pagina del buffer allocata nel nodo NUMA locale anziché dalla memoria esterna. 
* Gestione buffer supporta l'**aggiunta di memoria a caldo**, consentendo agli utenti di aggiungere memoria fisica senza riavviare il server. 
* Gestione buffer supporta **pagine di grandi dimensioni** su piattaforme a 64 bit. Le dimensioni delle pagine sono specifiche della versione di Windows utilizzata.

  > [!NOTE]
  > Prima di [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], per abilitare le pagine di grandi dimensioni in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è richiesto il [flag di traccia 834](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  

* In Gestione buffer sono disponibili strumenti di diagnostica aggiuntivi esposti tramite DMV. È possibile utilizzare queste viste per monitorare varie risorse del sistema operativo specifiche di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Ad esempio, è possibile usare la vista [sys.dm_os_buffer_descriptors](../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md) per monitorare le pagine nella cache del buffer.   

### <a name="disk-io"></a>I/O su disco
Gestione buffer esegue solo letture e scritture sul database. Altre operazioni su file e database, ad esempio apertura, chiusura, estensione e compattazione vengono eseguite dai componenti del gestore database e di File Manager. 

Le operazioni di I/O su disco eseguite da Gestione buffer hanno le caratteristiche seguenti:

* Tutti gli I/O vengono eseguiti in modo asincrono. In questo modo, il thread che esegue la chiamata può continuare l'elaborazione mentre l'operazione di I/O viene eseguita in background.
* Tutti gli I/O vengono eseguiti nei thread che eseguono la chiamata a meno che non sia in uso l'opzione affinity I/O. L'opzione affinity I/O mask associa l'I/O su disco di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a un subset di CPU specificato. Negli ambienti [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di fascia alta con elaborazione delle transazioni online (OLTP), questa estensione può migliorare le prestazioni dei thread di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che generano operazioni di I/O.
* Gli I/O di più pagine vengono eseguiti con un I/O non sequenziale, che consente il trasferimento di dati da e verso aree di memoria non contigue. Questo significa che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può riempire o scaricare rapidamente la cache buffer evitando più richieste di I/O fisici. 

#### <a name="long-io-requests"></a>Richieste di I/O lunghi  
Gestione buffer segnala qualsiasi richiesta di I/O che rimane in attesa per almeno 15 secondi. In questo modo, l'amministratore di sistema può distinguere tra problemi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e problemi del sottosistema I/O. Il messaggio di errore 833 viene restituito e visualizzato nel log degli errori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nel modo seguente:

`SQL Server has encountered ## occurrence(s) of I/O requests taking longer than 15 seconds to complete on file [##] in database [##] (#). The OS file handle is 0x00000. The offset of the latest long I/O is: 0x00000.` 

Un I/O lungo può essere un'operazione di lettura o scrittura. Questa indicazione non viene attualmente inclusa nel messaggio. I messaggi relativi a I/O lunghi sono avvisi e non messaggi di errore. Non indicano problemi con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ma con il sistema di I/O sottostante. ma vengono restituiti per consentire all'amministratore di sistema di individuare in modo più rapido la causa dei tempi di risposta lenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , nonché distinguere i problemi esterni al controllo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. I messaggi relativi a I/O lunghi non richiedono quindi alcuna azione. È comunque consigliabile che l'amministratore di sistema verifichi il motivo per il quale per la richiesta di I/O è stato necessario un tempo così prolungato e se esso è giustificato.

#### <a name="causes-of-long-io-requests"></a>Cause delle richieste di I/O lunghi  
Un messaggio di I/O lungo indica che un I/O è bloccato definitivamente e non verrà mai completato (I/O perso) o semplicemente che non è stato ancora completato. Non è possibile capire dal messaggio di quale scenario si tratta, sebbene un I/O perso sia spesso causa di un timeout di latch.

Gli I/O lunghi indicano spesso un carico di lavoro di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] troppo intenso per il sottosistema disco. Le situazioni seguenti possono indicare un sottosistema disco non adeguato:

* Più messaggi di I/O lunghi vengono visualizzati nel log degli errori durante un carico di lavoro elevato di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .
* I contatori Perfmon indicano latenze prolungate del disco, lunghe code del disco o nessun tempo di inattività del disco.  

Gli I/O lunghi possono inoltre essere causati da un componente nel percorso di I/O (ad esempio, un driver, un controller o un firmware) che ritarda continuamente la risposta a una richiesta di I/O precedente privilegiando richieste più recenti, più vicine alla posizione corrente della testina. La tecnica comune di elaborazione delle richieste che dà priorità alle richieste più vicine alla posizione corrente della testina di lettura/scrittura è detta "accodamento dei comandi nativi". Questo può essere difficile da verificare mediante lo strumento Monitor di sistema di Windows (PERFMON.EXE) in quanto la maggioranza degli I/O vengono gestiti immediatamente. Le richieste di I/O lunghi possono essere ulteriormente complicate da carichi di lavoro che eseguono grandi quantità di I/O sequenziali, ad esempio backup e ripristino, analisi delle tabelle, ordinamento, creazione di indici, caricamenti bulk e azzeramento dei file.

Gli I/O lunghi isolati apparentemente non correlati a una delle condizioni precedenti possono essere causati da un problema hardware o di driver. Il log eventi di sistema può contenere un evento correlato che consente di individuare il problema.

### <a name="memory-pressure-detection"></a>Rilevamento dell'utilizzo elevato della memoria
L'utilizzo elevato della memoria è una condizione risultante dall'insufficienza della memoria e può comportare:
- I/O aggiuntivo (ad esempio thread in background del Lazywriter molto attivo)
- Frequenza di ricompilazione maggiore
- Query con esecuzione più prolungata (in presenza di attese di concessione di memoria)
- Cicli di CPU aggiuntivi

Questa situazione può avere cause esterne o interne. Le cause esterne includono:
- La quantità di memoria fisica (RAM) disponibile è bassa. Il sistema deve ricorrere al trimming dei working set dei processi in esecuzione, con un possibile rallentamento generale. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] potrebbe ridurre la destinazione di commit del pool di buffer e avviare trimming più frequenti delle cache interne. 
- La memoria di sistema disponibile complessiva (incluso il file di paging del sistema) è bassa. Ciò può causare errori di allocazione della memoria del sistema, perché non è in grado di eliminare tramite paging la memoria attualmente allocata.
Le cause interne includono:
- Risposta una condizione di utilizzo elevato della memoria esterna, quando [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] imposta soglie massime di utilizzo della memoria inferiori.
- Riduzione manuale delle impostazioni della memoria riducendo l'opzione di configurazione *max server memory*. 
- Modifiche nella distribuzione di memoria dei componenti interni tra le varie cache.

[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] implementa un framework dedicato al rilevamento e alla gestione delle condizioni di utilizzo elevato della memoria, come parte della gestione della memoria dinamica. Questo framework include l'attività in background nota come **Monitoraggio risorse**. L'attività Monitoraggio risorse controlla lo stato degli indicatori di memoria esterna e interna. Quando uno di questi indicatori cambia stato, viene calcolata e trasmessa la notifica corrispondente. Queste notifiche sono messaggi interni da ognuno dei componenti del motore, archiviati nel buffer circolare. 

Due buffer circolari contengono le informazioni rilevanti per la gestione della memoria dinamica: 
- Il buffer circolare di Monitoraggio risorse, che tiene traccia delle attività di Monitoraggio risorse, ad esempio se è stata segnalata o meno una condizione di utilizzo elevato della memoria. Questo buffer circolare contiene informazioni sullo stato dipendenti dalla condizione corrente di *RESOURCE_MEMPHYSICAL_HIGH*, *RESOURCE_MEMPHYSICAL_LOW*, *RESOURCE_MEMPHYSICAL_STEADY* o *RESOURCE_MEMVIRTUAL_LOW*.
- Il buffer circolare del broker di memoria, che contiene i record delle notifiche relative alla memoria per ogni pool di risorse di Resource Governor. Quando viene rilevata una condizione di utilizzo elevato della memoria interna, vengono attivate le notifiche per memoria insufficiente per i componenti che allocano memoria, in modo da attivare azioni appropriate per bilanciare la memoria tra le cache. 

I broker di memoria mantengono monitorato l'utilizzo della memoria da parte di ogni componente e quindi, in base alle informazioni raccolte, viene calcolato il valore ottimale di memoria per ognuno di questi componenti. È presente un set di broker per ogni pool di risorse di Resource Governor. Queste informazioni vengono quindi trasmesse a ognuno dei componenti, che procederanno ad aumentare o ridurre l'utilizzo come richiesto.
Per altre informazioni sui broker di memoria, vedere [sys.dm_os_memory_brokers](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-brokers-transact-sql.md). 

### <a name="error-detection"></a>Rilevamento degli errori  
Per le pagine di database sono disponibili due meccanismi facoltativi, ovvero la protezione delle pagine incomplete e la protezione dei checksum, che consentono di assicurare l'integrità della pagina dal momento in cui viene scritta sul disco fino a quando viene letta di nuovo. Questi meccanismi offrono una modalità indipendente di verifica della correttezza non solo dell'archiviazione dei dati, ma anche di componenti hardware quali controller, driver, cavi e perfino del sistema operativo. La protezione viene aggiunta alla pagina immediatamente prima della scrittura sul disco e viene verificata dopo la lettura della pagina dal disco.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] esegue quattro tentativi per qualsiasi operazione di lettura non riuscita a causa di un errore di checksum, di pagina incompleta o di I/O. Se la lettura viene completata correttamente durante uno di questi tentativi, viene scritto un messaggio nel log degli errori e l'esecuzione del comando che ha attivato la lettura continua. Se tutti i tentativi hanno esito negativo, il comando viene interrotto con il messaggio di errore 824. 

Il tipo di protezione di pagina utilizzato è un attributo del database che contiene la pagina. La protezione dei checksum è la protezione predefinita per i database creati in [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] e versioni successive. Il meccanismo di protezione di pagina viene specificato al momento della creazione del database e può essere modificato usando ALTER DATABASE SET. È possibile determinare l'impostazione corrente per la protezione di pagina eseguendo una query nella colonna *page_verify_option* della vista del catalogo [sys.databases](../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o nella proprietà *IsTornPageDetectionEnabled* della funzione [DATABASEPROPERTYEX](../t-sql/functions/databasepropertyex-transact-sql.md). 

> [!NOTE]
> Se l'impostazione relativa alla protezione di pagina viene modificata, la nuova impostazione non viene immediatamente estesa all'intero database. Il livello di protezione del database corrente viene infatti esteso alle pagine a ogni successiva scrittura. Ciò significa che il database potrebbe essere costituito da pagine con tipi di protezione diversi. 

#### <a name="torn-page-protection"></a>Protezione delle pagine incomplete  
La protezione delle pagine incomplete, disponibile in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000, rappresenta essenzialmente un modo di rilevare i danneggiamenti della pagina dovuti a interruzioni dell'alimentazione. Ad esempio, un'interruzione dell'alimentazione imprevista può fare sì che solo una parte di una pagina venga scritta su disco. Se si usa la protezione delle pagine incomplete, quando la pagina viene scritta su disco, per ogni settore da 512 byte nella pagina di database da 8 kilobyte (KB) uno schema di firma a 2 bit specifico viene archiviato nell'intestazione di pagina del database. In fase di lettura della pagina dal disco, i bit per il rilevamento di pagine incomplete archiviati nell'intestazione della pagina vengono confrontati con le informazioni effettive sui settori della pagina. Lo scherma di firma alterna il binario 01 e 10 con ogni scrittura, pertanto è sempre possibile sapere quando solo una parte dei settori è stata scritta sul disco: se un bit si trova nello stato errato quando la pagina viene successivamente letta, la pagina è stata scritta in modo non corretto e viene rilevata una pagina incompleta. Per il rilevamento delle pagine incomplete vengono utilizzate risorse minime, tuttavia non vengono rilevati tutti gli errori causati da problemi hardware del disco. Per informazioni sull'impostazione del rilevamento delle pagine incomplete, vedere [Opzioni ALTER DATABASE SET &#40; Transact-SQL &#41;](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify).

#### <a name="checksum-protection"></a>Protezione dei checksum  
La protezione dei checksum, introdotta in [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], offre una più solida funzionalità di verifica dell'integrità dei dati. Un checksum viene calcolato per i dati in ogni pagina scritta e archiviato nell'intestazione di pagina. Ogni volta che una pagina con un checksum archiviato viene letta dal disco, il motore di database ricalcola il checksum per i dati della pagina e genera un errore 824 se il nuovo checksum è diverso da quello archiviato. La protezione dei checksum è in grado di intercettare più errori rispetto alla protezione delle pagine incomplete in quanto dipende da ogni byte della pagina. Tuttavia, si tratta di un'operazione a moderato consumo di risorse. Quando il checksum è abilitato, gli errori causati da interruzioni dell'alimentazione e da hardware o firmware difettosi possono essere rilevati ogni volta che Gestione buffer legge una pagina dal disco. Per informazioni sull'impostazione del checksum, vedere [Opzioni ALTER DATABASE SET &#40; Transact-SQL &#41;](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify).

> [!IMPORTANT]
> Quando un utente o un database di sistema viene aggiornato a [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] o versione successiva, il valore di [PAGE_VERIFY](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify), ovvero NONE o TORN_PAGE_DETECTION, rimane invariato. È consigliabile usare CHECKSUM.
> TORN_PAGE_DETECTION può consentire l'utilizzo di un numero più limitato di risorse, ma offre una protezione minore rispetto all'opzione CHECKSUM.

## <a name="understanding-non-uniform-memory-access"></a>Informazioni sull'architettura NUMA (Non-Uniform Memory Access)
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta l'architettura NUMA (Non-Uniform Memory Access) e offre buone prestazioni su hardware NUMA senza alcuna configurazione specifica. Con l'aumentare della velocità del clock e del numero di processori, diventa sempre più difficile ridurre la latenza di memoria necessaria per utilizzare questa ulteriore capacità di elaborazione. Per ovviare a questo problema, i fornitori di hardware offrono cache L3 di grandi dimensioni. Si tratta, tuttavia, di una soluzione soggetta a limiti. L'architettura NUMA offre una soluzione scalabile per questo problema. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è stato progettato per l'uso ottimale in computer basati su NUMA senza che sia necessario apportare alcuna modifica alle applicazioni. Per altre informazioni, vedere [Procedura: Configurare SQL Server per usare Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md).

## <a name="dynamic-partition-of-memory-objects"></a>Partizione dinamica di oggetti memoria
Gli allocatori di heap, detti oggetti memoria in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consentono al [!INCLUDE[ssde_md](../includes/ssde_md.md)] di allocare memoria dall'heap. È possibile monitorarli tramite la DMV [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md). CMemThread è un tipo di oggetto memoria thread-safe che consente allocazioni di memoria simultanee da più thread. Per il monitoraggio corretto, gli oggetti CMemThread si basano sui costrutti di sincronizzazione (mutex) per garantire che solo un singolo thread alla volta aggiorni parti cruciali delle informazioni. 

> [!NOTE]
> Il tipo di oggetto CMemThread viene usato in tutta la codebase di [!INCLUDE[ssde_md](../includes/ssde_md.md)] per molte allocazioni diverse e può essere partizionato a livello globale, per nodo o per CPU.   

Tuttavia, l'uso di mutex può causare contese se molti thread eseguono l'allocazione dallo stesso oggetto memoria in modo estremamente simultaneo. Pertanto, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta il concetto di oggetti memoria partizionati (PMO, Partitioned Memory Objects) e ogni partizione è rappresentata da un singolo oggetto CMemThread. Il partizionamento di un oggetto memoria è definito in modo statico e non può essere modificato dopo la creazione. Poiché i modelli di allocazione della memoria variano notevolmente in base ad aspetti quali l'utilizzo di hardware e memoria, non è possibile stabilire in anticipo il modello di partizionamento perfetto. Nella maggior parte dei casi, l'uso di una singola partizione è sufficiente, ma in alcuni scenari questo può causare contese che possono essere evitate solo con un oggetto memoria con partizionamento elevato. Non è consigliabile partizionare ogni oggetto memoria poiché più partizioni possono causare altre inefficienze e aumentare la frammentazione della memoria.

> [!NOTE]
> Prima di [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], era possibile usare il flag di traccia 8048 per forzare la conversione di un PMO basato su nodo in un PMO basato su CPU. A partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP2 e [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] questo comportamento è dinamico e controllato dal motore.

A partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP2 e [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], il [!INCLUDE[ssde_md](../includes/ssde_md.md)] può rilevare in modo dinamico la contesa per un oggetto CMemThread specifico e innalzare di livello l'oggetto a un'implementazione basata su nodo o su CPU.  L'innalzamento di livello del PMO rimane attivo fino al riavvio del processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La contesa di CMemThread può essere rilevata dalla presenza di attese CMEMTHREAD elevate nella DMV [sys.dm_os_wait_stats](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) e osservando le colonne *contention_factor*, *partition_type* *exclusive_allocations_count* e *waiting_tasks_count* della DMV [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).

## <a name="see-also"></a>Vedere anche
[Opzioni di configurazione del server Server Memory](../database-engine/configure-windows/server-memory-server-configuration-options.md)   
[Lettura di pagine](../relational-databases/reading-pages.md)   
[Scrittura di pagine](../relational-databases/writing-pages.md)   
[Procedura: Configurare SQL Server per usare Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md)   
[Requisiti per l'uso di tabelle ottimizzate per la memoria](../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)   
[Risolvere i problemi di memoria insufficiente con le tabelle ottimizzate per la memoria](../relational-databases/in-memory-oltp/resolve-out-of-memory-issues.md)