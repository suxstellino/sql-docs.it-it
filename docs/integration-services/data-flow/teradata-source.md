---
description: Connettersi all'origine Teradata
title: Connettersi all'origine Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0170a81d73edef1e893a16df93e1bc004415739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337807"
---
# <a name="connect-to-the-teradata-source"></a>Connettersi all'origine Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

L'origine Teradata estrae i dati dai database Teradata usando:
- Una tabella o una vista:
- Risultato di un'istruzione SQL.

L'origine usa la gestione connessione Teradata per connettersi all'origine Teradata. Per altre informazioni, vedere [Usare la gestione connessione Teradata](teradata-connection-manager.md).

## <a name="troubleshoot-the-teradata-source"></a>Risolvere i problemi dell'origine Teradata

È possibile registrare le chiamate eseguite dall'origine Teradata all'API Teradata Parallel Transporter (TPT). A tale scopo abilitare la registrazione dei pacchetti e selezionare l'evento **Diagnostic** a livello di pacchetto.

È possibile registrare le chiamate ODBC (Open Database Connectivity) che l'origine Teradata esegue al driver ODBC Teradata abilitando la traccia di Gestione driver ODBC. Per altre informazioni, vedere [Come generare un'analisi ODBC con l'amministratore origine dati ODBC](../../odbc/admin/setting-tracing-options.md).

## <a name="parallelism"></a>Parallelismo

L'origine Teradata supporta il parallelismo, in cui i processi di esportazione possono accedere alla stessa tabella o a tabelle diverse nello stesso momento. La variabile di database `MaxLoadTasks` imposta il numero massimo di processi di esportazione eseguibili contemporaneamente. È possibile definire questo numero massimo usando la variabile `MaxLoadTasks`.

## <a name="teradata-source-custom-properties"></a>Proprietà personalizzate dell'origine Teradata

Le proprietà dell'interfaccia Teradata sono elencate nella tabella seguente. Tutte le proprietà sono di lettura/scrittura.

|Nome proprietà|Tipo di dati|Descrizione|
|:-|:-|:-|
|AccessMode|Integer (enumerazione)|Modalità utilizzata per accedere al database. I valori possibili sono *Table Name* e *SQL Command*. Il valore predefinito è *Table Name*.|
|BlockSize|Integer|Dimensioni in byte del blocco usato per la restituzione di dati al client. Il valore predefinito è 1048576 (1 MB). Il valore minimo è 256 byte. Il valore massimo è 16775168 byte.<br> Questa proprietà si trova nel riquadro **Editor avanzato**.|
|BufferMaxSize|Integer|Dimensioni massime totali del buffer di dati restituito dalla funzione GetBuffer. Devono essere sufficienti per contenere almeno una riga di dati, che include l'intestazione di riga, la riga di dati vera e propria e la sequenza finale del buffer. Le dimensioni massime totali predefinite del buffer di dati sono pari a 16775552 byte. <br>Per altre informazioni, vedere [Esportare dati da un database Teradata usando GetBuffer](https://docs.teradata.com/reader/TvVKKmxaBAoyETJZD8zz_g/oaxiwNJmnCa6UctY4k498w).|
|BufferMode|Boolean|Il valore predefinito è *True*. Il valore deve essere *True* se viene usata la funzionalità PutBuffer. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|DataEncryption|Boolean|Il valore predefinito è *False*. Se il valore è *True* viene usata la crittografia di sicurezza completa.|
|DefaultCodePage|Integer|Tabella codici da usare quando l'origine dati non dispone di informazioni sulla tabella codici. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|DetailedTracingLevel|Integer (enumerazione)|Selezionare una delle opzioni seguenti per la traccia avanzata: <br> *Off*: Nessuna registrazione avanzata. <br> *General* (Generale): Viene registrata la traccia generale delle attività specifiche del driver. <br> *Interfaccia della riga di comando*: Viene registrata la traccia delle attività associate a CLIv2. <br> *Notify Method* (Metodo notifica): Viene registrata la traccia delle attività associate alla funzionalità di notifica. <br> *Common Library* (Libreria comune): viene registrata la traccia delle attività della libreria opcommon. <br> *All* (Tutto): Viene registrata la traccia di tutte le attività precedenti. <br> Il file di log di traccia avanzato è definito nella proprietà `DetailedTracingFile`. <br> La proprietà `DetailedTracingFile` deve essere impostata se l'opzione non è *Off*. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|DetailedTracingFile|string|Percorso del file di log che viene generato automaticamente quando *DetailedTracingLevel* non è *Off*. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|DiscardLargeRow|Boolean|Il valore predefinito è *False*. Ignora le righe grandi (di dimensioni superiori a 64 kB) se il valore è *True*.|
|ExtendedStringColumnsAllocation|Boolean|*Maximal Transfer Character Allocation Factor* (Fattore di allocazione massimo trasferimento caratteri) viene usato se il valore è *True*. <br> Questo valore deve essere impostato su *True* se la proprietà `Export Width Table ID` del database Teradata è impostata su *Maximal Defaults* (Valori predefiniti massimi). <br> Il valore predefinito è *False*.|
|JobMaxRowSize|Integer|È possibile supportare la dimensione massima delle righe. Questo valore è necessario se il valore `DiscardLargeRow` è *True*.<br>Valori validi: <br>*64* (valore predefinito): è possibile supportare lunghezze di riga a 2 byte. <br>*1024*: è possibile supportare lunghezze di riga a 4 byte.|
|MaxSessions|Integer|Numero massimo di sessioni registrate. Il valore deve essere maggiore di uno. Il valore predefinito è una sessione per ogni processore di modulo di accesso (AMP, Access Module Processor) disponibile.|
|MinSessions|Integer|Numero minimo di sessioni registrate. Il valore deve essere maggiore di uno. Il valore predefinito è una sessione per ogni AMP disponibile.|
|QueryBandSessInfo|Varchar|Espressione di banda di query basata sulla sessione e definita dall'utente in un formato connection-string. Questa proprietà viene usata per il monitoraggio e la governance del chargeback. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|SpoolMode|Varchar|I valori validi sono: <br>*Spool*: Usare il valore predefinito *Spool*. <br> *NoSpool*: Non usare *Spool*. Questo valore è valido solo se il server di database (DBS) supporta *NoSpool*. <br>  *NoSpoolOnly*: Non usare *Spool* in nessun caso. Il processo verrà terminato con un errore se il server di database non supporta *NoSpool*.|
|SqlCommand|string|Comando SQL da eseguire quando `AccessMode` è impostata su *SQL Command*.|
|TableName|string|Nome della tabella contenente i dati da usare quando la proprietà `AccessMode` è impostata su *Table Name*.|
|TenacityHours|Integer|Numero di ore per il quale il driver TPT prova ad accedere quando è già in esecuzione il numero massimo di operazioni di caricamento/esportazione. Il valore predefinito è *4 ore*. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|TenacitySleep|Integer|Numero di minuti di attesa del driver TPT prima che venga eseguito un tentativo di accesso quando viene raggiunto il limite. Il limite è definito dalle proprietà `MaxSessions` e `TenacityHours`. Il valore predefinito è 6 minuti. Questa proprietà si trova nel riquadro **Editor avanzato**.|
|UnicodePassThrough|Boolean|*Off* (valore predefinito): Disabilita il pass-through Unicode. <br>*Il*: Abilita il pass-through Unicode.|

## <a name="configure-the-teradata-source"></a>Configurare l'origine Teradata

È possibile configurare l'origine Teradata a livello di codice o tramite la finestra di progettazione SQL Server Integration Services (SSIS).

Il riquadro **Editor origine Teradata** è illustrato nella figura seguente. Per altre informazioni, vedere le sezioni seguenti dell'Editor origine Teradata:

- [Riquadro Gestione connessione](#the-connection-manager-pane)
- [Riquadro Colonne](#the-columns-pane)
- [Riquadro Output degli errori](#the-error-output-pane)

![Editor origine Teradata](media/teradata-source.png)

Il riquadro **Editor avanzato** contiene le proprietà impostabili a livello di codice. Per aprire il riquadro:
- Nella pagina **Flusso di dati** del progetto Integration Services fare clic con il pulsante destro del mouse sull'origine Teradata e selezionare **Visualizza editor avanzato**.

Per altre informazioni sulle proprietà impostabili nella finestra di dialogo **Editor avanzato**, vedere [Proprietà personalizzate dell'origine Teradata](#teradata-source-custom-properties).

## <a name="the-connection-manager-pane"></a>Riquadro Gestione connessione

Usare il riquadro **Gestione connessione Teradata** per selezionare l'istanza della gestione connessione Teradata per l'origine. In questo riquadro è anche possibile selezionare una tabella o una vista dal database. Per aprire il riquadro:

1. In SQL Server Data Tools aprire il pacchetto SSIS contenente l'origine Teradata.

1. Nella scheda **Flusso di dati** fare doppio clic sull'origine Teradata.

1. Nell'Editor origine Teradata selezionare la scheda **Gestione connessione**.

### <a name="options"></a>Opzioni

**Connection manager**

* Selezionare una gestione connessione esistente nell'elenco o creare una nuova istanza di gestione connessione Teradata selezionando **Nuovo**.

**Nuovo**

* Selezionare **Nuovo**. Viene visualizzato il riquadro **Editor gestione connessione Teradata**. Da questo riquadro è possibile creare una nuova gestione connessione.

**Modalità di accesso ai dati**

* Selezionare un metodo per la selezione dei dati dall'origine. Le opzioni disponibili vengono visualizzate nella tabella seguente.

    |Opzione|Descrizione|
    |:-|:-|
    |Nome tabella - Esportazione TPT|Consente di recuperare dati da una tabella o da una vista nell'origine dati Teradata. Quando questa opzione è selezionata, selezionare una tabella o una vista disponibile nell'elenco per **Nome tabella o vista**.|
    |Comando SQL - Esportazione TPT|Consente di recuperare dati dall'origine dati Teradata usando una query SQL. Quando questa opzione è selezionata, immettere una query in uno dei modi seguenti: <ul><li>Immettere il testo della query SQL nel campo **Testo comando SQL** .</li><li>Selezionare **Sfoglia** per caricare la query SQL da un file di testo.</li><li>Selezionare **Analizza query** per verificare la sintassi del testo della query.</li></ul>|

**Anteprima**

* Selezionare **Anteprima** per visualizzare un massimo di 200 righe dei dati estratti dalla tabella o dalla vista selezionata.

## <a name="the-columns-pane"></a>Riquadro Colonne

Usare il riquadro **Colonne** per eseguire il mapping di una colonna di output a ogni colonna esterna (di origine). Per aprire il riquadro:

1. In SQL Server Data Tools aprire il pacchetto SSIS contenente l'origine Teradata.

1. Nella scheda **Flusso di dati** fare doppio clic sull'origine Teradata.

1. Nell'Editor origine Teradata selezionare la scheda **Colonne**.

### <a name="options"></a>Opzioni

**Colonne esterne disponibili**

Questa tabella elenca le colonne esterne disponibili che è possibile selezionare per aggiungerle all'elenco **Colonne esterne**. È possibile elencare le colonne nell'ordine scelto. Non è possibile usare questa tabella per l'aggiunta o l'eliminazione di colonne.

* Per scegliere tutte le colonne selezionare la casella di controllo **Seleziona tutto**.

**Colonne esterne**

Le colonne (di origine) esterne selezionate sono elencate in ordine. Per modificare l'ordine, cancellare prima l'elenco **Colonne esterne disponibili**, quindi selezionare una o più colonne in un ordine diverso.

**Colonna di output**

Anche se il nome della colonna (di origine) esterna selezionata è il nome di output predefinito, è possibile immettere qualsiasi nome univoco.

>[!NOTE]
>>Se sono presenti colonne contenenti tipi di dati non supportati viene visualizzato un avviso con i tipi di dati non supportati e le colonne corrispondenti vengono rimosse dalle colonne di mapping.

## <a name="the-error-output-pane"></a>Riquadro Output degli errori

Usare il riquadro **Output degli errori** per selezionare le opzioni di gestione degli errori. Per aprire il riquadro:

1. In SQL Server Data Tools aprire il pacchetto SSIS contenente l'origine Teradata.

1. Nella scheda **Flusso di dati** fare doppio clic sull'origine Teradata.

1. Nell'Editor origine Teradata selezionare la scheda **Output degli errori**.

### <a name="options"></a>Opzioni

**Comportamento in caso di errore**

* Consente di selezionare come l'origine Teradata gestisce gli errori in un flusso: 
  * Ignorare l'errore
  * Reindirizzare la riga
  * Interrompere il componente

**Argomenti correlati**: Vedere [Gestione degli errori nei dati](error-handling-in-data.md).

**Troncamento**

* Consente di selezionare come l'origine Teradata gestisce il troncamento in un flusso: 
  * Ignorare l'errore
  * Reindirizzare la riga
  * Interrompere il componente

## <a name="next-steps"></a>Passaggi successivi

- Configurare la [destinazione di Teradata](teradata-destination.md).
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA6iwdw).