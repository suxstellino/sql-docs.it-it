---
description: Dati demo Iris per le esercitazioni di Python e R con Machine Learning in SQL
title: Set di dati demo Iris per le esercitazioni
titleSuffix: SQL machine learning
Description: Creare un database contenente il set di dati Iris e i modelli predittivi. Questo set di dati viene usato nelle esercitazioni di R e Python con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 25cc9c7c93786dcc0d88e8bcd152196c17bf91c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347309"
---
# <a name="iris-demo-data-for-python-and-r-tutorials-with-sql-machine-learning"></a>Dati demo Iris per le esercitazioni di Python e R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In questo esercizio si creerà un database in cui archiviare i dati del [set di dati Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) e i modelli basati su tali dati. I dati Iris sono inclusi nelle distribuzioni R e Python e vengono usati nelle esercitazioni di Machine Learning in SQL.

Per completare questo esercizio, è necessario avere [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) o un altro strumento in grado di eseguire query T-SQL.

Le esercitazioni e le guide di avvio rapido che usano questo set di dati includono:

+ [Avvio rapido: Creare un modello predittivo in Python e assegnare i punteggi](quickstart-python-train-score-model.md)

## <a name="create-the-database"></a>Creare il database

1. Avviare SQL Server Management Studio e aprire una nuova finestra **Query**.  

2. Creare un nuovo database per questo progetto e cambiare il contesto della finestra **Query** per usare il nuovo database.

    ```sql
    CREATE DATABASE irissql
    GO
    USE irissql
    GO
    ```

3. Aggiungere alcune tabelle vuote: una in cui archiviare i dati e l'altra in cui archiviare i modelli sottoposti a training. La tabella **iris_models** viene usata per l'archiviazione di modelli serializzati generati in altri esercizi.

    Il codice seguente crea la tabella per i dati di training.

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

4. Eseguire il codice seguente per creare la tabella da usare per archiviare il modello sottoposto a training. Per salvare i modelli Python (o R) in SQL Server, è necessario serializzarli e archiviarli in una colonna di tipo **varbinary (max)** .

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO

    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    Oltre al contenuto del modello, in genere è possibile aggiungere colonne per altri metadati utili, come il nome del modello, la data di training, l'algoritmo di origine e i parametri, i dati di origine e così via. Per semplicità, per il momento si userà solo il nome del modello.

## <a name="populate-the-table"></a>Popolare la tabella

È possibile ottenere dati Iris incorporati sia da R che da Python. È possibile usare Python o R per caricare i dati in un frame di dati e quindi inserirli in una tabella del database. Lo spostamento dei dati di training da una sessione esterna in una tabella è un processo in più passaggi:

+ Progettare una stored procedure che ottenga i dati desiderati.
+ Eseguire la stored procedure per ottenere effettivamente i dati.
+ Creare un'istruzione INSERT per specificare la posizione in cui salvare i dati recuperati.

1. Nei sistemi in cui è integrato Python creare la stored procedure seguente che usa codice Python per caricare i dati.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    Quando si esegue questo codice, dovrebbe essere visualizzato il messaggio "I comandi sono stati completati". Significa che la stored procedure è stata creata in base alle specifiche.

2. In alternativa, nei sistemi in cui è integrato R creare una stored procedure che usa R.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'R', 
    @script = N'
    library(RevoScaleR)
    data(iris)
    iris$SpeciesID <- c(unclass(iris$Species))
    iris_data <- iris
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

3. Per popolare effettivamente la tabella, eseguire la stored procedure e specificare la tabella in cui devono essere scritti i dati. Quando viene eseguita, la stored procedure esegue il codice Python o R, che carica il set di dati Iris incorporato e quindi inserisce i dati nella tabella **iris_data**.

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    Se non si ha familiarità con T-SQL, tenere presente che l'istruzione INSERT aggiunge semplicemente nuovi dati, non verifica la presenza di dati esistenti né elimina e ricompila la tabella. Per evitare di ottenere più copie degli stessi dati in una tabella, è possibile eseguire prima questa istruzione: `TRUNCATE TABLE iris_data`. L'istruzione T-SQL [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md) elimina i dati esistenti, ma mantiene intatta la struttura della tabella.

## <a name="query-the-data"></a>Eseguire una query sui dati

Come passaggio di convalida, eseguire una query per confermare che i dati sono stati caricati.

1. In Esplora oggetti fare clic con il pulsante destro del mouse sul database **irissql** in Database e avviare una nuova query.

2. Eseguire alcune query semplici:

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

## <a name="next-steps"></a>Passaggi successivi

Nella guida di avvio rapido successiva si creerà un modello di Machine Learning e lo si salverà in una tabella, quindi si userà il modello per generare risultati stimati.

+ [Avvio rapido: Creare un modello predittivo in Python e assegnare i punteggi](quickstart-python-train-score-model.md)