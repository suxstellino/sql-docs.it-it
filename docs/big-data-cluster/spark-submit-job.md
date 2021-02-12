---
title: 'Inviare processi Spark: Azure Data Studio'
titleSuffix: SQL Server Big Data Clusters
description: Inviare processi Spark nei cluster Big Data di SQL Server in Azure Data Studio.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: dadcdd9e4c50ad1944957f601a54dd1bb9687f22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045801"
---
# <a name="submit-spark-jobs-on-big-data-clusters-2019-in-azure-data-studio"></a>Inviare processi Spark di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Uno degli scenari chiave per i cluster Big Data di SQL Server è la possibilità di inviare processi Spark per SQL Server. La funzionalità di invio di processi Spark consente di inviare file Jar o Py locali con riferimenti ai cluster Big Data di SQL Server 2019. Consente inoltre di eseguire file Jar o Py che si trovano già nel file system HDFS. 

## <a name="prerequisites"></a>Prerequisiti

- [Strumenti per Big Data di SQL Server 2019](deploy-big-data-tools.md):
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
   - **kubectl**

- [Connettere Azure Data Studio al gateway HDFS/Spark del cluster Big Data](connect-to-big-data-cluster.md).

## <a name="open-spark-job-submission-dialog"></a>Aprire la finestra di dialogo di invio dei processi Spark

Ci sono diversi modi per aprire la finestra di dialogo di invio dei processi Spark. A tale scopo è possibile usare il dashboard, il menu di scelta rapida in Esplora oggetti e il riquadro comandi.

- Per aprire la finestra di dialogo di invio dei processi Spark, fare clic su **New Spark Job** (Nuovo processo Spark) nel dashboard.

    ![Menu per l'invio facendo clic nel dashboard](./media/submit-spark-job/new-spark-job.png)

- In alternativa, fare clic con il pulsante destro del mouse sul cluster in Esplora oggetti e scegliere **Submit Spark Job** (Invia processo Spark) dal menu di scelta rapida.

    ![Menu per l'invio facendo clic con il pulsante destro del mouse sul file](./media/submit-spark-job/submit-spark-job-1.png)


- Per aprire la finestra di dialogo di invio dei processi Spark con i campi relativi a Jar/Py prepopolati, fare clic con il pulsante destro del mouse su un file Jar/Py in Esplora oggetti e scegliere **Submit Spark Job** (Invia processo Spark) dal menu di scelta rapida.  

    ![Menu per l'invio facendo clic con il pulsante destro del mouse sul cluster](./media/submit-spark-job/submit-spark-job.png)

- Usare il comando **Submit Spark Job** (Invia processo Spark) dal riquadro comandi premendo **CTRL+MAIUSC+P** (in Windows) e **CMD+MAIUSC+P** (in Mac).

    ![Menu per l'invio nel riquadro comandi in Windows](./media/submit-spark-job/submit-spark-job-3.png)

    ![Menu per l'invio nel riquadro comandi in mac](./media/submit-spark-job/submit-spark-job-4.png)
  
 
## <a name="submit-spark-job"></a>Inviare un processo Spark 

La finestra di dialogo di invio dei processi Spark viene visualizzata come illustrato di seguito. Immettere il nome del processo, il percorso del file JAR/Py, la classe principale e gli altri campi. L'origine del file Jar/Py può essere locale o in HDFS. Se il processo Spark contiene file Py, file Jar di riferimento o file aggiuntivi, fare clic sulla scheda **ADVANCED** (AVANZATE) e immettere i percorsi di file corrispondenti. Fare clic su **Submit** (Invia) per inviare il processo Spark.

![Finestra di dialogo relativa a un nuovo processo Spark](./media/submit-spark-job/submit-spark-job-section.png)

![Finestra di dialogo relativa alle impostazioni avanzate](./media/submit-spark-job/submit-spark-job-section-1.png)

## <a name="monitor-spark-job-submission"></a>Monitorare l'invio del processo Spark

Una volta inviato il processo Spark, le informazioni sullo stato di invio e di esecuzione del processo vengono visualizzate nella cronologia attività a sinistra. I dettagli sullo stato di avanzamento e i log vengono visualizzati anche nella finestra **OUTPUT** nella parte inferiore.

- Quando il processo Spark è in corso, il pannello **Task History** (Cronologia attività) e la finestra **OUTPUT** vengono aggiornati in base allo stato di avanzamento.

    ![Visualizzare il processo Spark in corso](./media/submit-spark-job/monitor-spark-job-submission.png)

- Al termine del processo Spark, i collegamenti dell'interfaccia utente Spark e dell'interfaccia utente Yarn vengono visualizzati nella finestra **OUTPUT**. Fare clic sui collegamenti per altre informazioni.

    ![Collegamento del processo Spark nell'output](./media/submit-spark-job/monitor-spark-job-submission-2.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data di SQL Server e sugli scenari correlati, vedere [Che cosa sono i cluster Big Data di SQL Server?](big-data-cluster-overview.md)
