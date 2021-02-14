---
title: Importare file flat in SQL | Microsoft Docs
description: Importazione guidata file flat è un modo semplice per copiare dati da un file CSV o TXT a una nuova tabella di database. In questo articolo viene illustrato come e quando usare la procedura guidata.
ms.custom: ''
ms.date: 09/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: data-movement
ms.topic: conceptual
f1_keywords:
- sql13.swb.importflatfile.f1
author: yualan
ms.author: alayu
ms.reviewer: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 444489bc74106ca43d645e26564543b4452ac8ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351349"
---
# <a name="import-flat-file-to-sql-wizard"></a>Procedura guidata per l'importazione di file flat in SQL
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
> Per i contenuti correlati all'importazione ed esportazione guidata, vedere la pagina relativa all'[Importazione/Esportazione guidata SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

La procedura guidata per l'importazione di file flat consente di copiare facilmente i dati da un file flat (con estensione csv, txt) a una nuova tabella nel database.  L'Importazione guidata file flat supporta sia formati di file con valori delimitati da virgole che con larghezza fissa. In questa panoramica vengono descritti i motivi per l'uso di questa procedura guidata, come trovarla e un semplice esempio da seguire.

## <a name="why-would-i-use-this-wizard"></a>Perché usare questa procedura guidata?
Questa procedura guidata è stata creata per migliorare l'esperienza di importazione corrente sfruttando un framework intelligente noto come Program Synthesis using Examples ([PROSE](https://microsoft.github.io/prose/)). Per un utente senza una conoscenza specializzata dei domini, l'importazione dei dati può risultare un'attività complessa, soggetta a errori e impegnativa. Questa procedura guidata semplifica il processo di importazione: è sufficiente selezionare un file di input e un nome di tabella univoco e il framework PROSE gestisce il resto.

PROSE analizza i modelli di dati nel file di input per dedurre nomi di colonne, tipi, delimitatori e altro ancora. Questo framework apprende la struttura del file ed esegue l'intero lavoro al posto degli utenti.

Per comprendere meglio il miglioramento dell'esperienza utente della procedura guidata Importa file flat, vedere questo video:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Prerequisiti
Questa funzionalità è disponibile in SQL Server Management Studio (SSMS) v17.3 o successiva. Verificare che sia in uso la versione più recente. La versione più recente è disponibile [qui](../../ssms/download-sql-server-management-studio-ssms.md).
 
## <a name="getting-started"></a><a id="started"></a>Introduzione
Per accedere alla procedura guidata per l'importazione di file flat, procedere come segue:

1. Aprire **SQL Server Management Studio**.
2. Connettersi a un'istanza del motore di database di SQL Server o a localhost.
3. Espandere **Database**, fare clic con il pulsante destro del mouse su un database (vedere l'esempio seguente), scegliere **Attività** e fare clic su **Importa file flat** sopra a Importa dati.

![Menu della procedura guidata](media/import-flat-file-wizard/import-flat-file-menu.png)

Per altre informazioni sulle diverse funzioni della procedura guidata, vedere l'esercitazione seguente:

## <a name="tutorial"></a>Esercitazione
Ai fini di questa esercitazione, è possibile usare il proprio file flat. In caso contrario, servirsi del file CSV seguente da Excel, che può essere copiato liberamente. Se si usa questo CSV, denominarlo **esempio.csv** e salvarlo con l'estensione csv in un percorso semplice come ad esempio il desktop.

![Procedura guidata Excel](media/import-flat-file-wizard/import-flat-file-example.png)

Panoramica:
1. [Accedere alla procedura guidata](#step-1-access-wizard-and-intro-page)
2. [Specificare il file di input](#step-2-specify-input-file)
3. [Anteprima dei dati](#step-3-preview-data)
4. [Modificare le colonne](#step-4-modify-columns)
5. [Summary](#step-5-summary)
6. [Risultati](#step-6-results)

### <a name="step-1-access-wizard-and-intro-page"></a>Passaggio 1: Accedere alla procedura guidata e alla pagina introduttiva
Accedere alla procedura guidata come descritto [qui](#started).

La prima pagina della procedura guidata è la pagina di benvenuto. Se non si vuole più visualizzare questa pagina, è possibile fare clic su **Non visualizzare più questa pagina iniziale**.

![Pagina introduttiva della procedura guidata](media/import-flat-file-wizard/import-flat-file-intro.png)

### <a name="step-2-specify-input-file"></a>Passaggio 2: Specificare il file di input
Fare clic su Sfoglia per selezionare il file di input. Per impostazione predefinita, la procedura guidata cerca i file con estensione csv e txt. PROSE rileva se il formato del file è con valori delimitati da virgole o con larghezza fissa, indipendentemente dall'estensione.

Il nuovo nome di tabella deve essere univoco. In caso contrario, la procedura guidata non consente di proseguire.

![Finestra di specifica della procedura guidata](media/import-flat-file-wizard/import-flat-file-specify.png)

### <a name="step-3-preview-data"></a>Passaggio 3: Anteprima dei dati
La procedura guidata genera un'anteprima che consente di visualizzare le prime 50 righe. Se sono presenti problemi fare clic su Annulla, in caso contrario passare alla pagina successiva.

![Anteprima della procedura guidata](media/import-flat-file-wizard/import-flat-file-preview.png)

### <a name="step-4-modify-columns"></a>Passaggio 4: Modificare le colonne
La procedura guidata identifica quelli che presume essere i nomi di colonna, i tipi di dati, e così via, corretti. A questo punto è possibile modificare i campi se non sono corretti (ad esempio, il tipo di dati deve essere un valore float anziché un valore int).

Le colonne in cui vengono rilevati valori vuoti avranno l'opzione "Consenti valori Null" selezionata. Tuttavia, se si prevedono valori Null in una colonna e l'opzione "Consenti valori Null" non è selezionata, è possibile aggiornare la definizione di tabella per consentire i valori Null in una o in tutte le colonne.

Quando si è pronti, proseguire.

![Finestra di modifica della procedura guidata](media/import-flat-file-wizard/import-flat-file-modify.png)

### <a name="step-5-summary"></a>Passaggio 5: Summary
Si tratta semplicemente di una pagina di riepilogo che visualizza la configurazione corrente. Se sono presenti problemi, è possibile tornare alle sezioni precedenti. In caso contrario, fare clic su Fine per tentare il processo di importazione.

![Riepilogo della procedura guidata](media/import-flat-file-wizard/import-flat-file-summary.png)

### <a name="step-6-results"></a>Passaggio 6: Risultati
Questa pagina indica se l'importazione è riuscita. Se viene visualizzato un segno di spunta verde, l'importazione ha avuto esito positivo, in caso contrario potrebbe essere necessario rivedere la configurazione o il file di input per individuare eventuali errori.

![Risultati della procedura guidata](media/import-flat-file-wizard/import-flat-file-results.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi
Importazione guidata file flat rileva i tipi di dati in base alle prime 200 righe.  Negli scenari in cui i dati disponibili più avanti nel file flat non sono conformi ai tipi di dati rilevati, si verificherà un errore durante l'importazione. Il messaggio di errore sarà analogo al seguente:
```
Error inserting data into table. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type nvarchar of the specified target column. (System.Data)
String or binary data would be truncated. (System.Data)
```
Tattiche per risolvere questo errore:
- È possibile che l'espansione di una o più dimensioni di tipi di dati nel passaggio [Modifica colonne](#step-4-modify-columns), ad esempio la lunghezza di una colonna nvarchar, compensi per le variazioni nei dati della parte rimanente del file flat.
- L'abilitazione della segnalazione degli errori nel passaggio [Modifica colonne](#step-4-modify-columns), in particolare per un numero più basso, consentirà di rivelare una o più righe nel file flat che contengono dati che non corrispondono ai tipi di dati selezionati. In un file flat in cui la seconda riga introduce un errore, ad esempio, l'esecuzione dell'importazione con la segnalazione degli errori con un intervallo pari a 1 fornisce un messaggio di errore specifico.  Se si esamina il file direttamente nel percorso è possibile ottenere altre modifiche mirate per i tipi di dati in base ai dati nelle righe identificate.

![Errore durante la restituzione dei risultati](media/import-flat-file-wizard/import-flat-file-error.png)

```
Error inserting data into table occured while inserting rows 1 - 2. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type float of the specified target column. (System.Data)
Failed to convert parameter value from a String to a Double. (System.Data)
```


## <a name="learn-more"></a>Ulteriori informazioni

Altre informazioni sulla procedura guidata.
 
- **Altre informazioni sull'importazione di altre origini**. Se si sta cercando di importare più file flat, vedere la pagina relativa all'[Importazione/Esportazione guidata SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).
- **Altre informazioni su come connettersi alle origini dei file flat**. Se sono necessarie altre informazioni sulla connessione alle origini dei file flat, vedere [Connettersi a un'origine dati file flat](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md).
- **Altre informazioni su PROSE**. Se si sta cercando una panoramica del framework intelligente usato da questa procedura guidata, vedere la pagina relativa all'[SDK PROSE](https://microsoft.github.io/prose/).