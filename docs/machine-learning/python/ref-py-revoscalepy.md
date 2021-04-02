---
title: Pacchetto revoscalepy per Python
description: revoscalepy è un pacchetto Python di Microsoft che supporta l'elaborazione distribuita, i contesti di calcolo remoti e gli algoritmi di data science con prestazioni elevate.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: 15f526feb973387c0f1bcd5bc4e6de0019a5774e
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980932"
---
# <a name="revoscalepy-python-package-in-sql-server-machine-learning-services"></a>revoscalepy (pacchetto Python in Machine Learning Services per SQL Server)
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

**revoscalepy** è un pacchetto Python di Microsoft che supporta l'elaborazione distribuita, i contesti di calcolo remoti e gli algoritmi di data science con prestazioni elevate. Il pacchetto è incluso in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md).

Il pacchetto fornisce le funzionalità seguenti:

+ Contesti di calcolo locali e remoti in sistemi con la stessa versione di **revoscalepy**
+ Funzioni di visualizzazione e trasformazione dei dati
+ Funzioni di data science, scalabili tramite elaborazione distribuita o parallela
+ Prestazioni migliorate, incluso l'uso delle librerie Intel Math

Le origini dati e i contesti di calcolo creati in **revoscalepy** possono essere usati anche negli algoritmi di Machine Learning. Per un'introduzione a questi algoritmi, vedere [Modulo microsoftml per Python in SQL Server](ref-py-microsoftml.md).

## <a name="full-reference-documentation"></a>Documentazione di riferimento completa

Il pacchetto **revoscalepy** è distribuito in più prodotti Microsoft, ma l'utilizzo è lo stesso indipendentemente dal fatto che sia incluso in SQL Server o in un altro prodotto. Poiché le funzioni sono le stesse, la [documentazione per le singole funzioni di revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) viene pubblicata in una sola posizione nelle [informazioni di riferimento per Python](/machine-learning-server/python-reference/introducing-python-package-reference) per Microsoft Machine Learning Server. In caso di comportamenti specifici per un prodotto, le discrepanze verranno indicate nella pagina della Guida delle funzioni in questione.

## <a name="versions-and-platforms"></a>Versioni e piattaforme

Il modulo **revoscalepy** è basato su Python 3.5 ed è disponibile solo quando si installa uno dei prodotti o download Microsoft seguenti:

+ [SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)
+ [Microsoft Machine Learning Server 9.2.0 o versione successiva](/machine-learning-server/)
+ [Librerie client Python per un client di data science](setup-python-client-tools-sql.md)

> [!NOTE]
> Le versioni complete del prodotto sono solo per Windows in SQL Server 2017. Per **revoscalepy** in [SQL Server 2019](../../linux/sql-server-linux-setup-machine-learning.md) e versioni successive sono supportati sia Windows che Linux.

## <a name="functions-by-category"></a>Funzioni per categoria

Questa sezione elenca le funzioni per categoria per offrire un'idea di come vengono usate. È anche possibile usare il [sommario](/machine-learning-server/python-reference/introducing-python-package-reference) per trovare le funzioni in ordine alfabetico.

## <a name="1-data-source-and-compute"></a>1 - Origine dati e calcolo

**revoscalepy** include funzioni per la creazione di origini dati e l'impostazione della posizione, o *contesto di calcolo*, in cui vengono eseguiti i calcoli. Nella tabella seguente sono elencate le funzioni relative agli scenari con SQL Server.

In alcuni casi SQL Server e Python usano tipi di dati diversi. Per un elenco dei mapping tra i tipi di dati SQL e Python, vedere [Mapping dei tipi di dati tra Python e SQL](python-libraries-and-data-types.md).

| Funzione| Descrizione|
| ------- | ---------- |
| [RxInSqlServer](/machine-learning-server/python-reference/revoscalepy/rxinsqlserver) |  Crea un oggetto contesto di calcolo di SQL Server per eseguire il push dei calcoli in un'istanza remota. Diverse funzioni di **revoscalepy** accettano il contesto di calcolo come argomento. Per un esempio di cambio di contesto, vedere [Creare un modello usando revoscalepy](../tutorials/use-python-revoscalepy-to-create-model.md).|
| [RxSqlServerData](/machine-learning-server/python-reference/revoscalepy/rxsqlserverdata) | Crea un oggetto dati basato su una query o una tabella SQL Server. |
| [RxOdbcData](/machine-learning-server/python-reference/revoscalepy/rxodbcdata)| Crea un'origine dati basata su una connessione ODBC. |
| [RxXdfData](/machine-learning-server/python-reference/revoscalepy/rxxdfdata) | Crea un'origine dati basata su un file XDF locale. I file XDF vengono spesso usati per l'offload dei dati in memoria su disco. Un file XDF può essere utile quando si usano più dati di quanti sia possibile trasferire dal database in un batch o più dati di quanti sia possibile archiviare in memoria. Se ad esempio si spostano regolarmente grandi quantità di dati da un database a una workstation locale, invece di eseguire ripetutamente query sul database per ogni operazione R, è possibile usare il file XDF come una sorta di cache per salvare i dati localmente e quindi lavorare nell'area di lavoro R. |

> [!TIP]
> Se non si ha familiarità con il concetto di origini dati o di contesti di calcolo, è consigliabile iniziare con il [calcolo distribuito](/machine-learning-server/r/how-to-revoscaler-distributed-computing) nella documentazione di Microsoft Machine Learning Server.

## <a name="2-data-manipulation-etl"></a>2 - Manipolazione dei dati (ETL)

| Funzione | Descrizione |
|----------|-------------|
|[rx_import](/machine-learning-server/python-reference/revoscalepy/rx-import) | Importare i dati in un file XDF o un frame di dati.|
|[rx_data_step](/machine-learning-server/python-reference/revoscalepy/rx-data-step) | Trasforma i dati da un set di dati di input a un set di dati di output.|

<a name="bkmk_algorithms"></a>

## <a name="3-training-and-summarization"></a>3 - Training e riepilogo

| Funzione| Descrizione|
| ------- | ---------- |
|[rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) | Adatta gli alberi delle decisioni con boosting a gradienti stocastici|
|[rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) | Adatta le foreste delle decisioni di classificazione e regressione|
|[rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) | Adatta gli alberi di classificazione e regressione |
|[rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) | Creare un modello di regressione lineare|
|[rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) | Creare un modello di regressione logistica|
|[rx_summary](/machine-learning-server/python-reference/revoscalepy/rx-summary) | Genera riepiloghi univariati di oggetti in revoscalepy.|

Per altri approcci, è consigliabile vedere anche le funzioni di [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package).

<a name="ml-scoring"></a>

## <a name="4-scoring-functions"></a>4 - Funzioni di assegnazione dei punteggi

| Funzione| Descrizione|
| ------- | ---------- |
| [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) | Generare stime da un modello sottoposto a training e utilizzabili per il punteggio in tempo reale. |
|[rx_predict_default](/machine-learning-server/python-reference/revoscalepy/rx-predict-default) | Calcola i valori stimati e i residui usando gli oggetti rx_lin_mod e rx_logit. |
|[rx_predict_rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-predict-rx-dforest) | Calcola i valori stimati o adattati per un set di dati da un oggetto rx_dforest o rx_btrees. |
|[rx_predict_rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-predict-rx-dtree) | Calcola i valori stimati o adattati per un set di dati da un oggetto rx_dtree. |

## <a name="how-to-work-with-revoscalepy"></a>Come utilizzare revoscalepy

Le funzioni di **revoscalepy** possono essere chiamate nel codice Python incapsulato nelle stored procedure. La maggior parte degli sviluppatori compila in locale le soluzioni **revoscalepy** e quindi esegue la migrazione del codice Python completato alle stored procedure, come esercizio di distribuzione.

Durante un'esecuzione locale, si esegue in genere uno script Python dalla riga di comando o da un ambiente di sviluppo Python e si specifica un contesto di calcolo di SQL Server usando una delle funzioni di **revoscalepy**. È possibile usare il contesto di calcolo remoto per l'intero codice o per singole funzioni. Ad esempio, potrebbe essere necessario eseguire l'offload del training del modello nel server per usare i dati più recenti ed evitare lo spostamento dati.

Quando si è pronti per incapsulare lo script Python all'interno di una stored procedure, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), è consigliabile riscrivere il codice come una singola funzione con input e output chiaramente definiti. 

Gli input e gli output devono essere frame di dati **pandas**. Al termine dell'operazione, sarà possibile chiamare la stored procedure da qualsiasi client con supporto per T-SQL, passare facilmente query SQL come input e salvare i risultati in tabelle SQL. Per un esempio, vedere l'esercitazione [Analisi dei dati Python nel database per sviluppatori SQL](../tutorials/python-taxi-classification-introduction.md).

### <a name="using-revoscalepy-with-microsoftml"></a>Uso di revoscalepy con microsoftml

Le funzioni Python per [microsoftml](ref-py-microsoftml.md) sono integrate con i contesti di calcolo e le origini dati disponibili in revoscalepy. Quando si chiamano le funzioni da microsoftml, ad esempio quando si definisce un modello e ne si esegue il training, usare le funzioni revoscalepy per eseguire il codice Python in locale o in un contesto di calcolo remoto di SQL Server.

L'esempio seguente mostra la sintassi per l'importazione di moduli nel codice Python. È quindi possibile fare riferimento alle singole funzioni necessarie.

```python
from microsoftml.modules.logistic_regression.rx_logistic_regression import rx_logistic_regression
from revoscalepy.functions.RxSummary import rx_summary
from revoscalepy.etl.RxImport import rx_import_datasource
```

## <a name="see-also"></a>Vedere anche

+ [Esercitazioni di Python](../tutorials/python-tutorials.md)
+ [Informazioni di riferimento su Python (Microsoft Machine Learning Server)](/machine-learning-server/python-reference/introducing-python-package-reference)