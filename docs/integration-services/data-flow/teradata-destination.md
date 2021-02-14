---
description: Destinazione Teradata
title: Destinazione Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 83453cfa36ae4b2e9f6596acc7206162c2b82b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342277"
---
# <a name="teradata-destination"></a>Destinazione Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

La destinazione Teradata esegue il caricamento bulk dei dati nel database Teradata.

La destinazione usa Gestione connessione Teradata per la connessione a un'origine dati. Per altre informazioni, vedere [Gestione connessione Teradata](teradata-connection-manager.md).

## <a name="load-options"></a>Opzioni di caricamento

La destinazione Teradata supporta due modalità di caricamento dei dati:

- **Streaming TPT**: Questa modalità usa l'operatore Streaming dell'API TPT (protocollo Teradata TPump).

- **Caricamento TPT** (caricamento bulk rapido): Questa modalità usa l'operatore Caricamento dell'API TPT (protocollo Teradata FastLoad) per il caricamento bulk rapido.

La modalità di caricamento rapido presenta le restrizioni seguenti:

- Il limite di sessioni per il database Teradata è determinato dal fattore tra quelli sottostanti che viene rilevato per primo:
    - Limiti della sessione impostati tramite il comando SESSIONS
    - Limite del database Teradata di una sessione per AMP
    - Limite della piattaforma per il numero massimo di sessioni per applicazione: definito dalla variabile MaxSess nel file di software dell'interfaccia COP (Communications Processor), CLISPB.DAT. È possibile usare il comando TDP SET MAXSESSIONS per specificare un limite per la piattaforma. Il limite predefinito è uguale al valore MAXSESS per il server.

- Gli indici di join non sono supportati.
- I riferimenti di chiave esterna nelle tabelle di destinazione non sono supportati.
- Le tabelle di destinazione definite con un indice secondario non sono supportate.

Per altre informazioni sulle restrizioni di caricamento rapido di Teradata, vedere il riferimento per il caricamento rapido di Teradata.

È possibile impostare la modalità in [Editor destinazione Teradata (pagina Gestione connessione)](#teradata-destination-editor-connection-manager-page).

## <a name="error-handling"></a>Gestione degli errori

Gli errori restituiti durante il processo di caricamento vengono scritti in tabelle degli errori temporanee, che vengono bloccate durante il processo di caricamento.
La proprietà **Numero massimo di errori (MaxErrors)** in Editor avanzato imposta il numero massimo di errori che possono essere scritti in queste tabelle.

Se Numero massimo di errori è maggiore di zero vengono generate tabelle di errore con nomi univoci e viene visualizzato un messaggio informativo nel log del pacchetto. Gli errori possono essere recuperati tramite l'output degli errori del componente SSIS standard.

Le tabelle temporanee vengono eliminate al termine del processo di caricamento. Se le tabelle temporanee non possono essere lette dalla destinazione Teradata, non vengono eliminate a meno che non sia selezionata la proprietà **Elimina sempre la tabella errori**.  Se il processo di caricamento viene arrestato prima del completamento, potrebbe essere necessario eliminare manualmente queste tabelle. Le tabelle si trovano nello stesso database della tabella di destinazione.

Quando viene raggiunto il **Numero massimo di errori** lo stato della tabella di destinazione dipende dalla modalità usata.

- In modalità di caricamento rapido la tabella di destinazione non può essere usata. Per ripetere l'esecuzione è necessario troncare oppure eliminare e ricreare la tabella di destinazione. Il rollback non è supportato.
- In modalità Streaming TPT la destinazione Teradata viene eseguita tramite il meccanismo della riga memorizzata nel buffer. Se il processo si interrompe, tutte le modifiche completate (i buffer inviati) al momento dell'errore sono permanenti nella o nelle tabelle di destinazione. Il rollback non è supportato. Le tabelle degli errori verranno rimosse.

La destinazione Teradata include un output degli errori. Per altre informazioni, vedere [Editor destinazione Teradata (pagina Output degli errori)](#teradata-destination-editor-error-output-page).

## <a name="parallelism"></a>Parallelismo

Il parallelismo è limitato in modalità di caricamento rapido e più processi di caricamento rapido indipendenti non possono accedere contemporaneamente alla stessa tabella. Il numero di processi di caricamento rapido simultanei è limitato anche dalla variabile di database **MaxLoadTasks**.

Non esiste alcuna restrizione del parallelismo in modalità Streaming TPT. È possibile eseguire più destinazioni Teradata contemporaneamente sulla stessa tabella, anche se questo può ridurre le prestazioni per Teradata. Per altre informazioni, vedere la documentazione di Teradata.

## <a name="troubleshooting-the-teradata-destination"></a>Risoluzione dei problemi relativi alla destinazione Teradata

È possibile registrare le chiamate eseguite dall'origine Teradata all'API Teradata Parallel Transporter (API TPT). Per registrare le chiamate è possibile abilitare la registrazione dei pacchetti e selezionare l'evento **Diagnostic** a livello di pacchetto.

È possibile registrare le chiamate ODBC che l'origine Teradata esegue al driver ODBC Teradata abilitando la traccia di Gestione driver ODBC. Per altre informazioni, vedere la documentazione di Microsoft *Come generare un'analisi ODBC con l'amministratore origine dati ODBC*.

## <a name="teradata-destination-custom-properties"></a>Proprietà personalizzate della destinazione Teradata

La tabella seguente descrive le proprietà personalizzate della destinazione Teradata. Tutte le proprietà sono di lettura/scrittura.

|Nome proprietà|Tipo di dati|Descrizione|
|:-|:-|:-|
|AlwaysDropErrorTable|Boolean|L'impostazione predefinita è **False**. Se è **True** elimina tutte le tabelle di errore, anche in caso di errore di lettura della destinazione Teradata.|
|ArraySupport|Boolean|L'impostazione predefinita è **True**. Se è **True** i gruppi DML usano ArraySupport. Applicabile solo per Streaming TPT. Questa proprietà si trova in **Editor avanzato**.|
|Buffer|Integer|Numero dei buffer di richiesta da aumentare. Il valore può essere impostato da 2 a 64. Applicabile solo per Streaming TPT. Questa proprietà si trova in **Editor avanzato**.|
|BufferMode|Boolean|L'impostazione predefinita è **True**. Deve essere **True** se viene usata la funzionalità PutBuffer. Questa proprietà si trova in **Editor avanzato**.|
|BufferSize|Integer|Dimensioni del buffer di output (in KB) usato per l'invio di pacchetti di carico. Il valore predefinito è 1024. Applicabile solo per Caricamento TPT. Questa proprietà si trova in **Editor avanzato**.|
|DataEncryption|Boolean|L'impostazione predefinita è **False**. Se è **True** viene usata la crittografia di sicurezza completa.|
|DefaultCodePage|Integer|Tabella codici da usare quando l'origine dati non dispone di informazioni sulla tabella codici. <br>**Nota**: Questa proprietà si trova in **Editor avanzato**.|
|DetailedTracingLevel|Integer (enumerazione)|Selezionare una delle opzioni seguenti per la traccia avanzata: <br> **Off**: Nessuna registrazione avanzata. <br> **General** (Generale): Viene registrata la traccia generale delle attività specifiche del driver. <br> **Interfaccia della riga di comando**: Viene registrata la traccia delle attività associate a CLIv2. <br> **Notify Method** (Metodo notifica): Viene registrata la traccia delle attività associate alla funzionalità di notifica. <br> **Common Library** (Libreria comune): viene registrata la traccia delle attività della libreria opcommon. <br> **All** (Tutto): Viene registrata la traccia di tutte le attività precedenti. <br> Il file di log di traccia avanzato è definito nella proprietà **DetailedTracingFile**. <br> Se l'opzione non è Off la proprietà **DetailedTracingFile** deve essere impostata. <br> Questa proprietà si trova in **Editor avanzato**.|
|DetailedTracingFile|string|Percorso del file di log che viene generato automaticamente quando **DetailedTracingLevel** non è **Off**. Questa proprietà si trova in **Editor avanzato**.|
|DiscardLargeRow|Boolean|L'impostazione predefinita è **False**. Se è **True**, rimuove le righe di grandi dimensioni (maggiori di 64K)|
|ErrorTableName|string|Nome della tabella degli errori. L'impostazione predefinita è il nome della tabella di destinazione|
|ExtendedStringColumnsAllocation|Boolean|**Maximal Transfer Character Allocation Factor** (Fattore di allocazione massimo trasferimento caratteri) viene usato se **True**. <br> Questo valore deve essere impostato su **True** se la proprietà **Export Width Table ID** (ID tabella larghezza esportazione) del database Teradata è impostata su **Maximal Defaults** (Valori predefiniti massimi). <br> L'impostazione predefinita è **False**.|
|FastLoad|Boolean|Se è **True** viene usato il caricamento rapido. Il valore predefinito è **false**. Questa opzione può anche essere impostata in [Editor destinazione Teradata (pagina Gestione connessione)](#teradata-destination-editor-connection-manager-page).|
|MaxErrors|Integer|Numero di errori che possono verificarsi prima dell'arresto del flusso di dati. Il valore predefinito è **0**, ovvero nessun limite per il numero di errori.<br> Se è selezionato **Reindirizza flusso** nella pagina **Gestione degli errori**. Prima del raggiungimento del limite per il numero di errori, vengono restituiti tutti gli errori nell'output degli errori. Per altre informazioni, vedere [Editor destinazione Teradata (pagina Output degli errori)](#teradata-destination-editor-error-output-page).|
|MaxSessions|Integer|Numero massimo di sessioni registrate. Il valore deve essere maggiore di uno. Il valore predefinito è una sessione per ogni AMP disponibile.|
|MinSessions|Integer|Numero minimo di sessioni registrate. Il valore deve essere maggiore di uno. Il valore predefinito è una sessione per ogni AMP disponibile.|
|Pack|Integer|Numero di istruzioni da comprimere in una richiesta a più istruzioni. Il valore predefinito è 20, il valore massimo consentito è 2400. Applicabile solo per Streaming TPT. Questa proprietà si trova in **Editor avanzato**.|
|PackMaximum|Boolean|Se è **True** determina in modo dinamico il fattore di compressione massimo per il processo Streaming corrente. Applicabile solo per Streaming TPT. Questa proprietà si trova in **Editor avanzato**.|
|QueryBandSessInfo|Varchar|Espressione di banda di query basata su sessione definita dall'utente per abilitare il monitoraggio e la governance del chargeback. Questa proprietà deve essere in formato connection-string. Questa proprietà si trova in **Editor avanzato**.|
|ReplicationOverride|Integer (enumerazione)|Opzioni: <br> **Predefinita**: Non viene inviata al database nessuna istruzione SET SESSION OVERRIDE REPLICATION. Vengono usate le impostazioni predefinite del database. <br> **Il**: Viene eseguito l'override dei normali controlli del servizio di replica. <br> **Off**: Vengono usati i normali controlli del servizio di replica. <br> Questa proprietà è applicabile solo per Streaming TPT. <br> Questa proprietà si trova in **Editor avanzato**.|
|Robust|Boolean|Se è **True** viene usata la logica di riavvio Robust per le operazioni di ripristino e riavvio. Questa proprietà è applicabile solo per **Streaming TPT**. Questa proprietà si trova in **Editor avanzato**.|
|TableName|string|Nome della tabella con i dati in uso.|
|TenacityHours|Integer|Numero di ore per il quale il driver TPT prova ad accedere quando è già in esecuzione il numero massimo di operazioni di caricamento/esportazione. Il valore predefinito è 4 ore. Questa proprietà si trova in **Editor avanzato**.|
|TenacitySleep|Integer|Minuti di attesa del driver TPT prima che venga eseguito un tentativo di accesso quando viene raggiunto il limite. Il limite è definito dalle proprietà **MaxSessions** e **TenacityHours**. Il valore predefinito è 6 minuti. Questa proprietà si trova in **Editor avanzato**.|
|UnicodePassThrough|Boolean|Off (impostazione predefinita): Disabilita il pass-through Unicode. <br>On: Abilita il pass-through Unicode.|

## <a name="configuring-the-teradata-destination"></a>Configurazione della destinazione Teradata

È possibile configurare la destinazione Teradata a livello di codice o tramite Progettazione SSIS.

Editor destinazione Teradata è illustrato nell'immagine seguente. Contiene la pagina Gestione connessione, la pagina Mapping e la pagina Output degli errori.

Per ulteriori informazioni, vedere uno degli argomenti seguenti:

- [Editor destinazione Teradata (pagina Gestione connessione)](#teradata-destination-editor-connection-manager-page)
- [Editor destinazione Teradata (pagina Mapping)](#teradata-destination-editor-mappings-page)
- [Editor destinazione Teradata (pagina Output degli errori)](#teradata-destination-editor-error-output-page)

![editor destinazione](media/teradata-destination.png)

La finestra di dialogo **Editor avanzato** contiene le proprietà che è possibile impostare a livello di codice.
Per aprire la finestra di dialogo **Editor avanzato** :

- Nella schermata **Flusso di dati** del progetto Integration Services fare clic con il pulsante destro del mouse sulla destinazione Teradata e scegliere **Visualizza editor avanzato**.

Per altre informazioni sulle proprietà impostabili nella finestra di dialogo Editor avanzato, vedere [Proprietà personalizzate della destinazione Teradata](#teradata-destination-custom-properties).

## <a name="teradata-destination-editor-connection-manager-page"></a>Editor destinazione Teradata (pagina Gestione connessione)

Usare la pagina **Gestione connessione** della finestra di dialogo **Editor destinazione Teradata** per selezionare la gestione connessione Teradata per la destinazione. Tramite questa pagina è inoltre possibile selezionare una tabella o una vista del database.

Per aprire la pagina Gestione connessione dell'Editor destinazione Teradata

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con la destinazione Teradata.

- Nella scheda Flusso di dati fare doppio clic sull'origine Teradata.

- In Editor destinazione Teradata fare clic su Gestione connessione.

### <a name="options"></a>Opzioni

**Connection manager**

Consente di selezionare una gestione connessione esistente nell'elenco o di creare una nuova gestione connessione Teradata facendo clic su **Nuovo**.

**Nuovo**

Fare clic su **Nuovo**. Verrà visualizzata la finestra di dialogo **Editor gestione connessione Teradata**, in cui è possibile creare una nuova gestione connessione.

**Modalità di accesso ai dati**

Consente di selezionare il metodo per la selezione dei dati dall'origine. Le opzioni disponibili vengono visualizzate nella tabella seguente.

|Opzione|Descrizione|
|:-|:-|
|Nome tabella - Streaming TPT|Modalità incrementale con l'operatore Streaming TPT. <br>**Nome tabella o vista**: selezionare una tabella o vista esistente nell'elenco. Questo elenco visualizza solo le prime 1000 tabelle. È possibile digitare il prefisso del nome della tabella o sostituire qualsiasi parte del nome con il carattere jolly (*) per elencare la o le tabelle che si vuole usare.|
|Nome tabella - Caricamento TPT|Modalità di caricamento veloce (percorso diretto) che usa l'operatore di caricamento API TPT (Teradata FastLoad Protocol) e richiede che la tabella di destinazione sia vuota. <br>**Nome tabella o vista**: selezionare una tabella o vista esistente nell'elenco. Questo elenco visualizza solo le prime 1000 tabelle. È possibile digitare il prefisso del nome della tabella o sostituire qualsiasi parte del nome con il carattere jolly (*) per elencare la o le tabelle che si vuole usare.|

**Crittografia dei dati**: casella di controllo per abilitare la crittografia dei dati. Per impostazione predefinita l'opzione non è selezionata.

**Elimina sempre la tabella errori**: casella di controllo per eliminare le tabelle degli errori in tutte le istanze.

**Tabella errori**: nome della tabella in cui vengono scritti gli errori.

**Numero minimo di sessioni**: numero minimo di sessioni registrate. Il valore predefinito è una sessione per ogni AMP disponibile. Il valore deve essere maggiore di 1.

**Numero massimo di sessioni**: numero massimo di sessioni registrate. Il valore predefinito è una sessione per ogni AMP disponibile. Il valore deve essere maggiore di 1.

**Numero massimo di errori**: numero massimo di errori che possono essere restituiti prima che il flusso di dati venga interrotto o reindirizzato. 

## <a name="teradata-destination-editor-mappings-page"></a>Editor destinazione Teradata (pagina Mapping)

Usare la pagina **Mapping** della finestra di dialogo **Editor destinazione Teradata** per eseguire il mapping tra colonne di input e colonne di destinazione.

Per aprire la pagina Mapping di Editor destinazione Teradata

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con la destinazione Teradata.

- Nella scheda Flusso di dati fare doppio clic sull'origine Teradata.

- In Editor destinazione Teradata fare clic su Mapping.

### <a name="options"></a>Opzioni

**Colonne di input disponibili**

Elenco delle colonne di input disponibili. Trascinare un colonna di input in una colonna di destinazione disponibile per eseguire il mapping tra le colonne.

**Colonne di destinazione disponibili**

Elenco delle colonne di destinazione disponibili. Trascinare un colonna di destinazione in una colonna di input disponibile per eseguire il mapping tra le colonne.

**Colonna di input**

Consente di visualizzare le colonne di input selezionate dall'utente. È possibile rimuovere i mapping selezionando **< ignora >** per escludere colonne dall'output.

**Colonna di destinazione**

Consente di visualizzare tutte le colonne di destinazione disponibili, con o senza mapping eseguito.

>[!NOTE]
>
>Le colonne con tipi di dati non supportati vengono eliminate dal mapping con un avviso.

## <a name="teradata-destination-editor-error-output-page"></a>Editor destinazione Teradata (pagina Output degli errori)
Usare la pagina Output degli errori della finestra di dialogo Editor destinazione Teradata per selezionare le opzioni di gestione degli errori.

**Per aprire la pagina Output degli errori di Editor destinazione Teradata**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con la destinazione Teradata.

- Nella scheda Flusso di dati fare doppio clic sull'origine Teradata.

- In Editor destinazione Teradata fare clic su Output degli errori.

### <a name="options"></a>Opzioni

**Comportamento in caso di errore**

Consente di selezionare il modo in cui la destinazione Teradata deve gestire gli errori in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.

**Argomenti correlati**: [Gestione degli errori nei dati](./error-handling-in-data.md)

**Troncamento**

Consente di selezionare il modo in cui la destinazione Teradata deve gestire il troncamento in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.

## <a name="next-steps"></a>Passaggi successivi

- Configurare la [gestione connessione Teradata](teradata-connection-manager.md)
- Configurare l'[origine Teradata](teradata-source.md)
- Configurare la [destinazione Teradata](teradata-destination.md)
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA6iwdw).
