---
title: Dati demo di NYC Taxi per le esercitazioni
description: Creare un database contenente dati di esempio dei taxi di New York. Questo set di dati viene usato nelle esercitazioni di R e Python per Machine Learning Services per SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/31/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0a62131735fb5612af44f67cdbb03359563019f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347339"
---
# <a name="nyc-taxi-demo-data-for-sql-server-python-and-r-tutorials"></a>Dati demo NYC Taxi per le esercitazioni su Python e R in SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo illustra come configurare un database di esempio costituito da dati pubblici della [New York City Taxi and Limousine Commission](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml). Questi dati vengono usati in varie esercitazioni su R e Python per l'analisi nel database in SQL Server. Per velocizzare l'esecuzione del codice di esempio, abbiamo creato un campione rappresentativo dell'1% dei dati. Nel sistema dell'utente il file di backup del database è leggermente superiore a 90 MB, fornendo 1,7 milioni di righe nella tabella dati primaria.

Per completare questo esercizio, è necessario avere [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md?view=sql-server-2017&preserve-view=true) o un altro strumento in grado di ripristinare un file di backup del database ed eseguire query T-SQL.

Le esercitazioni e le guide di avvio rapido che usano questo set di dati includono:

+ [Informazioni sull'analisi nel database mediante R in SQL Server](r-taxi-classification-introduction.md)
+ [Informazioni sull'analisi nel database mediante Python in SQL Server](python-taxi-classification-introduction.md)

## <a name="download-files"></a>Scaricare i file

Il database di esempio è un file BAK di SQL Server 2016 ospitato da Microsoft. È possibile ripristinarlo in SQL Server 2016 e versioni successive. Il download del file inizia subito dopo aver fatto clic sul collegamento. 

Le dimensioni del file sono di circa 90 MB.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
>[!NOTE]
>Per ripristinare il database di esempio in [cluster Big Data di SQL Server](../../big-data-cluster/big-data-cluster-overview.md), scaricare [NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) e seguire le istruzioni in [Ripristinare un database nell'istanza master di un cluster Big Data di SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
>[!NOTE]
>Per ripristinare il database di esempio in [Machine Learning Services in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview), seguire le istruzioni riportate in [Avvio rapido: Ripristinare un database in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/restore-sample-database-quickstart) usando il file con estensione bak del database demo di NYC Taxi: [https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak).
::: moniker-end

1. Fare clic su [NYCTaxi_Sample. bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) per scaricare il file di backup del database.

2. Copiare il file nella cartella C:\Programmi\Microsoft SQL Server\MSSQL-nome_istanza\MSSQL\Backup.

3. In Management Studio fare clic con il pulsante destro del mouse su **Database** e scegliere **Ripristina file e filegroup**.

4. Immettere *NYCTaxi_Sample* come nome del database.

5. Fare clic su **Da dispositivo** e quindi aprire la pagina di selezione file per selezionare il file di backup. Fare clic su **Aggiungi** per selezionare NYCTaxi_Sample.bak.

6. Selezionare la casella di controllo **Ripristina** e fare clic su **OK** per ripristinare il database.

## <a name="review-database-objects"></a>Esaminare gli oggetti di database
   
Verificare l'esistenza degli oggetti di database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si dovrebbero vedere il database, le tabelle, le funzioni e le stored procedure.
  
   ![rsql_devtut_BrowseTables](media/rsql-devtut-browsetables.png "rsql_devtut_BrowseTables")

### <a name="objects-in-nyctaxi_sample-database"></a>Oggetti nel database NYCTaxi_Sample

La tabella seguente riepiloga gli oggetti creati nel database demo NYC Taxi.

|**Nome oggetto**|**Tipo oggetto**|**Descrizione**|
|----------|------------------------|---------------|
|**NYCTaxi_Sample** | database | Crea un database e due tabelle:<br /><br />Tabella dbo.nyctaxi_sample: contiene il set di dati NYC Taxi principale. Alla tabella viene aggiunto un indice columnstore cluster per migliorare le prestazioni di archiviazione e query. Un campione dell'1% del set di dati NYC Taxi è inserito in questa tabella.<br /><br />Tabella dbo.nyc_taxi_models: viene usata per salvare il modello di analisi avanzata sottoposto a training.|
|**fnCalculateDistance** |funzione a valori scalari | Calcola la distanza diretta tra le posizioni di salita e discesa del passeggero. Questa funzione viene usata in [Creare caratteristiche di dati](r-taxi-classification-create-features.md), [Eseguire il training e il salvataggio di un modello](r-taxi-classification-train-model.md) e [Eseguire stime usando R incorporato in una stored procedure](r-taxi-classification-deploy-model.md).|
|**fnEngineerFeatures** |funzione con valori di tabella | Crea nuove caratteristiche di dati per il training del modello. Questa funzione viene usata in [Creare caratteristiche di dati](r-taxi-classification-create-features.md) e [Eseguire stime usando R incorporato in una stored procedure](r-taxi-classification-deploy-model.md).|


Le stored procedure vengono create usando script R e Python disponibili in varie esercitazioni. La tabella seguente riepiloga le stored procedure che è possibile aggiungere facoltativamente al database demo NYC Taxi quando si eseguono gli script delle varie lezioni.

|**Stored procedure**|**Lingua**|**Descrizione**|
|-------------------------|------------|---------------|
|**RxPlotHistogram** |R | Chiama la funzione rxHistogram di RevoScaleR per tracciare l'istogramma di una variabile e restituisce il tracciato come oggetto binario. Questa stored procedure viene usata in [Esplorare e visualizzare i dati](r-taxi-classification-explore-data.md).|
|**RPlotRHist** |R| Crea un elemento grafico tramite la funzione Hist e salva l'output come file PDF locale. Questa stored procedure viene usata in [Esplorare e visualizzare i dati](r-taxi-classification-explore-data.md).|
|**RxTrainLogitModel**  |R| Esegue il training di un modello di regressione logistica mediante la chiamata di un pacchetto R. Il modello consente di stimare il valore della colonna indicata. Viene eseguito un training usando il 70% dei dati selezionati casualmente. L'output della stored procedure corrisponde al modello con training, che viene salvato nella tabella nyc_taxi_models. Questa stored procedure viene usata in [Eseguire il training e il salvataggio di un modello](r-taxi-classification-train-model.md).|
|**RxPredictBatchOutput**  |R | Chiama il modello con training per creare stime tramite il modello. La stored procedure accetta una query come parametro di input e restituisce una colonna di valori numerici contenente i punteggi per le righe di input. Questa stored procedure viene usata in [Stimare i risultati potenziali](r-taxi-classification-deploy-model.md).|
|**RxPredictSingleRow**  |R| Chiama il modello con training per creare stime tramite il modello. Questa stored procedure accetta una nuova osservazione come input, con i singoli valori della funzionalità passati come parametri in linea, e restituisce un valore che stima il risultato in base alla nuova osservazione. Questa stored procedure viene usata in [Stimare i risultati potenziali](r-taxi-classification-deploy-model.md).|

## <a name="query-the-data"></a>Eseguire una query sui dati

Come passaggio di convalida, eseguire una query per confermare che i dati sono stati caricati.

1. In Esplora oggetti fare clic con il pulsante destro del mouse sul database **NYCTaxi_Sample** in Database e avviare una nuova query.

2. Eseguire alcune query semplici:

    ```sql
    SELECT TOP(10) * FROM dbo.nyctaxi_sample;
    SELECT COUNT(*) FROM dbo.nyctaxi_sample;
    ```
Il database contiene 1,7 milioni di righe.

3. All'interno del database è presente una tabella **nyctaxi_sample** che contiene il set di dati. La tabella è stata ottimizzata per i calcoli basati su set con l'aggiunta di un [indice columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md). Eseguire questa istruzione per generare un breve riepilogo per la tabella.

    ```sql
    SELECT DISTINCT [passenger_count]
        , ROUND (SUM ([fare_amount]),0) as TotalFares
        , ROUND (AVG ([fare_amount]),0) as AvgFares
    FROM [dbo].[nyctaxi_sample]
    GROUP BY [passenger_count]
    ORDER BY  AvgFares DESC
    ````
I risultati dovrebbero essere simili a quelli mostrati nello screenshot seguente.

  ![Informazioni di riepilogo della tabella](media/nyctaxidatatablesummary.png "Risultati query")

## <a name="next-steps"></a>Passaggi successivi

I dati di esempio NYC Taxi sono ora disponibili per l'apprendimento pratico.

+ [Informazioni sull'analisi nel database mediante R in SQL Server](r-taxi-classification-introduction.md)
+ [Informazioni sull'analisi nel database mediante Python in SQL Server](python-taxi-classification-introduction.md)