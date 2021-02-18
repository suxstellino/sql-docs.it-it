---
title: Editor di query SSMS
description: Editor di query SQL di Server Management Studio (SSMS)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.query.advanced.f1
- sql13.swb.query.ansi.f1
- sql13.swb.query.general.f1
- sql13.swb.query.grid.f1
- sql13.swb.sqleditors.multiserverresultssettings
- sql13.swb.tsqlquery.f1
- sql13.swb.tsqlresults.f1
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], editor
- SQL Server Management Studio [SQL Server], Database Engine Query Editor
- SQL Server Management Studio [SQL Server], templates
- SQL Server Management Studio [SQL Server], query editor
- Query Editor [SQL Server Management Studio]
- Query Editor [Database Engine]
- Query Editor [Database Engine], Features
- Query Editor [Database Engine], Toolbar
- Query Editor [SQL Server Management Studio], full screen mode
- Query Editor [Database Engine], templates
- Query Editor [SQL Server Management Studio], about Query Editor
- Transact-SQL Editor See Query Editor [Database Engine]
- Database Engine Query Editor See Query Editor [Database Engine]
- Code Editor [SQL Server Management Studio], about Query Editor
- editors [SQL Server Management Studio], Database Engine Query Editor
- full screen mode [SQL Server Management Studio]
- writing scripts
- modifying scripts
- writing queries
- scripts [SQL Server], SQL Server Management Studio
- queries [SQL Server], SQL Server Management Studio
ms.assetid: 05cfae9b-96d5-4a35-a098-0bc3a548bcfc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019, contperf-fy21q1
ms.date: 08/28/2020
ms.openlocfilehash: 6ce879af451abb20e108e20b36fbe129024f4806
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355158"
---
# <a name="sql-server-management-studio-ssms-query-editor"></a>Editor di query SQL di Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Questo articolo illustra le caratteristiche e le funzionalità dell'editor di query in SQL Server Management Studio (SSMS).

> [!Note]
> Per informazioni su come usare la Guida sensibile al contesto di Transact-SQL (T-SQL), vedere la sezione [Guida di Transact-SQL](#transact-sql-f1-help).
>
> Per informazioni sulle attività che è possibile eseguire con l'editor, vedere la sezione [Attività degli editor](#editor-tasks).

Gli editor in SSMS condividono un'architettura tipica. L'editor di testo implementa il livello di base della funzionalità e può essere usato come editor di base per i file di testo. Gli altri editor, ovvero gli editor di query, estendono questa base di funzionalità includendo un servizio di linguaggio che definisce la sintassi di uno dei linguaggi supportati in SQL Server. Gli editor di query implementano inoltre vari livelli di supporto per caratteristiche dell'editor quali IntelliSense il debug. Gli editor di query includono l'editor di query del Motore di database per l'utilizzo nella compilazione di script che contengono istruzioni T-SQL e XQuery, l'editor MDX per il linguaggio MDX, l'editor DMX per il linguaggio DMX e l'editor XML/A per il linguaggio XML for Analysis.
L'editor di query può essere usato per creare ed eseguire script contenenti istruzioni Transact-SQL.

![Nuova query](media/database-engine-query-editor-sql-server-management-studio/new-query.png)

## <a name="sql-editor-toolbar"></a>Barra degli strumenti Editor SQL

Quando l'editor di query è aperto, la barra degli strumenti Editor SQL visualizza i pulsanti seguenti.

È anche possibile aggiungere la barra degli strumenti Editor SQL selezionando il menu **Visualizza** , **Barre degli strumenti**, quindi **Editor SQL**. Se la barra degli strumenti Editor SQL viene aggiunta quando non sono aperte finestre dell'editor di query, non è disponibile nessun pulsante.

![Barra degli strumenti dell'editor](media/database-engine-query-editor-sql-server-management-studio/editor-toolbar.png)

### <a name="connect-using-the-editor-toolbar"></a>Connetti - Barra degli strumenti dell'editor

Apre la finestra di dialogo **[Connetti al server](connect-to-server-database-engine.md)** . Utilizzare questa finestra di dialogo per stabilire una connessione a un server.

È anche possibile connettersi al database usando il [menu di scelta rapida](#connection-using-the-context-menu).

### <a name="change-connection-using-the-editor-toolbar"></a>Cambia connessione - Barra degli strumenti dell'editor

Consente di aprire la finestra di dialogo **Connetti al server** . Usare questa finestra di dialogo per stabilire una [connessione a un altro server](f1-help-for-server-connections-sql-server-management-studio.md).

È anche possibile cambiare connessione usando il [menu di scelta rapida](#connection-using-the-context-menu).

### <a name="available-databases-using-the-editor-toolbar"></a>Database disponibili - Barra degli strumenti dell'editor

Consente di passare a un database diverso sullo stesso server.

### <a name="execute-using-the-editor-toolbar"></a>Esegui - Barra degli strumenti dell'editor

Consente di eseguire il codice selezionato o, se non è selezionato alcun codice, di eseguire tutto il codice dell'editor di query.

È anche possibile attivare il comando **Esegui** per una query selezionando F5 o usando il [menu di scelta rapida](#execute-using-the-context-menu).

### <a name="cancel-executing-query-using-the-editor-toolbar"></a>Annulla esecuzione query - Barra degli strumenti dell'editor

Consente di inviare una richiesta di annullamento al server. Alcune query non possono essere annullate immediatamente, ma devono attendere una condizione di annullamento adatta. Quando le transazioni vengono annullate, è possibile che si verifichino ritardi durante il rollback delle transazioni.

È anche possibile annullare una query in esecuzione selezionando ALT+INTERR.

### <a name="parse-using-the-editor-toolbar"></a>Analizza - Barra degli strumenti dell'editor

Consente di controllare la sintassi del codice selezionato. Se non è selezionato alcun codice, viene controllata tutta la sintassi del codice presente nella finestra dell'editor di query.

Per controllare il codice nell'editor di query è anche possibile selezionare CTRL+F5.

### <a name="display-estimated-execution-plan-using-the-editor-toolbar"></a>Visualizza piano di esecuzione stimato - Barra degli strumenti dell'editor

Richiede un piano di esecuzione query a Query Processor senza eseguire la query e visualizza il piano nella finestra **Piano di esecuzione**. In tale piano vengono usate statistiche dell'indice per stimare il numero di righe restituite previste durante ogni parte dell'esecuzione della query. Il piano di query effettivo utilizzato può differire dal piano di esecuzione stimato. Ciò può verificarsi se il numero di righe restituite è diverso da quello stimato e Query Processor modifica il piano per renderlo più efficiente.

È anche possibile visualizzare un piano di esecuzione stimato premendo CTRL+L o usando il [menu di scelta rapida](#display-estimated-execution-plan-using-the-context-menu).

### <a name="query-options-using-the-editor-toolbar"></a>Opzioni query - Barra degli strumenti dell'editor

Consente di aprire la finestra di dialogo **Opzioni query** . Utilizzare questa finestra di dialogo per configurare le opzioni predefinite per l'esecuzione della query e per i relativi risultati.

In alternativa è possibile selezionare **Opzioni query** dal [menu di scelta rapida](#query-options-using-the-context-menu).

### <a name="intellisense-enabled-using-the-editor-toolbar"></a>IntelliSense abilitato - Barra degli strumenti dell'editor

Specifica se la funzionalità [IntelliSense](../scripting/configure-intellisense-sql-server-management-studio.md) è disponibile nell'editor di query del motore di database. Questa opzione è selezionata per impostazione predefinita.

È anche possibile selezionare **IntelliSense abilitato** premendo CTRL+B e quindi CTRL+I oppure usando il [menu di scelta rapida](#intellisense-enabled-using-the-context-menu).

### <a name="include-actual-execution-plan-using-the-editor-toolbar"></a>Includi piano di esecuzione effettivo - Barra degli strumenti dell'editor

Esegue la query, ne restituisce i risultati e usa il piano di esecuzione per la query. Le query vengono visualizzate come piano di query grafico nella finestra **Piano di esecuzione**.

È anche possibile selezionare **Includi piano di esecuzione effettivo stimato** premendo CTRL+M o usando il [menu di scelta rapida](#include-actual-execution-plan-using-the-context-menu).

### <a name="include-live-query-statistics-using-the-editor-toolbar"></a>Includi statistiche query dinamiche - Barra degli strumenti dell'editor

Offre informazioni approfondite in tempo reale sul processo di esecuzione della query, man mano che i controlli passano da un operatore del piano di query a un altro.

È anche possibile selezionare **Includi statistiche query dinamiche** dal [menu di scelta rapida](#include-live-query-statistics-using-the-context-menu).

### <a name="include-client-statistics-using-the-editor-toolbar"></a>Includi statistiche client - Barra degli strumenti dell'editor

Include una finestra **Statistiche client** contenente statistiche relative alla query e ai pacchetti di rete, nonché al tempo trascorso dall'inizio della query.

È anche possibile selezionare **Includi statistiche client** premendo MAIUSC+ALT+S o usando il [menu di scelta rapida](#include-client-statistics-using-the-context-menu).

### <a name="results-to-text-using-the-editor-toolbar"></a>Risultati in formato testo - Barra degli strumenti dell'editor

Restituisce i risultati della query in formato testo nella finestra **Risultati** .

È anche possibile restituire i risultati in formato testo premendo CTRL+T o usando il [menu di scelta rapida](#results-using-the-context-menu).

### <a name="results-to-grid-using-the-editor-toolbar"></a>Risultati in formato griglia - Barra degli strumenti dell'editor

Restituisce i risultati della query sotto forma di una o più griglie nella finestra **Risultati** . Per impostazione predefinita, questa opzione è attivata.

È anche possibile restituire i risultati in formato griglia premendo CTRL+D o usando [menu di scelta rapida](#results-using-the-context-menu).

### <a name="results-to-file-using-the-editor-toolbar"></a>Risultati in un file - Barra degli strumenti dell'editor

All'esecuzione della query viene visualizzata la finestra di dialogo **Salva risultati** . In **Salva in** selezionare la cartella in cui si desidera salvare il file. Digitare il nome del file in **Nome file**, quindi selezionare **Salva** per salvare i risultati della query come file **Report** con estensione RPT. Per visualizzare le opzioni avanzate, fare clic sulla freccia giù del pulsante **Salva** e quindi selezionare **Salva con codifica**.

È anche possibile restituire i risultati in un file premendo CTRL+MAIUSC+F o usando [menu di scelta rapida](#results-using-the-context-menu).

### <a name="comment-out-the-selected-lines-using-the-editor-toolbar"></a>Imposta le righe selezionate come commento - Barra degli strumenti dell'editor

Consente di contrassegnare la riga corrente come commento tramite l'aggiunta di un operatore di commento (--) all'inizio della riga.

È anche possibile contrassegnare una riga come commento premendo CTRL+K e quindi CTRL+C.

### <a name="uncomment-the-selected-lines-using-the-editor-toolbar"></a>Rimuovi simboli di commento dalle righe selezionate - Barra degli strumenti dell'editor

Consente di modificare la riga corrente in istruzione di origine attiva rimuovendo eventuali operatori di commento (--) presenti all'inizio della riga.

È anche possibile rimuovere i simboli di commento da una riga premendo CTRL+K e quindi CTRL+U.

### <a name="decrease-indent-using-the-editor-toolbar"></a>Riduci rientro - Barra degli strumenti dell'editor

Consente di spostare a sinistra il testo della riga rimuovendo spazi vuoti all'inizio della riga.

### <a name="increase-line-indent-using-the-editor-toolbar"></a>Aumenta rientro riga - Barra degli strumenti dell'editor

Consente di spostare a destra il testo della riga aggiungendo spazi vuoti all'inizio della riga.

### <a name="specify-values-for-template-parameters-using-the-editor-toolbar"></a>Imposta valori per parametri modello - Barra degli strumenti dell'editor

Consente di aprire una finestra di dialogo in cui è possibile specificare valori per i parametri inclusi in stored procedure e funzioni.

## <a name="context-menu"></a>Menu di scelta rapida

È possibile accedere al menu di scelta rapida *facendo clic con il pulsante destro del mouse* in qualsiasi punto dell'editor di query. Le opzioni del menu di scelta rapida sono simili a quelle della barra degli strumenti dell'editor SQL. Il menu di scelta rapida visualizza le stesse opzioni della barra, ad esempio **Connetti** ed **Esegui**, ma anche altre opzioni come **Inserisci frammento** e **Racchiudi tra**.

![Opzioni](media/database-engine-query-editor-sql-server-management-studio/context-menu.png)

### <a name="insert-snippet-using-the-context-menu"></a>Inserisci frammento - Menu di scelta rapida

Un [frammento di codice T-SQL](../scripting/add-transact-sql-snippets.md) è un modello che può essere usato come punto di partenza per la scrittura di nuove istruzioni Transact-SQL nell'editor di query.

### <a name="surround-with-using-the-context-menu"></a>Racchiudi tra - Menu di scelta rapida

Un frammento di inclusione è un modello che può essere usato come punto di partenza per includere un set di istruzioni Transact-SQL in un blocco BEGIN, IF o WHILE.

### <a name="connection-using-the-context-menu"></a>Connessione - Menu di scelta rapida

![Connessioni](media/database-engine-query-editor-sql-server-management-studio/context-menu-connections.png)

Nel menu di scelta rapida di SSMS sono disponibili più opzioni **Connessione** rispetto a quelle della barra degli strumenti.

- **Connetti**: apre la finestra di dialogo Connetti al server. Utilizzare questa finestra di dialogo per stabilire una connessione a un server.

- **Disconnetti**: disconnette l'editor di query corrente dal server.

- **Disconnetti tutte le query**: disconnette tutte le connessioni a query.

- **Cambia connessione**: apre la finestra di dialogo Connetti al server. Utilizzare questa finestra di dialogo per stabilire una connessione a un server diverso.

### <a name="open-server-in-object-explorer-using-the-context-menu"></a>Apri server in Esplora oggetti - Menu di scelta rapida

Esplora oggetti offre un'interfaccia utente gerarchica per visualizzare e gestire gli oggetti in ogni istanza di SQL Server. Il riquadro Dettagli Esplora oggetti presenta una vista tabulare di oggetti istanza e la funzionalità per cercare oggetti specifici. Le funzionalità di Esplora oggetti variano leggermente in base al tipo di server ma in genere comprendono le caratteristiche di sviluppo per i database e di gestione per tutti i tipi di server.

### <a name="execute-using-the-context-menu"></a>Esegui - Menu di scelta rapida

Consente di eseguire il codice selezionato o, se non è selezionata alcuna parte del codice, di eseguire tutto il codice incluso nell'editor di query.

### <a name="display-estimated-execution-plan-using-the-context-menu"></a>Visualizza piano di esecuzione stimato - Menu di scelta rapida

Richiede un piano di esecuzione query a Query Processor senza eseguire effettivamente la query e visualizza il piano nella finestra **Piano di esecuzione** . In tale piano vengono usate statistiche dell'indice per stimare il numero di righe restituite previste durante ogni parte dell'esecuzione della query. Il piano di query effettivo utilizzato può differire dal piano di esecuzione stimato. Ciò si verifica se il numero di righe restituite è diverso da quello stimato e Query Processor modifica il piano per renderlo più efficiente

### <a name="intellisense-enabled-using-the-context-menu"></a>IntelliSense abilitato - Menu di scelta rapida

Specifica se la funzionalità IntelliSense è disponibile nell'editor di query del motore di database. Questa opzione è selezionata per impostazione predefinita.

### <a name="trace-query-in-sql-server-profiler-using-the-context-menu"></a>Traccia query in SQL Server Profiler - Menu di scelta rapida

SQL Server Profiler è un'interfaccia per la creazione e la gestione di tracce e per l'analisi e la riproduzione dei risultati di tali tracce. Gli eventi vengono salvati in un file di traccia che può essere analizzato o usato successivamente per riprodurre una serie specifica di passaggi allo scopo di diagnosticare un problema.

### <a name="analyze-query-in-database-engine-tuning-advisor-using-the-context-menu"></a>Analizza query in Ottimizzazione guidata motore di database - Menu di scelta rapida

Ottimizzazione guidata motore di database di Microsoft analizza i database e offre raccomandazioni per ottimizzare le prestazioni delle query. Usare Ottimizzazione guidata motore di database per selezionare e creare un set ottimale di indici, viste indicizzate e partizioni di tabella anche senza conoscere in modo approfondito la struttura del database o le caratteristiche interne di SQL Server. Con DTA, è possibile eseguire le attività seguenti.

### <a name="design-query-in-editor-using-the-context-menu"></a>Progetta query nell'editor - Menu di scelta rapida

Progettazione query e Progettazione viste viene aperto quando si apre la definizione di una vista, si visualizzano i risultati di una query o di una vista oppure si crea o si apre una query.

### <a name="include-actual-execution-plan-using-the-context-menu"></a>Includi piano di esecuzione effettivo - Menu di scelta rapida

Esegue la query, ne restituisce i risultati e usa il piano di esecuzione per la query. Le query vengono visualizzate come piano di query grafico nella finestra **Piano di esecuzione**.

### <a name="include-live-query-statistics-using-the-context-menu"></a>Includi statistiche query dinamiche - Menu di scelta rapida

Offre informazioni approfondite in tempo reale sul processo di esecuzione della query, man mano che i controlli passano da un operatore del piano di query a un altro.

### <a name="include-client-statistics-using-the-context-menu"></a>Includi statistiche client - Menu di scelta rapida

Include una finestra **Statistiche client** contenente statistiche relative alla query e ai pacchetti di rete, nonché al tempo trascorso dall'inizio della query.

### <a name="results-using-the-context-menu"></a>Risultati - Menu di scelta rapida

![Opzioni per i risultati](media/database-engine-query-editor-sql-server-management-studio/context-menu-results.png)

È possibile scegliere l'opzione *Risultati* desiderata nel menu di scelta rapida.

- **Risultati in formato testo**: restituisce i risultati della query in formato testo nella finestra **Risultati**.

- **Risultati in formato griglia**: restituisce i risultati della query come una o più griglie nella finestra **Risultati**.

- **Risultati in un file**: all'esecuzione della query viene visualizzata la finestra di dialogo **Salva risultati**. In **Salva in** selezionare la cartella in cui si desidera salvare il file. In **Nome file** digitare il nome del file e quindi selezionare **Salva** per salvare i risultati della query come file **Report** con estensione RPT. Per visualizzare le opzioni avanzate, fare clic sulla freccia giù del pulsante **Salva** e quindi selezionare **Salva con codifica**.

### <a name="properties-window-using-the-context-menu"></a>Finestra Proprietà - Menu di scelta rapida

La [finestra Proprietà](../menu-help/properties-window-f1-help-management-studio.md) visualizza lo stato di un elemento in SQL Server Management Studio, ad esempio una connessione o un operatore Showplan, e riporta informazioni su oggetti di database come tabelle, visualizzazioni e finestre di progettazione.

Usare la finestra Proprietà per visualizzare le proprietà della connessione corrente. In questa finestra molte proprietà sono di sola lettura, ma è possibile modificarle altrove in Management Studio. La proprietà Database di una query, ad esempio, è di sola lettura nella finestra Proprietà ma può essere modificata sulla barra degli strumenti.

### <a name="query-options-using-the-context-menu"></a>Opzioni query - Menu di scelta rapida

Consente di aprire la finestra di dialogo **Opzioni query** . Usare questa finestra di dialogo per configurare le opzioni predefinite per l'esecuzione della query e i relativi risultati.

## <a name="transact-sql-f1-help"></a>Guida di Transact-SQL

L'editor di query supporta il collegamento all'argomento di riferimento per un'istruzione Transact-SQL specifica quando si seleziona F1. A tale scopo, evidenziare il nome di un'istruzione Transact-SQL, quindi selezionare F1. Il motore di ricerca della Guida cerca un argomento che dispone di un attributo della Guida corrispondente alla stringa evidenziata.

Se non vengono trovati argomenti con una parola chiave della Guida che corrisponde esattamente alla stringa evidenziata, viene visualizzato questo argomento. In tal caso esistono due approcci per trovare le informazioni che si stanno cercando:

- Copiare e incollare la stringa dell'editor evidenziata nella scheda di ricerca della documentazione online di SQL Server o effettuare una ricerca.

- Evidenziare solo la parte dell'istruzione Transact-SQL con maggiori probabilità di corrispondenza a una parola chiave della Guida applicata a un argomento e selezionare di nuovo F1. Il motore di ricerca richiede una corrispondenza esatta tra la stringa evidenziata e una parola chiave della Guida assegnata a un argomento. Se la stringa evidenziata contiene elementi univoci dell'ambiente, ad esempio nomi di colonna o di parametro, il motore di ricerca non restituisce corrispondenze. Di seguito sono riportati alcuni esempi di stringhe evidenziabili:

  - Nome di un'istruzione Transact-SQL, ad esempio SELECT, CREATE DATABASE o BEGIN TRANSACTION.

  - Nome di una funzione predefinita, ad esempio SERVERPROPERTY o @@VERSION.

  - Nome di una tabella di stored procedure di sistema o di una vista, ad esempio sys.data_spaces o sp_tableoption.

## <a name="editor-tasks"></a>Attività degli editor

| Descrizione dell'attività | Argomento |
|------------------|-------|
| Vengono descritti i vari modi in cui è possibile avviare gli editor in SSMS.| [Aprire un editor](../scripting/open-an-editor-sql-server-management-studio.md) |
| Configurare le opzioni per i vari editor, ad esempio la numerazione delle righe e le opzioni IntelliSense. | [Configurare gli editor](../scripting/configure-editors-sql-server-management-studio.md) |
| Gestire le modalità di visualizzazione, ad esempio usando il ritorno a capo automatico, la divisione di una finestra o le schede.| [Gestione dell'editor e della modalità di visualizzazione](../scripting/manage-the-editor-and-view-mode.md) |
| Impostare le opzioni di formattazione, ad esempio testo nascosto o rientri. | [Gestione della formattazione del codice](../scripting/manage-code-formatting.md) |
| Spostarsi nel testo in una finestra dell'editor mediante caratteristiche come la ricerca incrementale o la funzione "vai a". | [Spostarsi nel codice e nel testo](../scripting/navigate-code-and-text.md) |
| Impostare le opzioni di codifica a colori per varie classi di sintassi, semplificando la lettura di istruzioni complesse. | [Codifica con colori negli editor di query](../scripting/color-coding-in-query-editors.md) |
| Trascinare il testo da un percorso in uno script e rilasciarlo in un nuovo percorso.| [Trascinamento della selezione](../scripting/drag-and-drop-text.md) |
| Come impostare segnalibri per trovare più facilmente parti importanti del codice. | [Gestire i segnalibri](../scripting/manage-bookmarks.md) |
| Come stampare script o risultati in una finestra o una griglia.| [Stampa di codice e risultati](../scripting/print-code-and-results.md) |
| Modalità di visualizzazione e di utilizzo delle funzionalità di base dell'editor di query MDX. | [Creare script di Analysis Services](/analysis-services/instances/create-analysis-services-scripts-in-management-studio) |
| Modalità di visualizzazione e di utilizzo delle funzionalità di base dell'editor di query DMX. | [Creare una query DMX](/analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio) |
| Modalità di visualizzazione e di utilizzo delle funzionalità di base dell'editor XML/A. | [Editor XML](../scripting/xml-editor-sql-server-management-studio.md) |
| Come usare le funzionalità sqlcmd nell'editor di query del motore di database.| [Modificare gli script SQLCMD](../scripting/edit-sqlcmd-scripts-with-query-editor.md) |
| Come usare i frammenti di codice nell'editor di query del motore di database. I frammenti sono modelli per istruzioni o blocchi di uso comune e possono essere personalizzati o estesi per includere frammenti specifici del sito.| [Frammenti di codice T-SQL](../scripting/add-transact-sql-snippets.md) |
| Come usare il debugger Transact\-SQL per avanzare nel codice e visualizzare informazioni di debug, ad esempio i valori in variabili e parametri.| [Debugger Transact-SQL](../scripting/transact-sql-debugger.md) |

## <a name="see-also"></a>Vedere anche

- [Personalizzare i menu e i tasti di scelta rapida](../customize-menus-and-shortcut-keys.md)
- [Tasti di scelta rapida di SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)