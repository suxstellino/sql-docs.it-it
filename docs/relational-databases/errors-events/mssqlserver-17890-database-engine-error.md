---
description: MSSQLSERVER_17890
title: MSSQLSERVER_17890
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17890 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 09ea56c914385ee25a889262259a13073ad595d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196612"
---
# <a name="mssqlserver_17890"></a>MSSQLSERVER_17890
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|17890|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|SRV_WS_TRIMMED|
|Testo del messaggio|È stato eseguito il page out di una parte significativa della memoria del processo di SQL Server. Potrebbe verificarsi un calo di prestazioni. Durata: %d secondi. Working set (KB): %I64d, commit completato (KB): %I64d, utilizzo memoria: %d%%.|
||

## <a name="explanation"></a>Spiegazione

È possibile che venga visualizzato il messaggio di errore seguente nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel log eventi dell’applicazione di Windows.

> È stato eseguito il page out di una parte significativa della memoria del processo di SQL Server. Potrebbe verificarsi un calo di prestazioni. Duration (Durata): 0 secondi. Working set (KB): 3383250, committ completato (KB): 9112480, utilizzo memoria: 37%.

È anche possibile notare un calo improvviso delle prestazioni con l'esecuzione delle query e di tutte le altre operazioni nell’istanza di SQL Server.

## <a name="cause"></a>Causa

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] monitora le varie informazioni correlate alla memoria riguardo al processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questo caso è stato rilevato che il working set del processo è inferiore al 50% della memoria del processo di cui è stato eseguito il commit. Di conseguenza questo avviso viene stampato. Normalmente a causare l’avviso è una di queste situazioni:

- Il sistema operativo esegue il page out al file di paging di ampie sezioni della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di cui è stato eseguito il committ.
- Ciò può essere dovuto a un aumento improvviso della domanda di memoria da parte di altre applicazioni o delle esigenze del sistema operativo.
- Questo problema può verificarsi anche quando determinati driver di dispositivo richiedono allocazioni di memoria contigue per le proprie esigenze.

## <a name="user-action"></a>Azione utente

È possibile impedire al sistema operativo Windows di eseguire il page out della memoria del pool di buffer del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloccando la memoria allocata per il pool di buffer nella memoria fisica. Per bloccare la memoria, assegnare il diritto utente Blocco delle pagine in memoria all'account utente usato come account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tuttavia, prima di implementare questa soluzione, rivedere le sezioni relative ai[motivi per cui viene eseguito il page out della memoria di SQL Server](#what-causes-sql-server-memory-to-be-paged-out) e agli aspetti importanti da considerare prima di assegnare il diritto utente "Blocco delle pagine in memoria" per un'istanza di SQL Server

> [!NOTE]
> L'uso di Blocco delle pagine in memoria garantisce che non venga eseguito il page out della memoria gestita da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il sistema operativo può tuttavia effettuare il page out degli stack di thread, del file EXE, di tutte le immagini DLL, della memoria heap e della memoria CLR.
>
> A partire dall'aggiornamento cumulativo 2 per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 SP1, entrambe le edizioni Standard ed Enterprise di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono usare il diritto utente Blocco delle pagine in memoria. Per altre informazioni sul supporto per le pagine bloccate, vedere l’articolo [KB970070 relativo al supporto per le pagine bloccate nei sistemi SQL Server Standard Edition (64 bit)](https://support.microsoft.com/help/970070).

Per assegnare il diritto utente Blocco delle pagine in memoria, attenersi alla procedura seguente:

1. Fare clic su **Start**, fare clic su **Esegui**, digitare *gpedit.msc* e quindi fare clic su **OK**.
1. Nota: viene visualizzata la finestra di dialogo Criteri di gruppo.
1. Espandere **Configurazione computer** e quindi **Impostazioni di Windows**.
1. Espandere **Impostazioni sicurezza** e quindi espandere **Criteri locali**.
1. Fare clic su Assegnazione diritti utente, quindi fare doppio clic su **Blocco delle pagine** in memoria.
1. Nella finestra di dialogo **Impostazione criteri di protezione locali** fare clic su **Aggiungi utente** o **gruppo**.
1. Nella finestra di dialogo **Seleziona utenti** o **gruppi** aggiungere l'account autorizzato a eseguire il file sqlservr.exe, quindi fare clic su **OK**.
1. Chiudere la finestra di dialogo **Criteri di gruppo**.
1. Riavviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Dopo aver assegnato il diritto utente Blocco delle pagine in memoria e aver riavviato il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il sistema operativo Windows non esegue più il page out della memoria del pool di buffer nel processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tuttavia, il sistema operativo Windows può comunque eseguire il page out della memoria del pool non di buffer nel processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

È possibile convalidare l'uso del diritto utente da parte dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verificando che il messaggio seguente venga scritto nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'avvio: "Per il pool di buffer vengono utilizzate pagine bloccate"

Questo messaggio si applica solo a SQL Server. Per altre informazioni su questo messaggio nel log degli errori, vedere la sezione relativa all'[assegnazione del privilegio Blocco delle pagine in memoria nel sistema locale](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426)

Quando il sistema operativo Windows esegue il page out della memoria del pool non di buffer, è comunque possibile che si verifichino problemi di prestazioni. Tuttavia, i messaggi di errore menzionati nella sezione "Spiegazione" non vengono registrati nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="what-causes-sql-server-memory-to-be-paged-out"></a>Cause del page out della memoria di SQL Server

Questa situazione può essere causata da tre categorie generali di problemi:

- Problemi correlati alle applicazioni: Tutte le applicazioni insieme hanno esaurito la memoria fisica disponibile e il sistema operativo deve liberare memoria per le nuove richieste di risorse da parte delle applicazioni. In genere, l'approccio è individuare le applicazioni che esauriscono la memoria e adottare le misure necessarie per bilanciare la memoria tra le applicazioni senza esaurire la RAM.
- Problemi dei driver di dispositivo: I driver di dispositivo possono causare il paging del working set per tutti i processi se il driver chiama una funzione di allocazione della memoria in modo errato.
- Problemi del sistema operativo

Di seguito sono riportate informazioni su ognuna di queste categorie

- **Problemi correlati alle applicazioni**: Le applicazioni insieme possono consumare tutta la RAM del sistema. Se vengono effettuate nuove richieste di memoria, il sistema operativo tenta di soddisfarle e, se non è disponibile memoria, "taglia" il working set delle applicazioni in esecuzione per soddisfare le richieste di memoria. In questi casi si può osservare che il working set per la maggior parte delle applicazioni, se non tutte, si riduce in modo significativo. Per verificare questa situazione, raccogliere il seguente contatore del Monitor prestazioni per tutte le applicazioni del sistema:

  - Oggetto prestazione: Process
  - Contatore: Working Set
  
  Monitorare inoltre il contatore seguente per correlare la quantità di memoria fisica disponibile nel sistema.
  
  - Oggetto prestazione: Memoria
  - Contatore: Memoria disponibile (MB)
  
  Il comportamento tipico che è possibile osservare è la riduzione della memoria disponibile fino a quasi 0 MB e al tempo stesso un calo improvviso dei contatori del working set per la maggior parte (tutti) dei processi nel sistema. Se si osserva tale comportamento, può essere necessario eseguire alcune operazioni per ridurre l'utilizzo della memoria nel sistema, ad esempio ridurre la memoria massima del server per SQL Server.
  
    Le applicazioni possono anche usare eccessivamente la cache di sistema e causare una crescita elevata della cache. Per rispondere alla crescita della cache di sistema, il sistema esegue il page out del working set del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o di altre applicazioni. Se si verifica questo problema, è possibile usare alcune funzioni di gestione della memoria nell'applicazione. Queste funzioni controllano lo spazio della cache di sistema che le operazioni di I/O di file possono usare nell'applicazione. Ad esempio, è possibile usare la funzione SetSystemFileCacheSize e la funzione GetSystemFileCacheSize per controllare lo spazio della cache di sistema che le operazioni di I/O di file possono usare.
  
    È possibile utilizzare l'oggetto prestazioni di memoria per visualizzare i valori dei vari contatori in questo oggetto e determinare se il working set della cache di sistema usa una quantità eccessiva di memoria. Si possono, ad esempio, visualizzare i contatori Byte nella cache e Byte residenti cache di sistema. Per altre informazioni su questo argomento, vedere:
  
    - [Too Much Cache?](/archive/blogs/ntdebugging/too-much-cache) (Troppi dati nella cache?)
    - [Microsoft Windows Dynamic Cache Service](/archive/blogs/ntdebugging/microsoft-windows-dynamic-cache-service)
    - [Si verificano problemi di prestazioni nelle applicazioni e servizi quando la cache del file system utilizza la maggior parte della RAM fisica](https://support.microsoft.com/help/976618)
  
    È possibile scaricare e distribuire il servizio "Microsoft Windows Dynamic Cache Service" per controllare la memoria usata dalla cache di sistema.

- **Problemi dei driver di dispositivo**: Se un driver di dispositivo usa la funzione `MmAllocateContiguousMemory` e imposta il valore del parametro HighestAcceptableAddress su un valore inferiore a 4 gigabyte (GB), il sistema operativo Windows può eseguire il page out del working set dei processi nel sistema, incluso il processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per risolvere questo problema, contattare il fornitore del driver di dispositivo per gli aggiornamenti del driver.

    Quando un driver di dispositivo tenta di allocare memoria, il sistema operativo Windows può eseguire il page out del working set di altre applicazioni. Questo hotfix di Windows consente di usare la traccia eventi per trovare il driver di dispositivo che causa il problema. Per altre informazioni sul driver specifico che causa il comportamento di taglio dei working set, vedere l'articolo sull'[identificazione dei driver che allocano memoria contigua](/previous-versions/windows/desktop/xperf/identifying-drivers-that-allocate-contiguous-memory).

- **Problemi relativi al sistema operativo**: Per risolvere i problemi noti per cui il sistema operativo Windows esegue il page out del working set del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], applicare gli hotfix descritti negli articoli seguenti della Microsoft Knowledge Base.

  > [!NOTE]
  > Gli hotfix sono cumulativi. Una versione più recente di un hotfix contiene le versioni precedenti dello stesso hotfix.

  - Il set di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere tagliato se il sistema usa alcune funzionalità TCP avanzate. Per altre informazioni, vedere [Come risolvere i problemi relativi alle funzionalità avanzate di prestazioni di rete, ad esempio RSS e NetDMA](/troubleshoot/windows-server/networking/troubleshoot-network-performance-features-rss-netdma).

  - Se si esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Windows Server 2008, è necessario applicare le correzioni per i problemi noti che possono causare il taglio del working set o un uso eccessivo e non necessario della memoria da parte di altri componenti del sistema operativo. Per altre informazioni, consultare l'articolo seguente: [Il processo di generazione dei report potrebbe bloccarsi quando si esegue Perfmon.exe con il modello di diagnostica di Active Directory per generare un report in un controller di dominio basato su Windows Server 2008](/troubleshoot/windows-server/performance/report-generation-process-stops-responding).

  - Se si esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Windows Server 2008 R2, è necessario applicare correzioni per i problemi noti che possono causare il taglio del working set. Per altre informazioni, vedere gli articoli relativi ai seguenti argomenti:

    - [Un computer che esegue Windows 7 o Windows Server 2008 R2 non risponde quando si esegue un'applicazione di grandi dimensioni](https://support.microsoft.com/help/979149)
    - [In un computer con processori basati su NUMA e che esegue Windows Server 2008 R2 o Windows 7 le prestazioni non sono ottimali se un thread richiede una quantità elevata di memoria entro i primi 4 GB di memoria](https://support.microsoft.com/help/2155311)
    - [Il computer a intermittenza non funziona correttamente o smette di rispondere se si usa il driver Storport usato in Windows Server 2008 R2](https://support.microsoft.com/help/2468345)

## <a name="important-considerations-before-you-assign-the-lock-pages-in-memory-user-right"></a>Considerazioni importanti prima di assegnare il diritto utente "Blocco delle pagine in memoria"

Prima di assegnare il diritto utente "Blocco delle pagine in memoria", è necessario prendere in considerazione alcuni altri aspetti. Se si assegna questo diritto utente a sistemi configurati in modo non corretto, il sistema può diventare instabile o si può verificare un calo delle prestazioni dell'intero sistema. Inoltre, è possibile che l'evento con ID 333 venga registrato nel registro eventi.

Se si contatta il servizio di assistenza clienti di Microsoft per questi problemi, è possibile che i tecnici chiedano di revocare questo diritto utente per l'account utente usato come account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo passaggio può essere necessario per raccogliere importanti dati sulle prestazioni che saranno utili ai tecnici dell'assistenza per eseguire la necessaria configurazione delle diverse opzioni per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e per altre applicazioni in esecuzione nel sistema. Dopo che i tecnici hanno raccolto i dati sulle prestazioni, è possibile assegnare il diritto utente Blocco delle in memoria all'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Prima di assegnare il diritto utente Blocco delle pagine in memoria assicurarsi di acquisire un log di Monitor prestazioni per determinare i requisiti di memoria di varie applicazioni e servizi installati nel sistema. Queste applicazioni includono anche SQL Server. Per determinare i requisiti di memoria, raccogliere le seguenti informazioni di base:

- Assicurarsi di impostare correttamente le opzioni max server memory e min server memory. Queste opzioni riflettono solo il requisito di memoria del pool di buffer del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non includono la memoria allocata per altri componenti all'interno del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questi componenti includono:

  - I thread di lavoro di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  - Varie DLL e componenti caricati dal processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nello spazio degli indirizzi del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  - Le operazioni di backup e ripristino

- I componenti e le DLL includono diversi provider OLE DB, le stored procedure estese, gli oggetti COM Microsoft usati per la stored procedure sp_OACreate, i server collegati e la funzione CLR di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La memoria allocata per questi componenti rientra nell'area del pool non di buffer dello spazio degli indirizzi del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per determinare idealmente la quantità massima di memoria che l'intero processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare, è necessario sottrarre la memoria allocata per i componenti che non usano il pool di buffer dalla memoria totale che verrà usata dal processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quindi, è possibile usare il valore rimanente per impostare l'opzione max server memory. Prima di impostare l'opzione max server memory e l'opzione min server memory, è necessario rivedere con attenzione l'argomento "Impostazione manuale delle opzioni per la memoria" nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Determinare il requisito di memoria di altre applicazioni e dei componenti del sistema operativo Windows. Le applicazioni possono includere altri componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, gli agenti di replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Reporting Services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Integration Services e la ricerca full-text di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le applicazioni che eseguono operazioni di backup e di copia dei file possono usare una grande quantità di memoria, ad esempio l'operazione di copia bulk e l'agente di snapshot che generano I/O di file. Quando si determina il valore dell'opzione max server memory e dell'opzione min server memory, è necessario considerare il requisito di memoria di tutte queste applicazioni. È possibile usare il contatore Byte privati e il contatore Working set nell'oggetto Processo per ogni processo per determinare il requisito di memoria per un processo specifico.

- Per impostazione predefinita, il diritto utente Blocco delle pagine in memoria è già stato assegnato all'account Sistema locale predefinito. Per altre informazioni, nel sito Web Microsoft vedere la sezione relativa all'[assegnazione del privilegio Blocco delle pagine in memoria nel sistema locale](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426?advanced=false&collapse_discussion=true&search_type=thread).

- Se si usa un account utente di Windows a livello globale per tutti i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un dominio, determinare i diritti utente assegnati usando una configurazione Criteri di gruppo. Un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a 32 bit può usare questo account come account di avvio. Tuttavia, questo account richiede il diritto utente Blocco delle pagine in memoria per abilitare la funzionalità `Address Windowing Extensions` (AWE). Per altre informazioni, vedere l'argomento relativo all'uso della quantità massima di memoria per SQL Server nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Prima di configurare le opzioni max server memory e min server memory per più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], considerare i requisiti di memoria del pool non di buffer per ogni istanza di SQL Server. Quindi configurare queste opzioni per ogni istanza di SQL Server.

Idealmente queste informazioni di base vengono raccolte durante i picchi di carico. Pertanto, è possibile determinare i requisiti di memoria per varie applicazioni e componenti per supportare il picco di carico. I requisiti di memoria variano da un sistema all'altro, a seconda delle attività e delle applicazioni in esecuzione nel sistema. È possibile eseguire una query sulle informazioni visualizzate nella DMV sys.dm_os_process_memory per capire se il sistema sta riscontrando condizioni di memoria insufficiente. Per altre informazioni, vedere [sys.dm_os_memory_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md).

## <a name="improvements-added-in-windows-server-2008-and-r2-version"></a>Miglioramenti aggiunti in Windows Server 2008 e nella versione R2

In Windows Server 2008 e Windows Server 2008 R2 il meccanismo di allocazione della memoria contigua è ottimizzato. Questo miglioramento consente a Windows Server 2008 e Windows Server 2008 R2 di ridurre in una certa misura gli effetti del page out del working set delle applicazioni quando arrivano nuove richieste di memoria.

Di seguito è riportata una spiegazione dei miglioramenti estratta dal white paper Microsoft relativo ai miglioramenti della gestione della memoria in Windows:

*In Windows Server 2008 l'allocazione di memoria fisicamente contigua è estremamente avanzata. Le richieste di allocazione di memoria contigua hanno maggiori probabilità di riuscire perché la gestione della memoria ora sostituisce dinamicamente le pagine, in genere senza tagliare il working set o eseguire operazioni di I/O. Inoltre, molti più tipi di pagine, ad esempio gli stack del kernel e le pagine di metadati del file system, sono ora candidati per la sostituzione. Di conseguenza, in un dato momento è disponibile a livello generale una maggiore quantità di memoria contigua. In più, il costo per ottenere tali allocazioni è notevolmente ridotto.*

Per altre informazioni, vedere l'articolo sui [problemi relativi al taglio dei working set in SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/sql-server-working-set-trim-problems-consider/ba-p/315462?advanced=false&collapse_discussion=true&search_type=thread).

Gli strumenti di terze parti citati in questo articolo sono prodotti da società indipendenti di Microsoft. Microsoft non fornisce alcuna garanzia, implicita o esplicita, sulle prestazioni o sull'affidabilità di questi prodotti.