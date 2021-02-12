---
title: Eseguire un notebook di esempio con Spark
titleSuffix: SQL Server big data clusters
description: Questa esercitazione illustra come caricare l'esecuzione di un notebook Spark di esempio in un cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 03/30/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6996e2b9b7705511fc13d479e39e9436a5e054f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044103"
---
# <a name="run-a-sample-notebook-using-spark"></a>Eseguire un notebook di esempio con Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come caricare ed eseguire un notebook di Azure Data Studio in un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Questa operazione consente infatti ai data scientist e ai data engineer di eseguire codice Python, R o Scala sul cluster.

> [!TIP]
> Se si preferisce, è possibile scaricare ed eseguire uno script per i comandi descritti in questa esercitazione. Per istruzioni, vedere gli [esempi di Spark](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark) in GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
- [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md)

## <a name="download-the-sample-notebook-file"></a>Scaricare il file del notebook di esempio

Usare le istruzioni seguenti per caricare il file del notebook di esempio **spark-sql.ipynb** in Azure Data Studio.

1. Aprire un prompt dei comandi Bash (Linux) o Windows PowerShell.

1. Passare alla directory in cui si vuole scaricare il file del notebook di esempio.

1. Eseguire il comando **curl** seguente per scaricare il file del notebook da GitHub:

   ```bash
   curl https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/data-loading/transform-csv-files.ipynb -o transform-csv-files.ipynb
   ```

## <a name="open-the-notebook"></a>Aprire il notebook

Questa procedura illustra come aprire il file del notebook in Azure Data Studio:

1. In Azure Data Studio connettersi all'istanza master del cluster Big Data. Per altre informazioni, vedere [Connettersi a un cluster Big Data](connect-to-big-data-cluster.md).

1. Fare doppio clic sulla connessione del gateway HDFS/Spark nella finestra **Server**. Selezionare quindi **Open Notebook** (Apri notebook).

   ![Apri notebook](media/notebook-tutorial-spark/azure-data-studio-open-notebook.png)

1. Attendere che il **kernel** e il contesto di destinazione **(Connetti a**) siano popolati. Impostare il **kernel** su **PySpark3** e l'opzione **Connetti a** sull'indirizzo IP dell'endpoint del cluster Big Data.

   ![Imposta Kernel e Connetti a](media/notebook-tutorial-spark/set-kernel-and-attach-to.png)

## <a name="run-the-notebook-cells"></a>Eseguire le celle del notebook

È possibile eseguire ogni cella del notebook premendo il pulsante di riproduzione a sinistra della cella. I risultati verranno visualizzati nel notebook al termine dell'esecuzione della cella.

![Eseguire la cella del notebook](media/notebook-tutorial-spark/run-notebook-cell.png)

Eseguire tutte le celle del notebook di esempio in successione. Per altre informazioni sull'uso dei notebook con [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere le risorse seguenti:

- [Come usare i notebook](../azure-data-studio/notebooks/notebooks-guidance.md)
- [Come gestire i notebook in Azure Data Studio](notebooks-manage-bdc.md)

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui notebook:
> [!div class="nextstepaction"]
> [Come usare i notebook](../azure-data-studio/notebooks/notebooks-guidance.md)