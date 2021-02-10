---
title: Aggiornare i database tramite l'Assistente ottimizzazione query
description: Informazioni su come l'Assistente ottimizzazione query illustra il flusso di lavoro consigliato per garantire la stabilità delle prestazioni durante gli aggiornamenti alle versioni di SQL Server più recenti.
ms.custom: seo-dt-2019
ms.date: 02/13/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.querytuning.f1
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811e7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 1032f8e1ac32a08c9a6878c54fac0ee5e5c213e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015496"
---
# <a name="upgrading-databases-by-using-the-query-tuning-assistant"></a>Aggiornamento di database mediante l'Assistente ottimizzazione query

[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Quando si esegue la migrazione da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o versioni successive e si aggiorna il [livello di compatibilità del database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) alla versione più recente disponibile, un carico di lavoro può essere esposto al rischio di regressione delle prestazioni. Questo è anche possibile in misura minore durante l'aggiornamento da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a una nuova versione.

A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e per tutte le nuove versioni, tutte le modifiche di Query Optimizer sono associate al livello di compatibilità del database più recente, quindi i piani non vengono modificati esattamente nel punto di aggiornamento, ma quando un utente passa dall'opzione di database `COMPATIBILITY_LEVEL` alla versione più recente disponibile. Per altre informazioni sulle modifiche a Query Optimizer introdotte in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], vedere [Stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md). Per altre informazioni sui livelli di compatibilità e sul loro possibile effetto sugli aggiornamenti, vedere [Livelli di compatibilità e aggiornamenti del motore di database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades).

Questa funzionalità di collegamento offerta dal livello di compatibilità del database in combinazione con Query Store offre un livello ottimale di controllo delle prestazioni delle query nel processo di aggiornamento, se l'aggiornamento segue il flusso di lavoro consigliato illustrato di seguito. Per altre informazioni sul flusso di lavoro consigliato per l'aggiornamento del livello di compatibilità, vedere [Modificare la modalità di compatibilità del database e usare il Query Store](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). 

![Flusso di lavoro del processo di aggiornamento del database consigliato con Query Store](../../relational-databases/performance/media/query-store-usage-5.png "Flusso di lavoro del processo di aggiornamento del database consigliato con Query Store") 

Questo controllo sugli aggiornamenti è stato migliorato ulteriormente in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] con l'introduzione di [Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md) e consente di automatizzare l'ultimo passaggio del flusso di lavoro consigliato visualizzato sopra.

A partire da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 la nuova funzionalità **Assistente ottimizzazione query (QTA, Query Tuning Assistant)** assiste gli utenti nel flusso di lavoro consigliato per garantire la stabilità delle prestazioni durante l'aggiornamento alle versioni più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come documentato nella sezione *Mantenere la stabilità delle prestazioni durante l'aggiornamento alla nuova versione di SQL Server* di [Scenari di utilizzo di Query Store](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). L'Assistente ottimizzazione query, tuttavia, non esegue il rollback a un piano valido noto in precedenza, come illustrato nell'ultimo passaggio del flusso di lavoro consigliato. L'Assistente ottimizzazione query terrà invece traccia di eventuali regressioni trovate nella vista [**Query regredite** di Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed) ed eseguirà l'iterazione delle possibili permutazioni delle varianti del modello di ottimizzazione applicabili in modo che possa essere generato un piano nuovo e migliore.

> [!IMPORTANT]
> L'Assistente ottimizzazione query non genera carichi di lavoro utente. Se si esegue l'Assistente ottimizzazione query in un ambiente non usato dalle applicazioni, verificare che sia comunque possibile eseguire un carico di lavoro di test rappresentativo nel [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di destinazione in altro modo.

## <a name="the-query-tuning-assistant-workflow"></a>Flusso di lavoro dell'Assistente ottimizzazione query

Il punto di partenza dell'Assistente ottimizzazione query presuppone che un database di una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] venga spostato (tramite [CREATE DATABASE... FOR ATTACH](../..//relational-databases/databases/attach-a-database.md) o [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)) a una versione più recente di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e che il livello di compatibilità del database prima dell'aggiornamento non venga modificato immediatamente. L'Assistente ottimizzazione query aiuta nell'esecuzione dei passaggi seguenti:

1. Configurare Query Store in base alle impostazioni consigliate per la durata del carico di lavoro (in giorni) impostata dall'utente. Considerare la durata del carico di lavoro corrispondente al ciclo operativo tipico.
2. Richiedere l'avvio del carico di lavoro obbligatorio, in modo che Query Store possa raccogliere una baseline di dati del carico di lavoro (se non sono ancora disponibili).
3. Eseguire l'aggiornamento al livello di compatibilità del database di destinazione scelto dall'utente.
4. Richiedere la raccolta di una seconda estrazione di dati del carico di lavoro, per il confronto e il rilevamento della regressione.
5. Eseguire l'iterazione su eventuali regressioni in base alla vista [Query Store **Query regredite**](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), sperimentare raccogliendo statistiche di runtime sulle possibili permutazioni di variazioni del modello di ottimizzazione applicabile e misurare i risultati. 
6. Creare report sui miglioramenti misurati e facoltativamente consentire che le modifiche vengano rese persistenti usando [guide di piano](../../relational-databases/performance/plan-guides.md).

Per altre informazioni sul collegamento di un database, vedere [Collegamento e scollegamento di database](../../relational-databases/databases/database-detach-and-attach-sql-server.md#AttachDb).

Vedere di seguito come l'Assistente ottimizzazione query cambia solo gli ultimi passaggi del flusso di lavoro consigliato per aggiornare il livello di compatibilità mediante Query Store illustrato sopra. Invece di offrire la possibilità di scegliere tra il piano di esecuzione che attualmente risulta inefficiente e l'ultimo piano di esecuzione ottimale, l'Assistente ottimizzazione query offre opzioni di regolazione specifiche per le query regredite selezionate, al fine di creare un nuovo stato migliorato con i piani di esecuzione ottimizzati.

![Flusso di lavoro del processo di aggiornamento del database consigliato l'Assistente ottimizzazione query](../../relational-databases/performance/media/qta-usage.png "Flusso di lavoro del processo di aggiornamento del database consigliato con l'Assistente ottimizzazione query")

### <a name="qta-tuning-internal-search-space"></a>Spazio di ricerca interno per l'ottimizzazione dell'Assistente ottimizzazione query

L'Assistente ottimizzazione query prende in esame solo le query `SELECT` che possono essere eseguite da Query Store. Le query con parametri sono idonee se il parametro compilato è noto. Le query che dipendono da costrutti di runtime, ad esempio tabelle temporanee o variabili di tabella, non sono idonee in questa fase.

L'Assistente ottimizzazione query rileva possibili criteri noti di regressioni delle query derivanti da modifiche alle versioni di [Stima della cardinalità (CE, Cardinality Estimator)](../../relational-databases/performance/cardinality-estimation-sql-server.md). Ad esempio, quando si aggiorna un database da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e livello di compatibilità 110 a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e livello di compatibilità 140, alcune query potrebbero regredire, perché sono state progettate specificamente per funzionare con la versione di Stima della cardinalità presente in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (Stima della cardinalità 70). Ciò non significa che il ripristino da Stima della cardinalità 140 a Stima della cardinalità 70 sia l'unica opzione. Se la regressione è originata da una sola modifica specifica nella versione più recente, è possibile suggerire che la query usi solo la parte rilevante della versione di Stima della cardinalità precedente (che funzionava meglio per la query specifica) pur sfruttando tutti gli altri miglioramenti delle versioni di Stima della cardinalità più recenti. In questo modo sarà possibile consentire alle altre query del carico di lavoro che non hanno regredito di sfruttare i miglioramenti delle versioni più recenti di Stima della cardinalità.

I criteri di Stima della cardinalità cercati dall'Assistente ottimizzazione query sono i seguenti:

- **Indipendenza rispetto a correlazione**: se il presupposto di indipendenza offre stime migliori per la query specifica, l'hint per la query `USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES')` fa sì che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generi un piano di esecuzione usando una selettività minima quando stima predicati `AND` per i filtri per rendere conto della correlazione. Per altre informazioni, vedere [Hint per le query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) e [Versioni di Stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Indipendenza semplice rispetto a indipendenza del database**: se un'indipendenza di join diversa offre stime migliori per la query specifica, l'hint per la query `USE HINT ('ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS')` fa sì che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generi un piano di esecuzione mediante il presupposto Simple Containment (Indipendenza semplice) anziché mediante il presupposto predefinito Base Containment (Indipendenza di base). Per altre informazioni, vedere [Hint per le query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) e [Versioni di Stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Stima di cardinalità fissa della funzione con valori di tabella a più istruzioni (MSTVF)** di 100 righe rispetto a 1 riga: se la stima fissa predefinita per le funzioni con valori di tabella di 100 righe non restituisce un piano più efficiente di quello ottenuto usando la stima fissa predefinita per le funzioni con valori di tabella di 1 riga (corrispondente all'impostazione predefinita con il modello Query Optimizer Stima della cardinalità di [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] e versioni precedenti), l'hint per la query `QUERYTRACEON 9488` viene usato per generare un piano di esecuzione. Per altre informazioni su MSTVF, vedere [Creare funzioni definite dall'utente &#40;Motore di database&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

> [!NOTE]
> Come ultima risorsa, se gli hint con ambito limitato non restituiscono risultati soddisfacenti per i modelli di query idonei, viene preso in considerazione anche l'uso completo di Stima della cardinalità 70, usando l'hint per la query `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')` usato per generare un piano di esecuzione.

> [!IMPORTANT]
> Qualsiasi hint impone determinati comportamenti che potrebbero essere presi in considerazione per gli aggiornamenti futuri di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È consigliabile applicare gli hint solo quando non è disponibile nessun'altra opzione e si prevede di rivedere il codice con hint a ogni nuovo aggiornamento. Imponendo i comportamenti è possibile che si impedisca al carico di lavoro di sfruttare i vantaggi dei miglioramenti introdotti nelle versioni più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="starting-query-tuning-assistant-for-database-upgrades"></a>Avvio dell'Assistente ottimizzazione Query per gli aggiornamenti del database

L'Assistente ottimizzazione Query è una funzionalità basata sulla sessione che archivia lo stato della sessione nello schema `msqta` del database utente in cui una sessione viene creata per la prima volta. Nel tempo è possibile creare più sessioni di ottimizzazione per un singolo database, ma per un database specifico può essere presente una sola sessione attiva.

### <a name="creating-a-database-upgrade-session"></a>Creazione di una sessione di aggiornamento del database

1. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aprire Esplora oggetti e connettersi a [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. Per il database di cui si vuole aggiornare il livello di compatibilità, fare clic con il pulsante destro del mouse sul nome del database, selezionare **Attività**, **Aggiornamento database** e infine **Nuova sessione di aggiornamento database**.

3. Nella finestra di Assistente ottimizzazione query guidata sono necessari due passaggi per configurare una sessione:

    1. Nella finestra **Configura** configurare Query Store per acquisire l'equivalente di un ciclo aziendale completo di dati del carico di lavoro da analizzare e ottimizzare.
        - Immettere la durata prevista del carico di lavoro in giorni (il valore minimo è 1 giorno). Questo valore verrà usato per proporre impostazioni consigliate di Query Store e provare a consentire la raccolta dell'intera baseline. L'acquisizione di una baseline ottimale è importante per garantire che sia possibile esaminare le query regredite dopo la modifica del livello di compatibilità del database. 
        - Impostare il livello di compatibilità del database di destinazione previsto per il database utente al termine il flusso di lavoro l'Assistente ottimizzazione query.
        Al termine, selezionare **Avanti**.

       ![Finestra di configurazione della sessione di aggiornamento del nuovo database](../../relational-databases/performance/media/qta-new-session-setup.png "Finestra di configurazione di aggiornamento del nuovo database")  
  
    2. Nella finestra **Impostazioni** due colonne indicano lo stato **Corrente** di Query Store nel database di destinazione, nonché le impostazioni **Consigliate**. 
        - Le impostazioni consigliate sono selezionate per impostazione predefinita, ma, se si seleziona il pulsante di opzione sopra la colonna Corrente, si accettano le impostazioni correnti ed è anche possibile ottimizzare la configurazione corrente di Query Store.
        - L'impostazione *Soglia per query non aggiornate* è pari al doppio della durata in giorni prevista per il carico di lavoro. Questo dipende dal fatto che Query Store dovrà conservare informazioni sul carico di lavoro baseline e sul carico di lavoro successivo all'aggiornamento del database.
        Al termine, selezionare **Avanti**.

       ![Finestra delle impostazioni di aggiornamento del nuovo database](../../relational-databases/performance/media/qta-new-session-settings.png "Finestra delle impostazioni di aggiornamento del nuovo database")

       > [!IMPORTANT]
       > Il valore *Dimensioni massime* proposto è un valore arbitrario, che può essere adatto a un carico di lavoro di breve durata.
       > Tenere tuttavia presente che questo valore può essere insufficiente per contenere informazioni sui carichi di lavoro baseline e successivi all'aggiornamento del database in caso di carichi di lavoro particolarmente intensi, ad esempio quando è necessario generare più piani diversi.
       > Se si prevede uno scenario del genere immettere un valore più elevato, adatto alle esigenze contingenti.

4. La finestra **Regolazione** completa la configurazione della sessione e indica i passaggi successivi per aprire e svolgere la sessione. Al termine, fare clic su **Fine**.

    ![Finestra della regolazione di aggiornamento del nuovo database](../../relational-databases/performance/media/qta-new-session-tuning.png "Finestra della regolazione di aggiornamento del nuovo database")

> [!NOTE]
> Un possibile scenario alternativo inizia con il ripristino di un backup di database dal server di produzione (in cui il database è già stato sottoposto al flusso di lavoro di aggiornamento del livello di compatibilità del database consigliato) a un server di test.

### <a name="executing-the-database-upgrade-workflow"></a>Esecuzione del flusso di lavoro di aggiornamento del database

1. Per il database di cui si vuole aggiornare il livello di compatibilità, fare clic con il pulsante destro del mouse sul nome del database, selezionare **Attività**, **Aggiornamento database** e infine **Monitora sessioni**.

2. La pagina **Gestione delle sessioni** elenca la sessione corrente e le sessioni correnti e precedenti per il database nell'ambito. Selezionare la sessione desiderata e selezionare **Dettagli**.

    > [!NOTE]
    > Se la sessione corrente non è presente, selezionare il pulsante **Aggiorna**.

    L'elenco contiene le informazioni seguenti:
    - **ID sessione**
    - **Nome sessione**: nome generato dal sistema e costituito dal nome del database e dalla data e ora di creazione della sessione.
    - **Stato**: stato della sessione (Attiva o Chiusa).
    - **Descrizione**: elemento generato dal sistema che include il livello di compatibilità del database di destinazione selezionato dall'utente e il numero di giorni per il carico di lavoro del ciclo aziendale.
    - **Ora avvio**: Data e ora di creazione della sessione.

    ![Pagina Gestione delle sessioni dell'Assistente ottimizzazione query](../../relational-databases/performance/media/qta-session-management.png "Pagina Gestione delle sessioni dell'Assistente ottimizzazione query")

    > [!NOTE]
    > **Elimina sessione**: elimina tutti i dati archiviati per la sessione selezionata.
    > Tuttavia l'eliminazione di una sessione chiusa **non** elimina le guide di piano distribuite in precedenza.
    > Se si elimina una sessione che aveva distribuito guide di piano, non è possibile usare l'Assistente ottimizzazione query per eseguire il rollback.
    > Cercare invece le guide di piano mediante la tabella di sistema [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) ed eliminarle manualmente con [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).
  
3. Il punto di ingresso per una nuova sessione è il passaggio **Raccolta dati**.

    > [!NOTE]
    > Il pulsante **Sessioni** torna ad aprire la pagina **Gestione delle sessioni**, lasciando invariata la sessione attiva.

    Questo passaggio include tre passaggi secondari:

    1. **Raccolta di dati baseline** richiede all'utente di eseguire il ciclo del carico di lavoro rappresentativo, in modo che Query Store possa raccogliere una baseline. Dopo il completamento del carico di lavoro, selezionare **Esecuzione del carico di lavoro completata** e selezionare **Avanti**.

        > [!NOTE]
        > La finestra dell'Assistente ottimizzazione query può essere chiusa mentre il carico di lavoro è in esecuzione. Se in un secondo momento si torna alla sessione che rimane in stato attivo, questa verrà ripresa dallo stesso passaggio in cui è stata interrotta. 

        ![Passaggio secondario 1 del passaggio 2 dell'Assistente ottimizzazione query](../../relational-databases/performance/media/qta-step2-substep1.png "Passaggio secondario 1 del passaggio 2 dell'Assistente ottimizzazione query")

    2. **Aggiornamento del database** richiederà l'autorizzazione per aggiornare il livello di compatibilità del database al livello desiderato. Per procedere con il passaggio secondario successivo, selezionare **Sì**.

        ![Passaggio secondario 2 del passaggio 2 dell'Assistente ottimizzazione query - Non è stato possibile aggiornare il livello di compatibilità del database](../../relational-databases/performance/media/qta-step2-substep2-prompt.png "Passaggio secondario 2 del passaggio 2 dell'Assistente ottimizzazione query - Non è stato possibile aggiornare il livello di compatibilità del database")

        La pagina seguente conferma che il livello di compatibilità del database è stato aggiornato correttamente.

        ![Passaggio secondario 2 del passaggio 2](../../relational-databases/performance/media/qta-step2-substep2.png "Passaggio secondario 2 del passaggio 2 dell'Assistente ottimizzazione query")

    3. **Raccolta dati osservati** richiede all'utente di eseguire nuovamente il ciclo del carico di lavoro rappresentativo, per consentire a Query Store di acquisire una baseline di confronto che verrà usata per la ricerca di opportunità di ottimizzazione. Durante l'esecuzione del carico di lavoro, usare il pulsante **Aggiorna** per aggiornare l'elenco delle query regredite, se presenti. Modificare il valore **Query da mostrare** per limitare il numero di query visualizzate. L'ordine dell'elenco è influenzato dai valori **Metrica** (Durata o CpuTime) e **Aggregazione** (il valore predefinito è Media). Selezionare anche un valore per **Query da mostrare**. Dopo il completamento del carico di lavoro, selezionare **Esecuzione del carico di lavoro completata** e selezionare **Avanti**.

        ![Passaggio secondario 3 del passaggio 2](../../relational-databases/performance/media/qta-step2-substep3.png "Passaggio secondario 3 del passaggio 2 dell'Assistente ottimizzazione query")

        L'elenco contiene le informazioni seguenti:
        - **ID query** 
        - **Testo query**: istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che può essere espansa selezionando il pulsante **...** .
        - **Esecuzioni**: numero di esecuzioni della query per l'intera raccolta del carico di lavoro.
        - **Metrica baseline**: metrica selezionata (Durata o CpuTime) in millisecondi per la raccolta di dati baseline prima dell'aggiornamento di compatibilità del database.
        - **Metrica osservata**: metrica selezionata (Durata o CpuTime) in millisecondi per la raccolta di dati dopo l'aggiornamento di compatibilità del database.
        - **% di modifica**: percentuale di variazione della metrica selezionata tra lo stato prima e dopo l'aggiornamento di compatibilità del database. Un numero negativo rappresenta la quantità di regressione misurata per la query.
        - **Ottimizzabile**: *True* o *False* a seconda che la query sia o meno idonea per la sperimentazione.

4. **Visualizzazione analisi** consente la selezione delle query per la sperimentazione e il rilevamento di opportunità di ottimizzazione. Il valore **Query da mostrare** diventa l'ambito delle query idonee per la sperimentazione. Dopo la verifica delle query desiderate, selezionare **Avanti** per iniziare la sperimentazione.

    > [!NOTE]
    > Le query con l'impostazione Ottimizzabile = False non possono essere selezionate per la sperimentazione.

    > [!IMPORTANT]
    > Un messaggio informa che quando l'Assistente ottimizzazione query passa alla fase di sperimentazione, non sarà possibile tornare alla pagina Visualizzazione analisi.   
    > Se non si selezionano tutte le query idonee prima di passare alla fase di sperimentazione, sarà necessario creare una nuova sessione in un secondo momento e ripetere il flusso di lavoro. Questo richiede la reimpostazione del livello di compatibilità del database sul valore precedente.

    ![Passaggio 3 dell'Assistente ottimizzazione query](../../relational-databases/performance/media/qta-step3.png "Passaggio 3 dell'Assistente ottimizzazione query")

5. **Visualizzazione risultati** consente di selezionare le query a cui distribuire l'ottimizzazione proposta come guida di piano. 

    L'elenco contiene le informazioni seguenti:
    - **ID query**
    - **Testo query**: istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che può essere espansa selezionando il pulsante **...** .
    - **Stato**: visualizza lo stato di sperimentazione corrente per la query.
    - **Metrica baseline**: la metrica selezionata (Durata o CpuTime) in ms per le query eseguita nel **Passaggio 2 passaggio secondario 3**, che rappresenta la query regredita dopo l'aggiornamento di compatibilità del database.
    - **Metrica osservata**: la metrica selezionata (durata o CpuTime) in ms per la query dopo la sperimentazione, per un'ottimizzazione proposta con risultati soddisfacenti.
    - **% di modifica**: percentuale di modifica per la metrica selezionata tra lo stato prima e dopo la sperimentazione, che rappresenta la quantità di miglioramento misurata per la query con l'ottimizzazione proposta.
    - **Opzione di query**: collegamento all'hint proposto che migliora la metrica di esecuzione di query.
    - **È possibile distribuire**: *True* o *False* a seconda che l'ottimizzazione della query proposta possa essere o meno distribuita come guida di piano.

    ![Passaggio 4 dell'Assistente ottimizzazione query](../../relational-databases/performance/media/qta-step4.png "Passaggio 4 dell'Assistente ottimizzazione query")

6. **Verifica** Mostra lo stato di distribuzione delle query selezionate in precedenza per questa sessione. L'elenco in questa pagina è diverso da quello della pagina precedente, perché l'opzione **È possibile distribuire** qui è **È possibile eseguire il rollback**. Questa colonna può essere *True* o *False* a seconda che sia possibile o meno eseguire il rollback dell'ottimizzazione delle query distribuite e rimuovere la guida di piano.

    ![Passaggio 5 dell'Assistente ottimizzazione query](../../relational-databases/performance/media/qta-step5.png "Passaggio 5 dell'Assistente ottimizzazione query")

    Se in un secondo momento è necessario eseguire il rollback su un'ottimizzazione proposta, selezionare la query corrispondente e selezionare **Rollback**. La guida di piano della query viene rimossa e l'elenco viene aggiornato per rimuovere la query di cui è stato eseguito il rollback. Si noti che nell'immagine seguente è stata rimossa la query 8.

    ![Passaggio 5 dell'Assistente ottimizzazione query - Rollback](../../relational-databases/performance/media/qta-step5-rollback.png "Passaggio 5 dell'Assistente ottimizzazione query - Rollback")

    > [!NOTE]
    > L'eliminazione di una sessione chiusa **non** elimina le guide di piano distribuite in precedenza.
    > Se si elimina una sessione che aveva distribuito guide di piano, non è possibile usare l'Assistente ottimizzazione query per eseguire il rollback.
    > Cercare invece le guide di piano mediante la tabella di sistema [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) ed eliminarle manualmente con [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).
  
## <a name="permissions"></a>Autorizzazioni

È necessaria l'appartenenza al ruolo **db_owner**.
  
## <a name="see-also"></a>Vedere anche

- [Livelli di compatibilità e aggiornamenti del motore di database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)
- [Strumenti per il monitoraggio e l'ottimizzazione delle prestazioni](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Modificare la modalità di compatibilità del database e usare l'archivio query](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [Hint per le query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)
- [Stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md)   
- [Usare l'Assistente ottimizzazione query di SQL Server](/learn/modules/use-sql-server-query-tuning-assistant/)