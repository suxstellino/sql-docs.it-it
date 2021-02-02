---
description: Guida sull'architettura e gestione del log delle transazioni di SQL Server
title: Guida all'architettura e alla gestione del log delle transazioni
ms.custom: ''
ms.date: 10/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- transaction log architecture guide
- guide, transaction log architecture
- vlf
- transaction log guidance
- vlfs
- virtual log files
- virtual log size
- vlf size
- transaction log internals
ms.assetid: 88b22f65-ee01-459c-8800-bcf052df958a
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed532d22800cd10530ecc7db8595fcf75e3737da
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237936"
---
# <a name="sql-server-transaction-log-architecture-and-management-guide"></a>Guida sull'architettura e gestione del log delle transazioni di SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In ogni database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è incluso un log delle transazioni in cui vengono registrate tutte le transazioni e le modifiche apportate dalle transazioni stesse al database. Il log delle transazioni è un componente fondamentale del database e, in caso di errore di sistema, può essere necessario per ripristinare la coerenza del database. In questa guida vengono fornite informazioni sull'architettura fisica e logica del log delle transazioni. Le informazioni sull'architettura consentono di gestire più efficacemente i log delle transazioni.  

  
##  <a name="transaction-log-logical-architecture"></a><a name="Logical_Arch"></a> Architettura logica del log delle transazioni  
 Il log delle transazioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] funziona in modo logico come se si trattasse di una stringa di record di log. Ogni record di log è identificato da un numero di sequenza del file di log (LSN). Ogni nuovo record di log viene scritto nell'estremità finale logica del log con un LSN maggiore di quello del record precedente. I record di log vengono memorizzati in una sequenza seriale, dato che vengono creati in modo tale che se LSN2 è maggiore di LSN1, la modifica descritta dal record di log cui fa riferimento LSN2 si verifica dopo la modifica descritta dal record di log cui fa riferimento LSN1. Ogni record di log contiene l'ID della transazione a cui appartiene. Tutti i record di log associati a ogni transazione sono collegati singolarmente in una catena tramite puntatori ai record precedenti che consentono di eseguire più rapidamente il rollback della transazione.  
  
 Nei record di log relativi alle modifiche dei dati vengono registrate le operazioni logiche eseguite oppure immagini dei dati precedenti e successive alla modifica. Un'immagine precedente la modifica è una copia dei dati eseguita prima dell'operazione, mentre un'immagine successiva alla modifica è una copia dei dati eseguita dopo l'operazione.  
  
La procedura necessaria per recuperare un'operazione varia a seconda del tipo di record di log:  
  
-   Operazione logica registrata  
  
    -   Per eseguire il rollforward dell'operazione logica, questa viene eseguita nuovamente.  
  
    -   Per eseguire il rollback dell'operazione logica, questa viene eseguita al contrario.  
  
-   Immagine precedente e successiva (alla modifica dei dati) registrata  
  
    -   Per eseguire il rollforward dell'operazione, viene applicata l'immagine successiva.  
  
    -   Per eseguire il rollback dell'operazione, viene applicata l'immagine precedente.  
  
Nel log delle transazioni vengono registrati molti tipi di operazioni, tra cui:  
  
-   L'inizio e la fine di ogni transazione.  
  
-   Tutte le modifiche apportate ai dati, ovvero inserimento, aggiornamento o eliminazione, comprese le modifiche apportate da stored procedure di sistema o istruzioni DDL (Data Definition Language) a qualsiasi tabella, incluse le tabelle di sistema.  
  
-   Tutte le allocazioni o deallocazioni di pagina e di extent.  
  
-   Operazioni di creazione o eliminazione di una tabella o di un indice.  
  
 Nel log vengono registrate anche le operazioni di rollback. Ogni transazione riserva una determinata quantità di spazio nel log delle transazioni per garantire che nel log sia disponibile spazio sufficiente per supportare un rollback causato da un'istruzione di rollback esplicita o dal verificarsi di un errore. La quantità di spazio riservata varia in base alle operazioni eseguite nella transazione, ma in genere equivale alla quantità di spazio utilizzata per registrare nel log ogni operazione. Lo spazio riservato viene liberato al completamento della transazione.  
  
<a name="minlsn"></a> La sezione del file di log dal primo record di log che deve essere presente per la corretta esecuzione del rollback a livello di database all'ultimo record di log scritto è definita parte attiva del log, *log attivo* o *parte finale del log*. Questa sezione del log è necessaria per il [recupero](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery) completo del database. Non è possibile troncare nessuna parte del log attivo. Il numero di sequenza del file di log (LSN) di questo primo record di log è anche detto **LSN minimo del recupero (*MinLSN*)** . Per altre informazioni sulle operazioni supportate dal log delle transazioni, vedere [Log delle transazioni (SQL Server)](../relational-databases/logs/the-transaction-log-sql-server.md).  

Backup dei log e differenziali spostano il database ripristinato a un momento successivo, corrispondente a un LSN maggiore. 
  
##  <a name="transaction-log-physical-architecture"></a><a name="physical_arch"></a> Architettura fisica del log delle transazioni  
Del log delle transazioni di un database viene eseguito il mapping su uno o più file fisici. Concettualmente, il file di log è una stringa di record di log. Fisicamente, la sequenza di record di log viene archiviata in modo efficiente nel set di file fisici che implementano il log delle transazioni. È necessario che sia disponibile almeno un file di log per ogni database.  
  
In [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ogni file di log fisico viene diviso internamente in diversi file di log virtuali. I file di log virtuali non hanno dimensioni fisse e non è previsto un numero fisso di file di log virtuali per un file di log fisico. Il [!INCLUDE[ssDE](../includes/ssde-md.md)] definisce dinamicamente le dimensioni dei file di log virtuali durante la creazione o l'estensione. Il [!INCLUDE[ssDE](../includes/ssde-md.md)] tende a mantenere ridotto il numero di file virtuali. Le dimensioni dei file virtuali dopo l'estensione di un file di log corrispondono alla somma delle dimensioni del log esistente e del nuovo incremento del file. Le dimensioni o il numero di file di log virtuali non possono essere configurati o impostati dagli amministratori.  

> [!NOTE]
> La creazione di file di log virtuali (VLF) avviene nel modo seguente:
> - Se il valore growth successivo è inferiore a 1/8 del valore size del log fisico attuale, creare 1 file di log virtuale che copra l'aumento delle dimensioni (a partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)])
> - Se l'aumento successivo è superiore a 1/8 della dimensione corrente del log, usare il metodo pre-2014:
>    -  Se il valore growth è inferiore a 64 MB, creare 4 file di log virtuali che coprano l'aumento delle dimensioni (ad esempio con growth di 1 MB, creare quattro file virtuali di log da 256 KB)
>    -  Se il valore growth è compreso tra 64 MB e 1 GB, creare 8 file di log virtuali che coprano l'aumento delle dimensioni (ad esempio con growth di 512 MB, creare otto file virtuali di log da 64 MB)
>    -  Se il valore growth è superiore a 1 GB, creare 16 file di log virtuali che coprano l'aumento delle dimensioni (ad esempio con growth di 8 GB, creare sedici file virtuali di log da 512 MB)

Se le dimensioni dei file di log aumentano in modo considerevole a piccoli ma numerosi incrementi, il numero di file di log virtuali sarà elevato. **Questo può provocare un rallentamento delle operazioni di avvio del database e di backup e ripristino del log.** Se invece i file di log sono impostati su dimensioni elevate con uno o pochi incrementi, il numero di file di log virtuali molto grandi sarà ridotto. Per altre informazioni su una stima corretta per impostare le **dimensioni richieste** e l'**aumento di dimensioni automatico** di un log delle transazioni, fare riferimento alla sezione *Indicazioni* di [Gestire le dimensioni del file di log delle transazioni](../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md#Recommendations).

È consigliabile assegnare ai file di log un valore *size* simile a quello delle dimensioni finali necessarie, usando gli incrementi necessari per ottenere una distribuzione dei file di log virtuali ottimale, e un valore *growth_increment* relativamente alto. Vedere il suggerimento seguente per determinare la distribuzione dei file di log virtuali ottimale per le dimensioni correnti del log delle transazioni. 
 - Il valore *size*, impostato dall'argomento `SIZE` di `ALTER DATABASE`, corrisponde alle dimensioni iniziali del file di log.
 - Il valore *growth_increment*, vale a dire il valore di aumento automatico, impostato dall'argomento `FILEGROWTH` di `ALTER DATABASE`, è la quantità di spazio che viene aggiunta al file ogni volta che è richiesto spazio nuovo. 
 
Per altre informazioni sugli argomenti `FILEGROWTH` e `SIZE` di `ALTER DATABASE`, vedere [Opzioni per file e filegroup ALTER DATABASE & #40; Transact-SQL & #41;](../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

> [!TIP]
> Per determinare la distribuzione dei file di log virtuali ottimale per le dimensioni correnti del log delle transazioni di tutti i database in un'istanza specifica e gli incrementi della crescita necessari per ottenere le dimensioni richieste, vedere questo [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs).
  
 Il log delle transazioni è un file circolare. Si consideri ad esempio un database con un file di log fisico diviso in quattro file di log virtuali. Quando viene creato il database, il file di log logico comincia all'inizio del file di log fisico. Vengono aggiunti nuovi record di log alla fine del log logico, che si espandono verso la fine del log fisico. Il troncamento del log libera tutti i log virtuali i cui record vengono visualizzati tutti davanti al numero minimo di sequenza del file di log (MinLSN, Minimum Log Sequence Number) per il recupero. *MinLSN* è il numero di sequenza del file di log del record di log meno recente necessario per un corretto rollback a livello di database. Il log delle transazioni del database di esempio sarebbe simile a quello illustrato nella figura seguente.  
  
 ![Illustra il modo in cui un file di log fisico è suddiviso in log virtuali](../relational-databases/media/tranlog3.png)  
  
 Quando la fine del log logico raggiunge la fine del file di log fisico, i nuovi record di log vengono nuovamente inseriti a partire dall'inizio del file di log fisico.  
  
![Illustra il modo in cui viene eseguito il wrapping di un log delle transazioni logico nel relativo file di log fisico](../relational-databases/media/tranlog4.png)   
  
 Questo ciclo viene ripetuto all'infinito, a condizione che la fine del log logico non raggiunga mai l'inizio del log stesso. Se i vecchi record di log vengono troncati abbastanza frequentemente in modo da lasciare sempre spazio sufficiente per i nuovi record di log creati fino al checkpoint successivo, il log non viene mai riempito completamente. Se, tuttavia, la fine del log logico raggiunge l'inizio del log stesso, può verificarsi uno dei due eventi indicati di seguito:  
  
-   Se è abilitata l'impostazione `FILEGROWTH` per il log e sul disco vi è spazio disponibile, il file viene esteso in base al valore specificato nel parametro *growth_increment* e i nuovi record del log vengono aggiunti all'estensione. Per altre informazioni sull'impostazione `FILEGROWTH`, vedere [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
-   Se l'impostazione `FILEGROWTH` non è attiva oppure se lo spazio libero sul disco in cui risiede il file di log è inferiore a quello specificato in *growth_increment* viene generato un errore 9002. Per altre informazioni, fare riferimento a [Risolvere i problemi relativi a un log completo](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md).  
  
 Se il log include più file di log fisici, il log logico utilizzerà tutti i file di log fisici prima di tornare all'inizio del primo file di log fisico. 
 
> [!IMPORTANT]
> Per altre informazioni sulla gestione delle dimensioni del log delle transazioni, vedere [Gestione delle dimensioni del file di log delle transazioni](../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).
  
### <a name="log-truncation"></a>Troncamento del log  
 Il troncamento del log è essenziale per evitare il riempimento del log. Il troncamento del log comporta l'eliminazione dei file di log virtuali inattivi dal log delle transazioni logico di un database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , liberando spazio nel log logico per il riutilizzo da parte del log delle transazioni fisico. Se un log delle transazioni non viene mai troncato, è possibile che le sue dimensioni aumentino fino a occupare tutto lo spazio su disco allocato ai file di log fisici. Tuttavia, prima che sia possibile troncare il log, è necessario eseguire un'operazione su checkpoint. Tramite un checkpoint vengono scritte le pagine modificate in memoria correnti, note come pagine dirty, e le informazioni sul log delle transazioni dalla memoria sul disco. Quando viene eseguito il checkpoint, la parte inattiva del log delle transazioni viene contrassegnata come riutilizzabile. Successivamente, tale parte inattiva potrà essere liberata mediante il troncamento del log. Per altre informazioni sui checkpoint, vedere [Checkpoint di database &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md).  
  
 Nelle figure seguenti viene illustrato un log delle transazioni prima e dopo il troncamento. Nella prima figura viene illustrato un log delle transazioni che non è mai stato troncato. Attualmente, il log logico utilizza quattro file di log virtuali. Il log logico inizia prima del primo file di log virtuale e termina al log virtuale 4. Il record MinLSN si trova nel log virtuale 3. I log virtuali 1 e 2 contengono solo record di log inattivi. Questi record possono essere troncati. Il log virtuale 5 è ancora inutilizzato e non fa parte del log logico corrente.  
  
![Illustra il modo in cui viene visualizzato un log delle transazioni prima del troncamento](../relational-databases/media/tranlog2.png)  
  
 Nella seconda figura è illustrata la struttura del log dopo il troncamento. I log virtuali 1 e 2 sono stati liberati per il riutilizzo. Il log logico ora inizia all'inizio del log virtuale 3. Il log virtuale 5 è ancora inutilizzato e non fa parte del log logico corrente.  
  
![Illustra il modo in cui viene visualizzato un log delle transazioni dopo il troncamento](../relational-databases/media/tranlog3.png)  
  
 A meno che non venga posticipato per qualche motivo, il troncamento del log viene effettuato automaticamente dopo gli eventi seguenti:  
  
-   Nel modello di recupero con registrazione minima, dopo un checkpoint.  
-   Nel modello di recupero con registrazione completa o modello di recupero con registrazione minima delle operazioni bulk, dopo un backup del log se dal backup precedente si è verificato un checkpoint.  
  
 Il troncamento del log può essere posticipato da diversi fattori. Se si verifica un ritardo elevato nel troncamento del log, lo spazio del log delle transazioni può esaurirsi. Per informazioni, vedere [Fattori che possono posticipare il troncamento del log](../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation) e [Risolvere i problemi relativi a un log delle transazioni completo &#40;Errore di SQL Server 9002&#41;](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md).  
  
##  <a name="write-ahead-transaction-log"></a><a name="WAL"></a> Log delle transazioni write-ahead  
 In questa sezione viene descritto il ruolo del log delle transazioni write-ahead nella registrazione delle modifiche dei dati sul disco. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa un algoritmo di log write-ahead (WAL) che garantisce che le modifiche apportate ai dati non vengano scritte nel disco prima del record di log corrispondente. In questo modo, è possibile mantenere le proprietà ACID per una transazione.  
  
 Per comprendere il funzionamento dei log write-ahead, è importante conoscere la modalità con cui i dati modificati vengono scritti sul disco. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mantiene una cache buffer in cui vengono lette le pagine di dati quando questi ultimi devono essere recuperati. Quando una pagina viene modificata nella cache buffer, non viene immediatamente riscritta nel disco, ma viene contrassegnata come *dirty*. A una pagina di dati possono essere associate più scritture logiche prima di essere scritta fisicamente sul disco. Per ogni scrittura logica, viene inserito un record del log delle transazioni nella cache del log, per registrare la modifica. I record di log devono essere scritti sul disco prima che la pagina dirty associata venga rimossa dalla cache buffer e scritta sul disco. Tramite il processo di gestione dei checkpoint viene eseguita periodicamente l'analisi della cache buffer alla ricerca di buffer con pagine di un database specifico e tutte le pagine dirty vengono scritte nel disco. I checkpoint consentono di risparmiare tempo durante un successivo recupero, grazie alla creazione di un punto in cui è certo che tutte le pagine dirty siano state scritte sul disco.  
  
 La scrittura di una pagina di dati modificata dalla cache buffer al disco viene definita scaricamento della pagina. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dispone della logica tramite cui viene impedito lo scaricamento di una pagina dirty prima della scrittura del record di log associato. I record di log vengono scritti su disco quando i buffer di log vengono scaricati.  Ciò si verifica ogni volta che viene eseguito il commit di una transazione o che i buffer di log sono pieni.  
  
##  <a name="transaction-log-backups"></a><a name="Backups"></a> Backup di log delle transazioni  
 In questa sezione vengono introdotti concetti relativi al backup e al ripristino, vale a dire all'applicazione, di log delle transazioni. In base ai modelli di recupero con registrazione completa e con registrazione minima delle operazioni bulk, per poter recuperare i dati è necessario eseguire backup di routine dei log delle transazioni (*backup del log*). È possibile eseguire il backup del log mentre è in esecuzione un qualsiasi backup completo. Per altre informazioni sui modelli di recupero, vedere [Backup e ripristino di database SQL Server](../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  
  
 Prima di creare il primo backup del log, è necessario creare un backup completo, ad esempio un backup del database oppure il primo di un set di backup di file. Il ripristino di un database solo tramite backup di file può essere un'operazione complessa. Quando possibile, è pertanto consigliabile iniziare con un backup completo del database. Eseguire quindi regolarmente il backup del log delle transazioni. In questo modo, è possibile non solo limitare al minimo il rischio di perdita dei dati, ma anche abilitare il troncamento del log delle transazioni. In genere, il troncamento del log delle transazioni viene eseguito dopo ogni backup del log convenzionale,  
  
> [!IMPORTANT]
> È consigliabile eseguire backup del log sufficientemente frequenti da soddisfare i requisiti aziendali e in particolare il requisito relativo alla tolleranza per eventuali perdite di dati, che potrebbero ad esempio verificarsi in seguito al danneggiamento della risorsa di archiviazione dei log. La frequenza appropriata per l'esecuzione dei backup del log viene determinata in base al raggiungimento di un compromesso tra la tolleranza per il rischio di perdita dei dati e la quantità di backup del log che è possibile archiviare, gestire e potenzialmente ripristinare. Considerare gli obiettivi [RTO](https://wikipedia.org/wiki/Recovery_time_objective) e [RPO](https://wikipedia.org/wiki/Recovery_point_objective) richiesti quando si implementa la strategia di ripristino, in particolare la frequenza di backup del log.
> Potrebbe essere sufficiente eseguire un backup del log ogni 15 - 30 minuti. Se nella propria azienda è necessario limitare al minimo il rischio di perdita dei dati, valutare se eseguire i backup del log con una maggiore frequenza. L'esecuzione di backup del log più frequenti offre il vantaggio aggiuntivo di un aumento della frequenza del troncamento del log, con una conseguente riduzione delle dimensioni dei file di log.  
  
> [!IMPORTANT]
> Per limitare il numero di backup dei log che è necessario ripristinare, è fondamentale eseguire regolarmente il backup dei dati. Ad esempio, è possibile pianificare un backup completo del database una volta la settima e backup differenziali del database una volta al giorno.  
> Anche in questo caso considerare gli obiettivi [RTO](https://wikipedia.org/wiki/Recovery_time_objective) e [RPO](https://wikipedia.org/wiki/Recovery_point_objective) richiesti quando si implementa la strategia di ripristino, in particolare la frequenza del backup completo e differenziale del database.

Per altre informazioni sui backup del log delle transazioni, vedere [Backup di log delle transazioni &#40; SQL Server&#41;](../relational-databases/backup-restore/transaction-log-backups-sql-server.md).
  
### <a name="the-log-chain"></a>Catena di log  
 Una sequenza continua di backup del log è denominata *catena di log*. Una catena di log ha inizio con un backup completo del database. In genere, una nuova catena di log viene creata solo quando si esegue il backup del database per la prima volta oppure dopo il passaggio dal modello di recupero con registrazione minima al modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk. Se si sceglie di non sovrascrivere i set di backup esistenti durante la creazione di un backup completo del database, la catena di log esistente rimane intatta. Con la catena di log intatta, è possibile ripristinare il database da qualsiasi backup completo del database nel set di supporti, seguito da tutti i backup del log successivi tramite il punto di recupero specifico. Il punto di recupero può essere la fine dell'ultimo backup del log o un punto di recupero specifico in uno dei backup del log. Per altre informazioni, vedere [Backup di log delle transazioni &#40;SQL Server&#41;](../relational-databases/backup-restore/transaction-log-backups-sql-server.md).  
  
 Per ripristinare un database al punto in cui si è verificato l'errore, è necessario che la catena di log sia intatta. In altre parole, è necessario che una sequenza non interrotta di backup del log delle transazioni si estenda fino al punto di errore. Il punto in cui la sequenza del log deve iniziare dipende dal tipo di backup dei dati che si sta ripristinando, ovvero un backup del database, parziale o di file. Nel caso di un backup del database o parziale, la sequenza di backup del log si deve estendere dalla fine di un backup del database o parziale. Nel caso di un set di backup di file, la sequenza di backup del log si deve estendere dall'inizio di un intero set di backup di file. Per altre informazioni, vedere [Applicazione dei backup di log delle transazioni &#40;SQL Server&#41;](../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
### <a name="restore-log-backups"></a>Ripristinare i backup di log  
 Il ripristino di un backup del log determina il rollforward delle modifiche registrate nel log delle transazioni in modo da ricreare l'esatto stato del database esistente all'inizio dell'operazione di backup del log. Quando si ripristina un database, è necessario ripristinare i backup del log creati dopo il backup completo del database ripristinato oppure dall'inizio del primo backup di file ripristinato. In genere, dopo il ripristino del backup dei dati o del backup differenziale più recente, è necessario ripristinare una serie di backup del log fino al punto di recupero desiderato. Recuperare quindi il database. Verrà eseguito il rollback di tutte le transazioni incomplete nel momento in cui è iniziato il recupero e verrà attivata la modalità online per il database. Dopo il recupero del database, non è possibile ripristinare altri backup. Per altre informazioni, vedere [Applicazione dei backup di log delle transazioni &#40;SQL Server&#41;](../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  

## <a name="checkpoints-and-the-active-portion-of-the-log"></a>Relazione tra i checkpoint e la parte attiva del log  

I checkpoint scaricano le pagine di dati dirty dalla cache buffer del database corrente al disco riducendo al minimo la parte attiva del log da elaborare durante un recupero con registrazione completa del database, durante il quale vengono eseguiti i tipi seguenti di azioni:

* Rollforward dei record di log relativi alle modifiche non scaricate su disco prima dell'arresto del sistema.
* Rollback di tutte le modifiche associate a transazioni incomplete, ad esempio transazioni per cui non esiste un record di log COMMIT o ROLLBACK.

### <a name="checkpoint-operation"></a>Funzionamento dei checkpoint

Un checkpoint esegue i processi seguenti nel database:

* Scrive nel file di log un record che indica l'inizio del checkpoint.
* Archivia le informazioni registrate per il checkpoint in una catena di record di log relativi al checkpoint.  
 
    Una delle informazioni registrate nel checkpoint è il numero di sequenza del file di log (LSN) del primo record che deve essere presente per poter eseguire correttamente il rollback a livello di database. Questo numero LSN è denominato LSN minimo del recupero (MinLSN). Il numero MinLSN è il valore minimo tra: 

    * Numero LSN dell'inizio del checkpoint.
    * Numero LSN dell'inizio della transazione attiva meno recente.
    * Numero LSN dell'inizio della transazione di replica meno recente non ancora recapitata al database di distribuzione. 

    I record del checkpoint contengono inoltre un elenco di tutte le transazioni attive che hanno modificato il database.

* Se il database utilizza il modello di recupero con registrazione minima, contrassegna per il riutilizzo lo spazio che precede il numero MinLSN. 
* Scrive sul disco tutte le pagine di log e di dati dirty.
* Scrive nel file di log un record che indica la fine del checkpoint.
* Scrive il numero LSN corrispondente all'inizio della catena nella pagina di avvio del database.

#### <a name="activities-that-cause-a-checkpoint"></a>Attività che causano un checkpoint

I checkpoint vengono eseguiti nelle situazioni seguenti:

* Viene eseguita esplicitamente un'istruzione CHECKPOINT. Viene eseguito un checkpoint nel database corrente per la connessione.
* Nel database viene eseguita un'operazione con registrazione minima, ad esempio viene eseguita un'operazione di copia bulk in un database che utilizza il modello di recupero con registrazione minima delle operazioni bulk. 
* Vengono aggiunti o rimossi file di database utilizzando l'istruzione ALTER DATABASE.
* Un'istanza di SQL Server viene arrestata da un'istruzione SHUTDOWN o dall'arresto del servizio SQL Server (MSSQLSERVER). Entrambe le azioni causano un checkpoint in ogni database dell'istanza di SQL Server.
* Un'istanza di SQL Server genera periodicamente checkpoint automatici in ogni database per ridurre il tempo necessario all'istanza per il recupero del database.
* Viene eseguito un backup del database.
* Viene eseguita un'attività che richiede la chiusura di un database. Ad esempio, AUTO_CLOSE è impostata su ON e la connessione al database dell'ultimo utente viene chiusa, oppure viene eseguita una modifica a un'opzione di database che richiede un riavvio del database.

### <a name="automatic-checkpoints"></a>Checkpoint automatici

Il motore di database di SQL Server genera checkpoint automatici. L'intervallo fra i checkpoint automatici dipende dalla quantità di spazio di log utilizzata e dal tempo trascorso dall'ultimo checkpoint. Questo intervallo di tempo è estremamente variabile e se al database vengono apportate poche modifiche può essere molto lungo. I checkpoint automatici possono anche essere eseguiti di frequente, se si modificano grandi quantità di dati.

Usare l'opzione di configurazione del server dell' **intervallo di recupero** per calcolare l'intervallo tra i checkpoint automatici per tutti i database in un'istanza del server. Questa opzione specifica il periodo di tempo massimo che può essere usato dal motore di database per recuperare un database durante un riavvio del sistema. Il motore di database stima il numero di record di log che è possibile elaborare nell' **intervallo di recupero** durante un'operazione di recupero. 

L'intervallo fra i checkpoint automatici dipende anche dal modello di recupero:

* Se il database usa il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, viene generato un checkpoint automatico ogni volta che il numero di record di log raggiunge il valore che il motore di database stima sia possibile elaborare nel periodo di tempo specificato dall'opzione dell'intervallo di recupero.
* Se il database utilizza il modello di recupero con registrazione minima, viene generato un checkpoint automatico ogni volta che il numero di record di log raggiunge il minore tra i valori seguenti: 

    * Il log viene riempito al 70%.
    * Il numero di record di log raggiunge il valore che il motore di database stima sia possibile elaborare nel periodo di tempo specificato dall'opzione dell'intervallo di recupero.

Per altre informazioni sull'impostazione dell'intervallo di recupero, vedere [Configurare l'opzione di configurazione del server dell'intervallo di recupero](../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).

> [!TIP]  
> L'opzione di impostazione avanzata -k di SQL Server consente all'amministratore del database di limitare il comportamento di I/O del checkpoint in base alla velocità effettiva del sottosistema di I/O per alcuni tipi di checkpoint. L'opzione di impostazione -k si applica ai checkpoint automatici e ai checkpoint senza limitazione. 
 
I checkpoint automatici troncano la parte non utilizzata del log delle transazioni se il database utilizza il modello di recupero con registrazione minima, ma non se il database utilizza il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk. Per altre informazioni, vedere [Log delle transazioni &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md). 

L'istruzione CHECKPOINT offre ora l'argomento facoltativo checkpoint_duration che specifica il tempo necessario in secondi per il completamento dei checkpoint. Per altre informazioni, vedere [CHECKPOINT &#40;Transact-SQL&#41;](../t-sql/language-elements/checkpoint-transact-sql.md).

### <a name="active-log"></a>log attivo

La parte del file di log compresa tra il numero MinLSN e l'ultimo record di log scritto viene definita parte attiva del log o log attivo. ed è necessaria per eseguire il recupero con registrazione completa del database. Non è possibile troncare nessuna parte del log attivo. Tutti i record del log devono essere troncati dalle parti del log che precedono il numero MinLSN.

Nella figura seguente viene illustrata una versione semplificata della parte finale di un log delle transazioni con due transazioni attive. I record di checkpoint sono stati compattati in un unico record.

![Illustra un log di fine delle transazioni con due transazioni attive e un record di checkpoint compresso](../relational-databases/media/active-log.png) 

LSN 148 è l'ultimo record del log delle transazioni. Quando è stato elaborato il checkpoint registrato in corrispondenza del numero LSN 147, era stato eseguito il commit di Tran 1 e Tran 2 era l'unica transazione attiva. Pertanto, il primo record di log di Tran 2 è il meno recente di una transazione attiva al momento dell'ultimo checkpoint e, di conseguenza, il numero MinLSN corrisponde a LSN 142, ovvero al record di inizio della transazione Tran 2.

### <a name="long-running-transactions"></a>Transazioni con esecuzione prolungata
Il log attivo deve includere tutte le parti di tutte le transazioni di cui non è stato eseguito il commit. Un'applicazione che avvia una transazione e non ne esegue il commit o il rollback impedisce al [!INCLUDE[ssde_md](../includes/ssde_md.md)] di far aumentare il numero MinLSN. Ciò può causare due tipi di problemi:

* Se il sistema viene arrestato dopo che la transazione ha eseguito numerose modifiche di cui non è stato eseguito il commit, la fase di recupero del riavvio successivo può richiedere tempi notevolmente più lunghi rispetto al valore specificato dall'opzione dell' **intervallo di recupero** .
* È possibile che il log raggiunga dimensioni considerevoli in quanto non può essere troncato dopo il numero MinLSN. Ciò si verifica anche se il database utilizza il modello di recupero con registrazione minima, in base al quale il log delle transazioni viene in genere troncato in corrispondenza di ogni checkpoint automatico.

A partire da [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] e in [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)] è possibile evitare il ripristino di transazioni con esecuzione prolungata e i problemi descritti in precedenza usando il [recupero del database accelerato](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr).  

### <a name="replication-transactions"></a>Transazioni di replica
L'agente di lettura log esegue il monitoraggio del log delle transazioni di tutti i database configurati per la replica transazionale e copia le transazioni contrassegnate per la replica dal log delle transazioni al database di distribuzione. Il log attivo deve contenere tutte le transazioni contrassegnate per la replica, ma non ancora recapitate al database di distribuzione. Se tali transazioni non vengono replicate tempestivamente, potrebbero impedire il troncamento del log. Per altre informazioni, vedere [Replica transazionale](../relational-databases/replication/transactional/transactional-replication.md).

## <a name="see-also"></a>Vedere anche 
Per altre informazioni sul log delle transazioni e sulle procedure consigliate, vedere gli articoli e le pubblicazioni elencate di seguito.  
  
[Log delle transazioni &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md)    
[Gestione delle dimensioni del file di log delle transazioni](../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)   
[Backup di log delle transazioni &#40;SQL Server&#41;](../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
[Checkpoint di database &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md)   
[Configurare l'opzione di configurazione del server intervallo di recupero](../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)    
[Recupero del database accelerato](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr)       
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)    
[Informazioni sulla registrazione e il recupero in SQL Server di Paul Randall](/previous-versions/technet-magazine/dd392031(v=msdn.10))    
[Gestione del log delle transazioni di SQL Server di Tony Davis e Gail Shaw](https://www.simple-talk.com/books/sql-books/sql-server-transaction-log-management-by-tony-davis-and-gail-shaw/)  
  
