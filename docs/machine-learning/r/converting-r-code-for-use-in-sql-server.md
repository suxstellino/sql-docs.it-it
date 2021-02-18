---
title: Convertire il codice R per SQL
description: Eseguire la migrazione del codice R a una stored procedure di SQL Server per la distribuzione della soluzione e l'accesso ai dati relazionali in SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0b0d3e451c3b95bfa2401f80176dbdb659f1cb86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336289"
---
# <a name="convert-r-code-for-execution-in-sql-server-in-database-instances"></a>Convertire il codice R per l'esecuzione in istanze di SQL Server (In-Database)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo fornisce indicazioni generali su come modificare il codice R in modo che funzioni in SQL Server. 

Quando si sposta il codice R da R Studio o un altro ambiente a SQL Server, quasi sempre il codice funziona senza altre modifiche, ad esempio se il codice è semplice, come una funzione che accetta alcuni input e restituisce un valore. È anche più facile convertire le soluzioni che usano i pacchetti **RevoScaleR** o **MicrosoftML**, che supportano l'esecuzione in contesti di esecuzione diversi con modifiche minime.

Il codice potrebbe tuttavia richiedere modifiche sostanziali se si verifica una delle condizioni seguenti:

+ Si usano librerie R che accedono alla rete o che non possono essere installate in SQL Server.
+ Il codice effettua chiamate separate alle origini dati all'esterno di SQL Server, ad esempio fogli di lavoro di Excel, file in condivisioni e altri database. 
+ Si vuole eseguire il codice nel parametro *\@script* di [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) e impostare anche i parametri per la stored procedure.
+ La soluzione originale include più passaggi che potrebbero essere più efficienti in un ambiente di produzione se eseguiti in modo indipendente, ad esempio la preparazione dei dati o la progettazione delle caratteristiche rispetto al training del modello, all'assegnazione dei punteggi o alla creazione di report.
+ Si vogliono ottimizzare le prestazioni modificando le librerie, usando l'esecuzione parallela o eseguendo l'offload di alcuni processi in SQL Server. 

## <a name="step-1-plan-requirements-and-resources"></a>Passaggio 1. Pianificare risorse e requisiti

### <a name="packages"></a>Pacchetti

+ Determinare quali pacchetti sono necessari e verificare che funzionino in SQL Server.
 
+ Installare i pacchetti in anticipo, nella libreria di pacchetti predefinita usata da Machine Learning Services. Le librerie degli utenti non sono supportate.

### <a name="data-sources"></a>Origini dati

+ Se si intende incorporare il codice R in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), identificare le origini dati primarie e secondarie. 

  + Le origini dati **primarie** sono set di dati di grandi dimensioni, ad esempio dati di training del modello o dati di input per le stime. Pianificare il mapping dei set di dati di maggiori dimensioni al parametro di input di [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

  + Le origini dati **secondarie** sono in genere set di dati di dimensioni minori, ad esempio elenchi di fattori o variabili di raggruppamento aggiuntive. 
  
  Attualmente sp_execute_external_script supporta solo un singolo set di dati come input per la stored procedure. È tuttavia possibile aggiungere più input scalari o binari.

  Le chiamate di stored procedure precedute da EXECUTE non possono essere usate come input per [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). È possibile usare query, viste o qualsiasi altra istruzione SELECT valida.

+ Determinare gli output necessari. Se si esegue il codice R usando sp_execute_external_script, la stored procedure può restituire un solo frame di dati come risultato. Tuttavia, è anche possibile restituire più output scalari, inclusi i tracciati e i modelli in formato binario, nonché altri valori scalari derivati dal codice R o dai parametri SQL.

### <a name="data-types"></a>Tipi di dati

+ Creare un elenco di controllo dei possibili problemi con i tipi di dati.

  Tutti i tipi di dati R sono supportati da Machine Learning Services per SQL Server. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta tuttavia una maggiore varietà di tipi di dati rispetto a R, quindi alcune conversioni implicite dei tipi di dati vengono eseguite quando si inviano dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a R e viceversa. Potrebbe essere necessario eseguire il cast esplicito di alcuni dati o convertirli. 

  I valori NULL sono supportati. R usa tuttavia il costrutto di dati `na` per rappresentare un valore mancante, che è simile a un valore null.

+ Provare a eliminare la dipendenza dai dati che non possono essere usati da R: ad esempio, i tipi di dati rowid e GUID di SQL Server non possono essere utilizzati da R e generano errori.

  Per altre informazioni, vedere [Librerie R e tipi di dati](../r/r-libraries-and-data-types.md).

## <a name="step-2-convert-or-repackage-code"></a>Passaggio 2. Convertire o creare un nuovo pacchetto di codice

L'entità delle modifiche apportate al codice varia a seconda che si intenda inviare il codice R da un client remoto per l'esecuzione nel contesto di calcolo di SQL Server o che si intenda distribuire il codice come parte di una stored procedure, per poter migliorare le prestazioni e la sicurezza dei dati. Il wrapping del codice in una stored procedure impone alcuni requisiti aggiuntivi. 

+ Definire i dati di input primari come query SQL, laddove possibile, per evitare lo spostamento dati.

+ Quando si esegue R in una stored procedure, è possibile eseguire il pass-through di più input **scalari**. Per i parametri che si vuole usare nell'output, aggiungere la parola chiave **OUTPUT**. 

  Ad esempio, l'input scalare `@model_name` seguente contiene il nome del modello, che viene anche restituito nella relativa colonna nei risultati:

  ```sql
  EXEC sp_execute_external_script @model_name="DefaultModel" OUTPUT, @language=N'R', @script=N'R code here'
  ```

+ È necessario eseguire il mapping delle variabili passate come parametri della stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) alle variabili nel codice R. Per impostazione predefinita, il mapping delle variabili viene eseguito in base al nome.

  È anche necessario eseguire il mapping di tutte le colonne nel set di dati di input alle variabili nello script R.  Si supponga, ad esempio, che lo script R contenga una formula come la seguente:

  ```R
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  ```
  
  Se il set di dati di input non contiene colonne con i nomi ArrDelay, CRSDepTime, DayOfWeek, CRSDepHour e DayOfWeek corrispondenti, viene generato un errore.

+ In alcuni casi, è necessario definire in anticipo uno schema di output per i risultati.

  Per inserire i dati in una tabella, ad esempio, è necessario usare la clausola **WITH RESULT SET** per specificare lo schema.

  Lo schema di output è necessario anche se lo script R usa l'argomento `@parallel=1`. perché SQL Server potrebbe creare più processi per eseguire la query in parallelo, raccogliendo i risultati alla fine. Prima che possano essere creati processi paralleli, deve quindi essere preparato lo schema di output.
  
  In altri casi, è possibile omettere lo schema dei risultati usando l'opzione **WITH RESULT SETS UNDEFINED**. Questa istruzione restituisce il set di dati dallo script R senza denominare le colonne o specificare i tipi di dati SQL.

+ Provare a generare dati temporali o di rilevamento usando T-SQL invece di R.

  È ad esempio possibile passare l'ora di sistema o altre informazioni usate per il controllo e l'archiviazione aggiungendo una chiamata T-SQL che viene passata ai risultati, invece di generare dati simili nello script R. 

### <a name="improve-performance-and-security"></a>Migliorare le prestazioni e la sicurezza

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ Evitare di scrivere stime o risultati intermedi in un file. Scrivere invece le stime in una tabella, per evitare lo spostamento dati.
::: moniker-end

+ Eseguire tutte le query in anticipo ed esaminare i piani di query di SQL Server per identificare le attività che possono essere eseguite in parallelo.

  Se la query di input può essere eseguita in parallelo, impostare `@parallel=1` come parte degli argomenti su [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). 

  L'elaborazione parallela con questo flag è in genere possibile ogni volta che SQL Server può usare le tabelle partizionate o distribuire una query tra più processi e aggregare i risultati alla fine. L'elaborazione parallela con questo flag in genere non è possibile se si esegue il training dei modelli usando algoritmi che richiedono la lettura di tutti i dati o se è necessario creare aggregati.

+ Riesaminare il codice R per determinare se ci sono passaggi che possono essere eseguiti in modo indipendente o più efficiente, usando una chiamata alle stored procedure separata. È ad esempio possibile migliorare le prestazioni eseguendo separatamente la progettazione o l'estrazione di caratteristiche e salvando i valori in una tabella.

+ Cercare modi per usare T-SQL invece del codice R per i calcoli basati su set.

  ::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
  Questa soluzione R, ad esempio, mostra come le funzioni T-SQL definite dall'utente e R possono eseguire la stessa attività di progettazione delle caratteristiche: [Procedura dettagliata end-to-end di data science](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md).
  ::: moniker-end

+ Se possibile, sostituire le funzioni R convenzionali con funzioni **ScaleR** che supportano l'esecuzione distribuita. Per altre informazioni, vedere [Confronto di funzioni Base R e Scale R](/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r).

+ Consultare uno sviluppatore di database per individuare i metodi che consentono di migliorare le prestazioni usando le funzionalità di SQL Server, ad esempio le [tabelle ottimizzate per la memoria](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md) o, se si usa Enterprise Edition, [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

## <a name="step-3-prepare-for-deployment"></a>Passaggio 3. Preparare la distribuzione

+ Inviare una notifica all'amministratore in modo che i pacchetti possano essere installati e testati prima di distribuire il codice. 

  In un ambiente di sviluppo è possibile installare i pacchetti come parte del codice, ma è meglio evitare di farlo in un ambiente di produzione. 

  Le librerie utente non sono supportate, indipendentemente dal fatto che si usi una stored procedure o si esegua il codice R nel contesto di calcolo di SQL Server.

### <a name="package-your-r-code-in-a-stored-procedure"></a>Creare il pacchetto del codice R in una stored procedure

+ Se il codice è relativamente semplice, è possibile incorporarlo senza modifiche in una funzione T-SQL definita dall'utente, come descritto in questi esempi:

  + [Progettazione delle caratteristiche con T-SQL e R](../tutorials/r-taxi-classification-create-features.md)

+ Se il codice è più complesso, usare il pacchetto R **sqlrutils** per convertire il codice. Questo pacchetto è stato progettato per consentire agli utenti esperti di R di scrivere codice di stored procedure valido. 

  Il primo passaggio consiste nel riscrivere il codice R come singola funzione con input e output chiaramente definiti.

  Usare quindi il pacchetto **sqlrutils** per generare l'input e gli output nel formato corretto. Il pacchetto **sqlrutils** genera automaticamente il codice completo della stored procedure e può anche registrare la stored procedure nel database. 

  Per altre informazioni ed esempi, vedere [sqlrutils (SQL)](ref-r-sqlrutils.md).

### <a name="integrate-with-other-workflows"></a>Eseguire l'integrazione con altri flussi di lavoro

+ Sfruttare gli strumenti T-SQL e i processi ETL. Eseguire la progettazione di caratteristiche, l'estrazione di caratteristiche e la pulizia dei dati in anticipo come parte dei flussi di lavoro dei dati.

  Quando si lavora in un ambiente di sviluppo R dedicato, ad esempio [!INCLUDE[rsql_rtvs_md](../../includes/rsql-rtvs-md.md)] o RStudio, è possibile eseguire il pull dei dati al computer, l'analisi iterativa dei dati e infine la scrittura o la visualizzazione dei risultati. 
  
  Quando si esegue la migrazione di codice R autonomo a SQL Server, gran parte di questo processo può tuttavia essere semplificato o delegato ad altri strumenti di SQL Server. 

+ Usare strategie di visualizzazione asincrona sicura.

  Gli utenti di SQL Server spesso non possono accedere ai file nel server e gli strumenti client di SQL in genere non supportano il dispositivo di grafica R. Se si generano tracciati o altri elementi grafici come parte della soluzione, provare a esportare i tracciati come dati binari e a salvarli in una tabella oppure a scriverli.

+ Eseguire il wrapping delle funzioni di stima e di assegnazione dei punteggi nelle stored procedure per l'accesso diretto con le applicazioni.

## <a name="next-steps"></a>Passaggi successivi

Per visualizzare esempi di come una soluzione R può essere distribuita in SQL Server, vedere gli esempi seguenti:

+ [Esercitazione: Sviluppare un modello predittivo in R con Machine Learning in SQL](../tutorials/r-predictive-model-introduction.md)

+ [Esercitazione su R: Prevedere le tariffe dei taxi di NYC con la classificazione binaria](../tutorials/r-taxi-classification-introduction.md)

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Soluzione di data science end-to-end](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) - Include un confronto dell'ingegneria delle funzionalità in R e in T-SQL
::: moniker-end
