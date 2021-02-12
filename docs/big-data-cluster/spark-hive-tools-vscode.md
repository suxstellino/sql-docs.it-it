---
title: 'Eseguire i processi: Spark & Hive Tools per VS Code'
titleSuffix: SQL Server Big Data Clusters
description: Inviare processi Spark con Spark & Hive Tools per Visual Studio Code nel cluster Big Data di SQL Server.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f89450293597c28afb8db519f1c06d9d0b566046
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043481"
---
# <a name="submit-spark-jobs-on-sql-server-big-data-cluster-in-visual-studio-code"></a>Inviare processi Spark nel cluster Big Data di SQL Server in Visual Studio Code

Informazioni su come usare Spark & Hive Tools per Visual Studio Code per creare e inviare script PySpark per Apache Spark. Verrà prima di tutto descritto come installare gli strumenti Spark e Hive in Visual Studio Code e quindi verrà illustrato come inviare i processi a Spark.  

È possibile installare Spark & Hive Tools nelle piattaforme supportate da Visual Studio Code, tra cui Windows, Linux e macOS. Di seguito sono illustrati i prerequisiti per le diverse piattaforme.


## <a name="prerequisites"></a>Prerequisiti

Per completare i passaggi in questo articolo, è necessario quanto segue:

- Un cluster Big Data di SQL Server. Vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://docs.microsoft.com/sql/big-data-cluster/big-data-cluster-overview).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono è necessario solo per Linux e macOS.
- [Configurare l'ambiente interattivo PySpark per Visual Studio Code](/azure/hdinsight/set-up-pyspark-interactive-environment).
- Una directory locale denominata **SQLBDCexample**.  Questo articolo usa **C:\SQLBDC\SQLBDCexample**.

## <a name="install-spark--hive-tools"></a>Installare Spark & Hive Tools

Dopo aver completato i prerequisiti, è possibile installare Spark & Hive Tools per Visual Studio Code.  Per installare Spark & Hive Tools, eseguire questa procedura:

1. Aprire Visual Studio Code.

2. Dalla barra dei menu passare a **Visualizza** > **Estensioni**.

3. Nella casella di ricerca immettere **Spark & Hive**.

4. Selezionare **Spark & Hive Tools** nei risultati della ricerca e quindi selezionare **Installa**.  

   ![Installare l'estensione](./media/spark-hive-tools-vscode/extension.png)

5. Ricaricare quando necessario.

## <a name="open-work-folder"></a>Aprire la cartella di lavoro

Completare la procedura seguente per aprire una cartella di lavoro e creare un file in Visual Studio Code:

1. Dalla barra dei menu passare a **File** > **Apri cartella** > **C:\SQLBDC\SQLBDCexample** e quindi fare clic sul pulsante **Seleziona cartella**. La cartella verrà visualizzata nella visualizzazione **Explorer** a sinistra.

2. Dalla visualizzazione **Explorer** selezionare la cartella, **SQLBDCexample** e quindi l'icona **Nuovo file** accanto alla cartella di lavoro.

   ![Nuovo file](./media/spark-hive-tools-vscode/new-file.png)

3. Assegnare al nuovo file l'estensione `.py` (script Spark).  Questo esempio usa **HelloWorld.py**.
4. Copiare e incollare il codice seguente nel file di script:
   ```python
    import sys
    from operator import add
    from pyspark.sql import SparkSession, Row
    
    spark = SparkSession\
        .builder\
        .appName("PythonWordCount")\
        .getOrCreate()
    
    data = [Row(col1='pyspark and spark', col2=1), Row(col1='pyspark', col2=2), Row(col1='spark vs hadoop', col2=2), Row(col1='spark', col2=2), Row(col1='hadoop', col2=2)]
    df = spark.createDataFrame(data)
    lines = df.rdd.map(lambda r: r[0])

    counters = lines.flatMap(lambda x: x.split(' ')) \
        .map(lambda x: (x, 1)) \
        .reduceByKey(add)
    
    output = counters.collect()
    sortedCollection = sorted(output, key = lambda r: r[1], reverse = True)
    
    for (word, count) in sortedCollection:
        print("%s: %i" % (word, count))
   ```

## <a name="link-a-sql-server-big-data-cluster"></a>Collegare un cluster Big Data di SQL Server

Prima di poter inviare script ai cluster da Visual Studio Code, è necessario collegare un cluster Big Data di SQL Server.

1. Dalla barra dei menu passare a **Visualizza** > **Riquadro comandi** e immettere **Spark / Hive: Link a Cluster** (Spark/Hive: Collega un cluster).

   ![Comando per il collegamento del cluster](./media/spark-hive-tools-vscode/link-cluster-command.png)

2. Selezionare il tipo di cluster collegato **SQL Server Big Data** (Big Data di SQL Server).

3. Immettere l'endpoint dei Big Data di SQL Server.

4. Immettere il nome utente del cluster Big Data di SQL Server.

5. Immettere la password per l'amministratore utenti.

6. Impostare il nome visualizzato del cluster (facoltativo).

7. Elencare i cluster ed esaminare la visualizzazione **OUTPUT** per verificare.

## <a name="list-clusters"></a>Elencare i cluster

1. Dalla barra dei menu passare a **Visualizza** > **Riquadro comandi** e immettere **Spark / Hive: List Cluster** (Spark/Hive: Elenca cluster).

2. Esaminare la visualizzazione **OUTPUT**.  La visualizzazione mostrerà i cluster collegati.

    ![Impostare una configurazione del cluster predefinito](./media/spark-hive-tools-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Impostare il cluster predefinito

1. Aprire di nuovo la cartella **SQLBDCexample** creata [in precedenza](#open-work-folder), se è stata chiusa.  

2. Selezionare il file **HelloWorld.py** creato [in precedenza](#open-work-folder). Il file verrà aperto nell'editor di script.

3. Collegare un cluster, se non è ancora stato fatto.

4. Fare clic con il pulsante destro del mouse sull'editor di script e scegliere **Spark / Hive: Set Default Cluster** (Spark/Hive: Imposta cluster predefinito).   

5. Selezionare un cluster come predefinito per il file di script corrente. Gli strumenti aggiornano automaticamente il file di configurazione **.VSCode\settings.json**. 

   ![Impostare la configurazione del cluster predefinito](./media/spark-hive-tools-vscode/set-default-cluster-configuration.png)

## <a name="submit-interactive-pyspark-queries"></a>Inviare query PySpark interattive

È possibile inviare query PySpark interattive seguendo questa procedura:

1. Aprire di nuovo la cartella **SQLBDCexample** creata [in precedenza](#open-work-folder), se è stata chiusa.  

2. Selezionare il file **HelloWorld.py** creato [in precedenza](#open-work-folder). Il file verrà aperto nell'editor di script.

3. Collegare un cluster, se non è ancora stato fatto.

4. Selezionare tutto il codice, fare clic con il pulsante destro del mouse sull'editor di script e scegliere **Spark: PySpark Interactive** (Spark: PySpark interattivo) per inviare la query oppure usare i tasti di scelta rapida **CTRL+ALT+ I**.

   ![Menu di scelta rapida PySpark Interactive (PySpark interattivo)](./media/spark-hive-tools-vscode/pyspark-interactive-right-click.png)

5. Selezionare il cluster se non è stato specificato un cluster predefinito. Dopo alcuni istanti, i risultati di **Python interattivo** verranno visualizzati in una nuova scheda. Gli strumenti consentono anche di inviare un blocco di codice invece che l'intero file di script usando il menu di scelta rapida. 

   ![Finestra di Python interattivo per PySpark interattivo](./media/spark-hive-tools-vscode/pyspark-interactive-python-interactive-window.png) 

6. Immettere **"%%info"** e quindi premere **MAIUSC+INVIO** per visualizzare le informazioni sul processo. Facoltativa

   ![Visualizzare le informazioni sul processo](./media/spark-hive-tools-vscode/pyspark-interactive-view-job-information.png)

   > [!NOTE] 
   >
   > Quando l'opzione **Python Extension Enabled** (Estensione Python abilitata) è deselezionata nelle impostazioni (per impostazione predefinita è selezionata), i risultati dell'interazione PySpark inviata useranno la finestra precedente.
   >
   > ![Estensione Python per PySpark interattivo disabilitata](./media/spark-hive-tools-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>Inviare il processo batch PySpark

1. Aprire di nuovo la cartella **SQLBDCexample** creata [in precedenza](#open-work-folder), se è stata chiusa.  

2. Selezionare il file **HelloWorld.py** creato [in precedenza](#open-work-folder). Il file verrà aperto nell'editor di script.

3. Collegare un cluster, se non è ancora stato fatto.

4. Fare clic con il pulsante destro del mouse sull'editor di script e scegliere **Spark: PySpark Batch** (Spark: Batch PySpark) oppure usare i tasti di scelta rapida **CTRL+ALT+H**. 

5. Selezionare il cluster se non è stato specificato un cluster predefinito. Dopo aver inviato un processo Python, i log di invio vengono visualizzati nella finestra **OUTPUT** in Visual Studio Code. Vengono visualizzati anche i valori di **Spark UI URL** (URL UI Spark) e **Yarn UI URL** (URL UI Yarn). È possibile aprire l'URL in un Web browser per tenere traccia dello stato del processo.

   ![Risultato dell'invio del processo Python](./media/spark-hive-tools-vscode/submit-pythonjob-result.png) 

## <a name="apache-livy-configuration"></a>Configurazione di Apache Livy

La configurazione di [Apache Livy](https://livy.incubator.apache.org/) è supportata e può essere impostata in **.VSCode\settings.json** nella cartella dell'area di lavoro. Attualmente, la configurazione di Livy supporta solo script Python. Per altre informazioni, vedere il file [README di Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

### <a name="how-to-trigger-livy-configuration"></a><a id="triggerlivyconf"></a>**Come attivare la configurazione di Livy**

#### <a name="method-1"></a>Metodo 1

1. Dalla barra dei menu passare a **File** > **Preferenze** > **Impostazioni**.  
2. Nella casella di testo **Impostazioni di ricerca** immettere **HDInsight Job Sumission: Livy Conf** (Invio processo HDInsight: Conf. Livy).  
3. Selezionare **Edit in settings.json** (Modifica in settings.json) per il risultato della ricerca pertinente.

#### <a name="method-2"></a>Metodo 2

Inviare un file e osservare che la cartella `.vscode` viene aggiunta automaticamente alla cartella di lavoro. È possibile trovare la configurazione di Livy facendo clic su `.vscode\settings.json`.

+ Impostazioni del progetto:

    ![Configurazione di Livy](./media/spark-hive-tools-vscode/hdi-livyconfig.png)

>[!NOTE]
>Per le impostazioni **driverMomory** ed **executorMomry**, impostare il valore con l'unità, ad esempio 1g o 1024m. 

### <a name="supported-livy-configurations"></a>Configurazioni di Livy supportate

#### <a name="post-batches"></a>POST/batch

**Corpo della richiesta**

| name | description | type |
| :- | :- | :- |
| file | File contenente l'applicazione da eseguire | Percorso (obbligatorio) |
| proxyUser | Utente da rappresentare quando si esegue il processo | string |
| className | Classe principale Java/Spark dell'applicazione | string |
| args | Argomenti della riga di comando per l'applicazione | Elenco di stringhe |
| jars | File jar da usare in questa sessione | Elenco di stringhe |
| pyFiles | File Python da usare in questa sessione | Elenco di stringhe |
| files | File da usare in questa sessione | Elenco di stringhe |
| driverMemory | Quantità di memoria da usare per il processo del driver | string |
| driverCores | Numero di core da usare per il processo del driver | INT |
| executorMemory | Quantità di memoria da usare per un processo executor | string |
| executorCores | Numero di core da usare per ogni executor | INT |
| numExecutors | Numero di executor da avviare per questa sessione | INT |
| archives | Archivi da usare in questa sessione | Elenco di stringhe |
| coda | Nome della coda YARN dove è stato eseguito l'invio | string |
| name | Nome della sessione | string |
| conf | Proprietà di configurazione Spark | Mappa di chiave=valore |

#### <a name="response-body"></a>Corpo della risposta

Oggetto batch creato.

| name | description | type |
| :- | :- | :- |
| id | ID della sessione | INT |
| appId | ID applicazione della sessione | string |
| appInfo | Informazioni dettagliate sull'applicazione | Mappa di chiave=valore |
| log | Righe di log | Elenco di stringhe |
| state | Stato del batch | string |

>[!NOTE]
>La configurazione di Livy assegnata verrà visualizzata nel riquadro di output quando si invia lo script.

## <a name="additional-features"></a>Funzionalità aggiuntive

Spark & Hive Tools per Visual Studio Code supporta le funzionalità seguenti:

- **Completamento automatico di IntelliSense**. Popup con suggerimenti per parole chiave, metodi, variabili e così via. Le diverse icone rappresentano tipi diversi di oggetti.

    ![Tipi di oggetti IntelliSense in Spark & Hive Tools per Visual Studio Code](./media/spark-hive-tools-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Marcatori di errore di IntelliSense**. Il servizio di linguaggio sottolinea gli errori nello script Hive.     
- **Evidenziazioni della sintassi**. Il servizio di linguaggio usa colori diversi per distinguere variabili, parole chiave, tipi di dati, funzioni e così via. 

    ![Evidenziazioni della sintassi in Spark & Hive Tools per Visual Studio Code](./media/spark-hive-tools-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="unlink-cluster"></a>Scollegare il cluster

1. Dalla barra dei menu passare a **Visualizza** > **Riquadro comandi** e immettere **Spark / Hive: Unlink a Cluster** (Spark/Hive: Scollega cluster).  

2. Selezionare il cluster da scollegare.  

3. Esaminare la visualizzazione **OUTPUT** per verificare.  

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sui cluster Big Data di SQL Server e sugli scenari correlati, vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://docs.microsoft.com/sql/big-data-cluster/big-data-cluster-overview).
