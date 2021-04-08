---
title: Distribuire il codice R nelle stored procedure
description: Incorporare il codice del linguaggio R in una stored procedure di SQL Server per renderlo disponibile per qualsiasi applicazione client abbia accesso a un database SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 2bcb3b9eb38d0fa9b41563c3af5b5f2e4dad7c5f
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004121"
---
# <a name="operationalize-r-code-using-stored-procedures-in-sql-server-machine-learning-services"></a>Rendere operativo il codice R usando le stored procedure in Machine Learning Services per SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Quando si usano le caratteristiche di R e Python in Machine Learning Services per SQL Server, l'approccio più comune per spostare le soluzioni in un ambiente di produzione consiste nell'incorporare il codice nelle stored procedure. Questo articolo riepiloga i punti chiave che uno sviluppatore SQL deve tenere presenti quando rende operativo il codice R usando SQL Server.

## <a name="deploy-production-ready-script-using-t-sql-and-stored-procedures"></a>Distribuire uno script pronto per la produzione con T-SQL e le stored procedure

In passato l'integrazione di soluzioni data science richiedeva di riscrivere completamente il codice per supportare le prestazioni e l'integrazione. Machine Learning Services per SQL Server semplifica questa attività perché il codice R e Python può essere eseguito in SQL Server e chiamato usando le stored procedure. Per altre informazioni sui meccanismi di incorporamento del codice nelle stored procedure, vedere:

+ [Creare ed eseguire script R semplici in SQL Server](../tutorials/quickstart-r-create-script.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

Un esempio più esauriente della distribuzione del codice R nell'ambiente di produzione tramite stored procedure è disponibile in [Esercitazione su R: Prevedere le tariffe dei taxi di NYC con la classificazione binaria](../tutorials/r-taxi-classification-introduction.md).

## <a name="guidelines-for-optimizing-r-code-for-sql"></a>Linee guida per l'ottimizzazione del codice R per SQL

La conversione del codice R in SQL è più semplice se alcune ottimizzazioni vengono eseguite in anticipo nel codice R o Python, come, ad esempio, evitare i tipi di dati che provocano problemi, evitare conversioni di dati non necessarie e riscrivere il codice R come una singola chiamata di funzione che può essere parametrizzata. Per altre informazioni, vedere:

+ [Librerie e tipi di dati R](../r/r-libraries-and-data-types.md)
+ [Usare le funzioni helper sqlrutils](../r/ref-r-sqlrutils.md)

## <a name="integrate-r-and-python-with-applications"></a>Integrare R e Python con le applicazioni

Poiché è possibile eseguire R o Python da una stored procedure, è possibile eseguire gli script da qualsiasi applicazione in grado di inviare un'istruzione T-SQL e gestire i risultati. È ad esempio possibile ripetere il training di un modello in base a una pianificazione usando l'[attività di esecuzione T-SQL](../../integration-services/control-flow/execute-t-sql-statement-task.md) in Integration Services oppure usando un'altra utilità di pianificazione dei processi che può eseguire una stored procedure.

L'assegnazione dei punteggi è un'attività importante che può essere facilmente automatizzata o avviata da applicazioni esterne. Si esegue il training del modello in anticipo, usando R o Python oppure una stored procedure, e si [salva il modello in formato binario](../tutorials/walkthrough-build-and-save-the-model.md) in una tabella. Il modello può quindi essere caricato in una variabile durante una chiamata di stored procedure, usando una di queste opzioni per l'assegnazione dei punteggi da T-SQL:

+ Assegnazione dei punteggi in tempo reale, ottimizzata per piccoli batch
+ Assegnazione dei punteggi alle singole righe, per la chiamata da un'applicazione
+ [Assegnazione dei punteggi nativa](../predictions/native-scoring-predict-transact-sql.md), per la stima in batch veloce da SQL Server senza chiamare R

L'esercitazione seguente illustra un esempio di assegnazione dei punteggi in cui viene usata un stored procedure in modalità batch e a riga singola:

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Procedura dettagliata di data science end-to-end per R in SQL Server](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ [Esercitazione su R: Prevedere le tariffe dei taxi di NYC con la classificazione binaria](../tutorials/r-taxi-classification-introduction.md)
::: moniker-end

## <a name="boost-performance-and-scale"></a>Migliorare le prestazioni e la scalabilità

Anche se il linguaggio R Open Source presenta limitazioni note per quanto riguarda i set di dati di grandi dimensioni, le [API del pacchetto RevoScaleR](../r/ref-r-revoscaler.md) incluse in SQL Server machine learning servizio possono operare su set di dati di grandi dimensioni e trarre vantaggio dai calcoli a thread multipli, multicore e multiprocesso.

Se la soluzione R usa aggregazioni complesse o comporta grandi set di dati, è possibile sfruttare le efficienti aggregazioni in memoria e gli indici columnstore di SQL Server e lasciare che il codice R gestisca i calcoli statistici e l'assegnazione dei punteggi.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>Adattare il codice R per altre piattaforme o contesti di calcolo

Lo stesso codice R eseguito sui dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere usato con altre origini dati, ad esempio Spark su HDFS, quando si usa l'[opzione di server autonomo](../install/sql-machine-learning-standalone-windows-install.md) nella configurazione di SQL Server o quando si installa il prodotto senza marchio SQL, Microsoft Machine Learning Server (noto in precedenza come **Microsoft R Server**):

+ [Documentazione di Machine Learning Server](/r-server/)

::: moniker-end
