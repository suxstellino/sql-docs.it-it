---
title: 'Guida introduttiva: Funzioni Python'
titleSuffix: SQL machine learning
description: In questo argomento di avvio rapido viene descritto come usare le funzioni matematiche e di utilità Python con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5871307e3bdd5be271547624089c9c47d2932739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351442"
---
# <a name="quickstart-python-functions-with-sql-machine-learning"></a>Avvio rapido: Funzioni Python con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Questo argomento di avvio rapido descrive come usare funzioni matematiche e di utilità Python con [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md), [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) o [cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md). Le funzioni statistiche sono spesso complesse da implementare in T-SQL, ma possono essere eseguite in Python con poche righe di codice.

## <a name="prerequisites"></a>Prerequisiti

Per completare questo argomento di avvio rapido è necessario soddisfare i prerequisiti seguenti.

- Un database SQL in una di queste piattaforme:
  - [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). Per eseguire l'installazione, vedere la [guida all'installazione in Windows](../install/sql-machine-learning-services-windows-install.md) o la [guida all'installazione in Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Cluster Big Data di SQL Server. Vedere [Abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services per Istanza gestita di SQL di Azure. Per informazioni, vedere [Panoramica di Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Uno strumento per l'esecuzione di query SQL che contengono script Python. In questo argomento di avvio rapido viene usato [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Creare una stored procedure per generare numeri casuali

Per semplicità, si userà il pacchetto `numpy` Python, che viene installato e caricato per impostazione predefinita. Il pacchetto contiene centinaia di funzioni per le attività statistiche comuni, tra cui la funzione `random.normal`, che genera una quantità specificata di numeri casuali usando la distribuzione normale, in base a una deviazione e a una media standard.

Ad esempio, il codice Python seguente restituisce 100 numeri, in una media di 50, in base alla deviazione standard 3.

```Python
numpy.random.normal(size=100, loc=50, scale=3)
```

Per chiamare questa riga di codice Python da T-SQL, aggiungere la funzione Python nel parametro di script Python di `sp_execute_external_script`. L'output prevede un frame di dati, che può essere convertito con `pandas`.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=100, loc=50, scale=3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Si vuole semplificare la generazione di un set diverso di numeri casuali? Si definisce una stored procedure che ottiene gli argomenti dall'utente e quindi si passano tali argomenti nello script Python come variabili.

```sql
CREATE PROCEDURE MyPyNorm (
      @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=mynumbers, loc=mymean, scale=mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- La prima riga definisce ognuno dei parametri di input SQL necessari quando viene eseguita la stored procedure.

- La riga che inizia con `@params` definisce tutte le variabili usate dal codice Python e i tipi di dati SQL corrispondenti.

- Le righe immediatamente successive eseguono il mapping dei nomi dei parametri SQL ai nomi delle variabili Python corrispondenti.

Dopo aver eseguito il wrapping della funzione Python in una stored procedure, è possibile chiamare facilmente la funzione e passare valori diversi, in questo modo:

```sql
EXECUTE MyPyNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-python-utility-functions-for-troubleshooting"></a>Usare funzioni di utilità Python per la risoluzione dei problemi

I pacchetti Python forniscono un'ampia gamma di funzioni di utilità per l'analisi dell'ambiente Python corrente. Queste funzioni possono essere utili se si riscontrano discrepanze nell'esecuzione del codice Python in SQL Server e in ambienti esterni.

È possibile, ad esempio, usare le funzioni di calcolo del tempo di sistema disponibili nel pacchetto di `time` per misurare la quantità di tempo usata dai processi Python e analizzare i problemi di prestazioni.

```sql
EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
import time
start_time = time.time()

# Run Python processes

elapsed_time = time.time() - start_time
'
    , @input_data_1 = N' ;';
```

## <a name="next-steps"></a>Passaggi successivi

Per creare un modello di Machine Learning usando Python con Machine Learning in SQL, seguire questo argomento di avvio rapido:

> [!div class="nextstepaction"]
> [Avvio rapido: Creare un modello predittivo in Python e assegnare i punteggi](quickstart-python-train-score-model.md)