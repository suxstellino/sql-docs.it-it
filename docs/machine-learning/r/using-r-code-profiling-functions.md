---
title: Migliorare le prestazioni usando la funzione di profilatura del codice R
description: Raccogliere informazioni utili per migliorare le prestazioni e ottenere risultati più veloci per i calcoli R in SQL Server usando le funzioni di profilatura di R. La funzione *rprof* raccoglie e restituisce informazioni sulle chiamate di funzione interne.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 511a0d73bbc0e8df5c19ee563dca9c7eef8e53eb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597038"
---
# <a name="use-r-code-profiling-functions-to-improve-performance"></a>Migliorare le prestazioni tramite le funzioni di profilatura di R
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo descrive gli strumenti per le prestazioni forniti dai pacchetti R per ottenere informazioni sulle chiamate di funzione interne. Queste informazioni possono essere usate per migliorare le prestazioni del codice.

> [!TIP]
> Questo articolo presenta alcune risorse di base per iniziare. Per linee guida per utenti esperti, è consigliabile vedere la sezione *Performance* (Prestazioni) in ["Advanced R" (R avanzato) di Hadley Wickham](http://adv-r.had.co.nz).

## <a name="using-rprof"></a>Uso di RPROF

[*rprof*](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/Rprof) è una funzione inclusa nel pacchetto di base [**utils**](https://www.rdocumentation.org/packages/utils/versions/3.5.1), caricato per impostazione predefinita. 

In generale, la funzione *rprof* opera scrivendo lo stack di chiamate in un file a intervalli specificati. È quindi possibile usare la funzione [*summaryRprof*](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/summaryRprof) per elaborare il file di output. Un vantaggio di *rprof* è il fatto di eseguire il campionamento, riducendo il carico delle prestazioni sul monitoraggio.

Per usare il profiling R nel codice, chiamare questa funzione e specificarne i parametri, incluso il percorso del file di log che verrà scritto. È possibile attivare o disattivare il profiling nel codice. La sintassi seguente illustra l'utilizzo di base: 

```R
# Specify profiling output file.
varOutputFile <- "C:/TEMP/run001.log")
Rprof(varOutputFile)

# Turn off profiling
Rprof(NULL)
    
# Restart profiling
Rprof(append=TRUE)
```

> [!NOTE]
> Per poter usare questa funzione, è necessario che Windows Perl sia installato nel computer in cui viene eseguito il codice. Di conseguenza, è consigliabile profilare il codice durante lo sviluppo in un ambiente R e quindi distribuire il codice sottoposto a debug in SQL Server.  


## <a name="r-system-functions"></a>Funzioni di sistema R

Il linguaggio R include molte funzioni di pacchetto di base per la restituzione del contenuto delle variabili di sistema. Come parte del codice R, ad esempio, è possibile usare `Sys.timezone` per ottenere il fuso orario corrente o `Sys.Time` per ottenere l'ora di sistema da R. 

Per ottenere informazioni su singole funzioni di sistema R, digitare il nome della funzione come argomento per la funzione R `help()` da un prompt dei comandi di R.

```R
help("Sys.time")
```

## <a name="debugging-and-profiling-in-r"></a>Debug e profiling in R

La documentazione per Microsoft R Open, installata per impostazione predefinita, include un manuale sullo sviluppo di estensioni per il linguaggio R che descrive [profilatura e debug](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Debugging) in modo dettagliato.

## <a name="next-steps"></a>Passaggi successivi

+ Per altre informazioni sull'ottimizzazione degli script R in SQL Server, vedere [Ottimizzazione delle prestazioni e ottimizzazione dei dati per R](r-and-data-optimization-r-services.md).
+ Per informazioni complete sull'ottimizzazione delle prestazioni in SQL Server, vedere [Centro prestazioni per il motore di database di SQL Server e il database SQL di Azure](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).
+ Per altre informazioni sul pacchetto utils, vedere [The R Utils Package](https://www.rdocumentation.org/packages/utils/versions/3.5.1).
+ Per discussioni approfondite sulla programmazione R, vedere ["Advanced R" by Hadley Wickham](http://adv-r.had.co.nz).