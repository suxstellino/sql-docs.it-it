---
title: Estensione di virtualizzazione dei dati
description: Estensione di virtualizzazione dei dati per Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: alayu, sstein, maghan
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: a428322867df8585160a3363d174b2cd92bd1f17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052052"
---
# <a name="data-virtualization-extension-for-azure-data-studio"></a>Estensione di virtualizzazione dei dati per Azure Data Studio

L'estensione di virtualizzazione dei dati per Azure Data Studio offre il supporto per la [procedura guidata Tabella esterna con origini dati ODBC](../../relational-databases/polybase/data-virtualization.md).

## <a name="install-the-data-virtualization-extension"></a>Installare l'estensione di virtualizzazione dei dati

Per installare l'estensione per la virtualizzazione dei dati, vedere [Estendere la funzionalità di Azure Data Studio](./add-extensions.md).

## <a name="changes-in-release-10"></a>Modifiche nella versione 1.0

* Ridenominazione in "estensione di virtualizzazione dei dati".
* Procedura guidata di creazione tabella esterna:
  * Disponibilità di notebook guidati per la virtualizzazione di origini MongoDB e Teradata.
  * Introduzione di una finestra di dialogo per compilare le variabili nei notebook di virtualizzazione MongoDB e Teradata.

## <a name="changes-in-release-016"></a>Modifiche nella versione 0.16

* Procedura guidata di creazione tabella esterna:
  * Miglioramento della gestione degli errori durante il caricamento di tabelle e viste nella pagina di mapping degli oggetti.

## <a name="changes-in-release-015"></a>Modifiche nella versione 0.15

* Procedura guidata di creazione tabella esterna:
  *  Riduzione del tempo necessario per caricare le informazioni relative a tabelle e colonne nella pagina di mapping degli oggetti.
  * Correzione di un bug relativo al caricamento delle credenziali con ambito database esistenti nella pagina Dettagli connessione.
* Procedura guidata per la creazione di una tabella esterna da file CSV:
  * Aumento delle dimensioni predefinite del campione usato per l'analisi PROSE.

## <a name="changes-in-release-0141"></a>Modifiche nella versione 0.14.1

* Supporto per l'origine dati CTP 3.1

## <a name="changes-in-release-0121"></a>Modifiche nella versione 0.12.1

* Il tipo di connessione **cluster Big Data di SQL Server** è stato rimosso in questa versione. Tutte le funzionalità precedentemente disponibili dalla connessione cluster Big Data di SQL Server sono ora disponibili nella connessione SQL Server.
* L'esplorazione HDFS è disponibile nella cartella **Servizi dati**
* Per i notebook, PySpark e altri kernel Big Data funzionano quando si è connessi all'istanza master di SQL Server nel cluster Big Data di SQL Server.
* Procedura guidata di creazione tabella esterna:
  * Supporto per la creazione di una tabella esterna mediante un'origine dati esterna esistente.
  * Miglioramenti delle prestazioni nella procedura guidata.
  * Gestione migliorata dei nomi degli oggetti con caratteri speciali. In alcuni casi questi causavano l'esito negativo della procedura guidata.
  * Miglioramenti dell'affidabilità per la pagina del mapping oggetti.
  * Rimozione dei database di sistema `DWConfiguration`, `DWDiagnostics`, `DWQueue` dall'elenco a discesa dei database.
  * Supporto per l'impostazione del nome dell'oggetto Formato di file esterno nella procedura guidata **Create External Table from CSV Files** (Crea tabella esterna da file CSV).
  * È stato aggiunto un pulsante di aggiornamento alla prima pagina della procedura guidata **Create External Table from CSV Files** (Crea tabella esterna da file CSV).

## <a name="release-notes-v0110"></a>Note sulla versione (v0.11.0)

* Il supporto di Jupyter Notebook, in particolare il supporto per i kernel Python3 e Spark, è stato spostato in Azure Data Studio. Questa estensione non è più necessaria per poter usare i notebook.
* Più correzioni di bug nelle procedure guidate per i dati esterni:
  * I mapping dei tipi Oracle sono stati aggiornati in modo da corrispondere alle modifiche incluse in SQL Server 2019 CTP 2.3.
  * È stato risolto un problema per cui i nuovi schemi digitati nei controlli di mapping delle tabelle andavano persi.
  * È stato risolto un problema per cui il controllo di un nodo di database nei mapping di tabella non comportava il controllo di tutte le tabelle e le viste.

## <a name="release-notes-v0102"></a>Note sulla versione (v 0.10.2)

### <a name="sql-server-2019-support"></a>Supporto di SQL Server 2019

Il supporto per SQL Server 2019 è stato aggiornato. Dopo la connessione a un'istanza del cluster Big Data di SQL Server, nell'albero di esplorazione viene visualizzata una nuova cartella _Servizi dati_. Questa offre punti di avvio per azioni quali l'apertura di un nuovo notebook sulla connessione, l'invio di processi Spark e l'uso di HDFS. Per alcune azioni, ad esempio la _creazione di dati esterni_ in un file o una cartella HDFS, è necessario installare l'estensione _SQL Server 2019_.

### <a name="notebook-support"></a>Supporto di notebook

Sono stati apportati aggiornamenti significativi all'interfaccia utente per i notebook. L'obiettivo è quello di semplificare la lettura dei notebook condivisi con l'utente. Sono state rimosse tutte le caselle di contorno intorno alle celle, a meno che non siano selezionate o evidenziate con il mouse, aggiungendo il supporto del passaggio del mouse per semplificare le azioni a livello di cella senza bisogno di selezionare una cella. Lo stato di esecuzione è ora più chiaro grazie all'aggiunta del conteggio delle esecuzioni, di un pulsante _Arresta esecuzione_ animato e altro ancora. Sono state aggiunge scelte rapide da tastiera per _New Notebook_ (Nuovo notebook) (`Ctrl+Shift+N`), _Run Cell_ (Esegui cella) (`F5`), _New Code Cell_ (Nuova cella di codice) (`Ctrl+Shift+C`), _New Text Cell_ (Nuova cella di testo) (`Ctrl+Shift+T`). L'obiettivo è fare in modo che tutte le azioni chiave si possano lanciare tramite scelta rapida da tastiera, quindi ogni suggerimento sulle combinazioni mancanti è il benvenuto.

Altri miglioramenti e correzioni includono:

* L'estensione _SQL Server 2019_ richiede ora agli utenti di selezionare una directory di installazione per le dipendenze Python. Non include più Python nel `.vsix file`, riducendo così le dimensioni complessive dell'estensione. Le dipendenze Python supportano i kernel Spark e Python3.
* È stato aggiunto il supporto per l'avvio di un nuovo notebook dalla riga di comando. L'avvio con gli argomenti `--command=notebook.command.new --server=myservername` dovrebbe aprire un nuovo notebook e connettersi a questo server.
* Correzioni delle prestazioni per notebook con codice di lunghezza elevata nelle celle. Se le celle di codice sono lunghe più di 250 righe, viene aggiunta una barra di scorrimento.
* Miglioramento del supporto per i file con estensione `.ipynb`. È ora supportata la versione 3 o successiva. 

    >[!Note]
    > Salvataggio degli aggiornamenti dei file nella versione 4 o successiva.

* L'impostazione utente `notebook.enabled` è stata rimossa ora che il visualizzatore dei notebook predefinito è stabile.
* È ora supportato il tema a contrasto elevato, con una serie di correzioni al layout degli oggetti.
* È stato corretto il problema 3680, per cui gli output talvolta mostravano erroneamente una serie di caratteri `,,,`.
* È stato corretto il problema 3602, per cui quando si esce dalla pagina di Azure Data Studio scompare l'editor per le celle.
* È stato aggiunto il supporto per l'uso delle visualizzazioni griglia per il tipo MIME di output `application/vnd.dataresource+json`. Questo significa che molti notebook che lo usano, ad esempio impostando `pd.options.display.html.table_schema` in un notebook Python, hanno output tabulari più ordinati.

#### <a name="known-issues"></a>Problemi noti

* Quando si apre un notebook, viene visualizzata la finestra di dialogo per l'installazione di Python. Se si annulla questa installazione, gli elenchi a discesa Kernel e Collega a non visualizzano i valori previsti. La soluzione alternativa consiste nel completare l'installazione di Python.
* Quando un notebook viene aperto con un kernel non supportato, gli elenchi a discesa Kernel e _Collega a_ causano il blocco di Azure Data Studio. Chiudere Azure Data Studio e assicurarsi di usare un kernel supportato (Python3, Spark | R, Spark | Scala, PySpark, PySpark3).
* Il collegamento all'interfaccia utente Spark ha esito negativo quando si usa PySpark3 o altri kernel Spark sull'endpoint SQL Server. Come soluzione alternativa, selezionare Spark UI (Interfaccia utente Spark) nel dashboard o connettersi usando il tipo di connessione cluster Big Data di SQL Server, perché il collegamento ipertestuale all'interfaccia utente Spark è corretto.

### <a name="extensibility-improvements"></a>Miglioramenti dell'estendibilità

In questa versione sono stati aggiunti alcuni miglioramenti utili per i controlli Extender.

* Una nuova API `ObjectExplorerNodeProvider` consente alle estensioni di inserire cartelle in SQL Server o altri nodi di connessione. Questo è il modo in cui il nodo `Data Services` viene aggiunto nelle istanze di SQL Server 2019, ma l'API si può usare per aggiungere facilmente una cartella Monitoraggio o altre cartelle all'interfaccia utente.
* Sono disponibili due nuovi valori di chiave di contesto per visualizzare/nascondere i contributi al dashboard.
  * `mssql:iscluster` indica se si tratta di un cluster Big Data di SQL Server 2019
  * `mssql:servermajorversion` indica la versione del server (15 per SQL Server 2019, 14 per SQL Server 2017 e così via). Questo può essere utile se le funzionalità devono essere visualizzate, ad esempio, solo per SQL Server 2017 o versione successiva.

## <a name="release-notes-v080"></a>Note sulla versione (v0.8.0)

*Notebook*:

* L'aggiunta di celle prima/dopo le celle esistenti è ora supportata selezionando il pulsante della cella "Altre azioni"
* L'opzione **Aggiungi nuova connessione** è stata aggiunta alle connessioni nell'elenco a discesa "Collega a"
* È stato aggiunto un comando **Reinstall Notebook Dependencies** (Reinstalla dipendenze notebook) per facilitare gli aggiornamenti dei pacchetti Python e risolvere i casi in cui l'installazione è stata interrotta chiudendo l'applicazione. Può essere eseguito dal riquadro comandi (usare `Ctrl/Cmd+Shift+P` e digitare `Reinstall Notebook Dependencies`)
* Il pacchetto Python PROSE è stato aggiornato alla versione 1.1.0 e include una serie di correzioni di bug. Usare il comando **Reinstall Notebook Dependencies** (Reinstalla dipendenze notebook) per aggiornare questo pacchetto
* È ora supportato un comando **Cancella output** selezionando il pulsante della cella **Altre azioni**
* Sono stati corretti i seguenti problemi segnalati dai clienti:
  * Non era possibile avviare la sessione del notebook in Windows a causa di problemi di PATH
  * Non era possibile avviare il notebook dalla cartella radice di un'unità, ad esempio C:\ o D:\
  * [2820](https://github.com/Microsoft/azuredatastudio/issues/2820) Non è possibile modificare i notebook creati da ADS in VS Code
  * Il collegamento all'interfaccia utente Spark ora funziona quando si esegue un kernel Spark
  * Managed Packages (Pacchetti gestiti) è stato rinominato in "Installa pacchetti"

*Creazione di dati esterni*:

* I messaggi di errore sono copiabili e sono stati separati in un riepilogo e una visualizzazione dettagliata per semplificare la lettura
* Miglioramento del layout dell'interfaccia utente, dell'affidabilità e della gestione degli errori
* Sono stati corretti i seguenti problemi segnalati dai clienti:
  * Le tabelle con mapping di colonna non validi vengono visualizzate come disabilitate e un avviso spiega l'errore

## <a name="release-notes-v072"></a>Note sulla versione (v0.7.2)

* Azure Resource Explorer ora è incorporata in Azure Data Studio ed è stato rimosso da questa estensione. Grazie per i commenti e suggerimenti inviati.
* Miglioramento delle prestazioni dei notebook con molte celle di markdown.
* Ridimensionamento automatico delle celle di codice nel notebook. Esiste ancora una dimensione minima in base alla barra degli strumenti della cella.
* Notifica all'utente durante l'installazione delle dipendenze del notebook. In Windows, in particolare, questa operazione può richiedere molto tempo, quindi le notifiche vengono ora mostrate nella visualizzazione Attività.
* Supporto per la reinstallazione delle dipendenze del notebook. Utile se in precedenza l'utente ha chiuso Azure Data Studio durante l'installazione.
* Supporto per l'annullamento dell'esecuzione di celle nel notebook.
* Maggiore affidabilità nell'uso della procedura guidata di creazione dati esterni, in particolare quando si verificano errori di connessione.
* Blocco dell'uso della procedura guidata di creazione dati esterni se PolyBase non è abilitato o non è in esecuzione nel server di destinazione.
* Correzioni di ortografia e di denominazione correlate a SQL Server 2019 e alla creazione dati esterni.
* Rimozione di un numero elevato di errori dalla console di debug di Azure Data Studio.