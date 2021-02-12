---
title: 'Eseguire processi Spark: Toolkit di Azure per IntelliJ'
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come inviare processi Spark nei cluster Big Data di SQL Server in Azure Toolkit for IntelliJ tramite l'invio di un file Jar o Py locale.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.topic: conceptual
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 36718ef7ea9def99a2b2629119cbd7dfee63dac3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043641"
---
# <a name="submit-spark-jobs-on-big-data-clusters-2019-in-intellij"></a>Inviare processi Spark su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in IntelliJ

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Uno degli scenari chiave per [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] è la possibilità di inviare processi Spark. La funzionalità di invio di processi Spark consente di inviare file Jar o Py locali con riferimenti a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]. Consente inoltre di eseguire file Jar o Py che si trovano già nel file system HDFS. 

## <a name="prerequisites"></a>Prerequisiti

- Cluster Big Data di SQL Server.
- Java Development Kit Oracle. È possibile eseguire l'installazione dal [sito Web Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. È possibile eseguire l'installazione dal [sito Web JetBrains](https://www.jetbrains.com/idea/download/).
- Estensione Azure Toolkit for IntelliJ. Per le istruzioni di installazione, vedere [Installare Azure Toolkit for IntelliJ](/azure/azure-toolkit-for-intellij-installation).

## <a name="link-sql-server-big-data-cluster"></a>Collegare un cluster Big Data di SQL Server
1. Aprire lo strumento IntelliJ IDEA.

2. Se si usa un certificato autofirmato, disabilitare la convalida del certificato TLS/SSL: dal menu **Tools** (Strumenti) selezionare **Azure**, **Validate Spark Cluster SSL Certificate** (Convalida certificato SSL cluster Spark) e quindi **Disable** (Disabilita).

    ![Collegare un cluster Big Data di SQL Server - Disabilitare TLS/SSL](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-disableSSL.png)

3. Aprire Azure Explorer dal menu **View** (Visualizza), selezionare **Tool Windows** (Finestre strumenti) e quindi selezionare **Azure Explorer**.
4. Fare clic con il pulsante destro del mouse su **SQL Server big data cluster** (Cluster Big Data di SQL Server) e scegliere **Link SQL Server big data cluster** (Collega cluster Big Data di SQL Server). Immettere i valori per **Server**, **User Name** (Nome utente) e **Password** e quindi fare clic su **OK**.

    ![Collegare un cluster Big Data - Finestra di dialogo](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-dialog.png)

5. Quando viene visualizzata la finestra di dialogo relativa al certificato del server non attendibile, fare clic su **Accept** (Accetta). È possibile gestire il certificato in un secondo momento. Vedere [Certificati del server](https://www.jetbrains.com/help/idea/settings-tools-server-certificates.html).

6. Il cluster collegato viene elencato in **SQL Server Big Data cluster** (Cluster Big Data di SQL Server). È possibile monitorare il processo Spark aprendo l'interfaccia utente della cronologia di Spark e l'interfaccia utente di Yarn. È anche possibile scollegare il cluster facendo clic con il pulsante destro del mouse su di esso.

    ![Collegare un cluster Big Data - Menu di scelta rapida](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-contextmenu.png)

## <a name="create-a-spark-scala-application-from-spark-template"></a>Creare un'applicazione Spark in Scala da un modello Spark

1. Avviare IntelliJ IDEA e creare un progetto. Nella finestra di dialogo **New Project** (Nuovo progetto) eseguire questa procedura: 

   a. Selezionare **Azure Spark/HDInsight** > **Spark Project with Samples (Scala)** (Progetto Spark con esempi - Scala).

   b. Nell'elenco **Build tool** (Strumento di compilazione) selezionare una delle opzioni seguenti in base alle esigenze:

      * **Maven**, per il supporto della creazione guidata di un progetto Scala
      * **SBT**, per la gestione delle dipendenze e la compilazione per il progetto Scala

    ![Finestra di dialogo relativa al nuovo progetto](./media/spark-submit-job-intellij-tool-plugin/create-hdi-scala-app.png)

2. Selezionare **Avanti**.

3. La creazione guidata di un progetto Scala rileva automaticamente se è stato installato il plug-in Scala. Selezionare **Installa**.

   ![Controllo del plug-in Scala](./media/spark-submit-job-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. Per scaricare il plug-in Scala, selezionare **OK**. Seguire le istruzioni per riavviare IntelliJ. 

   ![Finestra di dialogo di installazione del plug-in Scala](./media/spark-submit-job-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. Nella finestra di dialogo **New Project** (Nuovo progetto) eseguire questa procedura:  

    ![Selezione di Spark SDK](./media/spark-submit-job-intellij-tool-plugin/hdi-new-project.png)

   a. Immettere il nome e la posizione di un progetto.

   b. Nell'elenco a discesa **Project SDK** (SDK progetto) selezionare **Java 1.8** per il cluster Spark 2.x o **Java 1.7** per il cluster Spark 1.x.

   c. Nell'elenco a discesa **Spark version** (Versione di Spark) la creazione guidata di un progetto Scala integra la versione corretta per Spark SDK e Scala SDK. Se la versione del cluster Spark è precedente alla 2.0, selezionare **Spark 1.x**. In caso contrario, selezionare **Spark2.x**. Questo esempio usa **Spark 2.0.2 (Scala 2.11.8)** .

6. Selezionare **Fine**.

7. Il progetto Spark crea automaticamente un artefatto. Per visualizzare l'artefatto, eseguire questa procedura:

   a. Dal menu **File** scegliere **Project Structure** (Struttura progetto).

   b. Nella finestra di dialogo **Project Structure** (Struttura progetto) selezionare **Artifacts** (Artefatti) per visualizzare l'artefatto predefinito creato. È anche possibile creare un artefatto personalizzato selezionando il segno più ( **+** ).

      ![Informazioni sull'artefatto nella finestra di dialogo](./media/spark-submit-job-intellij-tool-plugin/default-artifact.png)
      

## <a name="submit-application-to-sql-server-big-data-cluster"></a>Inviare l'applicazione al cluster Big Data di SQL Server
Dopo aver collegato un cluster Big Data di SQL Server, è possibile inviare al cluster un'applicazione.

1. Impostare la configurazione nella finestra **Run/Debug Configurations** (Configurazioni di esecuzione/debug), fare clic su +->**Apache Spark on SQL Server** (Apache Spark in SQL Server), selezionare la scheda **Remotely Run in Cluster** (Esecuzione remota nel cluster), impostare i parametri come illustrato di seguito e quindi fare clic su OK.

    ![Voce di aggiunta della configurazione nella console interattiva](./media/spark-submit-job-intellij-tool-plugin/interactive-console-add-config-entry.png)

    ![Collegare un cluster Big Data - Configurazione](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-config.png)

    * Per **Spark clusters (Linux only)** (Cluster Spark - solo Linux) selezionare il cluster in cui si vuole eseguire l'applicazione.

    * Selezionare un artefatto nel progetto IntelliJ oppure selezionarne uno dal disco rigido.

    * Campo **Main class name** (Nome classe principale) - Il valore predefinito corrisponde alla classe principale del file selezionato. È possibile modificare la classe selezionando i puntini di sospensione ( **...** ) e scegliendo una classe diversa.   

    * Campo **Job Configurations** (Configurazioni processo) -  I valori predefiniti sono impostati come nell'immagine precedente. È possibile modificare il valore o aggiungere una nuova coppia chiave/valore per l'invio del processo. Per altre informazioni: [API REST di Apache Livy](http://livy.incubator.apache.org./docs/latest/rest-api.html)

      ![Finestra di dialogo per l'invio Spark con la configurazione del processo](./media/spark-submit-job-intellij-tool-plugin/submit-job-configurations.png)

    * Campo **Command line arguments** (Argomenti della riga di comando) - È possibile immettere i valori degli argomenti separati da uno spazio per la classe principale, se necessario.

    * Campi **Referenced Jars** (Jar di riferimento) e **Referenced Files** (File di riferimento) - È possibile immettere i percorsi per file e jar di riferimento, se presenti. Per altre informazioni: [Configurazione di Apache Spark](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) 

      ![Finestra di dialogo per l'invio Spark con i file jar](./media/spark-submit-job-intellij-tool-plugin/jar-files-meaning.png)

       > [!NOTE]  
       > Per caricare file e jar di riferimento, vedere: [Come caricare le risorse nel cluster](/azure/storage/blobs/storage-quickstart-blobs-storage-explorer)
                         
    * **Upload Path** (Percorso di caricamento) - È possibile indicare il percorso di archiviazione per l'invio delle risorse del progetto Jar o Scala. Sono supportati diversi tipi di archiviazione: **Use Spark interactive session to upload** (Usa sessione interattiva di Spark per il caricamento ) e **Use WebHDFS to upload** (Usa WebHDFS per il caricamento)
    
2. Fare clic su **SparkJobRun** per inviare il progetto al cluster selezionato. La scheda **Remote Spark Job in Cluster** (Processo Spark remoto nel cluster) visualizza lo stato dell'esecuzione del processo, nella parte inferiore. È possibile arrestare l'applicazione facendo clic sul pulsante rosso.  

    ![Collegare un cluster Big Data - Eseguire](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-run.png)

## <a name="spark-console"></a>Console Spark
È possibile eseguire la console locale Spark (Scala) o eseguire la console della sessione Spark Livy interattiva (Scala).

### <a name="spark-local-consolescala"></a>Console locale Spark (Scala)
Assicurarsi di aver soddisfatto il prerequisito relativo al file WINUTILS.EXE.

1. Dalla barra dei menu passare a **Run** > **Edit Configurations...** (Esegui > Modifica configurazioni).

2. Nella finestra **Run/Debug Configurations** (Configurazioni di esecuzione/debug) nel riquadro sinistro passare a **Apache Spark on SQL Server big data cluster (Apache Spark nel cluster Big Data di SQL Server)**  >  **[Spark on SQL] myApp**.

3. Nella finestra principale selezionare la scheda **Locally Run** (Esecuzione locale).

4. Specificare i valori seguenti e quindi selezionare **OK**:

    |Proprietà |valore |
    |----|----|
    |Job main class (Classe principale del processo)|Il valore predefinito corrisponde alla classe principale del file selezionato. È possibile modificare la classe selezionando i puntini di sospensione ( **...** ) e scegliendo una classe diversa.|
    |Variabili di ambiente|Verificare che il valore di HADOOP_HOME sia corretto.|
    |WINUTILS.exe location (Percorso di WINUTILS.exe)|Assicurarsi che il percorso sia corretto.|

    ![Impostazione della configurazione nella console locale](./media/spark-submit-job-intellij-tool-plugin/console-set-configuration.png)

5. Da Project (Progetto) passare a **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Dalla barra dei menu passare a **Tools** > **Spark Console** > **Run Spark Local Console(Scala)** (Strumenti > Console Spark > Esegui console locale Spark - Scala).

7. Potrebbero venire visualizzate due finestre di dialogo in cui viene chiesto se si vuole correggere automaticamente le dipendenze. In caso affermativo, selezionare **Auto Fix** (Correggi automaticamente).

    ![Correzione automatica 1 Spark](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix1.png)

    ![Correzione automatica 2 Spark](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix2.png)

8. La console dovrebbe essere simile all'immagine seguente. Nella finestra della console digitare `sc.appName` e quindi premere CTRL+INVIO.  Verrà visualizzato il risultato. È possibile terminare la console locale facendo clic sul pulsante rosso.

    ![Risultato nella console locale](./media/spark-submit-job-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Console della sessione Spark Livy interattiva (Scala)
La console della sessione Spark Livy interattiva (Scala) è supportata solo in IntelliJ 2018.2 e 2018.3.

1. Dalla barra dei menu passare a **Run** > **Edit Configurations...** (Esegui > Modifica configurazioni).

2. Nella finestra **Run/Debug Configurations** (Configurazioni di esecuzione/debug) nel riquadro sinistro passare a **Apache Spark on SQL Server big data cluster (Apache Spark nel cluster Big Data di SQL Server)**  >  **[Spark on SQL] myApp**.

3. Dalla finestra principale selezionare la scheda **Remotely Run in Cluster** (Esecuzione remota nel cluster).

4. Specificare i valori seguenti e quindi selezionare **OK**:

    |Proprietà |valore |
    |----|----|
    |Cluster Spark (solo Linux)|Selezionare il cluster Big Data di SQL Server in cui si vuole eseguire l'applicazione.|
    |Nome della classe principale|Il valore predefinito corrisponde alla classe principale del file selezionato. È possibile modificare la classe selezionando i puntini di sospensione ( **...** ) e scegliendo una classe diversa.|

    ![Impostazione della configurazione nella console interattiva](./media/spark-submit-job-intellij-tool-plugin/interactive-console-configuration.png)

5. Da Project (Progetto) passare a **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Dalla barra dei menu passare a **Tools** > **Spark Console** > **Run Spark Livy Interactive Session Console(Scala)** (Strumenti > Console Spark > Esegui console della sessione Spark Livy interattiva -Scala).

7. La console dovrebbe essere simile all'immagine seguente. Nella finestra della console digitare `sc.appName` e quindi premere CTRL+INVIO.  Verrà visualizzato il risultato. È possibile terminare la console locale facendo clic sul pulsante rosso.

    ![Risultato nella console interattiva](./media/spark-submit-job-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Inviare la selezione alla console Spark

Per comodità, è possibile visualizzare il risultato dello script inviando codice alla console locale o alla console della sessione Livy interattiva (Scala). È possibile evidenziare il codice nel file Scala e quindi fare clic con il pulsante destro del mouse su **Send Selection To Spark Console** (Invia selezione alla console Spark). Il codice selezionato verrà inviato alla console ed eseguito. Il risultato verrà visualizzato dopo il codice nella console. La console controllerà gli errori, se presenti.  

   ![Inviare la selezione alla console Spark](./media/spark-submit-job-intellij-tool-plugin/send-selection-to-console.png)

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sui cluster Big Data di SQL Server e sugli scenari correlati, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)?