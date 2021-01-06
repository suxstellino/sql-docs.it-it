---
title: Finestre di dialogo di SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Esplorare i comandi e le impostazioni disponibili in SQL Server Profiler per l'uso in tracce, modelli, file, tabelle, contatori delle prestazioni e riproduzioni.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: reference
ms.assetid: e57b9160-4b78-4353-abb2-bfdbdf523d7a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 6b6e67a848b365a32a1177d2655b49992ef185eb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643440"
---
# <a name="sql-server-profiler-dialog-boxes"></a>Finestre di dialogo di SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Microsoft [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] è uno strumento in grado di acquisire eventi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un server. Gli eventi vengono salvati in un file di traccia che è possibile analizzare o utilizzare in un momento successivo per riprodurre una serie specifica di passaggi allo scopo di diagnosticare un problema. I seguenti sono i comandi e le impostazioni disponibili nelle finestre di dialogo di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
## <a name="trace-properties"></a>Proprietà traccia
### <a name="general-tab"></a>Scheda Generale
Usare la scheda **Generale** della finestra di dialogo **Proprietà traccia** per visualizzare o specificare le proprietà di una traccia.  

|Elemento|Descrizione
|---|---
|**Nome traccia** |Consente di specificare il nome della traccia.  
|**Nome provider di traccia**|Visualizza il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da inserire nella traccia. In questo campo viene inserito automaticamente il nome del server specificato al momento della connessione. Per modificare il nome del provider di traccia, fare clic su **Annulla** per chiudere la finestra di dialogo e avviare una nuova traccia.  
|**Tipo provider di traccia**|Visualizza il tipo di server che fornisce la traccia. Il campo **Tipo provider di traccia** viene popolato automaticamente dal file di definizione della traccia. Questo campo non può essere modificato.  
|**version**|Visualizza la versione del server che fornisce la traccia. Il campo **Versione** viene popolato automaticamente dal file di definizione della traccia. Questo campo non può essere modificato.  
|**Modello**|Consente di selezionare un modello dalla directory dei modelli. Questa directory viene popolata con i modelli predefiniti ed eventuali modelli definiti dall'utente creati per il tipo di provider di traccia corrente.  
|**Salva nel file**|Consente di acquisire i dati di traccia in un file trc. Il salvataggio dei dati di traccia risulta utile per eseguire analisi e controlli successivi.  
|**Dimensioni massime del file (MB)**|Se si sceglie di salvare i dati di traccia in un file, è necessario specificare le dimensioni massime del file di traccia. Il valore predefinito è 5 megabyte (MB). Le dimensioni massime sono limitate solo dal file system (NTFS, FAT) in cui viene salvato il file.  
|**Save As**|Se si è scelto di eseguire il salvataggio, è possibile fare clic su questa icona per modificare il nome del file.  
|**Consenti rollover dei file**|Selezionare questa opzione per abilitare la creazione di file aggiuntivi in cui acquisire i dati di traccia al raggiungimento delle dimensioni massime del file. Il nome di ogni nuovo file è composto dal nome del file trc originale e da un numero progressivo. Quando vengono ad esempio raggiunte le dimensioni massime del file **NewTrace.trc** , quest'ultimo viene chiuso e viene aperto un nuovo file, **NewTrace_1.trc**, seguito a sua volta da **NewTrace_2.trc** e così via. Quando si salva una traccia in un file, il rollover dei file è abilitato per impostazione predefinita.  
|**Dati di traccia elaborati dal server**|Consente di specificare che l'elaborazione dei dati di traccia deve essere eseguita dal server che esegue la traccia. Questa opzione consente di limitare l'overhead delle prestazioni causata dalla traccia. Se questa casella di controllo è selezionata nessun evento viene ignorato, anche in condizioni di sovraccarico. Se questa casella di controllo è deselezionata, l'elaborazione viene eseguita da SQL Server Profiler ed è possibile che alcuni eventi non vengano tracciati in condizioni di sovraccarico.  
|**Salva nella tabella**|Consente di memorizzare i dati di traccia in una tabella di database. Il salvataggio dei dati di traccia risulta utile per eseguire analisi e controlli successivi. Tuttavia il salvataggio dei dati di traccia in una tabella può causare un notevole overhead nel server in cui viene salvata la traccia. Se possibile, non salvare la tabella di traccia sullo stesso server tracciato.  
|**Tabella di destinazione**|Se si è scelto di eseguire il salvataggio dei dati della traccia in una tabella di database, è possibile fare clic su questa icona per modificare il nome della tabella.  
| **Numero massimo di righe (in migliaia)**|Consente di specificare il numero massimo di righe in cui salvare i dati. Il valore predefinito è 1000 righe. 
|**Data e ora di arresto della traccia**|Consente di impostare la data e l'ora di interruzione della traccia e la relativa chiusura. 

### <a name="events-selection-tab"></a>Scheda Selezione eventi
Usare la scheda **Selezione eventi** della finestra di dialogo **Proprietà traccia** per visualizzare o specificare le colonne di dati e gli eventi inseriti nella traccia.  

|Elemento|Descrizione
|---|---
|Colonna **Eventi**|È possibile specificare gli eventi inseriti nella traccia selezionando o deselezionando la casella di controllo nella colonna di evento. Le colonne **Eventi** sono organizzate in base alla categoria di evento. Le classi di evento specificate nel modello vengono selezionate automaticamente. Per altre informazioni, vedere [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Colonne di dati|È possibile specificare le colonne di dati inseriti nella traccia selezionando la casella che corrisponde all'evento e alla colonna di dati necessari. Tutte le colonne di evento significative sono selezionate per impostazione predefinita per ogni evento incluso della traccia.  
|Filtri|Per specificare i filtri, fare clic sull'intestazione della colonna di dati e immettere i criteri di filtro. Le colonne di dati filtrate sono contrassegnate da un'icona di filtro a sinistra dell'etichetta della colonna nella finestra di dialogo **Modifica filtro** . Per altre informazioni, vedere [SQL Server Profiler - Modifica filtro]().  
|**Mostra tutti gli eventi**|Consente di visualizzare tutti gli eventi disponibili. Per impostazione predefinita, vengono visualizzate solo le righe della griglia **Selezione eventi** selezionate. Deselezionare questa casella di controllo per nascondere tutti gli eventi non selezionati nella griglia **Selezione eventi** .  
|**Mostra tutte le colonne**|Consente di visualizzare tutte le colonne di dati disponibili. Per impostazione predefinita, vengono visualizzate solo le colonne di dati selezionate. Deselezionare questa casella di controllo per nascondere tutte le colonne di dati non selezionate nella griglia **Selezione eventi** .  
|**Filtri colonne**|Consente di aprire la finestra di dialogo **Modifica filtro** , utilizzabile per modificare i filtri delle colonne di dati.  
|**Organizza colonne**|Consente di modificare l'ordine delle colonne nella traccia e di raggruppare i risultati in base a una o più colonne.  

## <a name="trace-template-properties"></a>Proprietà modello di traccia 
### <a name="new-general-tab"></a>Nuovo (scheda Generale)
Utilizzare la scheda **Generale** della finestra di dialogo **Proprietà modello di traccia** per creare nuovi modelli di traccia utilizzando le opzioni seguenti. Per accedere a questa finestra di dialogo, nel menu **File** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] scegliere **Modelli** e quindi fare clic su **Nuovo**.

|Elemento|Descrizione
|---|---
|**Tipo server**|Consente di specificare il tipo di server per il quale verrà utilizzato il modello.  
|**Nome nuovo modello**|Consente di fornire un nome descrittivo per il modello.  
|**Basa il nuovo modello sul seguente modello esistente**|Consente di utilizzare un modello incluso nell'elenco come base per il modello corrente. Tutti gli eventi, le colonne di dati e i filtri corrispondono inizialmente a quelli presenti nel modello esistente e sarà possibile modificarli in base alle necessità.  
|**Usa come modello predefinito per il tipo di server selezionato**|Consente di utilizzare il modello corrente per impostazione predefinita per le tracce create per questo tipo di server.  

### <a name="edit-general-tab"></a>Modifica (scheda Generale)
 Utilizzare la scheda **Generale** della finestra di dialogo **Proprietà modello di traccia** per visualizzare o modificare i modelli di traccia esistenti utilizzando le opzioni seguenti. Per accedere a questa finestra di dialogo, scegliere **Modelli** dal menu **File** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] e quindi fare clic su **Modifica modello**.  

|Elemento|Descrizione
|---|---
|**Tipo server**|Consente di specificare il tipo di server per il quale verrà utilizzato il modello.  
|**Nome modello**|Consente di selezionare il modello che si desidera modificare.  
|**Usa come modello predefinito per il tipo di server selezionato**|Consente di utilizzare il modello corrente per impostazione predefinita per le tracce create per questo tipo di server.  

### <a name="events-selection-tab"></a>Scheda Selezione eventi
Utilizzare la scheda **Selezione eventi** della finestra di dialogo **Proprietà modello di traccia** per visualizzare, modificare o specificare classi di evento e colonne di dati da includere in un modello di traccia di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  

|Elemento|Descrizione
|---|---
|Colonna **Eventi**|Consente di specificare gli eventi che devono essere inseriti nella traccia selezionando o deselezionando la casella di controllo nella colonna di evento. Gli eventi sono organizzati in base alla categoria. Se è stato selezionato **Basa il nuovo modello sul seguente modello esistente** nella scheda **Generale** , gli eventi vengono selezionati automaticamente in base al modello indicato. Per ulteriori informazioni sulle classi degli eventi, vedere [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Colonne di dati|Consente di specificare le colonne di dati che devono essere inserite nella traccia selezionando la casella corrispondente all'evento e alla colonna di dati necessari. Per ogni evento incluso nella traccia, se la casella di controllo corrispondente all'evento è selezionata, per impostazione predefinita vengono selezionate tutte le relative colonne di dati. Se è stato selezionato **Basa il nuovo modello sul seguente modello esistente** nella scheda **Generale** , le colonne di dati e i filtri vengono selezionati automaticamente in base al modello indicato.  
|Filtri|Per specificare i filtri, fare clic sull'intestazione della colonna di dati e immettere i criteri di filtro. Le colonne di dati filtrate sono contrassegnate da un'icona di filtro a sinistra dell'etichetta della colonna nella finestra di dialogo **Modifica filtro** .  
|**Mostra tutti gli eventi**|Consente di visualizzare tutti gli eventi disponibili. Se si sta creando un nuovo modello non basato su uno esistente, questa opzione è selezionata per impostazione predefinita. Deselezionare questa casella di controllo per nascondere tutti gli eventi non selezionati nella griglia **Selezione eventi** .  
|**Mostra tutte le colonne**|Consente di visualizzare tutte le colonne di dati disponibili. Se si sta creando un nuovo modello non basato su uno esistente, questa opzione è selezionata per impostazione predefinita. Deselezionare questa casella di controllo per nascondere tutte le colonne di dati non selezionate nella griglia **Selezione eventi** .  
|**Filtri colonne**|Consente di aprire la finestra di dialogo **Modifica filtro** , che visualizza un'icona di filtro a sinistra dell'etichetta della colonna di dati. Utilizzare la finestra di dialogo **Modifica filtro** per modificare i filtri delle colonne di dati.  
|**Organizza colonne**|Consente di modificare l'ordine delle colonne nella traccia e di raggruppare i risultati in base a una o più colonne. 

## <a name="trace-file-properties"></a>Proprietà file di traccia 
### <a name="general-tab"></a>Scheda Generale
Utilizzare la scheda **Generale** della finestra di dialogo **Proprietà file di traccia** per visualizzare le proprietà di un file di traccia.  
Per visualizzare questa finestra, aprire un file di traccia e quindi scegliere **Proprietà** dal menu **File**.  

|Elemento|Descrizione
|---|---
|**Nome file**|Il percorso e il nome del file di traccia visualizzato.  
|**Nome provider di traccia**|Visualizza il nome dell'istanza tracciata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
|**Tipo provider di traccia**|Visualizza il tipo di server che ha fornito la traccia.  
|**version**|Visualizza la versione del server che ha fornito la traccia.  
|**Dimensioni file (KB)**|Dimensioni in kilobyte (KB) del file di traccia.  
|**Creato**|Data e ora di creazione del file di traccia.  
|**Ultima modifica** |Data e ora di modifica del file di traccia.  

### <a name="events-selection-tab"></a>Scheda Selezione eventi
Utilizzare la scheda **Selezione eventi** della finestra di dialogo **Proprietà modello di traccia** per visualizzare le proprietà delle colonne della traccia o per eliminare colonne di dati dalla traccia.  
Per visualizzare questa finestra, aprire un file di traccia Scegliere **Proprietà** dal menu **File** e quindi fare clic sulla scheda **Selezione eventi** .  

|Elemento|Descrizione
|---|---
|Colonna **Eventi**|Consente di visualizzare gli eventi inseriti nella traccia organizzati per categoria. Inizialmente sono selezionati tutti gli eventi della traccia. È possibile selezionare gli eventi selezionando le rispettive caselle o colonne di dati. Se si seleziona la casella corrispondente all'evento, verranno selezionate tutte le colonne di dati disponibili per tale evento. Se si seleziona la colonna di dati corrispondente all'evento, quest'ultimo viene selezionato insieme a tutte le altre colonne necessarie. Se si sta visualizzando un file o una tabella di traccia, è possibile deselezionare le caselle di controllo relative agli eventi o alle colonne di dati per ridurre la quantità di dati visibili nella finestra di traccia, facilitandone in tal modo l'analisi. È inoltre possibile modificare i filtri colonna per ridurre la quantità di dati visibili nella finestra di traccia. Per ulteriori informazioni sulle classi degli eventi, vedere [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Colonne di dati|È possibile visualizzare le colonne di dati inserite nella traccia. Tutte le colonne di dati pertinenti nella traccia sono selezionate per impostazione predefinita per ogni evento incluso nella traccia.  
|Filtri|Per specificare i filtri, fare clic sull'intestazione della colonna di dati e immettere i criteri di filtro. Le colonne di dati filtrate sono contrassegnate da un'icona di filtro a sinistra dell'etichetta della colonna nella finestra di dialogo **Modifica filtro** .  
|**Mostra tutti gli eventi**|Consente di visualizzare tutti gli eventi disponibili. Per impostazione predefinita, vengono visualizzate solo le righe della griglia **Selezione eventi** selezionate. Deselezionare questa casella di controllo per nascondere tutti gli eventi non selezionati nella griglia **Selezione eventi** . Se la casella di controllo **Mostra tutti gli eventi** è selezionata e si sta visualizzando un file o una tabella di traccia, tutti gli eventi registrati nella traccia vengono visualizzati nella finestra di traccia.  
|**Mostra tutte le colonne**|Consente di visualizzare tutte le colonne di dati disponibili. Per impostazione predefinita, vengono visualizzate solo le colonne di dati selezionate. Deselezionare questa casella di controllo per nascondere tutte le colonne di dati non selezionate nella griglia **Selezione eventi** .  
|**Filtri colonne**|Apre la finestra di dialogo **Modifica filtro** che visualizza un'icona di filtro a sinistra delle etichette delle colonne di dati filtrate. Utilizzare la finestra di dialogo **Modifica filtro** per modificare i filtri delle colonne di dati.  
|**Organizza colonne**|Dopo avere selezionato **Eventi** e le colonne di dati da inserire nella traccia, fare clic su **Organizza colonne** per forzare il riordinamento della colonna nella finestra dei risultati della traccia.  

## <a name="trace-table-properties"></a>Proprietà tabella di traccia
### <a name="events-selection-tab"></a>Scheda Selezione eventi
Utilizzare la scheda **Selezione eventi** nella finestra di dialogo **Proprietà tabella di traccia** per visualizzare le proprietà degli eventi e delle colonne di dati della traccia o per rimuovere eventi e colonne.  
Per visualizzare questa finestra, aprire una tabella di traccia mediante [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , scegliere **Proprietà** dal menu **File** e quindi fare clic sulla scheda **Selezione eventi** .  

|Elemento|Descrizione
|---|---
|Colonna **Eventi**|Consente di visualizzare gli eventi inseriti nella traccia organizzati per categoria. È possibile selezionare gli eventi selezionando le rispettive caselle o colonne di dati. Se si seleziona la casella corrispondente all'evento, verranno selezionate tutte le colonne di dati disponibili per tale evento. Se si seleziona la colonna di dati corrispondente all'evento, quest'ultimo viene selezionato insieme a tutte le altre colonne necessarie. Se si sta visualizzando un file o una tabella di traccia, è possibile deselezionare le caselle di controllo relative agli eventi o alle colonne di dati per ridurre la quantità di dati visibili nella finestra di traccia, facilitandone in tal modo l'analisi. È inoltre possibile modificare i filtri colonna per ridurre la quantità di dati visibili nella finestra di traccia. Per ulteriori informazioni sulle classi degli eventi, vedere [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Altre colonne di dati|È possibile visualizzare le colonne di dati inserite nella traccia. Tutte le colonne di dati pertinenti nella traccia sono selezionate per impostazione predefinita per ogni evento incluso nella traccia.  
|Filtri|Per specificare i filtri, fare clic sull'intestazione della colonna di dati e immettere i criteri di filtro. Le colonne di dati filtrate sono contrassegnate da un'icona di filtro a sinistra dell'etichetta della colonna nella finestra di dialogo **Modifica filtro** .  
|**Mostra tutti gli eventi**|Consente di visualizzare tutti gli eventi disponibili. Per impostazione predefinita, vengono visualizzate solo le righe della griglia **Selezione eventi** selezionate. Deselezionare questa casella di controllo per nascondere tutti gli eventi non selezionati nella griglia **Selezione eventi** . Se la casella di controllo **Mostra tutti gli eventi** è selezionata e si sta visualizzando un file o una tabella di traccia, tutti gli eventi registrati nella traccia vengono visualizzati nella finestra di traccia.  
|**Mostra tutte le colonne**|Consente di visualizzare tutte le colonne di dati disponibili. Per impostazione predefinita, vengono visualizzate solo le colonne di dati selezionate. Deselezionare questa casella di controllo per nascondere tutte le colonne di dati non selezionate nella griglia **Selezione eventi** .  
|**Filtri colonne**|Consente di aprire la finestra di dialogo **Modifica filtro** che visualizza un'icona di filtro a sinistra dell'etichetta di colonna. È possibile utilizzare questa finestra di dialogo per modificare i filtri delle colonne di dati.  
|**Organizza colonne** |Dopo avere selezionato **Eventi** e le colonne di dati da inserire nella traccia, fare clic su **Organizza colonne** per forzare il riordinamento della colonna nella finestra dei risultati della traccia.  

## <a name="performance-counters-limit"></a>Limite contatori prestazioni
Utilizzare la finestra di dialogo Limite contatori prestazioni per limitare le informazioni generate da un file di log delle prestazioni del monitoraggio di sistema quando viene correlato con una traccia di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . La finestra di dialogo consente di selezionare i contatori da visualizzare e utilizzare per la correlazione.  
La finestra di dialogo **Limite contatori prestazioni** viene popolata con i contatori e gli oggetti prestazioni contenuti nel file di log delle prestazioni.  
### <a name="to-select-performance-objects-and-counters-to-correlate-with-a-trace"></a>Per selezionare i contatori e gli oggetti prestazioni da correlare con una traccia  
1.  Espandere un oggetto prestazione per visualizzare i contatori inclusi nel file di log delle prestazioni.  
2.  Selezionare i contatori da correlare con il file di traccia di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  

Per selezionare tutti i contatori relativi a un oggetto prestazione, selezionare la casella adiacente all'oggetto in questione. Se si seleziona il nodo di livello più alto, che indica il computer, verranno selezionati tutti i contatori e gli oggetti prestazioni contenuti nel file di log delle prestazioni. 
## <a name="toolsoptions-general-options-page"></a>Strumenti/Opzioni (pagina Opzioni generali)
Usare la finestra di dialogo **Opzioni generali** per visualizzare o specificare le opzioni seguenti.  
### <a name="display-options"></a>Opzioni di visualizzazione  

|Elemento|Descrizione
|---|---
|**Nome carattere**|Visualizza il nome del carattere utilizzato nella griglia dei risultati della traccia durante l'esecuzione delle tracce.  
|**Dimensioni carattere**|Visualizza le dimensioni del carattere utilizzato nella griglia dei risultati della traccia durante l'esecuzione delle tracce.  
|**Scegli carattere**|Consente di aprire una finestra di dialogo per la modifica delle impostazioni del carattere.  
|**Visualizza valori di data e ora in base alle impostazioni internazionali**|Visualizza i valori di data e ora in base alle impostazioni internazionali configurate nel computer in uso. Se questa opzione non viene selezionata, i valori di data e ora vengono visualizzati in base al formato predefinito utilizzato da Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]in cui sono inclusi i millisecondi. Si noti che l'attivazione o la disattivazione di questa casella di controllo modifica il formato di visualizzazione delle colonne relative all'ora, ad esempio **StartTime** ed **EndTime**. Tuttavia, non vengono modificati i parametri del valore **DateTime** negli eventi del linguaggio o nelle RPC (Remote Procedure Call).  
|**Mostra i valori nella colonna Durata in microsecondi**|Visualizza i valori in microsecondi nella colonna di dati **Durata** delle tracce. Per impostazione predefinita, i valori nella colonna **Durata** vengono visualizzati in millisecondi.  

### <a name="tracing-options"></a>Opzioni di traccia  

|Elemento|Descrizione
|---|---
|**Avvia traccia non appena viene stabilita una connessione**|La traccia viene avviata utilizzando il modello predefinito non appena viene stabilita una connessione.  
|**Aggiorna la definizione di traccia in caso di modifica della versione del provider**|Consente di applicare a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la definizione di traccia più recente quando il provider viene aggiornato. Questo elemento non è selezionato per impostazione predefinita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] viene forzato a recuperare la definizione di traccia dal server e a ricreare il file su disco, se un file esiste.  

### <a name="file-rollover-options"></a>Opzioni rollover file  

|Elemento|Descrizione
|---|---
|**Carica tutti i file di rollover in sequenza senza chiedere conferma**|I file di rollover vengono caricati automaticamente all'apertura del file di traccia. Se durante la creazione della traccia sono stati creati più file, la selezione di questa opzione determina il caricamento automatico di tutti i file di rollover.  
|**Chiedi conferma prima di caricare file di rollover**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] chiede conferma prima di aggiungere un file di rollover quando è aperto un file di traccia.  
|**Non caricare mai file di rollover successivi**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] non esegue mai il caricamento di file di rollover consecutivi quando è aperto un file di traccia.  

### <a name="replay-options"></a>Opzioni di riproduzione  

|Elemento|Descrizione
|---|---
|**Numero predefinito di thread di riproduzione**|Consente di specificare il numero di thread di riproduzione da utilizzare simultaneamente. Un numero elevato determina un maggior consumo di risorse durante la riproduzione, ma migliora la simultaneità della riproduzione.  
|**Intervallo di attesa predefinito Health Monitor (sec)**|Consente di specificare l'intervallo di attesa in secondi per la riproduzione. Il valore predefinito è 3600 secondi (1 ora). Questa impostazione influisce sulla quantità di tempo durante la quale è consentita l'esecuzione di un thread prima che questo venga terminato da Health Monitor.  
|**Intervallo di polling predefinito Health Monitor (sec)**|Consente di specificare l'intervallo di polling in secondi di Health Monitor durante la riproduzione. Il valore predefinito è 60 secondi. Questo valore consente di configurare la frequenza con cui Health Monitor esegue il polling di candidati per la terminazione.

## <a name="source-table-database-engine-tuning-advisor-select-workload-table"></a>Tabella di origine (Seleziona tabella carico di lavoro di Ottimizzazione guidata motore di database)
Questa finestra di dialogo viene usata in Microsoft SQL Server Profiler e in Ottimizzazione guidata per selezionare tabelle.  
- In Profiler usare la finestra di dialogo **Tabella di origine** per specificare una tabella di origine per una tabella di traccia. Si tratta di una tabella dalla quale viene caricata una traccia e il cui contenuto viene visualizzato o utilizzato per riprodurre la traccia.  
- In Ottimizzazione guidata usare la finestra di dialogo **Seleziona tabella carico di lavoro** per selezionare una tabella di database contenente informazioni di traccia del profiler da usare come carico di lavoro per l'ottimizzazione o per visualizzare un'anteprima del contenuto della tabella prima di avviare l'analisi per l'ottimizzazione.  

|Elemento|Descrizione
|---|---
|**SQL Server**|Specifica l'istanza di SQL Server a cui si è attualmente connessi. Questo campo viene popolato automaticamente e non è possibile aggiornarlo.  
|**Database**|Consente di specificare il database che include la tabella di traccia.  
|**Proprietario**|Indica il proprietario della tabella di traccia. Questo campo viene popolato automaticamente con il valore **dbo**.  
|**Tabella**|Consente di specificare il nome della tabella di traccia dalla quale leggere la traccia.  

## <a name="destination-table"></a>Tabella di destinazione
Utilizzare la finestra di dialogo **Tabella di destinazione** per specificare la tabella in cui si desidera archiviare la traccia.  

|Elemento|Descrizione
|---|---
|**SQL Server**|Indica l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è attualmente connessi. Questo campo viene popolato automaticamente e non è possibile aggiornarlo. Per modificare il server fare clic su **Annulla** e connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si desidera archiviare la tabella di traccia.  
|**Database**|Consente di specificare il database in cui si desidera archiviare la tabella di traccia.  
|**Proprietario**|Indica il proprietario della tabella di traccia. Questo campo viene popolato automaticamente con il valore **dbo**.  
|**Tabella**|Consente di specificare il nome della tabella in cui si desidera archiviare la traccia.  

## <a name="replay-configuration"></a>Configurazione riproduzione
### <a name="basic-replay-options"></a>Opzioni di base di riproduzione
Nella finestra di dialogo **Configurazione riproduzione** usare la pagina **Opzioni di base di riproduzione** per specificare la modalità di riproduzione di un file o una tabella di traccia.  
Per visualizzare questa finestra utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per aprire un file o una tabella di traccia che contiene gli eventi appropriati per la riproduzione. Per altre informazioni, vedere [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md). Con la tabella o il file di traccia aperto, scegliere **Avvia** dal menu **Riproduci** e quindi connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui riprodurre la traccia.  

|Elemento|Descrizione
|---|---
|**Server di riproduzione**|Consente di visualizzare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui connettersi per la riproduzione.  
|**Cambia...**|Consente di aprire la finestra di dialogo **Connetti al server** per stabilire una connessione a un altro server.  
|**Salva nel file** |Consente di salvare i risultati di riproduzione in un file. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza la finestra di dialogo dei file standard in cui è possibile specificare la posizione in cui salvare il file.  
|**Salva nella tabella**|Consente di salvare i risultati di riproduzione in una tabella. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza la finestra di dialogo di selezione della tabella in cui è possibile specificare la posizione in cui salvare la tabella.  
|**Numero di thread di riproduzione**|Consente di specificare il numero di thread di riproduzione da utilizzare simultaneamente. Un numero elevato determina un maggior consumo di risorse durante la riproduzione, ma la riproduzione viene eseguita in modo più veloce e simultaneo.  
|**Riproduci gli eventi nell'ordine in cui sono stati inseriti nella traccia**|Gli eventi vengono riprodotti in modo sequenziale. Utilizzare questa opzione per riprodurre una traccia a scopi di debug.  
|**Riproduci gli eventi usando più thread** |Gli eventi vengono riprodotti simultaneamente. Questa opzione offre una riproduzione più veloce rispetto alla riproduzione degli eventi sequenziale, ma non permette l'utilizzo a scopi di debug. Gli eventi vengono ordinati in base ai relativi identificatori di processo di sistema (SPID).  
|**Visualizza risultati di riproduzione**|Consente di visualizzare i risultati di riproduzione in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. 

### <a name="advanced-replay-options"></a>Opzioni avanzate di riproduzione
La scheda **Opzioni avanzate di riproduzione** della finestra di dialogo **Configurazione riproduzione** consente di specificare la modalità di riproduzione di un file di traccia.  
Per visualizzare questa finestra utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per aprire un file o una tabella di traccia che contiene gli eventi appropriati per la riproduzione. Per altre informazioni, vedere [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md). Con la tabella o il file di traccia aperto, scegliere **Avvia** dal menu **Riproduci**, connettersi all'istanza di SQL Server in cui si vuole riprodurre la traccia e quindi fare clic sulla scheda **Opzioni avanzate di riproduzione** .  

|Elemento|Descrizione
|---|---
|**Riproduci SPID di sistema**|Consente di specificare se [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] esegue la riproduzione degli SPID (System Process Identifier).  
|**Riproduci un solo SPID**|Consente di riprodurre solo l'attività del file di traccia di origine correlata allo SPID selezionato.  
|**SPID da riprodurre**|Consente di specificare lo SPID da riprodurre.  
|**Limite di tempo per la riproduzione**|Consente di riprodurre solo una parte del file di traccia di origine.  
|**Ora di inizio**|Data e ora nel file di traccia di origine in corrispondenza delle quali deve iniziare la riproduzione.  
|**Ora fine**|Data e ora nel file di traccia di origine in corrispondenza delle quali deve essere arrestata la riproduzione.  
|**Intervallo di attesa Health Monitor (sec)**|Consente di specificare l'intervallo di attesa in secondi per la riproduzione. Il valore predefinito è 3600 secondi (1 ora). Questa impostazione influisce sulla quantità di tempo durante la quale è consentita l'esecuzione di un processo prima che venga terminato da Health Monitor.  
|**Intervallo di polling Health Monitor (sec)**|Consente di specificare l'intervallo di polling in secondi di Health Monitor durante la riproduzione. Il valore predefinito è 60 secondi. Questo valore consente di configurare la frequenza con cui Health Monitor esegue il polling di candidati per la terminazione.  
|**Abilita monitoraggio processi bloccati di SQL Server**|Consente di abilitare un processo che esegue la ricerca di processi bloccati e di blocco.  
|**Intervallo di attesa monitoraggio processi bloccati (sec)**|Consente di configurare la frequenza con cui i processi bloccati o di blocco vengono cercati tramite il monitoraggio dei processi bloccati.  

## <a name="find-dialog-box"></a>Trova - finestra di dialogo
Usare la finestra di dialogo **Trova** per eseguire la ricerca all'interno di una traccia in base a parole o caratteri specifici. Per annullare la ricerca in corso, premere ESC.  
 Per aprire questa finestra di dialogo in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], scegliere **Trova** dal menu **Modifica**.  

|Elemento|Descrizione
|---|---
|**Trova**|Immettere il testo che si desidera cercare. La ricerca individua tutte le stringhe contenenti la stringa specificata. Se ad esempio si cerca "Completed", viene individuata la stringa "SQL:BatchCompleted". I caratteri jolly (*, ? e così via) non sono supportati.  
|**Cerca nella colonna**|Fare clic su una colonna di dati in cui eseguire la ricerca o su **\<All columns>** per eseguire la ricerca in tutte le colonne di dati presenti nella traccia.  
|**Maiuscole/minuscole**|Consente di trovare una stringa di testo con le stesse lettere maiuscole e minuscole di quella specificata nella casella **Trova** . Deselezionare questa casella di controllo per trovare stringhe di testo nella traccia che corrispondono al testo specificato indipendentemente dai caratteri maiuscoli o minuscoli.  
|**Parola intera**|Consente di limitare l'ambito della ricerca alle parole intere. Deselezionare la casella di controllo **Parola intera** per cercare un insieme di caratteri all'interno di una parola.  
|**Trova successivo**|Consente di trovare l'esempio successivo dei caratteri specificati nella casella **Trova** .  
|**Trova precedente**|Consente di eseguire una ricerca all'indietro per trovare l'esempio precedente dei caratteri specificati nella casella **Trova** .  

 ## <a name="organize-columns"></a>Organizza colonne
Utilizzare la finestra di dialogo **Organizza colonne** per selezionare le colonne di dati per il raggruppamento o l'aggregazione degli eventi visualizzati in una traccia, in modo da semplificare la visualizzazione e l'analisi di tabelle o di file di traccia di grandi dimensioni.  
- A seguito di un'operazione di aggregazione, tutti gli eventi presenti nella traccia vengono spostati e compressi in corrispondenza del rispettivo tipo di classe di evento. A sinistra del nome della classe di evento viene visualizzato un segno più ( **+** ). Quando si fa clic sul segno più, la classe di evento si espande ed è possibile visualizzare tutti gli eventi del tipo in questione.  
- A seguito di un'operazione di raggruppamento, vengono organizzate insieme tutte le classi di evento di un tipo specifico nell'area di visualizzazione della finestra di traccia. Gli eventi non vengono tuttavia compressi in corrispondenza del tipo di classe di evento.  

Quando si raggruppano o aggregano gli eventi nell'area di visualizzazione della finestra di traccia, le colonne selezionate per il raggruppamento o l'aggregazione rimangono fisse nella finestra di visualizzazione, ma è possibile scorrere a destra o a sinistra per visualizzare tutte le altre colonne di dati.  
Per accedere a questa finestra di dialogo, aprire una tabella o un file di traccia esistente e quindi scegliere **Proprietà** dal menu **File** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Nella finestra di dialogo **Proprietà traccia** selezionare la scheda **Selezione eventi** e quindi fare clic su **Organizza colonne**. È inoltre possibile fare clic su **Organizza colonne** nella scheda **Selezione eventi** durante la creazione di una nuova traccia.  
Consente di spostare i nomi delle colonne di dati sotto **Gruppi** per raggruppare o aggregare le classi di evento nella finestra di traccia.
- Per aggregare gli eventi, spostare una colonna di dati all'interno di **Gruppi**. In questo modo, tutti gli eventi di un tipo specifico vengono compressi in corrispondenza del nome del tipo di classe di evento nell'area di visualizzazione della finestra di traccia. A sinistra del nome della classe di evento viene visualizzato un segno più ( **+** ). Fare clic su questo segno per espandere il tipo di classe di evento e visualizzare tutti gli eventi. È possibile attivare e disattivare l'aggregazione e il raggruppamento scegliendo **Visualizzazione aggregata** o **Visualizzazione a gruppi** dal menu **Visualizza** .
- Per raggruppare gli eventi, spostare più colonne di dati all'interno di **Gruppi**. In questo modo, tutti gli eventi di un tipo specifico vengono raggruppati insieme nell'area di visualizzazione della finestra di traccia, senza venire compressi in corrispondenza di ogni nome del tipo di classe di evento. È possibile passare dalla visualizzazione a gruppi a quella non a gruppi e viceversa scegliendo **Visualizzazione a gruppi** dal menu Visualizza. Se si spostano più colonne di dati all'interno di **Gruppi**, l'opzione che consente di passare a una **visualizzazione aggregata** non è disponibile.

|Elemento|Descrizione
|---|---
|**Colonne**|Elenco delle colonne di dati disponibili che possono essere spostate in **Gruppi**. Fare clic sul segno più ( **+** ) a sinistra di **Colonne** per espandere l'elenco.  
|**Attivo**|Dopo aver selezionato una colonna di dati, fare clic su **Su** per spostare le colonne di dati all'interno di **Gruppi**. Facendo clic su **Su** , è inoltre possibile riorganizzare la visualizzazione delle colonne nella finestra di traccia.  
|**Giù**|Dopo aver selezionato una colonna di dati, fare clic su **Giù** per spostare le colonne di dati all'esterno di **Gruppi**. Facendo clic su **Giù** , è inoltre possibile riorganizzare la visualizzazione delle colonne nella finestra di traccia.  

## <a name="edit-filter"></a>Modifica filtro
Utilizzare la finestra di dialogo **Modifica filtro** per creare e modificare i filtri delle colonne di dati in una traccia. Fare clic sul nome di una colonna di dati nell'elenco per visualizzare nel riquadro adiacente i criteri di filtro disponibili per la colonna di dati. Immettere i criteri di filtro e fare clic su **OK** per applicarli alla colonna di dati selezionata. La presenza dell'icona del filtro a sinistra del nome della colonna di dati nell'elenco indica che la colonna dispone già di un filtro configurato.  
 >[!NOTE]
 >Per le colonne di dati di tipo stringa, i criteri di filtro saranno visualizzati come un valore di stringa LIKE o NOT LIKE.  

## <a name="select-template-name"></a>Nome modello
Utilizzare la finestra di dialogo **Seleziona nome modello** per selezionare un modello di traccia esistente di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ed esportarlo in un file nel sistema operativo. È inoltre possibile utilizzare questa finestra di dialogo per selezionare o immettere un nome diverso per il salvataggio di un modello di traccia, ad esempio quando si modifica un modello di traccia esistente. Per accedere a questa finestra di dialogo durante l'esportazione di un modello, scegliere **Modelli** dal menu **File** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] e quindi fare clic su **Esporta modello**. Per accedere a questa finestra di dialogo durante la modifica del nome di un modello, scegliere **Modelli** dal menu **File**, quindi scegliere **Modifica modello** e fare clic su **Salva con nome**.  

|Elemento|Descrizione
|---|---
|**Tipo di server**|Selezionare il tipo di server dal quale si desidera scegliere un modello. Questa opzione è disponibile solo durante l'esportazione di un modello.  
|**Nome modello**|Digitare un nuovo nome per il modello oppure selezionarne uno nell'elenco. Se si sta esportando un modello, è consentita solo la selezione di un nome di modello nell'elenco. 

## <a name="see-also"></a>Vedere anche 
[SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
[Monitoraggio delle prestazioni e dell'attività del server](../../relational-databases/performance/server-performance-and-activity-monitoring.md)  
  
