---
description: Miglioramento delle prestazioni di indici full-text
title: Migliorare le prestazioni degli indici full-text | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- performance [SQL Server], full-text search
- full-text queries [SQL Server], performance
- crawls [full-text search]
- full-text indexes [SQL Server], performance
- full-text search [SQL Server], performance
- batches [SQL Server], full-text search
ms.assetid: ef39ef1f-f0b7-4582-8e9c-31d4bd0ad35d
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2ec5532f22f50258334f815d12202eb4645b4b17
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241852"
---
# <a name="improve-the-performance-of-full-text-indexes"></a>Miglioramento delle prestazioni di indici full-text
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Questo argomento descrive alcune delle cause comuni della riduzione delle prestazioni per gli indici e le query full-text. Vengono inoltre forniti alcuni suggerimenti per limitare i problemi e migliorare le prestazioni.
  
##  <a name="common-causes-of-performance-issues"></a><a name="causes"></a> Common causes of performance issues
### <a name="hardware-resource-issues"></a>Problemi relativi alle risorse hardware
Le prestazioni di esecuzione dell'indicizzazione e delle query full-text possono dipendere da risorse hardware quali memoria e velocità del disco e della CPU, nonché dall'architettura del computer.  

La causa principale del calo delle prestazioni di esecuzione dell'indicizzazione full-text è data dai limiti delle risorse hardware.  
  
-   **CPU**. Se l'uso della CPU da parte del processo host del daemon di filtri (fdhost.exe) o del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (sqlservr.exe) ha quasi raggiunto il 100%, il collo di bottiglia è rappresentato dalla CPU stessa.  
  
-   **Memoria**. In caso di memoria fisica insufficiente, il collo di bottiglia è rappresentato dalla memoria.  

-   **Disco**. Se la lunghezza media della coda di attesa del disco è superiore al doppio del numero di testine, il collo di bottiglia è rappresentato dal disco. La soluzione alternativa principale consiste nella creazione di cataloghi full-text separati dai file e dai log del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Posizionare i log, i file di database e i cataloghi full-text su dischi separati. Per migliorare le prestazioni di esecuzione dell'indicizzazione, è inoltre possibile installare dischi più veloci e usare RAID.  
  
    > [!NOTE]  
    > A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], il motore di ricerca full-text può usare memoria AWE, in quanto parte del processo sqlservr.exe. Per altre informazioni, vedere [Architettura della ricerca full-text](../../relational-databases/search/full-text-search.md#architecture).  

### <a name="full-text-batching-issues"></a>Problemi relativi ai batch full-text
 Se nel sistema non vengono rilevati colli di bottiglia a livello dell'hardware, le prestazioni di indicizzazione della ricerca full-text dipendono principalmente dagli elementi seguenti:  
  
-   Tempo necessario per la creazione di batch full-text in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Velocità di utilizzo di tali batch da parte del daemon di filtri.  

### <a name="full-text-index-population-issues"></a>Problemi relativi al popolamento dell'indice full-text
-   **Tipo di popolamento**. A differenza del popolamento completo, i popolamenti incrementale, manuale e con rilevamento automatico delle modifiche non sono progettati per ottimizzare le risorse hardware ai fini di una maggiore velocità. Di conseguenza, i suggerimenti per l'ottimizzazione in questo argomento potrebbero non migliorare le prestazioni per l'indicizzazione full-text quando si usa il popolamento incrementale, manuale o con rilevamento automatico delle modifiche.  
  
-   **Unione nell'indice master**. Al termine di un popolamento, viene attivato un processo di unione conclusivo che associa i frammenti di indice in un singolo indice full-text master. Ciò consente prestazioni di query superiori poiché è necessario eseguire query solo sull'indice master anziché su alcuni frammenti di indice ed è possibile utilizzare statistiche di punteggio migliori per la classificazione della pertinenza. Tuttavia, l'unione nell'indice master può richiedere l'esecuzione di molte operazioni di I/O, in quanto è necessario leggere e scrivere grandi quantità di dati, ma questa operazione non blocca le query in entrata.  
  
    L'unione nell'indice master di una grande quantità di dati può comportare la creazione di una transazione con esecuzione prolungata, con il conseguente ritardo del troncamento del log delle transazioni durante il checkpoint. In questo caso, le dimensioni del log delle transazioni potrebbero aumentare notevolmente, se si utilizza il modello di recupero con registrazione completa. È consigliabile verificare che il log delle transazioni contenga spazio sufficiente per una transazione con esecuzione prolungata prima di riorganizzare un indice full-text di grandi dimensioni in un database in cui viene utilizzato il modello di recupero con registrazione completa. Per altre informazioni, vedere [Gestione delle dimensioni del file di log delle transazioni](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
##  <a name="tune-the-performance-of-full-text-indexes"></a><a name="tuning"></a> Ottimizzare le prestazioni degli indici full-text  
Per ottimizzare le prestazioni degli indici full-text, implementare le procedure consigliate seguenti:  
  
-   Per usare al meglio tutti i processori o i core CPU, impostare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) '**max full-text crawl range**' sul numero di CPU nel sistema. Per informazioni su questa opzione di configurazione, vedere [Opzione di configurazione del server max full-text crawl range](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md).  
  
-   Verificare che la tabella di base includa un indice cluster. Utilizzare un tipo di dati integer per la prima colonna dell'indice cluster. Evitare l'utilizzo di GUID nella prima colonna dell'indice cluster. Un popolamento a più intervalli in un indice cluster garantisce la massima velocità di popolamento. È consigliabile che la colonna utilizzata come chiave full-text sia di un tipo di dati integer.  
  
-   Aggiornare le statistiche della tabella di base utilizzando l'istruzione [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) . Un'operazione ancora più importante consiste nell'aggiornamento delle statistiche nell'indice cluster o nella chiave full-text per un popolamento completo. In questo modo, tramite un popolamento a più intervalli è possibile generare partizioni ottimali nella tabella.  
  
-   Prima di eseguire un popolamento completo in un computer di grandi dimensioni con più CPU, è consigliabile limitare temporaneamente la dimensione del pool di buffer impostando il valore di **max server memory** in modo tale da lasciare una quantità di memoria sufficiente per il processo fdhost.exe e il sistema operativo. Per altre informazioni, vedere [Stima dei requisiti di memoria del processo host del daemon di filtri (fdhost.exe)](#estimate) più avanti in questo argomento.

-   Se si usa il popolamento incrementale basato su una colonna timestamp, compilare un indice secondario sulla colonna **timestamp** per migliorare le prestazioni di esecuzione del popolamento incrementale.  
  
##  <a name="troubleshoot-the-performance-of-full-populations"></a><a name="full"></a> Risolvere i problemi relativi alle prestazioni di popolamenti completi  
### <a name="review-the-full-text-crawl-logs"></a>Esaminare i log di ricerca per indicizzazione full-text
 Per diagnosticare problemi di prestazioni, analizzare i log della ricerca per indicizzazione full-text.
 
Quando si verifica un errore durante una ricerca per indicizzazione, la funzionalità di registrazione corrispondente per la ricerca full-text crea e gestisce un log di tipo ricerca per indicizzazione in formato testo normale. Ogni log di tipo ricerca per indicizzazione corrisponde a un catalogo full-text specifico. Per impostazione predefinita, i log di ricerca per indicizzazione per un'istanza specifica, ad esempio l'istanza predefinita, si trovano nella cartella `%ProgramFiles%\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\LOG`.
 
Il file del log di tipo ricerca per indicizzazione segue lo schema di denominazione seguente:  
  
`SQLFT<DatabaseID\><FullTextCatalogID\>.LOG[<n\>]`
  
Di seguito sono riportate le parti variabili del nome del file del log di ricerca per indicizzazione.
-   \<**DatabaseID**> - ID di un database. <**dbid**> è un numero a cinque cifre con zeri iniziali.  
-   <**IDCatalogoFullText**>: ID del catalogo full-text. \<**catid**> è un numero a cinque cifre con zeri iniziali.  
-   <**n**>: numero intero che indica l'esistenza di uno o più log di ricerca per indicizzazione per lo stesso catalogo full-text.  
  
 Ad esempio, `SQLFT0000500008.2` è il file del log di ricerca per indicizzazione per un database con ID database = 5 e ID catalogo full-text = 8. Il 2 alla fine del nome file indica che sono disponibili due file del log di tipo ricerca per indicizzazione per questa coppia di database/catalogo.  

### <a name="check-physical-memory-usage"></a>Controllare l'utilizzo della memoria fisica  
 Durante un popolamento full-text, è possibile che la memoria disponibile per `fdhost.exe` o `sqlservr.exe` diventi insufficiente o si esaurisca.
-   Se il log delle ricerche per indicizzazione full-text indica il riavvio frequente del processo `fdhost.exe` o la restituzione del codice di errore 8007008, uno di questi processi non dispone di memoria sufficiente.
-   Se `fdhost.exe` produce dump, in particolare in computer di grandi dimensioni con più CPU, è possibile che la memoria sia in esaurimento.  
-   Per ottenere informazioni sui buffer di memoria usati da una ricerca per indicizzazione full-text, vedere [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md).  
  
 Di seguito sono elencate le possibili cause di memoria insufficiente:  
  
-   **Memoria insufficiente**. Se la quantità di memoria fisica disponibile durante un popolamento completo è pari a zero, è possibile che il pool di buffer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stia utilizzando la maggior parte della memoria fisica presente nel sistema.  
  
     Il processo `sqlservr.exe` tenta di acquisire tutta la memoria disponibile per il pool di buffer, fino alla quantità massima di memoria del server configurata. Se l'allocazione di **max server memory** è eccessiva, per il processo fdhost.exe possono verificarsi condizioni di memoria insufficiente e l'impossibilità di allocare memoria condivisa.  
  
     È possibile risolvere questo problema impostando in modo appropriato il valore **max server memory** del pool di buffer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Stima dei requisiti di memoria del processo host del daemon di filtri (fdhost.exe)](#estimate) più avanti in questo argomento. Può inoltre risultare utile ridurre la dimensione del batch per l'indicizzazione full-text.  

-   **Contesa di memoria**. Durante un popolamento full-text in un computer con più CPU, tra fdhost.exe e sqlservr.exe può verificarsi una contesa per la memoria del pool di buffer. La conseguente mancanza di memoria condivisa genera tentativi batch, sovraccarico della memoria e dump da parte del processo fdhost.exe.  

-   **Problemi di paging**. Anche le dimensioni insufficienti del file di paging, ad esempio in un sistema con un file di paging ridotto con crescita limitata, possono generare condizioni di memoria insufficiente per fdhost.exe o sqlservr.exe. Se nei log delle ricerche per indicizzazione full-text non sono riportati errori di memoria, è probabile che le prestazioni non siano ottimali a causa del paging eccessivo.  
  
### <a name="estimate-the-memory-requirements-of-the-filter-daemon-host-process-fdhostexe"></a><a name="estimate"></a> Stimare i requisiti di memoria per il processo host del daemon di filtri (fdhost.exe)  
 La quantità di memoria richiesta dal processo fdhost.exe per un popolamento dipende principalmente dal numero di intervalli di ricerca per indicizzazione full-text utilizzati, dalla dimensione della memoria condivisa in ingresso e dal numero massimo di istanze di tale memoria.  
  
 È possibile stimare approssimativamente la quantità di memoria (in byte) utilizzata dall'host del daemon di filtri tramite la formula seguente:  
  
`number_of_crawl_ranges * ism_size * max_outstanding_isms * 2`  
  
 I valori predefiniti delle variabili nella formula precedente sono i seguenti:  
  
|**Variabile**|**Valore predefinito**|  
|------------------|-----------------------|  
|*number_of_crawl_ranges*|Numero di CPU|  
|*ism_size*|1 MB per computer x86<br /><br /> 4 MB, 8 MB o 16 MB per computer x64, a seconda della memoria fisica totale|  
|*max_outstanding_isms*|25 per computer x86<br /><br /> 5 per computer x64|  
  
 Nella tabella seguente vengono illustrate le linee guida da seguire per stimare i requisiti di memoria di fdhost.exe. Le formule contenute in questa tabella utilizzano i valori riportati di seguito.  
  
-   *F*: stima della memoria richiesta da fdhost.exe (in MB).  
  
-   *T*: memoria fisica totale disponibile nel sistema (in MB).  
  
-   *M*: impostazione ottimale per **max server memory** .  
  
Per informazioni essenziali sulle formule seguenti, vedere le note dopo la tabella.  
  
|Piattaforma|Stima dei requisiti di memoria di fdhost.exe in MB:*F*^1|Formula per il calcolo di max server memory:*M*^2|  
|--------------|-----------------------------------------------------------|-----------------------------------------------------|  
|x86|*F* = *Numero di intervalli di ricerca per indicizzazione* \* 50|*M* =minimo(*T*, 2000) - F - 500|  
|x64|*F* = *Numero di intervalli di ricerca per indicizzazione* \* 10 \* 8|*M* = *T* - *F* - 500|  

**Note sulle formule**
1.  Se sono in corso più popolamenti completi, calcolare i requisiti di memoria di fdhost.exe per ciascuno separatamente, ad esempio *F1*, *F2* e così via. Calcolare quindi *M* come _T_ **-** sigma **(** _F_i **)** .  
2.  500 MB è una stima della memoria necessaria per gli altri processi del sistema. Se nel sistema sono in corso processi aggiuntivi, aumentare questo valore di conseguenza.  
3.  .*ism_size* sia 8 MB per le piattaforme x64.  
  
 #### <a name="example-estimate-the-memory-requirements-of-fdhostexe"></a>Esempio: stima dei requisiti di memoria di fdhost.exe  
  
 Questo esempio è relativo a un computer a 64 bit con 8 GB di RAM e 4 processori dual core. Il primo calcolo consente di stimare i requisiti di memoria di fdhost.exe, ovvero *F*. Il numero di intervalli di ricerca per indicizzazione è `8`.  
  
 `F = 8*10*8 = 640`  
  
 Il calcolo successivo ottiene il valore ottimale per **max server memory** -*M*. La memoria fisica totale disponibile in questo sistema in MB, *T*, è `8192`.  
  
 `M = 8192-640-500 = 7052`  
  
 #### <a name="example-setting-max-server-memory"></a>Esempio: impostazione della memoria massima del server  
  
 Questo esempio usa le istruzioni [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) e [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md) di [!INCLUDE[tsql](../../includes/tsql-md.md)] per impostare **max server memory** sul valore calcolato per *M* nell'esempio precedente, `7052`:  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'max server memory', 7052;  
GO  
RECONFIGURE;  
GO  
```  
  
Per altre informazioni sulle opzioni per la memoria del server, vedere [Opzioni di configurazione del server Server Memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
### <a name="check-cpu-usage"></a>Controllare l'utilizzo della CPU  
Le prestazioni di esecuzione dei popolamenti completi non sono ottimali quando l'utilizzo medio della CPU è inferiore al 30%. Di seguito vengono illustrati alcuni fattori che influiscono sull'utilizzo della CPU.  
  
-   Tempi di attesa lunghi delle pagine  
  
     Per determinare il tempo di attesa delle pagine, eseguire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```sql  
    SELECT TOP 10 * FROM sys.dm_os_wait_stats ORDER BY wait_time_ms DESC;  
    ```  
  
     Nella tabella seguente vengono descritti i tipi di attesa relativi a questo contesto.  
  
    |Tipo di attesa|Descrizione|Possibile soluzione|  
    |---------------|-----------------|-------------------------|  
    |PAGEIO_LATCH_SH (_EX o _UP)|Può indicare un collo di bottiglia a livello di IO, caso in cui anche la lunghezza media della coda del disco sarebbe elevata.|Lo spostamento dell'indice full-text in un filegroup diverso in un disco diverso potrebbe contribuire a ridurre il collo di bottiglia a livello di IO.|  
    |PAGELATCH_EX (o _UP)|Può indicare contese tra i thread che tentano di scrivere nello stesso file di database.|L'aggiunta di file al filegroup in cui risiede l'indice full-text potrebbe contribuire ad attenuare queste contese.|  
  
     Per altre informazioni, vedere [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
-   Analisi inefficaci della tabella di base  
  
     Un popolamento completo esegue l'analisi della tabella di base per produrre batch. Tali analisi potrebbero risultare inefficaci negli scenari seguenti:  
  
    -   Se la tabella di base dispone di una percentuale elevata di colonne esterne alle righe sottoposte a indicizzazione full-text, il collo di bottiglia potrebbe essere causato proprio dall'analisi della tabella di base per produrre batch. In questo caso, lo spostamento dei dati di dimensioni inferiori all'interno delle righe tramite **varchar(max)** o **nvarchar(max)** potrebbe risolvere il problema.  
  
    -   Se la tabella di base è molto frammentata, l'analisi potrebbe risultare inefficace. Per informazioni sul calcolo dei dati esterni alle righe e sulla frammentazione dell'indice, vedere [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) e [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).  
  
         Per ridurre la frammentazione, è possibile riorganizzare o ricompilare l'indice cluster. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
##  <a name="troubleshoot-slow-indexing-of-documents"></a><a name="filters"></a> Risolvere i problemi relativi all'indicizzazione lenta dei documenti

> [!NOTE]
> Questa sezione descrive un problema che riguarda solo gli utenti che indicizzano documenti (ad esempio documenti di Microsoft Word) in cui sono incorporati altri tipi di documento.

Il motore di ricerca full-text usa due tipi di filtri durante il popolamento di un indice full-text: a thread singolo e multithread.
-   Alcuni documenti, quali i documenti di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word, vengono filtrati utilizzando un filtro multithread,
-   mentre altri, ad esempio i documenti PDF (Portable Document Format) di Adobe Acrobat, vengono filtrati utilizzando un filtro a thread singolo.  
  
 Ai fini della sicurezza, i filtri vengono caricati dai processi dell'host del daemon di filtri. In un'istanza del server viene usato un processo a thread multipli per tutti i filtri a thread multipli e un processo a thread singolo per tutti i filtri a thread singolo. Quando un documento che utilizza un filtro multithread contiene un documento incorporato che utilizza un filtro a thread singolo, il motore di ricerca full-text avvia un processo a thread singolo per il documento incorporato. Nel caso di un documento di Word che contiene un documento PDF, il motore di ricerca full-text usa il processo multithread per esaminare il contenuto in formato Word e avvia un processo a thread singolo per esaminare il contenuto in formato PDF. Un filtro a thread singolo potrebbe tuttavia non funzionare in modo corretto in questo ambiente e potrebbe destabilizzare il processo di filtraggio. In alcune situazioni in cui i documenti incorporati rappresentano una prassi comune, la destabilizzazione potrebbe causare arresti anomali del processo. In questo caso, il motore di ricerca full-text reindirizza tutti i documenti che hanno provocato l'errore, ad esempio un documento di Word in cui è incorporato contenuto in formato PDF, al processo di filtraggio a thread singolo. Se il reindirizzamento viene eseguito di frequente, le prestazioni del processo di indicizzazione full-text risultano ridotte.  
  
Per risolvere questo problema, è necessario contrassegnare il filtro per il documento contenitore, in questo esempio il documento di Word, come filtro a thread singolo. Per contrassegnare un filtro come filtro a thread singolo, impostare il valore **ThreadingModel** del Registro di sistema per il filtro su **Apartment Threaded**. Per informazioni sugli apartment a thread singolo, vedere il white paper [Understanding and Using COM Threading Models](/previous-versions/ms809971(v=msdn.10)).  
  
## <a name="see-also"></a>Vedere anche  
 [Opzioni di configurazione del server Server Memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [Opzione di configurazione del server max full-text crawl range](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md)   
 [Popolamento degli indici full-text](../../relational-databases/search/populate-full-text-indexes.md)   
 [Creazione e gestione di indici full-text](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)   
 [sys.dm_fts_memory_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)   
 [Risoluzione dei problemi nell'indicizzazione full-text](../../relational-databases/search/troubleshoot-full-text-indexing.md)  
 [Architettura della ricerca full-text](../../relational-databases/search/full-text-search.md#architecture) 
