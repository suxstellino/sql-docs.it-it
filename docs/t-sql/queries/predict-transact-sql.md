---
description: PREDICT (Transact-SQL)
title: PREDICT (Transact-SQL)
titleSuffix: SQL machine learning
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||>=azure-sqldw-latest'
ms.openlocfilehash: 0baf8812e8bcfc6c5095a5e082b85b62bfa9db7e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207609"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Genera un valore o punteggi stimati in base a un modello archiviato. Per altre informazioni, vedere [Assegnazione di punteggi nativi tramite la funzione T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).

## <a name="syntax"></a>Sintassi

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```syntaxsql
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

::: moniker-end

::: moniker range=">=azure-sqldw-latest"

```syntaxsql
PREDICT  
(  
  MODEL = <model_object>,
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

<model_object> ::=
  {
    model_literal
    | model_variable
    | ( scalar_subquery )
  }
```

::: moniker-end

### <a name="arguments"></a>Argomenti

**MODEL**

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Il parametro `MODEL` viene usato per specificare il modello usato per l'assegnazione dei punteggi o la stima. Il modello viene specificato come variabile, valore letterale o espressione scalare.

`PREDICT` supporta i modelli sottoposti a training usando i pacchetti [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) e [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Il parametro `MODEL` viene usato per specificare il modello usato per l'assegnazione dei punteggi o la stima. Il modello viene specificato come variabile, valore letterale o espressione scalare.

In Istanza gestita di SQL di Azure, `PREDICT` supporta modelli in formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) o modelli sottoposti a training con i pacchetti [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) e [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range=">=azure-sqldw-latest"
Il parametro `MODEL` viene usato per specificare il modello usato per l'assegnazione dei punteggi o la stima. Il modello viene specificato come variabile, valore letterale, espressione scalare o una sottoquery scalare.

In Azure Synapse Analytics, `PREDICT` supporta i modelli in formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html).
::: moniker-end

**DATI**

Il parametro DATA viene usato per specificare i dati usati per l'assegnazione dei punteggi o la stima. I dati vengono specificati nella query sotto forma di un'origine di tabella. L'origine di tabella può essere una tabella, un alias di tabella, un alias di espressione di tabella comune, una vista o una funzione con valori di tabella.

**RUNTIME = ONNX**

> [!IMPORTANT]
> L'argomento `RUNTIME = ONNX` è disponibile solo in [Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview), [Azure SQL Edge](/azure/sql-database-edge/onnx-overview) e [Azure Synapse Analytics](/azure/synapse-analytics/overview-what-is).

Indica il motore di Machine Learning usato per l'esecuzione del modello. Il valore del parametro `RUNTIME` è sempre `ONNX`. In Azure SQL Edge e Azure Synapse Analytics il parametro è obbligatorio, mentre in Istanza gestita di SQL di Azure è facoltativo e viene inserito solo quando si usano i modelli ONNX.

**WITH ( <result_set_definition> )**

La clausola WITH viene usata per specificare lo schema dell'output restituito dalla funzione `PREDICT`.

Oltre alle colonne restituiti dalla funzione `PREDICT` stessa, tutte le colonne che fanno parte dell'input di dati sono disponibili per l'uso nella query.

### <a name="return-values"></a>Valori restituiti

Non è disponibile uno schema predefinito. Non vengono convalidati né il contenuto del modello né i valori di colonna restituiti.

- La funzione `PREDICT` interessa le colonne come input.
- La funzione `PREDICT`, poi, genera nuove colonne. Il numero delle colonne e i tipi di dati corrispondenti dipendono dal tipo di modello usato per la stima.

Eventuali messaggi di errore correlati ai dati, al modello o al formato delle colonne vengono restituiti dalla funzione di stima sottostante associata al modello.

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017"
## <a name="remarks"></a>Osservazioni

La funzione `PREDICT` è supportata da tutte le edizioni di SQL Server 2017 o versione successiva in Windows e in Linux. Non è necessario abilitare [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) per usare `PREDICT`.
::: moniker-end

### <a name="supported-algorithms"></a>Algoritmi supportati

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Il modello da usare deve essere stato creato con uno degli algoritmi supportati del pacchetto [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) o [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Per un elenco dei modelli attualmente supportati, vedere [Assegnazione dei punteggi nativa tramite la funzione T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end
::: moniker range="=azure-sqldw-latest"
Sono supportati gli algoritmi che possono essere convertiti nel formato dei modelli [ONNX](https://onnx.ai/).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Sono supportati gli algoritmi che possono essere convertiti nel formato dei modelli [ONNX](https://onnx.ai/) e dei modelli creati usando uno degli algoritmi supportati dai pacchetti [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) e [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Per un elenco degli algoritmi attualmente supportati in RevoScaleR e revoscalepy, vedere [Assegnazione di punteggi nativa tramite la funzione PREDICT T-SQL](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end

### <a name="permissions"></a>Autorizzazioni

Per `PREDICT` non sono necessarie autorizzazioni specifiche. L'utente, tuttavia, ha bisogno dell'autorizzazione `EXECUTE` per il database e dell'autorizzazione per eseguire query su tutti i dati usati come input. Se il modello è stato archiviato in una tabella, l'utente deve anche essere in grado di leggerlo dalla tabella.

## <a name="examples"></a>Esempi

Gli esempi seguenti illustrano la sintassi per chiamare la funzione `PREDICT`.

### <a name="using-predict-in-a-from-clause"></a>Uso di PREDICT in una clausola FROM

Questo esempio fa riferimento alla funzione `PREDICT` nella clausola `FROM` di un'istruzione `SELECT`:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d) WITH (Score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT test_model FROM scoring_model WHERE model_id = 1);

SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

::: moniker-end

L'alias **d** specificato per l'origine di tabella nel parametro `DATA` viene usato per fare riferimento alle colonne appartenenti a `dbo.mytable`. L'alias **p** specificato per la funzione `PREDICT` viene usato per fare riferimento alle colonne restituite dalla funzione `PREDICT`.

- Il modello viene archiviato come colonna `varbinary(max)` nella tabella **Modelli**. Altre informazioni, ad esempio l'**ID** e la **descrizione**, vengono salvate nella tabella per identificare la modalità.
- L'alias **d** specificato per l'origine di tabella nel parametro `DATA` viene usato per fare riferimento alle colonne appartenenti a `dbo.mytable`. I nomi delle colonne di dati di input devono corrispondere ai nomi degli input relativi al modello.
- L'alias **p** specificato per la funzione `PREDICT` viene usato per fare riferimento alla colonna stimata restituita dalla funzione `PREDICT`. Il nome della colonna deve corrispondere al nome di output del modello.
- Tutte le colonne di dati di input e le colonne stimate sono disponibili per la visualizzazione nell'istruzione SELECT.

::: moniker range=">=azure-sqldw-latest"

La query di esempio precedente può essere riscritta per creare una vista specificando `MODEL` come sottoquery scalare:

```sql
CREATE VIEW predictions
AS
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT test_model FROM scoring_model WHERE model_id = 1),
             DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

:::moniker-end

### <a name="combining-predict-with-an-insert-statement"></a>Combinazione di PREDICT con un'istruzione INSERT

Un caso d'uso comune per la stima consiste nella generazione di un punteggio per i dati di input e quindi nell'inserimento dei valori stimati in una tabella. L'esempio seguente presuppone che l'applicazione chiamante usi una stored procedure per inserire una riga contenente il valore stimato in una tabella:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d) WITH(score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH(score FLOAT) AS p;
```

:::moniker-end

- I risultati di `PREDICT` vengono archiviati in una tabella denominata PredictionResults. 
- Il modello viene archiviato come colonna `varbinary(max)` nella tabella **Modelli**. Nella tabella è possibile salvare informazioni aggiuntive come l'ID e la descrizione per identificare il modello.
- L'alias **d** specificato per l'origine di tabella nel parametro `DATA` viene usato per fare riferimento alle colonne appartenenti a `dbo.mytable`. I nomi delle colonne di dati di input devono corrispondere al nome degli input relativi al modello.
- L'alias **p** specificato per la funzione `PREDICT` viene usato per fare riferimento alla colonna stimata restituita dalla funzione `PREDICT`. Il nome della colonna deve corrispondere al nome di output del modello.
- Tutte le colonne di input e la colonna stimata sono disponibili per la visualizzazione nell'istruzione SELECT.

## <a name="next-steps"></a>Passaggi successivi

- [Assegnazione di punteggi nativi tramite la funzione T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)
::: moniker range="=azure-sqldw-latest||=azuresqldb-mi-current"
-   [Altre informazioni sui modelli ONNX](/azure/machine-learning/concept-onnx#get-onnx-models)
::: moniker-end
