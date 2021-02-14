---
description: Destinazione Oracle
title: Destinazione Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3b28d420d66e55565c882c9fa2ae41f86e1c0764
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343228"
---
# <a name="oracle-destination"></a>Destinazione Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Tramite la destinazione Oracle viene eseguito il caricamento bulk di dati in Oracle Database.

La destinazione usa Gestione connessione Oracle per la connessione a un'origine dati. Per altre informazioni, vedere [Gestione connessione Oracle](oracle-connection-manager.md).

Una destinazione Oracle include i mapping tra le colonne di input e le colonne presenti nell'origine dati di destinazione. Non è necessario eseguire il mapping delle colonne di input a tutte le colonne di destinazione ma, a seconda delle proprietà delle colonne di destinazione, se alle colonne di destinazione non corrispondono colonne di input è possibile che vengano generati errori. Se, ad esempio, una colonna di destinazione non ammette valori Null, sarà necessario eseguire il mapping di una colonna di input a tale colonna. Inoltre, se i dati di input non sono compatibili per il tipo di colonna di destinazione, si verifica un errore in fase di esecuzione. A seconda dell'impostazione del comportamento in seguito all'errore, l'errore viene ignorato, provoca un problema o la riga viene reindirizzata all'output degli errori.

La destinazione Oracle include un input regolare e un output degli errori.

Le colonne con tipi di dati non supportati vengono eliminate dalle colonne con un avviso prima del mapping.
Per altre informazioni, vedere [Supporto dei tipi di dati](oracle-data-type-support.md).

## <a name="load-options"></a>Opzioni di caricamento

Sono supportate due modalità di caricamento di accesso. La modalità può essere impostata in [Editor destinazione Oracle (pagina Gestione connessione)](#oracle-destination-editor-connection-manager-page). Le due modalità sono:

- Caricamento batch: questa modalità consente di caricare i dati nella tabella Oracle in batch e l'intero batch viene inserito nella stessa transazione.
Per informazioni dettagliate su come configurare questa modalità, vedere [Editor destinazione Oracle (pagina Gestione connessione)](#oracle-destination-editor-connection-manager-page) e [Proprietà personalizzate della destinazione Oracle](#oracle-destination-custom-properties).

- Caricamento rapido con percorso diretto: questa modalità consente di usare la modalità con percorso diretto del driver per il caricamento della tabella Oracle. Sono previste restrizioni per l'uso di questa modalità. Per informazioni dettagliate, vedere la documentazione di Oracle.  
Per informazioni dettagliate su come configurare questa modalità, vedere [Editor destinazione Oracle (pagina Gestione connessione)](#oracle-destination-editor-connection-manager-page) e [Proprietà personalizzate della destinazione Oracle](#oracle-destination-custom-properties).

## <a name="error-handling"></a>Gestione degli errori

La destinazione Oracle include un output degli errori. L'output degli errori del componente include le colonne di output seguenti:

- **Error Code** (Codice errore): numero che rappresenta il tipo di errore dell'errore corrente. Il codice di errore può essere generato da:
    - Server Oracle. Vedere la descrizione dettagliata dell'errore nella documentazione di Oracle Database.
    - Runtime SSIS. Per un elenco dei codici di errore SSIS, vedere la Guida di riferimento ai messaggi e ai codici di errore SSIS.
- **Error Column**(Colonna errore): numero della colonna di origine che causa gli errori di conversione.

- **Error Data Columns** (Colonne dati errore): dati che causano l'errore.

I tipi di errori di output supportati durante il processo di caricamento sono: conversione dei dati, troncamento o violazione di vincoli e così via. Vedere [Editor destinazione Oracle (pagina Output degli errori)](#oracle-destination-editor-error-output-page).

La proprietà **Numero massimo di errori (MaxErrors)** imposta il numero massimo di errori che possono verificarsi. Quando viene raggiunto il numero massimo, l'esecuzione viene arrestata e restituisce gli errori. Inoltre, vengono inclusi nella tabella di destinazione solo i record di esecuzione prima del raggiungimento del numero massimo di errori. Per informazioni dettagliate sulla configurazione, vedere [Editor destinazione Oracle (pagina Gestione connessione)](#oracle-destination-editor-connection-manager-page).

## <a name="parallelism"></a>Parallelismo

In modalità di caricamento batch non esiste alcuna restrizione per la configurazione dell'esecuzione parallela, ma le prestazioni potrebbero essere influenzate dal meccanismo standard di blocco dei record. L'entità della riduzione delle prestazioni dipende dall'organizzazione dei dati e delle tabelle.

Nel protocollo con percorso diretto (caricamento rapido) può essere configurata una sola destinazione Oracle per l'esecuzione su una stessa tabella, ma è possibile fare uso della modalità parallela.

Un percorso diretto parallelo consente il caricamento di più percorsi diretti, con cui è possibile configurare più destinazioni Oracle per l'esecuzione simultanea sulla stessa tabella. Poiché Oracle non blocca la tabella di destinazione in modalità esclusiva per l'uso nella sessione di caricamento rapido, ciò consente l'esecuzione di ulteriori componenti di destinazione di caricamento rapido per caricare la stessa tabella di destinazione in parallelo.
Il percorso diretto parallelo è più restrittivo. Qualsiasi uso del parallelismo deve essere pianificato in anticipo.

Non esiste alcun motivo per usare una singola sessione parallela.

Per informazioni sulle restrizioni per l'uso di caricamenti con percorsi diretti paralleli, vedere la documentazione di Oracle.

Per altre informazioni, vedere [Proprietà personalizzate della destinazione Oracle](#oracle-destination-custom-properties).

## <a name="troubleshooting-the-oracle-destination"></a>Risoluzione dei problemi relativi alla destinazione Oracle

È possibile registrare le chiamate ODBC eseguite dall'origine Oracle alle origini dati Oracle per risolvere i problemi relativi all'esportazione dei dati. Per registrare le chiamate ODBC eseguite dall'origine Oracle alle origini dati Oracle, abilitare la traccia di Gestione driver ODBC. Per altre informazioni, vedere la documentazione di Microsoft su *come generare una traccia ODBC con l'amministratore dell'origine dati ODBC*.

## <a name="oracle-destination-custom-properties"></a>Proprietà personalizzate della destinazione Oracle

La tabella seguente descrive le proprietà personalizzate della destinazione Oracle. Tutte le proprietà sono di lettura/scrittura.

|Nome proprietà|Tipo di dati|Descrizione|Modalità di caricamento|
|:-|:-|:-|:-|
|BatchSize|Integer|Dimensioni del batch per il caricamento bulk. Si tratta del numero di righe caricate come batch.|Usato solo in modalità batch.|
|DefaultCodePage|Integer|Tabella codici da usare quando l'origine dati non dispone di informazioni sulla tabella codici. <br>**Nota**: Questa proprietà è impostata solo da **Editor avanzato**.|Usato per entrambe le modalità.|
|FastLoad|Boolean|Indica se viene usato il caricamento rapido. Il valore predefinito è **false**. Questa opzione può anche essere impostata in [Editor destinazione Oracle (pagina Gestione connessione)](#oracle-destination-editor-connection-manager-page). |Usato per entrambe le modalità.|
|MaxErrors|Integer|Numero di errori che possono verificarsi prima dell'arresto del flusso di dati. Il valore predefinito è **0**, ovvero nessun limite per il numero di errori.<br> Se è selezionato **Reindirizza flusso** nella pagina **Gestione degli errori**. Prima del raggiungimento del limite per il numero di errori, vengono restituiti tutti gli errori nell'output degli errori. Per altre informazioni, vedere [Gestione degli errori](#error-handling).|Usato solo in modalità di caricamento rapido.|
|NoLogging|Boolean|Indica se la registrazione del database è disabilitata. Il valore predefinito è **False**, ovvero la registrazione è abilitata.|Usato per entrambe le modalità.|
|Parallelo|Boolean|Indica se il caricamento parallelo è consentito. **True** indica che è consentita l'esecuzione di altre sessioni di caricamento sulla stessa tabella di destinazione.<br> Per altre informazioni, vedere [Parallelismo](#parallelism).|Usato solo in modalità di caricamento rapido.|
|TableName|string|Nome della tabella con i dati in uso.|Usato per entrambe le modalità.|
|TableSubName|string|Nome secondario o partizione secondaria. Questo valore è facoltativo.<br> **Nota**: Questa proprietà può essere impostata solo in **Editor avanzato**.|Usato solo in modalità di caricamento rapido.|
|TransactionSize|Integer|Numero di inserimenti che possono essere eseguiti in una singola transazione. Il valore predefinito è **BatchSize**.|Usato solo in modalità batch.|
|TransferBufferSize|Integer|Dimensioni del buffer di trasferimento. Il valore predefinito è 64 KB.|Usato solo in modalità di caricamento rapido.|

## <a name="configuring-the-oracle-destination"></a>Configurazione della destinazione Oracle

È possibile configurare la destinazione Oracle a livello di codice o tramite Progettazione SSIS.

Editor destinazione Oracle è illustrato nell'immagine seguente. Contiene la pagina Gestione connessione, la pagina Mapping e la pagina Output degli errori.

Per altre informazioni, vedere una delle sezioni seguenti:

- [Editor destinazione Oracle (pagina Gestione connessione)](#oracle-destination-editor-connection-manager-page)
- [Editor destinazione Oracle (pagina Mapping)](#oracle-destination-editor-mappings-page)
- [Editor destinazione Oracle (pagina Output degli errori)](#oracle-destination-editor-error-output-page)

![Destinazione Oracle](media/oracle-destination.png)

La finestra di dialogo **Editor avanzato** contiene le proprietà che è possibile impostare a livello di codice.
Per aprire la finestra di dialogo **Editor avanzato** :

- Nella schermata **Flusso di dati** del progetto di Integration Services fare clic con il pulsante destro del mouse sulla destinazione Oracle e scegliere **Visualizza editor avanzato**.

Per altre informazioni sulle proprietà che è possibile impostare nella finestra di dialogo Editor avanzato, vedere [Proprietà personalizzate della destinazione Oracle](#oracle-destination-custom-properties).

## <a name="oracle-destination-editor-connection-manager-page"></a>Editor destinazione Oracle (pagina Gestione connessione)

Usare la pagina **Gestione connessione** della finestra di dialogo **Editor destinazione Oracle** per selezionare la gestione connessione Oracle per la destinazione. Tramite questa pagina è inoltre possibile selezionare una tabella o una vista del database.

**Per aprire la pagina Gestione connessione di Editor destinazione Oracle**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con la destinazione Oracle.

- Nella scheda Flusso di dati fare doppio clic sulla destinazione Oracle.

- In Editor destinazione Oracle fare clic su Gestione connessione.

### <a name="options"></a>Opzioni

**Connection manager**

Consente di selezionare una gestione connessione esistente nell'elenco o di creare una nuova gestione connessione Oracle facendo clic su **Nuovo**.

**Nuovo**

Fare clic su **Nuovo**. Verrà visualizzata la finestra di dialogo **Configura gestione connessione Oracle**, in cui è possibile creare una nuova gestione connessione.

**Modalità di accesso ai dati**

Consente di selezionare il metodo per la selezione dei dati dall'origine. Le opzioni disponibili vengono visualizzate nella tabella seguente.

|Opzione|Descrizione|
|:-|:-|
|Nome tabella|Configurare la destinazione Oracle per il funzionamento in modalità batch. Opzioni:<br><br> **Nome tabella o vista**: selezionare una tabella o vista disponibile del database dall'elenco.<br><br> **Dimensioni transazione**: immettere il numero di inserimenti che possono essere eseguiti in una singola transazione. Il valore predefinito è **BatchSize**.<br><br> **Dimensioni batch**: tipo di dimensioni (numero di righe caricate) del batch per il caricamento bulk.
|Nome tabella - Caricamento rapido|Configurare la destinazione Oracle per il funzionamento in modalità di caricamento rapido (percorso diretto). <br><br>Sono disponibili le opzioni seguenti:<br><br> **Nome tabella o vista**: selezionare una tabella o vista disponibile del database dall'elenco.<br><br> **Caricamento parallelo**: indica se il caricamento parallelo è abilitato. Per altre informazioni, vedere [Parallelismo](#parallelism).<br><br> **Nessuna registrazione**: questa casella di controllo consente di disabilitare la registrazione del database. Questa registrazione è un database Oracle usato a scopo di ripristino, non correlato alla traccia.<br><br> **Numero massimo di errori**: numero massimo di errori che possono verificarsi prima dell'arresto del flusso di dati. Il valore predefinito è 0, ovvero nessun limite per il numero di errori.<br><br> Tutti gli errori che si verificano sono restituiti nell'output degli errori.<br><br> **Dimensioni buffer di trasferimento (KB)** : immettere le dimensioni del buffer di trasferimento. Il valore predefinito è 64 KB.|

**Visualizza dati esistenti**

Fare clic su **Visualizza dati esistenti** per visualizzare fino a 200 dati per la tabella selezionata.

## <a name="oracle-destination-editor-mappings-page"></a>Editor destinazione Oracle (pagina Mapping)

Usare la pagina **Mapping** della finestra di dialogo **Editor destinazione Oracle** per eseguire il mapping tra colonne di input e colonne di destinazione.

**Per aprire la pagina Mapping di Editor destinazione Oracle**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con la destinazione Oracle.

- Nella scheda Flusso di dati fare doppio clic sulla destinazione Oracle.

- In Editor destinazione Oracle fare clic su Mapping.

### <a name="options"></a>Opzioni

**Colonne di input disponibili**

Elenco delle colonne di input disponibili. Trascinare un colonna di input in una colonna di destinazione disponibile per eseguire il mapping tra le colonne.

**Colonne di destinazione disponibili**

Elenco delle colonne di destinazione disponibili. Trascinare un colonna di destinazione in una colonna di input disponibile per eseguire il mapping tra le colonne.

**Colonna di input**

Consente di visualizzare le colonne di input selezionate dall'utente. È possibile rimuovere i mapping selezionando **< ignora >** per escludere colonne dall'output.

**Colonna di destinazione**

Consente di visualizzare tutte le colonne di destinazione disponibili, con o senza mapping eseguito.

> [!NOTE]
>
>Le colonne con tipi di dati non supportati vengono eliminate dal mapping con un avviso.

## <a name="oracle-destination-editor-error-output-page"></a>Editor destinazione Oracle (pagina Output degli errori)

Usare la pagina Output degli errori della finestra di dialogo Editor destinazione Oracle per selezionare le opzioni di gestione degli errori.

**Per aprire la pagina Output degli errori di Editor destinazione Oracle**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con la destinazione Oracle.

- Nella scheda Flusso di dati fare doppio clic sulla destinazione Oracle.

- In Editor destinazione Oracle fare clic su Output degli errori.

### <a name="options"></a>Opzioni

**Comportamento in caso di errore**

Consente di selezionare il modo in cui l'origine Oracle deve gestire gli errori in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.
**Sezione correlata**: [Gestione degli errori nei dati](./error-handling-in-data.md)

**Troncamento**

Consente di selezionare il modo in cui l'origine Oracle deve gestire il troncamento in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.

## <a name="next-steps"></a>Passaggi successivi

- Configurare [Gestione connessione Oracle](oracle-connection-manager.md).
- Configurare l'[origine Oracle](oracle-source.md).
- Configurare la [destinazione Oracle](oracle-destination.md).
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA5u35j).
