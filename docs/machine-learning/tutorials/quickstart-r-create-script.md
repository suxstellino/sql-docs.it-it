---
title: 'Guida introduttiva: Eseguire gli script R'
titleSuffix: SQL machine learning
description: Eseguire un set di script R semplici con Machine Learning in SQL. Informazioni su come usare la stored procedure sp_execute_external_script per eseguire lo script.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 540804d2a35758627b24586ec8c29ca2dbab6e5f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101926"
---
# <a name="quickstart-run-simple-r-scripts-with-sql-machine-learning"></a>Guida introduttiva: Eseguire script R semplici con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In questo argomento di avvio rapido verrà eseguito un set di script R semplici con [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) oppure in [cluster Big Data](../../big-data-cluster/machine-learning-services.md). Si apprenderà come usare la stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per eseguire lo script in un'istanza di SQL Server.
::: moniker-end
::: moniker range="=sql-server-2017"
In questo argomento di avvio rapido verrà seguito un set di semplici script R con [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). Si apprenderà come usare la stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per eseguire lo script in un'istanza di SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
In questo argomento di avvio rapido verrà eseguito un set di script R semplici con [R Services per SQL Server](../r/sql-server-r-services.md). Si apprenderà come usare la stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per eseguire lo script in un'istanza di SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In questo argomento di avvio rapido verrà eseguito un set di script R semplici con [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Si apprenderà come usare la stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per eseguire lo script nel database in uso.
::: moniker-end

## <a name="prerequisites"></a>Prerequisiti

Per completare questo argomento di avvio rapido è necessario soddisfare i prerequisiti seguenti.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- Machine Learning Services per SQL Server. Per installare Machine Learning Services, vedere la [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md) o la [guida all'installazione di Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). È anche possibile [abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- Machine Learning Services per SQL Server. Per installare Machine Learning Services, vedere la [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- R Services per SQL Server 2016. Per installare R Services, vedere la [guida all'installazione di Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Machine Learning Services per Istanza gestita di SQL di Azure. Per informazioni, vedere [Panoramica di Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Uno strumento per l'esecuzione di query SQL che contengono script R. In questo argomento di avvio rapido viene usato [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Eseguire uno script semplice

Per eseguire uno script R, passarlo come argomento alla stored procedure di sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Questa stored procedure di sistema avvia il runtime R, passa i dati a R, gestisce le sessioni utente di R in modo sicuro e restituisce i risultati al client.

Nei passaggi seguenti si eseguirà questo script R di esempio:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Aprire **Azure Data Studio** e connettersi al server.

1. Passare lo script R completo alla stored procedure `sp_execute_external_script`.

   Lo script viene passato tramite l'argomento `@script`. Tutti gli elementi all'interno dell'argomento `@script` devono essere costituiti da codice R valido.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Viene calcolato il risultato corretto e la funzione `print` di R restituisce il risultato nella finestra **Messaggi**.

   Il risultato sarà simile al seguente.

    **Risultati**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Eseguire uno script Hello World

Uno script di esempio tipico è quello che restituisce come output la stringa "Hello World". Eseguire il comando seguente.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Gli input per la stored procedure `sp_execute_external_script` includono:

| Input | Descrizione |
|-|-|
| @language | Definisce l'estensione del linguaggio da chiamare, in questo caso R |
| @script | Definisce i comandi passati al runtime R. L'intero script R deve essere incluso in questo argomento, come testo Unicode. È anche possibile aggiungere il testo a una variabile di tipo **nvarchar** e quindi chiamare la variabile |
| @input_data_1 | Dati restituiti dalla query, passati al runtime R, che restituisce i dati come frame di dati |
|WITH RESULT SETS | Clausola che definisce lo schema della tabella dati restituita, aggiungendo "Hello World" come nome di colonna e **int** per il tipo di dati |

Il comando restituisce il testo seguente:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Usare input e output

Per impostazione predefinita, `sp_execute_external_script` accetta un singolo set di dati come input, che in genere viene fornito sotto forma di query SQL valida. Restituisce quindi un singolo frame di dati R come output.

Per il momento, verranno usate le variabili di input e di output predefinite di `sp_execute_external_script`: **InputDataSet** e **OutputDataSet**.

1. Creare una piccola tabella di dati di test.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Usare l'istruzione `SELECT` per eseguire una query sulla tabella.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Risultati**

    ![Contenuto della tabella RTestData](./media/select-rtestdata.png)

1. Eseguire lo script R seguente. Questo script recupera i dati dalla tabella usando l'istruzione `SELECT`, li passa nel runtime R e restituisce i dati come frame di dati. La clausola `WITH RESULT SETS` definisce lo schema della tabella dati restituita per SQL, aggiungendo il nome di colonna *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Risultati**

    ![Output dello script R che restituisce i dati di una tabella](./media/r-output-rtestdata.png)

1. Modificare ora i nomi delle variabili di input e di output. I nomi delle variabili di input e di output predefiniti sono **InputDataSet** e **OutputDataSet**. Questo script cambia i nomi in **SQL_in** e **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Si noti che R fa distinzione tra maiuscole e minuscole. Le variabili di input e di output usate nello script R (**SQL_out**, **SQL_in**) devono corrispondere ai nomi definiti con `@input_data_1_name` e `@output_data_1_name`, inclusa la distinzione tra maiuscole e minuscole.

   > [!TIP]
   > È possibile passare come parametro un solo set di dati di input e si può restituire un solo set di dati. Tuttavia, è possibile chiamare altri set di dati dall'interno del codice R e restituire output di altri tipi in aggiunta al set di dati. È anche possibile aggiungere la parola chiave OUTPUT a qualsiasi parametro per fare in modo che venga restituito con i risultati.

1. È anche possibile generare i valori usando solo lo script R senza dati di input (`@input_data_1` è impostato su un valore vuoto).

   Lo script seguente restituisce il testo "hello" e "world".

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Risultati**

    ![Risultati di query con @script come input](./media/r-data-generated-output.png)

## <a name="check-r-version"></a>Controllare la versione di R

Per vedere la versione di R installata, eseguire lo script seguente.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

La funzione R `print` restituisce la versione nella finestra **Messaggi**. Nell'output di esempio seguente si può notare che in questo caso è installata la versione di R 3.4.4.

**Risultati**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Elencare i pacchetti R
::: moniker range=">=sql-server-2017"
Microsoft offre una serie di pacchetti R preinstallati con Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Microsoft offre una serie di pacchetti R preinstallati con R Services.
::: moniker-end

Per visualizzare un elenco dei pacchetti R installati, incluse le informazioni su versione, dipendenze, licenza e percorso della libreria, eseguire lo script seguente.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

L'output proviene da `installed.packages()` in R e viene restituito come set di risultati.

**Risultati**

![Pacchetti installati in R](./media/rsql-installed-packages.png) 

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come usare le strutture di dati quando si usa R con Machine Learning in SQL, seguire questo argomento di avvio rapido:

> [!div class="nextstepaction"]
> [Gestire tipi di dati e oggetti usando R con Machine Learning in SQL](quickstart-r-data-types-and-objects.md)