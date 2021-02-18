---
title: Novità di Machine Learning Services per SQL Server
titleSuffix: ''
description: Annunci di nuove funzionalità per ogni versione di Machine Learning Services per SQL Server e di R Services per SQL Server 2016.
ms.date: 11/17/2020
ms.topic: overview
author: dphansen
ms.author: davidph
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 31ecd76efc4796aebe2a3ee40a8bf40891d38a83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272932"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>Novità di SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../includes/applies-to-version/sqlserver2016.md)]

Questo articolo descrive le nuove capacità e funzionalità incluse in ogni versione di [SQL Server Machine Learning Services](sql-server-machine-learning-services.md). In ogni versione di SQL Server vengono aggiunte funzionalità di Machine Learning man mano che l'integrazione tra piattaforma dati, analisi avanzata e data science viene espansa, estesa e approfondita. 

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="new-in-sql-server-2019"></a>Novità di SQL Server 2019

In questa versione sono state aggiunte le funzionalità più richieste per le operazioni di Machine Learning R e Python in SQL Server. Per altre informazioni su tutte le funzionalità di questa versione, vedere [Novità di SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) e [Note sulla versione per SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

> [!NOTE]
> Per la documentazione sulle novità di Java in SQL Server 2019, vedere [Novità nelle estensioni del linguaggio di SQL Server](../language-extensions/language-extensions-whats-new.md)

Di seguito sono elencate le nuove funzionalità di Machine Learning Services per SQL Server disponibili sia in **Windows** che in **Linux**:

- Il supporto della piattaforma Linux è stato aggiunto in Machine Learning Services per Python e R. Per iniziare, vedere [Installare Machine Learning Services per SQL Server in Linux](../linux/sql-server-linux-setup-machine-learning.md).
- [Connessione loopback a SQL Server da uno script Python o R](connect/loopback-connection.md). 
- [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md) per Python e R.
- La stored procedure [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) introduce due nuovi parametri che consentono di generare facilmente più modelli da dati partizionati. Per altre informazioni, vedere l'esercitazione [Creare modelli basati su partizioni in R](tutorials/r-tutorial-create-models-per-partition.md).
- Il supporto per i cluster di failover è disponibile per il servizio Launchpad a condizione che Launchpad di SQL Server venga avviato in tutti i nodi. Per altre informazioni, vedere [Installazione del cluster di failover di SQL Server](../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md).
- Modifiche al meccanismo di isolamento per Machine Learning Services. Per altre informazioni, vedere [SQL Server 2019 in Windows: Modifiche al meccanismo di isolamento per Machine Learning Services](install/sql-server-machine-learning-services-2019.md).

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="new-in-sql-server-2017"></a>Novità di SQL Server 2017

In questa versione sono stati aggiunti il [supporto di Python e algoritmi di Machine Learning leader del settore](https://cloudblogs.microsoft.com/sqlserver/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/). Rinominato per riflettere il nuovo ambito, SQL Server 2017 vede l'introduzione di [SQL Server Machine Learning Services (In-Database)](sql-server-machine-learning-services.md), con supporto del linguaggio sia per Python che per R. 

Per informazioni complete sugli annunci di funzionalità, vedere [Novità di SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

### <a name="r-enhancements"></a>Miglioramenti di R

Il componente R di SQL Server Machine Learning Services rappresenta la nuova generazione di SQL Server 2016 R Services, con versioni aggiornate di pacchetti R di base, RevoScaleR e altri pacchetti.

Le nuove funzionalità di R includono la [**gestione di pacchetti**](package-management/install-r-packages-with-tsql.md), con le interessanti caratteristiche seguenti: 

+ I ruoli del database aiutano gli amministratori di database a gestire i pacchetti e assegnare le autorizzazioni per l'installazione dei pacchetti.
+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) consente agli amministratori di database di gestire i pacchetti nel linguaggio T-SQL familiare.
+ Le funzioni [RevoScaleR](package-management/install-r-packages-with-revoscaler.md) aiutano a installare, rimuovere o elencare i pacchetti di proprietà degli utenti. Per altre informazioni, vedere [Usare funzioni RevoScaleR per trovare o installare pacchetti R in SQL Server](package-management/install-r-packages-with-revoscaler.md).

### <a name="r-libraries"></a>Librerie R

| Pacchetto | Descrizione |
|---------|-------------|
| [**MicrosoftML**](r/ref-r-microsoftml.md) | In questa versione, il pacchetto MicrosoftML è incluso in un'installazione di R predefinita, eliminando il passaggio di aggiornamento necessario in precedenza in SQL Server 2016 R Services. MicrosoftML fornisce algoritmi di Machine Learning e trasformazioni dei dati all'avanguardia che è possibile ridimensionare o eseguire in contesti di calcolo remoti. Gli algoritmi includono reti neurali profonde personalizzabili, foreste delle decisioni e alberi delle decisioni rapidi, regressione lineare e regressione logistica.  |

### <a name="python-integration-for-in-database-analytics"></a>Integrazione di Python per l'analisi nel database

Python è un linguaggio che offre flessibilità e potenza straordinarie per un'ampia gamma di attività di Machine Learning. Le librerie open source per Python includono diverse piattaforme per reti neurali personalizzabili, nonché librerie popolari per l'elaborazione del linguaggio naturale. 

Poiché Python si integra con il motore di database, è possibile mantenere l'analisi vicina ai dati ed eliminare i costi e i rischi di sicurezza associati allo spostamento dei dati. È possibile distribuire soluzioni di Machine Learning basate su Python usando strumenti come Visual Studio. Le applicazioni di produzione possono ottenere stime, modelli o oggetti visivi dal runtime di Python 3.5 usando metodi di accesso ai dati di SQL Server.

L'integrazione di T-SQL e Python è supportata tramite la stored procedure di sistema [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). È possibile chiamare qualsiasi codice Python usando questa stored procedure. Il codice viene eseguito in un'architettura doppia sicura, che consente la distribuzione di livello aziendale di script e modelli Python, che è possibile richiamare da un'applicazione usando una semplice stored procedure. Ulteriori miglioramenti delle prestazioni vengono ottenuti tramite la trasmissione del flusso di dati dai processi SQL ai processi Python e la parallelizzazione dei segnali MPI.

È possibile usare la funzione T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) per eseguire l'[assegnazione di punteggi a livello nativo](predictions/native-scoring-predict-transact-sql.md) in un modello con training preliminare salvato in precedenza nel formato binario richiesto.

### <a name="python-libraries"></a>Librerie Python

| Pacchetto | Descrizione |
|---------|-------------|
|[**revoscalepy**](python/ref-py-revoscalepy.md)| Equivalente Python di RevoScaleR. È possibile creare modelli Python per regressioni lineari e logistiche, alberi delle decisioni, alberi con boosting e foreste casuali, tutti parallelizzabili ed eseguibili in contesti di calcolo remoti. Questo pacchetto supporta l'uso di più origini dati e contesti di calcolo remoti. Il data scientist o lo sviluppatore può eseguire codice Python in un'istanza remota di SQL Server, per esplorare i dati o creare modelli senza spostare i dati. |
|[**microsoftml**](python/ref-py-microsoftml.md) |Equivalente Python del pacchetto MicrosoftML R. |

### <a name="pre-trained-models"></a>Modelli con training preliminare

Sono disponibili [**modelli con training preliminare**](install/sql-pretrained-models-install.md) per Python e R. Usare questi modelli per il riconoscimento delle immagini e l'analisi del sentiment positivo-negativo, per generare stime sui dati. 

### <a name="standalone-server-as-a-shared-feature-in-sql-server-setup"></a>Server autonomo come funzionalità condivisa nell'installazione di SQL Server

In questa versione viene aggiunto anche [SQL Server Machine Learning Server (Standalone)](r/r-server-standalone.md), un server di data science completamente indipendente che supporta l'analisi predittiva e statistica in R e Python. Come per R Services, questo server è la versione successiva di SQL Server 2016 R Server (Standalone). Con il server autonomo è possibile distribuire e ridimensionare soluzioni R o Python senza dipendenze in SQL Server.
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="new-in-sql-server-2016"></a>Novità di SQL Server 2016

In questa versione sono state introdotte funzionalità di Machine Learning in SQL Server tramite **SQL Server 2016 R Services**, un motore di analisi nel database per l'elaborazione di script R sui dati residenti all'interno di un'istanza del motore di database.

È stato inoltre rilasciato **SQL Server 2016 R Server (Standalone)** come strumento per l'installazione di R Server in un server Windows. Inizialmente, l'installazione di SQL Server rappresentava l'unico modo per installare R Server per Windows. Con le versioni successive, sviluppatori e data scientist che vogliono installare R Server in Windows possono usare un altro programma di installazione autonomo per ottenere lo stesso obiettivo. Il server autonomo in SQL Server è equivalente dal punto di vista funzionale al prodotto server autonomo [Microsoft R Server per Windows](/machine-learning-server/install/r-server-install-windows).

Per informazioni complete sugli annunci di funzionalità, vedere [Novità di SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

| Versione |Aggiornamento delle funzionalità |
|---------|----------------|
| Aggiunte tramite aggiornamento cumulativo | L'[**assegnazione di punteggi in tempo reale**](predictions/real-time-scoring.md) si basa sulle librerie C++ native per leggere un modello archiviato in un formato binario ottimizzato e quindi generare stime senza dover chiamare il runtime di R. Ciò rende molto più rapide le operazioni di assegnazione dei punteggi. Con l'assegnazione dei punteggi in tempo reale, è possibile eseguire una stored procedure o assegnare punteggi in tempo reale dal codice R. L'assegnazione dei punteggi in tempo reale è disponibile anche per SQL Server 2016, se l'istanza viene aggiornata alla versione più recente di [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)]. |
| Versione iniziale | [**Integrazione di R per l'analisi nel database**](r/sql-server-r-services.md). <br/><br/> Pacchetti R per la chiamata di funzioni R in T-SQL e viceversa. Le funzioni RevoScaleR forniscono funzionalità di analisi R su larga scala tramite suddivisione dei dati in parti componenti, coordinamento e gestione dell'elaborazione distribuita e aggregazione dei risultati. In SQL Server 2016 R Services (In-Database), il motore RevoScaleR è integrato con un'istanza del motore di database, quindi dati e analisi vengono riuniti nello stesso contesto di elaborazione. <br/><br/>Integrazione di T-SQL e R tramite [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). È possibile chiamare qualsiasi codice R usando questa stored procedure. Questa infrastruttura sicura consente la distribuzione di livello aziendale di script e modelli R, che è possibile chiamare da un'applicazione usando una semplice stored procedure. Ulteriori miglioramenti delle prestazioni vengono ottenuti tramite la trasmissione del flusso di dati dai processi SQL ai processi R e la parallelizzazione dei segnali MPI. <br/><br/>È possibile usare la funzione T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) per eseguire l'[assegnazione di punteggi a livello nativo](predictions/native-scoring-predict-transact-sql.md) in un modello con training preliminare salvato in precedenza nel formato binario richiesto.|

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="linux-support"></a>Supporto di Linux

In SQL Server 2019 viene aggiunto il supporto di Linux per R e Python quando si installano i pacchetti di Machine Learning con un'istanza del motore di database. Per altre informazioni, vedere [Installare SQL Server Machine Learning Services in Linux](../linux/sql-server-linux-setup-machine-learning.md).

In Linux, SQL Server 2017 non ha l'integrazione di R o Python, ma è possibile usare l'[assegnazione di punteggi a livello nativo](predictions/native-scoring-predict-transact-sql.md) in Linux perché tale funzionalità è disponibile tramite la funzione T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md), che viene eseguita in Linux. L'assegnazione di punteggi a livello nativo consente prestazioni elevate per l'assegnazione di punteggi da un modello con training preliminare, senza necessità di chiamate e nemmeno del runtime R.
::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

+ [Installare SQL Server Machine Learning Services (In-Database)](install/sql-machine-learning-services-windows-install.md)
