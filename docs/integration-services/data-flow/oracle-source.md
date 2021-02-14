---
description: Origine Oracle
title: Origine Oracle
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6d26daafb62777091cb962332d47d8c00620e4d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343199"
---
# <a name="oracle-source"></a>Origine Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

L'origine Oracle consente di estrarre dati da Oracle Database con le modalità seguenti:

- Vista o tabella.

- Risultato di un'istruzione SQL.

Per la connessione all'origine Oracle viene usata una gestione connessione Oracle. Per altre informazioni, vedere [Gestione connessione Oracle](oracle-connection-manager.md).

## <a name="error-output"></a>Output degli errori

L'output degli errori include le colonne seguenti:  

- **Error Code** (Codice errore): numero che rappresenta il tipo di errore dell'errore corrente. Il codice di errore può essere generato da:
    - Server Oracle. Vedere la descrizione dettagliata dell'errore nella documentazione di Oracle Database.
    - Runtime SSIS. Per un elenco dei codici di errore SSIS, vedere la Guida di riferimento ai messaggi e ai codici di errore SSIS.
- **Error Column**(Colonna errore): numero della colonna di origine che causa gli errori di conversione.

- **Error Data Columns** (Colonne dati errore): dati che causano l'errore.

L'origine Oracle restituisce gli errori che si sono verificati durante il processo di caricamento e di estrazione nell'output degli errori. Per altre informazioni, vedere [Editor origine Oracle (pagina Output degli errori)](#oracle-source-editor-error-output-page).

## <a name="troubleshooting-the-oracle-source"></a>Risoluzione dei problemi relativi all'origine Oracle

È possibile registrare le chiamate ODBC eseguite dall'origine Oracle alle origini dati Oracle per risolvere i problemi relativi all'esportazione dei dati. Per registrare le chiamate ODBC eseguite dall'origine Oracle alle origini dati Oracle, abilitare la traccia di Gestione driver ODBC. Per altre informazioni, vedere la documentazione di Microsoft su *come generare una traccia ODBC con l'amministratore dell'origine dati ODBC*.

## <a name="oracle-source-custom-properties"></a>Proprietà personalizzate dell'origine Oracle

Di seguito sono elencate le proprietà personalizzate dell'origine Oracle. Tutte le proprietà sono di lettura/scrittura.

|Nome proprietà|Tipo di dati|Descrizione|
|:-|:-|:-|
|AccessMode|Integer (enumerazione)|Modalità utilizzata per accedere al database. I valori possibili sono **Table Name** e **SQL Command**. Il valore predefinito è **Table Name**.|
|BatchSize|Integer|Dimensioni del batch per il caricamento bulk. Corrisponde al numero di record estratti come matrice. <br>Questa proprietà è impostata solo da **Editor avanzato**|
|DefaultCodePage|Integer|Tabella codici da usare quando l'origine dati non dispone di informazioni sulla tabella codici. <br>Questa proprietà è impostata solo da **Editor avanzato**.|
|PreFetchCount|Integer|Numero di righe pre-recuperate. <br>Questa proprietà è impostata solo da **Editor avanzato**.|
|SqlCommand|string|Comando SQL da eseguire quando la proprietà AccessMode è impostata su SQL Command.|
|TableName|string|Nome della tabella con i dati da usare quando la proprietà AccessMode è impostata su Table Name.|

## <a name="configuring-the-oracle-source"></a>Configurazione dell'origine Oracle

È possibile configurare l'origine Oracle a livello di codice o tramite Progettazione SSIS.

Editor origine Oracle è illustrato nell'immagine seguente. Contiene la pagina Gestione connessione, la pagina Colonne e la pagina Output degli errori.

Per altre informazioni, vedere una delle sezioni seguenti:

- [Editor origine Oracle (pagina Gestione connessione)](#oracle-source-editor-connection-manager-page)
- [Editor origine Oracle (pagina Colonne)](#oracle-source-editor-columns-page)
- [Editor origine Oracle (pagina Output degli errori)](#oracle-source-editor-error-output-page)

![Origine Oracle](media/oracle-source.png)

La **finestra di dialogo Editor avanzato** contiene le proprietà che è possibile impostare a livello di codice.

Per aprire la finestra di dialogo **Editor avanzato** :

- Nella schermata **Flusso di dati** del progetto di Integration Services fare clic con il pulsante destro del mouse sull'origine Oracle e scegliere **Visualizza editor avanzato**.

Per altre informazioni sulle proprietà che è possibile impostare nella finestra di dialogo **Editor avanzato**, vedere [Proprietà personalizzate dell'origine Oracle](#oracle-source-custom-properties).

## <a name="oracle-source-editor-connection-manager-page"></a>Editor origine Oracle (pagina Gestione connessione)

La pagina **Gestione connessione** della finestra di dialogo **Editor origine Oracle** consente di selezionare Oracle Database come origine, tabella o vista del database.

**Per aprire la pagina Gestione connessione di Editor origine Oracle**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con l'origine Oracle.

- Nella scheda Flusso di dati fare doppio clic sull'origine Oracle.
### <a name="options"></a>Opzioni

**Connection manager**

Consente di selezionare una gestione connessione esistente nell'elenco o di creare una nuova gestione connessione Oracle facendo clic su **Nuovo**.

**Nuovo**

Fare clic su **Nuovo**. Verrà visualizzata la finestra di dialogo **Configura gestione connessione Oracle**, in cui è possibile creare una nuova gestione connessione.

**Modalità di accesso ai dati**

Consente di selezionare il metodo per la selezione dei dati dall'origine. Le opzioni disponibili vengono visualizzate nella tabella seguente.

|Opzione|Descrizione|
|:-|:-|
|Tabella o vista|Consente di recuperare dati da una tabella o da una vista nell'origine dati Oracle. Quando questa opzione è selezionata, selezionare una tabella o una vista disponibile nell'elenco per **Nome tabella o vista**.|
|Comando SQL|Consente di recuperare dati dall'origine dati Oracle usando una query SQL. Quando questa opzione è selezionata, immettere una query in uno dei modi seguenti: <br>Immettere il testo della query SQL nel campo **Testo comando SQL** . <br>Fare clic su **Sfoglia** per caricare la query SQL da un file di testo. <br>Fare clic su **Analizza query** per verificare la sintassi del testo della query.|

**Anteprima**

Fare clic su **Anteprima** per visualizzare un massimo di 200 righe dei dati estratti dalla tabella o dalla vista selezionata.

## <a name="oracle-source-editor-columns-page"></a>Editor origine Oracle (pagina Colonne)

La pagina **Colonne** della finestra di dialogo **Editor origine Oracle** consente di eseguire il mapping tra una colonna di output e ogni colonna esterna (di origine).

**Per aprire la pagina Colonne di Editor origine Oracle**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con l'origine Oracle.

- Nella scheda Flusso di dati fare doppio clic sull'origine Oracle.

- In Editor origine Oracle fare clic su Colonne.

### <a name="options"></a>Opzioni

**Colonne esterne disponibili**

Elenco delle colonne esterne disponibili che è possibile selezionare per l'aggiunta all'elenco **Colonna esterna** nell'ordine in cui vengono selezionate. Non è possibile usare questa tabella per l'aggiunta o l'eliminazione di colonne.

Selezionare la casella di controllo **Seleziona tutto** per selezionare tutte le colonne.

**Colonne esterne**

Le colonne (di origine) esterne selezionate sono elencate in ordine.
Per modificare l'ordine, cancellare prima l'elenco "Available External Column" (Colonna esterna disponibile), quindi selezionare una o più colonne con un ordine diverso.

**Colonna di output**

Il nome della colonna (di origine) esterna selezionata è il nome di output predefinito. È possibile immettere qualsiasi nome univoco.
> [!NOTE]
>
>Se sono presenti colonne con tipi di dati non supportati, verrà visualizzato un avviso che indica che i tipi di dati non sono supportati e le colonne correlate verranno rimosse dal mapping delle colonne.

## <a name="oracle-source-editor-error-output-page"></a>Editor origine Oracle (pagina Output degli errori)

Usare la pagina **Output degli errori** della finestra di dialogo **Editor origine Oracle** per selezionare le opzioni di gestione degli errori.

**Per aprire la pagina Output degli errori di Editor origine Oracle**

- In SQL Server Data Tools aprire il pacchetto di SQL Server Integration Services (SSIS) con l'origine Oracle.

- Nella scheda Flusso di dati fare doppio clic sull'origine Oracle.

- In Editor origine Oracle fare clic su Output degli errori.

### <a name="options"></a>Opzioni

**Comportamento in caso di errore**

Consente di selezionare il modo in cui l'origine Oracle deve gestire gli errori in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.
**Sezione correlata**: [Gestione degli errori nei dati](./error-handling-in-data.md)

**Troncamento**

Consente di selezionare il modo in cui l'origine Oracle deve gestire il troncamento in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.

## <a name="next-steps"></a>Passaggi successivi

- Configurare la [destinazione Oracle](oracle-destination.md).
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA5u35j).
