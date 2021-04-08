---
title: Modificare il codice R/Python per l'esecuzione in SQL Server
description: Informazioni su come modificare il codice R o Python per l'esecuzione come SQL Server stored procedure per migliorare le prestazioni durante l'accesso ai dati SQL.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/05/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperf-fy21q3
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3b9fd50821df27f88ecaa21f29080ae5cc051c22
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004125"
---
# <a name="modify-rpython-code-to-run-in-sql-server-in-database-instances"></a>Modificare il codice R/Python per l'esecuzione in istanze di SQL Server (in-database)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo fornisce indicazioni generali su come modificare il codice R o Python per l'esecuzione come SQL Server stored procedure per migliorare le prestazioni durante l'accesso ai dati SQL.

Quando si sposta il codice R/Python da un IDE locale o da un altro ambiente a SQL Server, il codice funziona in genere senza ulteriori modifiche. Questo vale soprattutto per il codice semplice, ad esempio una funzione che accetta alcuni input e restituisce un valore. È anche più facile trasferire le soluzioni che usano i pacchetti **RevoScaleR** / **revoscalepy** o **MicrosoftML** , che supportano l'esecuzione in contesti di esecuzione diversi con modifiche minime.

Il codice potrebbe tuttavia richiedere modifiche sostanziali se si verifica una delle condizioni seguenti:

+ Si usano le librerie che accedono alla rete o che non possono essere installate in SQL Server.
+ Il codice effettua chiamate separate alle origini dati all'esterno di SQL Server, ad esempio fogli di lavoro di Excel, file in condivisioni e altri database.
+ Si desidera parametrizzare il stored procedure ed eseguire il codice nel parametro di *\@ script* di [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
+ La soluzione originale include più passaggi che potrebbero essere più efficienti in un ambiente di produzione se eseguiti in modo indipendente, ad esempio la preparazione dei dati o la progettazione delle caratteristiche rispetto al training del modello, all'assegnazione dei punteggi o alla creazione di report.
+ Si vogliono ottimizzare le prestazioni modificando le librerie, usando l'esecuzione parallela o eseguendo l'offload di alcuni processi in SQL Server.

## <a name="step-1-plan-requirements-and-resources"></a>Passaggio 1. Pianificare risorse e requisiti

### <a name="packages"></a>Pacchetti

+ Determinare quali pacchetti sono necessari e verificare che funzionino in SQL Server.

+ Installare i pacchetti in anticipo, nella libreria di pacchetti predefinita usata da Machine Learning Services. Le librerie degli utenti non sono supportate.

### <a name="data-sources"></a>Origini dati

+ Se si intende incorporare il codice in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), identificare le origini dati primarie e secondarie.

  + Le origini dati **primarie** sono set di dati di grandi dimensioni, ad esempio dati di training del modello o dati di input per le stime. Pianificare il mapping dei set di dati di maggiori dimensioni al parametro di input di [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

  + Le origini dati **secondarie** sono in genere set di dati di dimensioni minori, ad esempio elenchi di fattori o variabili di raggruppamento aggiuntive.
  
  Attualmente sp_execute_external_script supporta solo un singolo set di dati come input per la stored procedure. È tuttavia possibile aggiungere più input scalari o binari.

  Le chiamate di stored procedure precedute da EXECUTE non possono essere usate come input per [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). È possibile usare query, viste o qualsiasi altra istruzione SELECT valida.

+ Determinare gli output necessari. Se si esegue il codice utilizzando sp_execute_external_script, l'stored procedure può restituire solo un frame di dati di conseguenza. Tuttavia, è anche possibile restituire più output scalari, inclusi i tracciati e i modelli in formato binario, nonché altri valori scalari derivati dal codice o dai parametri SQL.

### <a name="data-types"></a>Tipi di dati

Per informazioni dettagliate sui mapping dei tipi di dati tra R/Python e SQL Server, vedere questi articoli:
+ [Mapping dei tipi di dati tra R e SQL Server](../r/r-libraries-and-data-types.md)
+ [Mapping dei tipi di dati tra Python e SQL Server](../python/python-libraries-and-data-types.md)

Esaminare i tipi di dati usati nel codice R/Python ed eseguire le operazioni seguenti:

+ Creare un elenco di controllo dei possibili problemi con i tipi di dati.

  Tutti i tipi di dati R/Python sono supportati da SQL Server Machine Learning Services. Supporta tuttavia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] una maggiore varietà di tipi di dati rispetto a R o Python. Pertanto, vengono eseguite alcune conversioni implicite di tipi di dati quando si trasferiscono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dati da e verso il codice. Potrebbe essere necessario eseguire il cast esplicito di alcuni dati o convertirli.

  I valori NULL sono supportati. R usa tuttavia il costrutto di dati `na` per rappresentare un valore mancante, che è simile a un valore null.

+ Si consiglia di eliminare la dipendenza dai dati che non possono essere usati da R: ad esempio, i tipi di dati ROWID e GUID da SQL Server non possono essere utilizzati da R e generano errori.

## <a name="step-2-convert-or-repackage-code"></a>Passaggio 2. Convertire o creare un nuovo pacchetto di codice

La modifica del codice varia a seconda che si intenda inviare il codice da un client remoto per l'esecuzione nel contesto SQL Server calcolo o si intende distribuire il codice come parte di un stored procedure. Quest'ultimo può garantire prestazioni e sicurezza dei dati migliori, anche se impone requisiti aggiuntivi.

+ Definire i dati di input primari come query SQL, laddove possibile, per evitare lo spostamento dei dati.

+ Quando si esegue il codice in una stored procedure, è possibile passare attraverso più input **scalari** . Per i parametri che si vuole usare nell'output, aggiungere la parola chiave **OUTPUT**.

  Ad esempio, l'input scalare `@model_name` seguente contiene il nome del modello, che viene anche restituito nella relativa colonna nei risultati:

  ```sql
  EXECUTE sp_execute_external_script @model_name = "DefaultModel" OUTPUT
    ,@language = N'R'
    ,@script = N'R code here'
  ```

+ Tutte le variabili passate come parametri della stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) devono essere mappate a variabili nel codice. Per impostazione predefinita, il mapping delle variabili viene eseguito in base al nome. 
  Anche tutte le colonne nel set di dati di input devono essere mappate a variabili nello script.
  
  Si supponga, ad esempio, che lo script R contenga una formula come la seguente:

  ```R
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  ```
  
  Se il set di dati di input non contiene colonne con i nomi ArrDelay, CRSDepTime, DayOfWeek, CRSDepHour e DayOfWeek corrispondenti, viene generato un errore.

+ In alcuni casi, è necessario definire in anticipo uno schema di output per i risultati.

  Per inserire i dati in una tabella, ad esempio, è necessario usare la clausola **WITH RESULT SET** per specificare lo schema.

  Lo schema di output è necessario anche se lo script usa l'argomento `@parallel=1` . perché SQL Server potrebbe creare più processi per eseguire la query in parallelo, raccogliendo i risultati alla fine. Prima che possano essere creati processi paralleli, deve quindi essere preparato lo schema di output.
  
  In altri casi, è possibile omettere lo schema dei risultati usando l'opzione **WITH RESULT SETS UNDEFINED**. Questa istruzione restituisce il set di dati dallo script senza denominare le colonne o specificare i tipi di dati SQL.

+ Provare a generare dati di temporizzazione o rilevamento usando T-SQL anziché R/Python.

  Ad esempio, è possibile passare l'ora di sistema o altre informazioni usate per il controllo e l'archiviazione aggiungendo una chiamata T-SQL passata ai risultati, anziché generare dati simili nello script.

### <a name="improve-performance-and-security"></a>Migliorare le prestazioni e la sicurezza

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ Evitare di scrivere stime o risultati intermedi in un file. Scrivere le stime in una tabella per evitare lo spostamento dei dati.
::: moniker-end

+ Eseguire tutte le query in anticipo ed esaminare i piani di query di SQL Server per identificare le attività che possono essere eseguite in parallelo.

  Se la query di input può essere eseguita in parallelo, impostare `@parallel=1` come parte degli argomenti su [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

  L'elaborazione parallela con questo flag è in genere possibile ogni volta che SQL Server può usare le tabelle partizionate o distribuire una query tra più processi e aggregare i risultati alla fine. L'elaborazione parallela con questo flag non è in genere possibile se si sta eseguendo il training di modelli tramite algoritmi che richiedono la lettura di tutti i dati o se è necessario creare aggregazioni.

+ Esaminare il codice per determinare se sono presenti passaggi che possono essere eseguiti in modo indipendente o eseguiti in modo più efficiente utilizzando una chiamata di stored procedure separata. Ad esempio, è possibile ottenere prestazioni migliori eseguendo la progettazione delle funzioni o l'estrazione delle funzionalità separatamente e salvando i valori in una tabella.

+ Cercare i modi per usare T-SQL anziché il codice R/Python per i calcoli basati su set.

  ::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
  Questa soluzione R, ad esempio, mostra come le funzioni T-SQL definite dall'utente e R possono eseguire la stessa attività di progettazione delle caratteristiche: [Procedura dettagliata end-to-end di data science](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md).
  ::: moniker-end

+ Consultare uno sviluppatore di database per determinare i modi per migliorare le prestazioni usando SQL Server funzionalità come le [tabelle ottimizzate](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)per la memoria o, se si dispone di Enterprise Edition, [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

+ Se si usa R, è possibile sostituire le funzioni R convenzionali con funzioni **RevoScaleR** che supportano l'esecuzione distribuita. Per altre informazioni, vedere [confronto tra le funzioni di base R e RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r).

## <a name="step-3-prepare-for-deployment"></a>Passaggio 3. Preparare la distribuzione

+ Inviare una notifica all'amministratore in modo che i pacchetti possano essere installati e testati prima di distribuire il codice.

  In un ambiente di sviluppo è possibile installare i pacchetti come parte del codice, ma è meglio evitare di farlo in un ambiente di produzione.

  Le librerie utente non sono supportate, indipendentemente dal fatto che si usi un stored procedure o si esegua codice R/Python nel contesto di calcolo SQL Server.

### <a name="package-your-rpython-code-in-a-stored-procedure"></a>Creare il pacchetto del codice R/Python in un stored procedure

+ Creare una funzione T-SQL definita dall'utente, incorporare il codice usando l'istruzione [SP-Execute-External-script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) .

+ Se si dispone di codice R complesso, usare il pacchetto R **sqlrutils** per convertire il codice. Questo pacchetto è stato progettato per consentire agli utenti esperti di R di scrivere codice di stored procedure valido.
  Il codice R viene riscritto come una singola funzione con input e output chiaramente definiti, quindi si usa il pacchetto **sqlrutils** per generare l'input e gli output nel formato corretto. Il pacchetto **sqlrutils** genera automaticamente il codice completo della stored procedure e può anche registrare la stored procedure nel database.

  Per altre informazioni ed esempi, vedere [sqlrutils (SQL)](../r/ref-r-sqlrutils.md).

### <a name="integrate-with-other-workflows"></a>Eseguire l'integrazione con altri flussi di lavoro

+ Sfruttare gli strumenti T-SQL e i processi ETL. Eseguire la progettazione di caratteristiche, l'estrazione di caratteristiche e la pulizia dei dati in anticipo come parte dei flussi di lavoro dei dati.

  Quando si lavora in un ambiente di sviluppo dedicato, è possibile eseguire il pull dei dati nel computer, analizzare i dati in modo iterativo e quindi scrivere o visualizzare i risultati.
  Tuttavia, quando si esegue la migrazione di codice autonomo a SQL Server, gran parte di questo processo può essere semplificata o delegata ad altri strumenti di SQL Server.

+ Usare strategie di visualizzazione asincrona sicura.

  Gli utenti di SQL Server spesso non possono accedere ai file nel server e gli strumenti client SQL in genere non supportano i dispositivi grafici R/Python. Se si generano tracciati o altri elementi grafici come parte della soluzione, provare a esportare i tracciati come dati binari e a salvarli in una tabella oppure a scriverli.

+ Eseguire il wrapping delle funzioni di stima e di assegnazione dei punteggi nelle stored procedure per l'accesso diretto con le applicazioni.

## <a name="next-steps"></a>Passaggi successivi

Per esempi di come è possibile distribuire le soluzioni R e Python in SQL Server, vedere le esercitazioni seguenti:

### <a name="r-tutorials"></a>Esercitazioni di R

+ [Sviluppare un modello predittivo in R con SQL Machine Learning](../tutorials/r-predictive-model-introduction.md)

+ [Prevedere le tariffe dei taxi di NYC con classificazione binaria](../tutorials/r-taxi-classification-introduction.md)

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Sviluppo SQL per data scientist R](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

### <a name="python-tutorials"></a>Esercitazioni di Python

+ [Prevedere il noleggio di sci con regressione lineare con SQL Machine Learning](../tutorials/python-ski-rental-linear-regression.md)

+ [Prevedere le tariffe dei taxi di NYC con classificazione binaria](../tutorials/python-taxi-classification-introduction.md)
