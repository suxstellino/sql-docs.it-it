---
title: Creare una tabella usando rxDataStep
description: Informazioni su come spostare i dati tra i frame di dati in memoria, il contesto SQL Server e i file locali usando rxDataStep.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 011f2c80f7b59f28fbc5ba49de3613716b27a68a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470612"
---
# <a name="create-new-sql-server-table-using-rxdatastep-sql-server-and-revoscaler-tutorial"></a>Creare una nuova tabella di SQL Server usando rxDataStep (esercitazione su SQL Server e RevoScaleR)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questa è l'esercitazione 11 della [serie di esercitazioni per RevoScaleR](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md) dedicate all'uso delle [funzioni di RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) con SQL Server.

In questa esercitazione verrà illustrato come spostare i dati tra i frame di dati in memoria, il contesto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i file locali.

> [!NOTE]
> Questa esercitazione usa un set di dati diverso. Il set di dati dei ritardi delle compagnie aeree è un set di dati pubblico ampiamente usato per gli esperimenti di Machine Learning. I file di dati usati in questo esempio sono disponibili nella stessa directory degli altri esempi del prodotto.

## <a name="load-data-from-a-local-xdf-file"></a>Caricare i dati da un file XDF locale

Nella prima metà di questa serie di esercitazione è stata usata la funzione **RxTextData** per importare i dati in R da un file di testo e quindi è stata usata la funzione **RxDataStep** per spostare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Per questa esercitazione l'approccio è diverso e si usano i dati di un file salvato in [formato XDF](https://en.wikipedia.org/wiki/Extensible_Data_Format). Dopo avere eseguito alcune piccole trasformazioni sui dati usando il file XDF, i dati trasformati vengono salvati in una nuova tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

**Che cos'è XDF?**

Il formato XDF è uno standard XML sviluppato per dati di dimensioni elevate e costituisce il formato di file nativo usato da [Machine Learning Server](/machine-learning-server/r/concept-what-is-xdf). Si tratta di un formato di file binario con un'interfaccia R che ottimizza l'elaborazione e l'analisi di righe e colonne.  È possibile usarlo per spostare i dati e archiviare subset di dati utili per l'analisi.

1. Impostare il contesto di calcolo sulla workstation locale. **Per questa operazione sono necessarie autorizzazioni DDL.**

    ```R
    rxSetComputeContext("local")
    ```
  
2. Definire un nuovo oggetto origine dati usando la funzione **RxXdfData** . Per definire un'origine dati XDF, specificare il percorso del file di dati.  

    È possibile specificare il percorso del file usando una variabile di testo. Tuttavia, in questo caso è disponibile un metodo più rapido, che consiste nell'usare la funzione **rxGetOption** e ottenere il file (AirlineDemoSmall.xdf) dalla directory dei dati di esempio.
  
    ```R
    xdfAirDemo <- RxXdfData(file.path(rxGetOption("sampleDataDir"),  "AirlineDemoSmall.xdf"))
    ```

3. Chiamare [rxGetVarInfo](/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) nei dati in memoria per visualizzare un riepilogo del set di dati.
  
    ```R
    rxGetVarInfo(xdfAirDemo)
    ```

**Risultati**

```R
Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
Var 3: DayOfWeek 7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday
```

> [!NOTE]
> 
> Si noti che non è stato necessario chiamare altre funzioni per caricare i dati nel file XDF e che è stato possibile chiamare immediatamente **rxGetVarInfo** nei dati. Ciò avviene perché XDF è il metodo di archiviazione provvisorio predefinito per **RevoScaleR**. Oltre ai file XDF, la funzione **rxGetVarInfo** supporta ora più tipi di origini.

## <a name="move-contents-to-sql-server"></a>Spostare il contenuto in SQL Server

Con l'origine dati XDF creata nella sessione di R locale, è ora possibile spostare i dati in una tabella di database, archiviando *DayOfWeek* come integer con valori compresi tra 1 e 7.

1. Definire un oggetto di origine dati di SQL Server, specificando una tabella che contenga i dati e la connessione al server remoto.
  
    ```R
    sqlServerAirDemo <- RxSqlServerData(table = "AirDemoSmallTest", connectionString = sqlConnString)
    ```
  
2. Come misura precauzionale, includere un passaggio che verifichi se esiste già una tabella con lo stesso nome e, se esiste, eliminarla. La presenza di una tabella preesistente con lo stesso nome impedisce di crearne una nuova.
  
    ```R
    if (rxSqlServerTableExists("AirDemoSmallTest",  connectionString = sqlConnString))  rxSqlServerDropTable("AirDemoSmallTest",  connectionString = sqlConnString)
    ```
  
3. Caricare i dati nella tabella usando **rxDataStep**. Questa funzione sposta i dati tra due origini dati già definite e può facoltativamente trasformare i dati in transito.
  
    ```R
    rxDataStep(inData = xdfAirDemo, outFile = sqlServerAirDemo,
        transforms = list( DayOfWeek = as.integer(DayOfWeek),
        rowNum = .rxStartRow : (.rxStartRow + .rxNumRows - 1) ),
        overwrite = TRUE )
    ```
  
    Si tratta di una tabella di dimensioni piuttosto grandi, quindi attendere che venga visualizzato un messaggio di stato finale simile al seguente: *Righe lette: 200000 - Totale righe elaborate: 600000*.
     
## <a name="load-data-from-a-sql-table"></a>Caricare i dati da una tabella SQL

Una volta che i dati sono presenti nella tabella, è possibile caricarli usando una semplice query SQL. 

1. Creare una nuova origine dati di SQL Server. L'input è una query sulla nuova tabella appena creata e caricata con i dati. Questa definizione aggiunge livelli di fattore per la colonna *DayOfWeek*, usando l'argomento *colInfo* di **RxSqlServerData**.
  
    ```R
    sqlServerAirDemo2 <- RxSqlServerData(
        sqlQuery = "SELECT * FROM AirDemoSmallTest",
        connectionString = sqlConnString,
        rowsPerRead = 50000,
        colInfo = list(DayOfWeek = list(type = "factor",  levels = as.character(1:7))))
    ```
  
2. Chiamare **rxSummary** ancora una volta per esaminare un riepilogo dei dati nella query.
  
    ```R
    rxSummary(~., data = sqlServerAirDemo2)
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Eseguire un'analisi in blocchi tramite rxDataStep](../../machine-learning/tutorials/deepdive-perform-chunking-analysis-using-rxdatastep.md)