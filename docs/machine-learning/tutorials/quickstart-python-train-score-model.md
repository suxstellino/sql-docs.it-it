---
title: 'Guida introduttiva: Eseguire il training di un modello in Python'
titleSuffix: SQL machine learning
description: Questo argomento di avvio rapido illustra come creare ed eseguire il training di un modello predittivo con Python. Il modello verrà salvato in una tabella nel database e quindi verrà usato per stimare i valori dei nuovi dati con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 85cc5fafd2efb3fba605cc80da28946bc5215941
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273002"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-python-with-sql-machine-learning"></a>Avvio rapido: Creare un modello predittivo in Python e assegnare i punteggi con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Questo argomento di avvio rapido illustra come creare ed eseguire il training di un modello predittivo con Python. Il modello verrà salvato in una tabella nell'istanza di SQL Server in uso, quindi si userà il modello per stimare i valori dei nuovi dati tramite [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md), [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) o [cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).

Si creeranno due stored procedure che verranno eseguite in SQL. La prima usa il classico set di dati Iris e genera un modello Naïve Bayes per stimare una specie di iris in base alle caratteristiche del fiore. La seconda stored procedure, per l'assegnazione dei punteggi, chiama il modello generato nella prima stored procedure per restituire un set di stime basate sui nuovi dati. Inserendo il codice Python in una stored procedure SQL, le operazioni sono contenute in SQL, sono riutilizzabili e possono essere chiamate da altre stored procedure e applicazioni client.

Completando questo argomento di avvio rapido si apprenderà:

> [!div class="checklist"]
> - Come incorporare codice Python in una stored procedure
> - Come passare input al codice tramite input nella stored procedure
> - Come usare le stored procedure per rendere operativi i modelli

## <a name="prerequisites"></a>Prerequisiti

Per completare questo argomento di avvio rapido è necessario soddisfare i prerequisiti seguenti.

- Un database SQL in una di queste piattaforme:
  - [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). Per eseguire l'installazione, vedere la [guida all'installazione in Windows](../install/sql-machine-learning-services-windows-install.md) o la [guida all'installazione in Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Cluster Big Data di SQL Server. Vedere [Abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services per Istanza gestita di SQL di Azure. Per informazioni, vedere [Panoramica di Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Uno strumento per l'esecuzione di query SQL che contengono script Python. In questo argomento di avvio rapido viene usato [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

- In questo esercizio vengono usati i dati di esempio Iris. Seguire le istruzioni nei [dati demo del set di dati Iris](demo-data-iris-in-sql.md) per creare il database di esempio **irissql**.

## <a name="create-a-stored-procedure-that-generates-models"></a>Creare una stored procedure che genera modelli

In questo passaggio si creerà una stored procedure che genera un modello per la stima dei risultati.

1. Aprire Azure Data Studio, connettersi all'istanza di SQL e aprire una nuova finestra di query.

1. Stabilire la connessione al database irissql.

    ```sql
    USE irissql
    GO
    ```

1. Copiare il codice seguente per creare una nuova stored procedure.

   Quando viene eseguita, la stored procedure chiama [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per avviare una sessione di Python. 

   Gli input necessari per il codice Python vengono passati come parametri di input nella stored procedure. L'output sarà un modello con training basato sulla libreria Python **scikit-learn** per l'algoritmo di Machine Learning.

   Questo codice usa [**pickle**](https://docs.python.org/2/library/pickle.html) per serializzare il modello. Il training del modello verrà eseguito usando i dati delle colonne da 0 a 4 della tabella **iris_data**. 

   I parametri nella seconda parte della stored procedure articolano gli input dei dati e gli output del modello. Per quanto possibile, il codice Python in esecuzione in una stored procedure dovrebbe disporre di input e output chiaramente definiti che eseguono il mapping a input e output della stored procedure passati in fase di esecuzione.

    ```sql
    CREATE PROCEDURE generate_iris_model (@trained_model VARBINARY(max) OUTPUT)
    AS
    BEGIN
        EXECUTE sp_execute_external_script @language = N'Python'
            , @script = N'
    import pickle
    from sklearn.naive_bayes import GaussianNB
    GNB = GaussianNB()
    trained_model = pickle.dumps(GNB.fit(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]], iris_data[["SpeciesId"]].values.ravel()))
    '
            , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
            , @input_data_1_name = N'iris_data'
            , @params = N'@trained_model varbinary(max) OUTPUT'
            , @trained_model = @trained_model OUTPUT;
    END;
    GO
    ```

1. Verificare che la stored procedure esista. 

   Se lo script T-SQL del passaggio precedente è stato eseguito senza errori, una nuova stored procedure denominata **generate_iris_model** viene creata e aggiunta al database **irissql**. In Azure Data Studio le stored procedure sono disponibili in **Esplora oggetti**, **Programmabilità**.

## <a name="execute-the-procedure-to-create-and-train-models"></a>Eseguire la stored procedure per creare ed eseguire il training di modelli

In questo passaggio viene eseguita la stored procedura per eseguire il codice incorporato, creando un modello sottoposto a training e serializzato come output. 

I modelli archiviati per il riutilizzo nel database vengono serializzati come flusso di byte e archiviati in una colonna VARBINARY(MAX) in una tabella di database. Dopo la creazione, il training, la serializzazione e il salvataggio in un database, il modello può essere chiamato da altre stored procedure o dalla funzione [PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md) nei carichi di lavoro di assegnazione dei punteggi.

1. Eseguire lo script seguente per eseguire la stored procedure. L'istruzione specifica per l'esecuzione di una stored procedure è `EXECUTE` nella quarta riga.

   Questo particolare script elimina un modello esistente con lo stesso nome ("Naive Bayes") per creare spazio per quelli nuovi creati ripetendo la stessa stored procedure. Se non si elimina il modello, si verifica un errore che indica che l'oggetto esiste già. Il modello viene archiviato in una tabella denominata **iris_models**, di cui è stato effettuato il provisioning al momento della creazione del database **irissql**.

    ```sql
    DECLARE @model varbinary(max);
    DECLARE @new_model_name varchar(50)
    SET @new_model_name = 'Naive Bayes'
    EXECUTE generate_iris_model @model OUTPUT;
    DELETE iris_models WHERE model_name = @new_model_name;
    INSERT INTO iris_models (model_name, model) values(@new_model_name, @model);
    GO
    ```

1. Verificare che il modello sia stato inserito.

    ```sql
    SELECT * FROM dbo.iris_models
    ```

    **Risultati**

    | model_name  | model |
    |---|-----------------|
    | Naive Bayes | 0x800363736B6C6561726E2E6E616976655F62617965730A... | 

## <a name="create-and-execute-a-stored-procedure-for-generating-predictions"></a>Creare ed eseguire una stored procedure per generare le stime

Ora che un modello è stato creato, sottoposto a training e salvato, procedere con il passaggio successivo, ossia la creazione di una stored procedure che genera stime. A questo scopo occorre chiamare `sp_execute_external_script` per eseguire uno script Python che carica il modello serializzato e gli passa nuovi input di dati a cui assegnare i punteggi.

1. Eseguire il codice seguente per creare la stored procedure che esegue l'assegnazione dei punteggi. In fase di esecuzione, questa stored procedure caricherà un modello binario, userà le colonne `[1,2,3,4]` come input e specificherà le colonne `[0,5,6]` come output.

   ```sql
   CREATE PROCEDURE predict_species (@model VARCHAR(100))
   AS
   BEGIN
       DECLARE @nb_model VARBINARY(max) = (
               SELECT model
               FROM iris_models
               WHERE model_name = @model
               );
   
       EXECUTE sp_execute_external_script @language = N'Python'
           , @script = N'
   import pickle
   irismodel = pickle.loads(nb_model)
   species_pred = irismodel.predict(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]])
   iris_data["PredictedSpecies"] = species_pred
   OutputDataSet = iris_data[["id","SpeciesId","PredictedSpecies"]] 
   print(OutputDataSet)
   '
           , @input_data_1 = N'select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
           , @input_data_1_name = N'iris_data'
           , @params = N'@nb_model varbinary(max)'
           , @nb_model = @nb_model
       WITH RESULT SETS((
                   "id" INT
                 , "SpeciesId" INT
                 , "SpeciesId.Predicted" INT
                   ));
   END;
   GO
   ```

2. Eseguire la stored procedure, specificando il nome di modello "Naive Bayes" in modo che possa individuare il modello da usare.

   ```sql
   EXECUTE predict_species 'Naive Bayes';
   GO
   ```

   Quando viene eseguita, la stored procedure restituisce un data.frame Python. Questa riga di T-SQL specifica lo schema dei risultati restituiti: `WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));`. È possibile inserire i risultati in una nuova tabella o restituirli a un'applicazione.

   ![Set di risultati dall'esecuzione della stored procedure](media/train-score-using-python-NB-model-results.png)

   I risultati sono 150 stime sulle specie usando caratteristiche floreali come input. Per la maggior parte delle osservazioni, le specie stimate corrispondono alle specie effettive.

   Questo esempio è stato reso semplice usando il set di dati Iris di Python sia per il training che per l'assegnazione dei punteggi. Un approccio più comune consiste nell'eseguire una query SQL per ottenere i nuovi dati e quindi passarli in Python come `InputDataSet`.

## <a name="conclusion"></a>Conclusioni

In questo esercizio si è appreso come creare stored procedure dedicate a diverse attività, in cui ognuna ha usato la stored procedure di sistema `sp_execute_external_script` per avviare un processo Python. Gli input del processo Python vengono passati a `sp_execute_external` come parametri. Sia lo script Python che le variabili di dati in un database vengono passati come input.

In genere è consigliabile pianificare l'uso di Azure Data Studio solo con codice Python pulito o codice Python semplice che restituisce output basato su riga. Come strumento, Azure Data Studio supporta linguaggi di query come T-SQL e restituisce set di righe bidimensionali. Se il codice genera un output visivo come un grafico a dispersione o un istogramma, sono necessari uno strumento o un'applicazione per utenti finali separati in grado di eseguire il rendering dell'immagine all'esterno della stored procedure.

Per alcuni sviluppatori Python abituati a scrivere script onnicomprensivi che gestiscono una serie di operazioni, l'organizzazione delle attività in procedure separate potrebbe sembrare superflua. Il training e l'assegnazione dei punteggi hanno però casi d'uso diversi. Separando queste attività, è possibile inserirle in pianificazioni diverse e assegnare le autorizzazioni in base a ogni operazione.

Un vantaggio finale è che i processi possono essere modificati usando parametri. In questo esercizio, il codice Python che ha creato il modello di esempio denominato "Naive Bayes" è stato passato come input a una seconda stored procedure che chiama il modello in un processo di assegnazione dei punteggi. Questo esercizio usa solo un modello, ma si può immaginare come la parametrizzazione del modello in un'attività di assegnazione dei punteggi possa rendere lo script più utile.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle esercitazioni per Python con Machine Learning in SQL, vedere:

- [Esercitazioni di Python](python-tutorials.md)