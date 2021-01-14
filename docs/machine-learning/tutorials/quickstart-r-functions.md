---
title: 'Guida introduttiva: Funzioni R'
titleSuffix: SQL machine learning
description: Questo argomento di avvio rapido descrive come usare funzioni matematiche e di utilità R con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0da73dd1202d4b5eab2c14077dfe54b04dfe9bd8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092667"
---
# <a name="quickstart-r-functions-with-sql-machine-learning"></a>Guida introduttiva: Funzioni R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Questo argomento di avvio rapido descrive come usare funzioni matematiche e di utilità R con [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) o in [cluster Big Data](../../big-data-cluster/machine-learning-services.md). Le funzioni statistiche sono spesso complesse da implementare in T-SQL, ma possono essere eseguite in R con poche righe di codice.
::: moniker-end
::: moniker range="=sql-server-2017"
Questo argomento di avvio rapido descrive come usare funzioni matematiche e di utilità R con [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). Le funzioni statistiche sono spesso complesse da implementare in T-SQL, ma possono essere eseguite in R con poche righe di codice.
::: moniker-end
::: moniker range="=sql-server-2016"
Questo argomento di avvio rapido descrive come usare funzioni matematiche e di utilità R con [R Services per SQL Server](../r/sql-server-r-services.md). Le funzioni statistiche sono spesso complesse da implementare in T-SQL, ma possono essere eseguite in R con poche righe di codice.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In questo argomento di avvio rapido viene descritto come usare le strutture dei dati e i tipi di dati con R in [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Si apprenderà come spostare i dati tra R e Istanza gestita di SQL e verranno illustrati i problemi comuni che possono verificarsi.
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

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Creare una stored procedure per generare numeri casuali

Per semplicità, si userà il pacchetto `stats` di R, che viene installato e caricato per impostazione predefinita. Il pacchetto contiene centinaia di funzioni per le attività statistiche comuni, tra cui la funzione `rnorm`, che genera una quantità specificata di numeri casuali usando la distribuzione normale, in base a una deviazione e a una media standard.

Ad esempio, il codice R seguente restituisce 100 numeri, in una media di 50, in base alla deviazione standard 3.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Per chiamare questa riga di codice R da T-SQL, aggiungere la funzione R nel parametro di script R di `sp_execute_external_script`, come illustrato di seguito:

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));'
    , @input_data_1 = N'   ;'
      WITH RESULT SETS (([Density] float NOT NULL));
```

Si vuole semplificare la generazione di un set diverso di numeri casuali?

Questa operazione è semplice se eseguita in combinazione con T-SQL. Si definisce una stored procedure che ottiene gli argomenti dall'utente e quindi si passano tali argomenti nello script R come variabili.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- La prima riga definisce ognuno dei parametri di input SQL necessari quando viene eseguita la stored procedure.

- La riga che inizia con `@params` definisce tutte le variabili usate dal codice R e i tipi di dati SQL corrispondenti.

- Le righe immediatamente successive eseguono il mapping dei nomi dei parametri SQL ai nomi delle variabili R corrispondenti.

Dopo aver eseguito il wrapping della funzione R in una stored procedure, è possibile chiamare facilmente la funzione e passare valori diversi, in questo modo:

```sql
EXECUTE MyRNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Usare funzioni di utilità R per la risoluzione dei problemi

Il pacchetto **utils**, installato per impostazione predefinita, offre un'ampia gamma di funzioni di utilità per analizzare l'ambiente R corrente. Queste funzioni possono essere utili se si riscontrano discrepanze nell'esecuzione del codice R in SQL Server e in ambienti esterni.

È ad esempio possibile usare le funzioni relative al tempo di sistema in R, come `system.time` e `proc.time`, per acquisire il tempo usato dai processi R e analizzare i problemi di prestazioni. Per un esempio, vedere l'esercitazione [Creare caratteristiche dei dati](../tutorials/walkthrough-create-data-features.md) in cui le funzioni di calcolo del tempo R sono incorporate nella soluzione.

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
        library(utils);
        start.time <- proc.time();
        
        # Run R processes
        
        elapsed_time <- proc.time() - start.time;'
```

Per altre funzioni utili, vedere [Migliorare le prestazioni tramite le funzioni di profilatura di R](../r/using-r-code-profiling-functions.md).

## <a name="next-steps"></a>Passaggi successivi

Per creare un modello di Machine Learning usando R con Machine Learning in SQL, seguire questo argomento di avvio rapido:

> [!div class="nextstepaction"]
> [Creare e assegnare i punteggi a un modello predittivo in R con Machine Learning in SQL](quickstart-r-train-score-model.md)