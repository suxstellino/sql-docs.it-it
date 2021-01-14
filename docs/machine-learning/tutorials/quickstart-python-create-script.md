---
title: 'Guida introduttiva: Eseguire script Python'
titleSuffix: SQL machine learning
description: Eseguire un set di script Python semplici usando Machine Learning Services in SQL Server, cluster Big Data o Istanza gestita di SQL di Azure. Informazioni su come usare la stored procedure sp_execute_external_script per eseguire lo script.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q1
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 95c009dda19220fb0f5f5901f1da9d5539ca9e11
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101955"
---
# <a name="quickstart-run-simple-python-scripts-with-sql-machine-learning"></a>Avvio rapido: Eseguire script Python semplici con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In questo argomento di avvio rapido verrà eseguito un set di semplici script Python con [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md), [Machine Learning Services di Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) o [cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md). Si apprenderà come usare la stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per eseguire lo script in un'istanza di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

Per completare questo argomento di avvio rapido è necessario soddisfare i prerequisiti seguenti.

- Un database SQL in una di queste piattaforme:
  - [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). Per eseguire l'installazione, vedere la [guida all'installazione in Windows](../install/sql-machine-learning-services-windows-install.md) o la [guida all'installazione in Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Cluster Big Data di SQL Server. Vedere [Abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services per Istanza gestita di SQL di Azure. Per informazioni, vedere [Panoramica di Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Uno strumento per l'esecuzione di query SQL che contengono script Python. In questo argomento di avvio rapido viene usato [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Eseguire uno script semplice

Per eseguire uno script Python, è necessario passarlo come argomento alla stored procedure di sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Questa stored procedure di sistema avvia il runtime Python nel contesto della funzione di Machine Learning in SQL, passa i dati a Python, gestisce le sessioni utente di Python in modo sicuro e restituisce i risultati al client.

Nei passaggi seguenti si eseguirà questo script Python di esempio nel database:

```python
a = 1
b = 2
c = a/b
d = a*b
print(c, d)
```

1. In **Azure Data Studio** aprire una nuova finestra di query connessa all'istanza di SQL.

1. Passare lo script Python completo alla stored procedure `sp_execute_external_script`.

   Lo script viene passato tramite l'argomento `@script`. Tutti gli elementi all'interno dell'argomento `@script` devono essere costituiti da codice Python valido.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Il risultato corretto viene calcolato e la funzione Python `print` restituisce il risultato nella finestra **Messaggi**.

   Il risultato sarà simile al seguente.

    **Risultati**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Eseguire uno script Hello World

Uno script di esempio tipico è quello che restituisce come output la stringa "Hello World". Eseguire il comando seguente.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'OutputDataSet = InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Gli input per la stored procedure `sp_execute_external_script` includono:

| Input | Descrizione |
|-|-|
| @language | Definisce l'estensione del linguaggio da chiamare, in questo caso Python |
| @script | Definisce i comandi passati al runtime Python. L'intero script Python deve essere incluso in questo argomento come testo Unicode. È anche possibile aggiungere il testo a una variabile di tipo **nvarchar** e quindi chiamare la variabile |
| @input_data_1 | Dati restituiti dalla query, passati al runtime Python, che restituisce i dati come frame di dati |
| WITH RESULT SETS | Clausola che definisce lo schema della tabella dati restituita per Machine Learning in SQL, aggiungendo "Hello World" come nome di colonna e **int** per il tipo di dati |

Il comando restituisce il testo seguente:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Usare input e output

Per impostazione predefinita, `sp_execute_external_script` accetta un singolo set di dati come input, che in genere viene fornito sotto forma di query SQL valida. Restituisce quindi un singolo frame di dati Python come output.

Per il momento, verranno usate le variabili di input e di output predefinite di `sp_execute_external_script`: **InputDataSet** e **OutputDataSet**.

1. Creare una piccola tabella di dati di test.

    ```sql
    CREATE TABLE PythonTestData (col1 INT NOT NULL)
    
    INSERT INTO PythonTestData
    VALUES (1);
    
    INSERT INTO PythonTestData
    VALUES (10);
    
    INSERT INTO PythonTestData
    VALUES (100);
    GO
    ```

1. Usare l'istruzione `SELECT` per eseguire una query sulla tabella.
  
    ```sql
    SELECT *
    FROM PythonTestData
    ```

    **Risultati**

    ![Contenuto della tabella PythonTestData](./media/select-pythontestdata.png)

1. Eseguire lo script Python seguente. Questo script recupera i dati dalla tabella usando l'istruzione `SELECT`, li passa nel runtime Python e restituisce i dati come frame di dati. La clausola `WITH RESULT SETS` definisce lo schema della tabella dati restituita per SQL, aggiungendo il nome di colonna *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'OutputDataSet = InputDataSet;'
        , @input_data_1 = N'SELECT * FROM PythonTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Risultati**

    ![Output dello script Python che restituisce i dati di una tabella](./media/python-output-pythontestdata.png)

1. Modificare ora i nomi delle variabili di input e di output. I nomi delle variabili di input e di output predefiniti sono **InputDataSet** e **OutputDataSet**. Lo script seguente cambia i nomi in **SQL_in** e **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'SQL_out = SQL_in;'
        , @input_data_1 = N'SELECT 12 as Col;'
        , @input_data_1_name  = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Si noti che Python fa distinzione tra maiuscole e minuscole. Le variabili di input e di output usate nello script Python (**SQL_out**, **SQL_in**) devono corrispondere ai nomi definiti con `@input_data_1_name` e `@output_data_1_name`, inclusa la distinzione tra maiuscole e minuscole.

    > [!TIP]
    > È possibile passare come parametro un solo set di dati di input e si può restituire un solo set di dati. È tuttavia possibile chiamare altri set di dati dall'interno del codice Python e restituire output di altri tipi oltre al set di dati. È anche possibile aggiungere la parola chiave OUTPUT a qualsiasi parametro per fare in modo che venga restituito con i risultati.

1. È anche possibile generare i valori usando solo lo script Python senza dati di input (`@input_data_1` è impostato su un valore vuoto).

    Lo script seguente restituisce il testo "hello" e "world".

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    import pandas as pd
    mytextvariable = pandas.Series(["hello", " ", "world"]);
    OutputDataSet = pd.DataFrame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Risultati**

    ![Risultati di query con @script come input](./media/python-data-generated-output.png)

> [!NOTE]
> Python usa gli spazi iniziali per raggruppare le istruzioni. Quindi, quando lo script Python incorporato si estende su più righe, come nello script precedente, non provare a far rientrare i comandi Python in modo che siano allineati con i comandi SQL. Ad esempio, questo script genera un errore:
>
> ```sql
> EXECUTE sp_execute_external_script @language = N'Python'
>       , @script = N'
>       import pandas as pd
>       mytextvariable = pandas.Series(["hello", " ", "world"]);
>       OutputDataSet = pd.DataFrame(mytextvariable);
>       '
>       , @input_data_1 = N''
> WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
> ```

## <a name="check-python-version"></a>Controllare la versione di Python

Per vedere la versione di Python installata nel server, eseguire lo script seguente.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import sys
print(sys.version)
'
GO
```

La funzione Python `print` restituisce la versione nella finestra **Messaggi**. Nell'output di esempio seguente si può notare che in questo caso è installata la versione di Python 3.5.2.

**Risultati**

```text
STDOUT message(s) from external script:
3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
```

## <a name="list-python-packages"></a>Elencare i pacchetti Python

Microsoft fornisce una serie di pacchetti Python preinstallati con Machine Learning Services.

Per visualizzare un elenco dei pacchetti Python installati, inclusa la versione, eseguire lo script seguente.

```SQL
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import pkg_resources
import pandas
dists = [str(d) for d in pkg_resources.working_set]
OutputDataSet = pandas.DataFrame(dists)
'
WITH RESULT SETS(([Package] NVARCHAR(max)))
GO
```

L'elenco proviene da `pkg_resources.working_set` in Python e viene restituito a SQL come frame di dati.

**Risultati**

:::image type="content" source="media/python-package-list.png" alt-text="Elenco dei pacchetti Python installati":::

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come usare le strutture di dati quando si usa Python con Machine Learning in SQL, seguire questo argomento di avvio rapido:

> [!div class="nextstepaction"]
> [Avvio rapido: Strutture dei dati e oggetti con Python](quickstart-python-data-structures.md)