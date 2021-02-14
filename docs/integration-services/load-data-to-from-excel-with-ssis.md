---
title: Importare da Excel o esportare in Excel con SSIS | Microsoft Docs
description: Informazioni su come importare o esportare i dati di Excel con SQL Server Integration Services (SSIS), insieme a prerequisiti, problemi noti e limitazioni.
ms.date: 06/29/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e497e0f2781aedc2adccc4263cc3b37e756a86da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345705"
---
# <a name="import-data-from-excel-or-export-data-to-excel-with-sql-server-integration-services-ssis"></a>Importare i dati da Excel o esportarli in Excel con SQL Server Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Questo articolo descrive le informazioni di connessione che è necessario specificare e le impostazioni che è necessario configurare, per importare i dati da Excel o esportare dati in Excel con SQL Server Integration Services (SSIS).

Le sezioni seguenti contengono le informazioni necessarie per usare Excel correttamente con SSIS e per comprendere e risolvere i problemi più comuni:

1.  Gli [strumenti](#tools) disponibili.

2.  I [file](#files-you-need) necessari.

3.  Le informazioni di connessione che è necessario specificare e le impostazioni che è necessario configurare, quando si caricano dati da o in Excel con SSIS.
    -   [Specificare Excel](#specify-excel) come origine dati.
    -   Specificare il [percorso e il nome del file di Excel](#excel-file).
    -   Selezionare la [versione di Excel](#excel-version).
    -   Specificare se la [prima riga contiene nomi di colonna](#first-row).
    -   Specificare il [foglio di lavoro o un intervallo contenente i dati](#sheets-ranges).

4.  Problemi noti e limitazioni.
    -   Problemi relativi ai [tipi di dati](#issues-types).
    -   Problemi di [importazione](#issues-importing).
    -   Problemi di [esportazione](#issues-exporting).

## <a name="tools-you-can-use"></a><a name="tools"></a> Strumenti disponibili

È possibile importare dati da Excel o esportare dati in Excel con SSIS usando uno degli strumenti seguenti:

-   **SQL Server Integration Services (SSIS)** . Creare un pacchetto SSIS che usa l'origine Excel o la destinazione Excel con Gestione connessione Excel. (Questo articolo non descrive come creare pacchetti SSIS.)

-   L'**Importazione/Esportazione guidata SQL Server** basata su SSIS. Per altre informazioni, vedere [Importare ed esportare dati con l'Importazione/Esportazione guidata SQL Server](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) e [Connettersi a un'origine dati Excel (Importazione/Esportazione guidata SQL Server)](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md).

## <a name="get-the-files-you-need-to-connect-to-excel"></a><a name="files-you-need"></a> Ottenere i file necessari per connettersi a Excel

Prima di poter importare dati da Excel o esportare dati in Excel con SSIS, può essere necessario scaricare i componenti di connettività per Excel se non sono già installati. I componenti di connettività per Excel non sono installati per impostazione predefinita.

Scaricare la versione più recente dei componenti di connettività per Excel qui: [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920). La versione più recente dei componenti può aprire file creati da versioni precedenti di Excel.

### <a name="notes-about-the-download-and-installation"></a>Note sul download e l'installazione

-   Assicurarsi di eseguire il download di Access Database Engine 2016 *Redistributable* e non di Microsoft Access 2016 *Runtime*.

-   Se nel computer è già installata una versione a 32 bit di Office, è necessario installare la versione a 32 bit dei componenti. Verificare inoltre che il pacchetto SSIS venga eseguito in modalità a 32 bit oppure eseguire la versione a 32 bit dell'Importazione/Esportazione guidata.

-   Se si ha una sottoscrizione di Microsoft 365, è possibile che appaia un messaggio di errore quando si esegue il programma di installazione. L'errore indica che non è possibile installare il download affiancato ai componenti di Office A portata di clic. Per ignorare questo messaggio di errore, eseguire l'installazione in modalità non interattiva aprendo una finestra del prompt dei comandi ed eseguendo il file con estensione EXE scaricato con l'opzione `/quiet`. Ad esempio:

    `C:\Users\<user_name>\Downloads\AccessDatabaseEngine.exe /quiet`

    In caso di problemi durante l'installazione della versione 2016 Redistributable, installare la versione 2010 Redistributable da qui: [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/download/details.aspx?id=13255). Non esiste un componente ridistribuibile per Excel 2013.

## <a name="specify-excel-as-your-data-source"></a><a name="specify-excel"></a> Specificare Excel come origine dati

Il primo passaggio è indicare che si vuole eseguire la connessione a Excel.

### <a name="in-ssis"></a>In SSIS
In SSIS creare una gestione connessione Excel per connettersi al file di origine o di destinazione di Excel. Esistono diversi modi per creare la gestione connessione:

-   Fare clic con il pulsante destro del mouse nell'area **Gestioni connessioni** e selezionare **Nuova connessione**. Nella finestra di dialogo **Aggiungi gestione connessione SSIS** selezionare **EXCEL** e quindi **Aggiungi**.
 
-   Selezionare **Nuova connessione** dal menu **SSIS**. Nella finestra di dialogo **Aggiungi gestione connessione SSIS** selezionare **EXCEL** e quindi **Aggiungi**.

-   Creare la gestione connessione mentre si configura l'**origine Excel** o la **destinazione Excel** nella pagina **Gestione connessione** dell' **Editor origine Excel** o dell'**Editor destinazione Excel**.

### <a name="in-the-sql-server-import-and-export-wizard"></a>Nell'Importazione/Esportazione guidata SQL Server
Nella pagina **Scegliere un'origine dati** o **Scegliere una destinazione** della procedura guidata di importazione/esportazione selezionare **Microsoft Excel** nell'elenco **Origine dati**.

Se Excel non è incluso nell'elenco delle origini dati, verificare se è in esecuzione la procedura guidata a 32 bit. I componenti di connettività di Excel in genere sono file a 32 bit e non sono visibili nella procedura guidata a 64 bit.

## <a name="excel-file-and-file-path"></a><a name="excel-file"></a> Percorso e file di Excel

La prima parte di informazioni da specificare è costituita dal percorso e dal nome del file di Excel. Queste informazioni devono essere specificate nell'**Editor di gestione connessioni Excel** in un pacchetto SSIS o nella pagina **Scegliere un'origine dati** o **Scegliere una destinazione** dell'Importazione/Esportazione guidata.

Immettere il percorso e il nome del file nel formato seguente:

-   Per un file nel computer locale, **C:\\TestData.xlsx**.

-   Per un file in una condivisione di rete, **\\\\Sales\\Data\\TestData.xlsx**.

Oppure fare clic su **Sfoglia** per individuare il foglio di calcolo usando la finestra di dialogo **Apri**.  
  
> [!IMPORTANT]
> Non è possibile connettersi a un file di Excel protetto da password.

## <a name="excel-version"></a><a name="excel-version"></a> Versione di Excel

Nella seconda parte di informazioni si specifica la versione del file di Excel. Queste informazioni devono essere specificate nell'**Editor di gestione connessioni Excel** in un pacchetto SSIS o nella pagina **Scegliere un'origine dati** o **Scegliere una destinazione** dell'Importazione/Esportazione guidata.

Selezionare la versione di Microsoft Excel usata per creare il file o un'altra versione compatibile. Se ad esempio si verifica un problema durante l'installazione dei componenti di connettività 2016, è possibile installare i componenti della versione 2010 e selezionare **Microsoft Excel 2007-2010** da questo elenco.

Può non essere possibile selezionare le versioni più recenti di Excel presenti nell'elenco se sono installate solo le versioni precedenti dei componenti di connettività. L'elenco **Versione di Excel** include tutte le versioni di Excel supportate da SSIS. La presenza di elementi in questo elenco non indica che i componenti di connettività necessari siano installati. Ad esempio, **Microsoft Excel 2016** appare nell'elenco anche se non sono installati i componenti di connettività 2016.

## <a name="first-row-has-column-names"></a><a name="first-row"></a> Nomi di colonna nella prima riga

Se si importano i dati da Excel, il passaggio successivo è indicare se la prima riga di dati contiene nomi di colonna. Specificare queste informazioni nell'**Editor di gestione connessioni Excel** in un pacchetto SSIS o nella pagina **Scegliere un'origine dati** dell'Importazione/Esportazione guidata.

-   Se si disabilita questa opzione perché i dati di origine non contengono nomi di colonna, la procedura guidata usa F1, F2 e così via come intestazioni di colonna.
-   Se i dati contengono nomi di colonna e si disabilita questa opzione, la procedura guidata importa i nomi di colonna come prima riga di dati.
-   Se i dati non contengono nomi di colonna e si abilita questa opzione, la procedura guidata usa la prima riga di dati di origine come nomi di colonna. In questo caso la prima riga di dati di origine non è più inclusa nei dati stessi.

Se si esportano dati da Excel e si abilita questa opzione, la prima riga di dati esportati include i nomi di colonna.

## <a name="worksheets-and-ranges"></a><a name="sheets-ranges"></a> Fogli di lavoro e intervalli

Esistono tre tipi di oggetti di Excel che si possono usare come origine o destinazione per i dati: un foglio di lavoro, un intervallo denominato o un intervallo di celle senza nome specificato con il relativo indirizzo.

-   **Foglio di lavoro.** Per specificare un foglio di lavoro, aggiungere il carattere `$` alla fine del nome del foglio e aggiungere delimitatori all'inizio e alla fine della stringa, ad esempio **[Foglio1$]** . Oppure cercare un nome che termina con il carattere `$` nell'elenco di tabelle e viste esistenti.

-   **Intervallo denominato.** Per specificare un intervallo denominato, indicare il nome dell'intervallo, ad esempio **MioIntervalloDati**. Oppure cercare un nome che non termina con il carattere `$` nell'elenco di tabelle e viste esistenti.
    
-   **Intervallo senza nome.** Per specificare un intervallo di celle a cui non è stato assegnato un nome, aggiungere il carattere $ alla fine del nome del foglio, aggiungere la specifica dell'intervallo e aggiungere delimitatori all'inizio e alla fine della stringa, ad esempio **[Foglio1$A1:B4]** .

Per selezionare o specificare il tipo di oggetto di Excel da usare come origine o destinazione per i dati, eseguire una delle operazioni seguenti:

### <a name="in-ssis"></a>In SSIS

In SSIS, nella pagina **Gestione connessione** dell'**Editor origine Excel** o dell'**Editor destinazione Excel**, eseguire una delle operazioni seguenti:

-   Per usare un **foglio di lavoro** o un **intervallo denominato**, selezionare **Tabella o vista** come **modalità di accesso ai dati**. Quindi, selezionare il foglio di lavoro o l'intervallo denominato dall'elenco **Nome del foglio di Excel**.

-   Per usare un **intervallo senza nome** specificato con il relativo indirizzo, selezionare **Comando SQL** come **modalità di accesso ai dati**. Quindi, immettere nel campo **Testo comando SQL** una query simile all'esempio che segue:

    ```sql
    SELECT * FROM [Sheet1$A1:B5]
    ```

### <a name="in-the-sql-server-import-and-export-wizard"></a>Nell'Importazione/Esportazione guidata SQL Server
Nella procedura di importazione/esportazione guidata eseguire una delle operazioni seguenti:

-   Quando si **importano** dati da Excel effettuare una delle seguenti operazioni:

    -   Per usare un **foglio di lavoro** o un **intervallo denominato**, nella pagina **Impostazione copia tabella o query** selezionare **Copia i dati da una o più tabelle o viste**. Nella colonna **Origine** della pagina **Seleziona tabelle e viste di origine** selezionare i fogli di lavoro e gli intervalli denominati di origine.

    -   Per usare un **intervallo senza nome** specificato con il relativo indirizzo, nella pagina **Impostazione copia tabella o query** selezionare **Scrivi una query per specificare i dati da trasferire**. Specificare quindi una query simile a quella dell'esempio seguente nella pagina **Impostazione query di origine**:

        ```sql
        SELECT * FROM [Sheet1$A1:B5]
        ```

-   Quando si **esportano** dati in Excel effettuare una delle seguenti operazioni:

    -   Per usare un **foglio di lavoro** o un **intervallo denominato**, nella colonna **Destinazione** della pagina **Seleziona tabelle e viste di origine** selezionare i fogli di lavoro e gli intervalli denominati di destinazione.

    -   Per usare un **intervallo senza nome** specificato con il relativo indirizzo, nella colonna **Destinazione** della pagina **Seleziona tabelle e viste di origine** immettere l'intervallo nel formato seguente senza delimitatori: `Sheet1$A1:B5`. La procedura guidata aggiunge i delimitatori.

Dopo aver selezionato o immesso gli oggetti di Excel da importare o esportare, è anche possibile effettuare le operazioni seguenti nella pagina **Seleziona tabelle e viste di origine** della procedura guidata:

-   Rivedere i mapping a livello di colonne tra l'origine e la destinazione selezionando **Modifica mapping**.

-   Visualizzare in anteprima i dati di esempio per verificare che siano quelli previsti selezionando **Anteprima**.

## <a name="issues-with-data-types"></a><a name="issues-types"></a> Problemi relativi ai tipi di dati

### <a name="data-types"></a>Tipi di dati

Il driver per Excel riconosce solo un set limitato di tipi di dati. Tutte le colonne numeriche vengono interpretate come valori double (DT_R8) e tutte le colonne di tipo stringa (con tipo di dati diverso da memo) vengono interpretate come stringhe Unicode di 255 caratteri (DT_WSTR). SSIS esegue il mapping dei tipi di dati di Excel nel modo seguente:

-   Numero - Numero a virgola mobile e precisione doppia (DT_R8)

-   Valuta - Valuta (DT_CY)

-   Valore booleano - Valore booleano (DT_BOOL)

-   Data/ora - datetime (DT_DATE)

-   Stringa - Stringa Unicode di 255 caratteri (DT_WSTR)

-   Memo - Flusso di testo Unicode (DT_NTEXT)

### <a name="data-type-and-length-conversions"></a>Conversioni di tipo e lunghezza dei dati

SSIS non esegue la conversione implicita dei tipi di dati. Di conseguenza, può essere necessario usare trasformazioni Colonna derivata o Conversione dati per convertire i dati di Excel in modo esplicito prima di caricarli in una destinazione diversa da Excel o per convertire dati di un'origine diversa da Excel prima di caricarli in una destinazione Excel.

Di seguito sono riportati alcuni esempi delle conversioni che possono rendersi necessarie:  
  
-   Conversione tra colonne di Excel di tipo stringa Unicode e colonne di tipo stringa non Unicode con tabelle codici specifiche.
  
-   Conversione tra colonne di Excel di tipo stringa di 255 caratteri e colonne di tipo stringa di lunghezze diverse
  
-   Conversione tra colonne di Excel di tipo numerico a precisione doppia e colonne numeriche di altro tipo

> [!TIP]
> Se si usa l'Importazione/Esportazione guidata e i dati richiedono alcune di queste conversioni, la procedura guidata configura automaticamente le conversioni necessarie. Di conseguenza, anche se si usa un pacchetto SSIS, può essere utile creare il pacchetto iniziale usando l'Importazione/Esportazione guidata. Consentire alla procedura guidata di creare e configurare automaticamente gestioni connessioni, origini, trasformazioni e destinazioni.

## <a name="issues-with-importing"></a><a name="issues-importing"></a> Problemi di importazione

### <a name="empty-rows"></a>Righe vuote

Quando si specifica un foglio di lavoro o un intervallo denominato come origine, il driver legge il blocco di celle *contigue* che inizia con la prima cella non vuota nell'angolo superiore sinistro del foglio di lavoro o dell'intervallo. Di conseguenza, i dati non devono necessariamente iniziare nella riga 1, ma non possono essere presenti righe vuote nei dati di origine. Ad esempio, non è possibile avere una riga vuota tra le intestazioni di colonna e le righe di dati o un titolo seguito da righe vuote nella parte superiore del foglio di lavoro.

Se sono presenti righe vuote sopra i dati, non è possibile eseguire query sui dati nel foglio di lavoro. In Excel è necessario selezionare l'intervallo di dati e assegnare un nome all'intervallo, quindi eseguire una query nell'intervallo denominato anziché nel foglio di lavoro.

### <a name="missing-values"></a>Valori mancanti

Per determinare il tipo di dati di ogni colonna, il driver per Excel legge un determinato numero di righe (otto per impostazione predefinita) nell'origine specificata. Se una colonna contiene tipi di dati diversi, soprattutto se sono presenti sia dati numerici che di testo, il driver adotta il tipo di dati a cui corrisponde il maggior numero di elementi e restituisce valori Null per le celle che contengono dati di tipo diverso. In caso di parità, viene adottato il tipo numerico. La maggior parte delle opzioni di formattazione utilizzate nei fogli di lavoro di Excel non influisce sulla determinazione del tipo di dati.

È possibile modificare questo comportamento del driver per Excel specificando la Modalità di importazione per importare tutti i valori come testo. Per specificare la Modalità di importazione, aggiungere `IMEX=1` al valore di **Proprietà estese** nella stringa di connessione della gestione connessione Excel nella finestra Proprietà. 

### <a name="truncated-text"></a>Testo troncato

Se il driver determina che una colonna di Excel contiene dati di tipo text, seleziona il tipo di dati (string o memo), in base al più lungo valore campionato. Se il driver non individua valori contenenti più di 255 caratteri nelle righe campionate, gestirà la colonna come una colonna di stringhe di 255 caratteri, anziché come una colonna con tipo di dati memo. I valori contenenti più di 255 caratteri potrebbero essere pertanto troncati.

Per importare dati da una colonna di tipo memo senza troncamenti, sono disponibili due opzioni:

-   Verificare che la colonna memo in almeno una delle righe campionate contenga un valore di lunghezza superiore a 255 caratteri

-   Aumentare il numero di righe campionate dal driver per includere tale riga. Per aumentare il numero di righe campionate, è possibile incrementare il valore di **TypeGuessRows** nella seguente chiave del Registro di sistema:

| Versione dei componenti ridistribuibili | Chiave del Registro di sistema |
|---|---|
| Excel 2016 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\16.0\Access Connectivity Engine\Engines\Excel |
| Excel 2010 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\14.0\Access Connectivity Engine\Engines\Excel |
| | |

## <a name="issues-with-exporting"></a><a name="issues-exporting"></a> Problemi di esportazione

### <a name="create-a-new-destination-file"></a>Creare un nuovo file di destinazione

#### <a name="in-ssis"></a>In SSIS

Creare una gestione connessione Excel con il percorso e il nome del nuovo file di Excel che si vuole creare. Quindi, nell'**Editor destinazione Excel** selezionare **Nuovo** per **Nome del foglio di Excel** per creare il foglio di lavoro di destinazione. A questo punto SSIS crea il nuovo file di Excel con il foglio di lavoro specificato.

#### <a name="in-the-sql-server-import-and-export-wizard"></a>Nell'Importazione/Esportazione guidata SQL Server

Nella pagina **Scegliere una destinazione** selezionare **Sfoglia**. Nella finestra di dialogo **Apri** passare alla cartella in cui si vuole creare il nuovo file di Excel, specificare un nome per il nuovo file e quindi selezionare **Apri**.

### <a name="export-to-a-large-enough-range"></a>Esportare in un intervallo di dimensioni sufficienti

Quando si specifica un intervallo come destinazione si verifica un errore se l'intervallo ha meno *colonne* rispetto ai dati di origine. Tuttavia, se l'intervallo specificato ha meno *righe* rispetto ai dati di origine, la procedura guidata continua a scrivere le righe senza errori ed estende la definizione dell'intervallo in base al nuovo numero di righe.

### <a name="export-long-text-values"></a>Esportare valori di testo lungo

Per poter salvare correttamente stringhe di oltre 255 caratteri in una colonna Excel, il driver deve riconoscere il tipo di dati della colonna di destinazione come **memo** invece di **string**.

-   Se una tabella di destinazione esistente contiene già righe di dati, le prime righe campionate dal driver devono contenere almeno un'istanza di un valore composto da più di 255 caratteri nella colonna di tipo memo.

## <a name="related-content"></a>Contenuti correlati

Per altre informazioni sulle procedure e sui componenti descritti in questo articolo, vedere gli articoli seguenti:

### <a name="about-ssis"></a>Informazioni su SSIS
[Gestione connessione Excel](connection-manager/excel-connection-manager.md)  
[Origine Excel](data-flow/excel-source.md)  
[Destinazione Excel](data-flow/excel-destination.md)  
[Esecuzione di un ciclo su file e tabelle di Excel usando un contenitore Ciclo Foreach](control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)  
[Utilizzo di file di Excel con l'attività Script](extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)

### <a name="about-the-sql-server-import-and-export-wizard"></a>Informazioni sull'Importazione/Esportazione guidata SQL Server
[Connettersi a un'origine dati Excel](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)  
[Iniziare con questo semplice esempio dell'Importazione/Esportazione guidata](import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)

### <a name="other-articles"></a>Altri articoli
[Importare dati da Excel a SQL Server o al database SQL di Azure](../relational-databases/import-export/import-data-from-excel-to-sql.md)  
