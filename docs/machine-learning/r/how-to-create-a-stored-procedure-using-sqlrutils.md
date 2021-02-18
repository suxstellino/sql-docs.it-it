---
title: Creare una funzione dal codice R in bundle usando il pacchetto sqlrutils
description: Usare il pacchetto R sqlrutils in SQL Server per aggregare il codice in linguaggio R in una singola funzione che può essere passata come argomento a una stored procedure.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 04bcac677a4cdbf3e96f60bb43244f2cc6a0eec7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336277"
---
# <a name="create-a-stored-procedure-using-sqlrutils"></a>Creare una stored procedure con sqlrutils
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo descrive i passaggi per la conversione del codice R per l'esecuzione come stored procedure T-SQL. Per ottenere i migliori risultati possibili, può essere necessario modificare il codice per garantire che tutti gli input possano essere parametrizzati.

## <a name="step-1-rewrite-r-script"></a><a name="bkmk_rewrite"></a>Passaggio 1. Riscrivere lo script R

Per ottenere risultati ottimali, è consigliabile riscrivere il codice R per incapsularlo come funzione singola.

Tutte le variabili usate dalla funzione devono essere definite all'interno della funzione oppure devono essere definite come parametri di input. Vedere il [codice di esempio](#samples) in questo articolo.

Poiché i parametri di input per la funzione R diventeranno parametri di input della stored procedure SQL, è anche necessario assicurarsi che gli input e gli output siano conformi ai requisiti del tipo seguente:

### <a name="inputs"></a>Input

Tra i parametri di input può esistere al massimo un frame di dati.

Gli oggetti nel frame di dati e altri parametri di input della funzione devono essere dei tipi di dati R seguenti:
- POSIXct
- NUMERIC
- character
- integer
- logical
- raw

Se un tipo di input non è uno dei tipi elencati in precedenza, deve essere serializzato e passato alla funzione come *raw*. In questo caso, la funzione deve inoltre includere il codice per deserializzare l'input.

### <a name="outputs"></a>Output

La funzione può restituire uno degli elementi seguenti:

- Un frame di dati contenente i tipi di dati supportati. Tutti gli oggetti nel frame di dati devono usare uno dei tipi di dati supportati.
- Un elenco denominato contenente al massimo un frame di dati. Tutti i membri dell'elenco devono usare uno dei tipi di dati supportati.
- Un valore NULL, se la funzione non restituisce alcun risultato

## <a name="step-2-generate-required-objects"></a>Passaggio 2. Generare gli oggetti necessari

Una volta che il codice R è stato pulito e può essere chiamato come funzione singola, si useranno le funzioni nel pacchetto **sqlrutils** per preparare gli input e gli output in un formato che possa essere passato al costruttore che compila effettivamente la stored procedure.

**sqlrutils** fornisce le funzioni che definiscono lo schema e il tipo di dati di input e definiscono lo schema e il tipo di dati di output. Include anche le funzioni che possono convertire gli oggetti R nel tipo di output richiesto. Si potrebbero eseguire più chiamate alle funzioni per creare gli oggetti necessari, a seconda dei tipi di dati usati dal codice.

### <a name="inputs"></a>Input

Se la funzione accetta gli input, per ogni input chiamare le funzioni seguenti:

- `setInputData` se l'input è un frame di dati
- `setInputParameter` per tutti gli altri tipi di input

Quando si esegue la chiamata a ogni funzione, viene creato un oggetto R che verrà passato successivamente come argomento a `StoredProcedure`, per creare la stored procedure completa.

### <a name="outputs"></a>Output

**sqlrutils** offre più funzioni per la conversione di oggetti R, ad esempio gli elenchi, nel frame di dati richiesto da SQL Server.
Se la funzione restituisce un frame di dati direttamente, senza prima eseguirne il wrapping in un elenco, è possibile ignorare questo passaggio.
È anche possibile ignorare questo passaggio di conversione se la funzione restituisce NULL.

Quando si converte un elenco o si ottiene un particolare elemento da un elenco, scegliere una delle funzioni seguenti:

- `setOutputData` se la variabile da ottenere dall'elenco è un frame di dati
- `setOutputParameter` per tutti gli altri membri dell'elenco

Quando si esegue la chiamata a ogni funzione, viene creato un oggetto R che verrà passato successivamente come argomento a `StoredProcedure`, per creare la stored procedure completa.

## <a name="step-3-generate-the-stored-procedure"></a>Passaggio 3. Generare la stored procedure

Quando tutti i parametri di input e di output sono pronti, effettuare una chiamata al costruttore `StoredProcedure`.

**Utilizzo**

`StoredProcedure (func, spName, ..., filePath = NULL ,dbName = NULL, connectionString = NULL, batchSeparator = "GO")`

Per capire meglio, si supponga di voler creare una stored procedure denominata **sp_rsample** con questi parametri:

- Usa una funzione **foosql** esistente. La funzione era basata sul codice esistente nella funzione R **foo**, ma è stata riscritta per renderla conforme ai requisiti descritti in [questa sezione](#bkmk_rewrite). La funzione aggiornata è stata quindi denominata **foosql**.
- Usa il frame di dati **queryinput** come input
- Genera come output un frame di dati con il nome della variabile R, **sqloutput**
- Si vuole creare il codice T-SQL come file nella cartella `C:\Temp`, in modo che sia possibile eseguirlo in un secondo momento con SQL Server Management Studio

```R
StoredProcedure (foosql, sp_rsample, queryinput, sqloutput, filePath = "C:\\Temp")
```

> [!NOTE]
> Poiché si sta scrivendo il file nel file system, è possibile omettere gli argomenti che definiscono la connessione di database.

L'output della funzione è una stored procedure T-SQL che può essere eseguita in un'istanza di SQL Server 2016 (è necessario R Services) o di SQL Server 2017 (è necessario Machine Learning Services con R). 

Per altri esempi, vedere la guida del pacchetto chiamando `help(StoredProcedure)` da un ambiente R.

## <a name="step-4-register-and-run-the-stored-procedure"></a>Passaggio 4. Registrare ed eseguire la stored procedure

È possibile eseguire la stored procedure in due modi:

- Con T-SQL, da qualsiasi client supporti le connessioni all'istanza di SQL Server 2016 o di SQL Server 2017
- Da un ambiente R

Per entrambi i metodi è necessario che la stored procedure sia registrata nel database in cui si intende usarla.

### <a name="register-the-stored-procedure"></a>Registrare la stored procedure

È possibile registrare la stored procedure usando R oppure è possibile eseguire l'istruzione CREATE PROCEDURE in T-SQL.

- Con T-SQL.  Se si ha familiarità con T-SQL, aprire SQL Server Management Studio (o qualsiasi altro client in grado di eseguire i comandi SQL DDL) ed eseguire l'istruzione CREATE PROCEDURE usando il codice preparato dalla funzione `StoredProcedure`.
- Con R. Mentre è ancora visualizzato l'ambiente R, è possibile usare la funzione `registerStoredProcedure` in **sqlrutils** per registrare la stored procedure con il database.

  È ad esempio possibile registrare la stored procedure **sp_rsample** nell'istanza e nel database definiti in *sqlConnStr*, effettuando questa chiamata R:

  ```R
  registerStoredProcedure(sp_rsample, sqlConnStr)
  ```


> [!IMPORTANT]
> Indipendentemente dal fatto che si usi R o SQL, è necessario eseguire l'istruzione usando un account con le autorizzazioni per creare nuovi oggetti di database.

### <a name="run-using-sql"></a>Esecuzione con SQL

Una volta creata la stored procedure, aprire una connessione al database SQL usando un client che supporti T-SQL e passare i valori per tutti i parametri richiesti dalla stored procedure.

### <a name="run-using-r"></a>Esecuzione con R

Per eseguire la stored procedure dal codice R invece che da SQL Server, sono necessarie alcune operazioni di preparazione aggiuntive. Se ad esempio la stored procedure richiede valori di input, è necessario impostare i parametri di input prima che la funzione possa essere eseguita e quindi passare tali oggetti alla stored procedure nel codice R.

Il processo generale di chiamata della stored procedure SQL preparata è il seguente:

1. Chiamare `getInputParameters` per ottenere un elenco di oggetti parametro di input.
2. Definire `$query` o impostare `$value` per ogni parametro di input.
3. Usare `executeStoredProcedure` per eseguire la stored procedure dall'ambiente di sviluppo R, passando l'elenco di oggetti parametro di input impostato.

## <a name="example"></a><a name = "samples"></a>Esempio

Questo esempio mostra la versione originale e quella modificata di uno script R che ottiene i dati da un database di SQL Server, esegue alcune trasformazioni sui dati e li salva in un altro database.

Questo semplice esempio viene usato solo per illustrare come è possibile riordinare il codice R per facilitare la conversione in una stored procedure.

### <a name="before-code-preparation"></a>Prima della preparazione del codice


```R
sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineSrc;Trusted_Connection=Yes;"
  
sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineTest;Trusted_Connection=Yes;"
  
sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"
  
dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
xFunc <- function(data) {
    data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
    return(data)
    }
  
xVars <- c("CRSDepTime")
  
sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
rxOpen(dsSqlFrom)
rxOpen(dsSqlTo)
  
if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo))   {
    rxSqlServerDropTable("cleanData")}
  
rxDataStep(inData = dsSqlFrom, 
     outFile = dsSqlTo,
     transformFunc = xFunc,
     transformVars = xVars,
     overwrite = TRUE)
```

> [!NOTE]
> 
> Quando si usa una connessione ODBC, invece di richiamare la funzione *RxSqlServerData* è necessario aprire la connessione usando *rxOpen* prima di poter eseguire le operazioni nel database.


### <a name="after-code-preparation"></a>Dopo la preparazione del codice

Nella versione aggiornata la prima riga definisce il nome della funzione. Il resto del codice dalla soluzione R originale diventa parte di tale funzione.

```R
myetl1function <- function() { 
   sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=Airline01;Trusted_Connection=Yes;"
   sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer02;Database=Airline02;Trusted_Connection=Yes;"
    
   sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"

   dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
   dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
   xFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)}
  
   xVars <- c("CRSDepTime")
  
   sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
   if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo)) {rxSqlServerDropTable("cleanData")}
  
   rxDataStep(inData = dsSqlFrom, 
        outFile = dsSqlTo,
        transformFunc = xFunc,
        transformVars = xVars,
        overwrite = TRUE)
   return(NULL)
}
```

> [!NOTE]
> 
> Anche se non è necessario aprire la connessione ODBC in modo esplicito come parte del codice, è comunque necessaria una connessione ODBC per usare **sqlrutils**.

## <a name="see-also"></a>Vedere anche

[sqlrutils (SQL)](ref-r-sqlrutils.md)


