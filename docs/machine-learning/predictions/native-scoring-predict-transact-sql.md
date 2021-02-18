---
title: Assegnazione dei punteggi nativa con PREDICT T-SQL
titleSuffix: SQL machine learning
description: Informazioni su come usare l'assegnazione dei punteggi nativa con la funzione PREDICT T-SQL per generare valori di stima per i nuovi input di dati near real-time.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest'
ms.openlocfilehash: 74fc66dd6ed235d7508f774e7397c9b4d55e80a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352541"
---
# <a name="native-scoring-using-the-predict-t-sql-function-with-sql-machine-learning"></a>Assegnazione dei punteggi nativa tramite la funzione PREDICT T-SQL con Machine Learning in SQL

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Informazioni su come usare l'assegnazione dei punteggi nativa con la [funzione PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md) per generare valori di stima per i nuovi input di dati near real-time. Per l'assegnazione dei punteggi nativa è necessario disporre di un modello già sottoposto a training.

La funzione `PREDICT` usa le funzionalità dell'estensione C++ native in [Machine Learning in SQL](../index.yml). Questa metodologia assicura la velocità di elaborazione più rapida possibile per i carichi di lavoro di previsione e stima e supporta i modelli in formato [ONNX (Open Neural Network Exchange)](https://onnx.ai/get-started.html) o i modelli sottoposti a training usando i pacchetti [RevoScaleR](../r/ref-r-revoscaler.md) e [revoscalepy](../python/ref-py-revoscalepy.md).

## <a name="how-native-scoring-works"></a>Funzionamento dell'assegnazione dei punteggi nativa

L'assegnazione dei punteggi nativa usa librerie in grado di leggere i modelli in ONNX o in un formato binario predefinito e genera punteggi per i nuovi input di dati forniti. Poiché il modello viene sottoposto a training, distribuito e archiviato, può essere usato per l'assegnazione dei punteggi senza dover chiamare l'interprete R o Python. Ciò significa che il sovraccarico delle interazioni di più processi viene ridotto e le prestazioni di stima risultano di conseguenza più veloci.

Per usare l'assegnazione dei punteggi nativa, chiamare la funzione `PREDICT` T-SQL e passare gli input richiesti seguenti:

+ Un modello compatibile basato su un modello e un algoritmo supportati.
+ Dati di input, in genere definiti come query T-SQL.

La funzione restituisce le stime per i dati di input, insieme alle eventuali colonne di dati di origine da passare.

## <a name="prerequisites"></a>Prerequisiti

`PREDICT` è disponibile in:

+ Tutte le edizioni di SQL Server 2017 e versioni successive in Windows e Linux
+ Istanza gestita di SQL di Azure
+ database SQL di Azure
+ SQL Edge di Azure
+ Azure Synapse Analytics

La funzione è abilitata per impostazione predefinita. Non è necessario installare R o Python, né abilitare funzionalità aggiuntive.

## <a name="supported-models"></a>Modelli supportati

I formati di modello supportati dalla funzione `PREDICT` dipendono dalla piattaforma SQL in cui si esegue l'assegnazione dei punteggi nativa. Vedere la tabella seguente per informazioni sui formati di modello supportati in base alla piattaforma.

| Piattaforma | Formato di modello ONNX | Formato di modello RevoScale |
|-|-|-|
| SQL Server | No | Sì |
| Istanza gestita di SQL di Azure | sì | sì |
| database SQL di Azure | No | Sì |
| SQL Edge di Azure | Sì | No |
| Azure Synapse Analytics | Sì | No |

::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="onnx-models"></a>Modelli ONNX

Il modello deve essere in un formato [ONNX (Open Neural Network Exchange)](https://onnx.ai/get-started.html).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current"
### <a name="revoscale-models"></a>Modelli RevoScale

Il training del modello deve essere eseguito in anticipo con uno degli algoritmi **rx** supportati elencati di seguito usando il pacchetto [RevoScaleR](../r/ref-r-revoscaler.md) o [revoscalepy](../python/ref-py-revoscalepy.md).

Serializzare il modello usando [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) per R e [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) per Python. Queste funzioni di serializzazione sono state ottimizzate per supportare l'assegnazione rapida dei punteggi.

<a name="bkmk_native_supported_algos"></a> 

#### <a name="supported-revoscale-algorithms"></a>Algoritmi RevoScale supportati

Gli algoritmi seguenti sono supportati in revoscalepy e RevoScaleR.

+ Algoritmi revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) 
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) 
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) 
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) 

+ Algoritmi RevoScaleR

  + [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

Se è necessario usare un algoritmo di MicrosoftML o microsoftml, usare l'[assegnazione dei punteggi in tempo reale con sp_rxPredict](../predictions/real-time-scoring.md).

I tipi di modelli non supportati includono i seguenti:

+ Modelli contenenti altre trasformazioni
+ Modelli che usano gli algoritmi `rxGlm` o `rxNaiveBayes` negli equivalenti RevoScaleR o revoscalepy
+ Modelli PMML
+ Modelli creati con altre librerie open source o di terze parti
::: moniker-end

## <a name="examples"></a>Esempi
::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="predict-with-an-onnx-model"></a>PREDICT con un modello ONNX

Questo esempio mostra come usare un modello ONNX archiviato nella tabella `dbo.models` per l'assegnazione dei punteggi nativa.

```sql
DECLARE @model VARBINARY(max) = (
        SELECT DATA
        FROM dbo.models
        WHERE id = 1
        );

WITH predict_input
AS (
    SELECT TOP (1000) [id]
        , CRIM
        , ZN
        , INDUS
        , CHAS
        , NOX
        , RM
        , AGE
        , DIS
        , RAD
        , TAX
        , PTRATIO
        , B
        , LSTAT
    FROM [dbo].[features]
    )
SELECT predict_input.id
    , p.variable1 AS MEDV
FROM PREDICT(MODEL = @model, DATA = predict_input, RUNTIME=ONNX) WITH (variable1 FLOAT) AS p;
```

> [!NOTE]
> Poiché le colonne e i valori restituiti da **PREDICT** possono variare in base al tipo di modello, è necessario definire lo schema dei dati restituiti usando una clausola **WITH**.
::: moniker-end

::: moniker range=">=sql-server-2017||=azuresqldb-mi-current||>=sql-server-linux-2017"
### <a name="predict-with-revoscale-model"></a>PREDICT con il modello RevoScale

In questo esempio viene creato un modello usando **RevoScaleR** in R e quindi viene chiamata la funzione di stima in tempo reale da T-SQL.

#### <a name="step-1-prepare-and-save-the-model"></a>Passaggio 1. Preparare e salvare il modello

Eseguire il codice seguente per creare il database di esempio e le tabelle richieste.

```sql
CREATE DATABASE NativeScoringTest;
GO
USE NativeScoringTest;
GO
DROP TABLE IF EXISTS iris_rx_data;
GO
CREATE TABLE iris_rx_data (
    "Sepal.Length" float not null, "Sepal.Width" float not null
  , "Petal.Length" float not null, "Petal.Width" float not null
  , "Species" varchar(100) null
);
GO
```

Usare l'istruzione seguente per popolare la tabella dati con i dati del set di dati **iris**.

```sql
INSERT INTO iris_rx_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width" , "Species")
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'iris_data <- iris;'
  , @input_data_1 = N''
  , @output_data_1_name = N'iris_data';
GO
```

Creare ora una tabella per archiviare i modelli.

```sql
DROP TABLE IF EXISTS ml_models;
GO
CREATE TABLE ml_models ( model_name nvarchar(100) not null primary key
  , model_version nvarchar(100) not null
  , native_model_object varbinary(max) not null);
GO
```

Il codice seguente crea un modello basato sul set di dati **iris** e lo salva nella tabella denominata **models**.

```sql
DECLARE @model varbinary(max);
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'
    iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
    iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, data = iris[iris.sub, ])
    model <- rxSerializeModel(iris.dtree, realtimeScoringOnly = TRUE)
    '
  , @params = N'@model varbinary(max) OUTPUT'
  , @model = @model OUTPUT
  INSERT [dbo].[ml_models]([model_name], [model_version], [native_model_object])
  VALUES('iris.dtree','v1', @model) ;
```

> [!NOTE]
> Assicurarsi di usare la funzione [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) da RevoScaleR per salvare il modello. La funzione `serialize` R standard non è in grado di generare il formato richiesto.

Per visualizzare il modello archiviato in formato binario, è possibile eseguire un'istruzione come la seguente:

```sql
SELECT *, datalength(native_model_object)/1024. as model_size_kb
FROM ml_models;
```

#### <a name="step-2-run-predict-on-the-model"></a>Passaggio 2. Eseguire PREDICT sul modello

Con la semplice istruzione PREDICT seguente si ottiene una classificazione dal modello di albero delle decisioni usando la funzione di **assegnazione dei punteggi nativa**. Consente di stimare le specie di iris in base agli attributi specificati, ovvero lunghezza e larghezza dei petali.

```sql
DECLARE @model varbinary(max) = (
  SELECT native_model_object
  FROM ml_models
  WHERE model_name = 'iris.dtree'
  AND model_version = 'v1');
SELECT d.*, p.*
  FROM PREDICT(MODEL = @model, DATA = dbo.iris_rx_data as d)
  WITH(setosa_Pred float, versicolor_Pred float, virginica_Pred float) as p;
go
```

Se si ottiene l'errore "Si è verificato un errore durante l'esecuzione della funzione PREDICT. Il modello è danneggiato o non è valido", in genere significa che la query non ha restituito un modello. Verificare se il nome del modello è stato digitato correttamente o se la tabella models è vuota.

> [!NOTE]
> Poiché le colonne e i valori restituiti da **PREDICT** possono variare in base al tipo di modello, è necessario definire lo schema dei dati restituiti usando una clausola **WITH**.
::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

+ [Funzione PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md)
+ [Documentazione per Machine Learning in SQL](../index.yml)
+ [Machine Learning e intelligenza artificiale con ONNX in SQL Edge](/azure/azure-sql-edge/onnx-overview)
+ [Distribuire ed eseguire stime con un modello ONNX in SQL Edge di Azure](/azure/azure-sql-edge/deploy-onnx)