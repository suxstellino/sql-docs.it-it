---
title: 'Esercitazione su Python: Categorizzare i clienti'
titleSuffix: SQL machine learning
description: In questa serie di esercitazioni in quattro parti si eseguirà il clustering dei clienti tramite K-Means in un database usando Python con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/26/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: dc119f4afe15606dc0d97e69abf0e3f055235bd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101958"
---
# <a name="python-tutorial-categorizing-customers-using-k-means-clustering-with-sql-machine-learning"></a>Esercitazione su Python: Categorizzazione dei clienti tramite clustering K-Means con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In questa serie di esercitazioni in quattro parti si userà Python per sviluppare e distribuire un modello di clustering K-Means in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) oppure in [cluster Big Data](../../big-data-cluster/machine-learning-services.md) per categorizzare i dati dei clienti.
::: moniker-end
::: moniker range="=sql-server-2017"
In questa serie di esercitazioni in quattro parti si userà Python per sviluppare e distribuire un modello di clustering K-Means in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) per il clustering dei dati dei clienti.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In questa serie di esercitazioni in quattro parti si userà Python per sviluppare e distribuire un modello di clustering K-means in [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) per il clustering dei dati dei clienti.
::: moniker-end

Nella prima parte della serie verranno configurati i prerequisiti per l'esercitazione e quindi verrà ripristinato un set di dati di esempio in un database. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di clustering in Python con Machine Learning in SQL.

Nelle seconda e nella terza parte della serie si svilupperanno alcuni script Python in un notebook di Azure Data Studio per analizzare e preparare i dati ed eseguire il training di un modello di Machine Learning. Nella quarta parte verranno quindi eseguiti gli script Python all'interno di un database mediante stored procedure.

Per *clustering* si intende l'organizzazione dei dati in gruppi in cui i membri di ciascun gruppo sono simili per certi aspetti. Per questa serie di esercitazioni, si supponga di essere proprietari di un'azienda di vendita al dettaglio. Si userà l'algoritmo **K-Means** per eseguire il clustering dei clienti in un set di dati di acquisti e resi di prodotti. Il clustering dei clienti favorisce attività di marketing più mirate rivolte a gruppi specifici. Il clustering K-Means è un algoritmo di *apprendimento non supervisionato* che cerca schemi nei dati in base ad analogie.

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Ripristinare un database di esempio

Nella [seconda parte](python-clustering-model-prepare-data.md) si apprenderà come preparare i dati di un database per il clustering.

Nella [terza parte](python-clustering-model-build.md) si apprenderà come creare ed eseguire il training di un modello di clustering K-Means in Python.

Nella [quarta parte](python-clustering-model-deploy.md) si apprenderà come creare una stored procedure in un database in grado di eseguire il clustering in Python in base ai nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) con l'opzione del linguaggio Python: seguire le istruzioni di installazione nella [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md) o nella [guida all'installazione di Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true). È anche possibile [abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) con l'opzione del linguaggio Python: seguire le istruzioni di installazione nella [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services per Istanza gestita di SQL di Azure. Per informazioni, vedere [Panoramica di Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) per il ripristino del database di esempio in Istanza gestita di SQL di Azure.
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Si userà un notebook in Azure Data Studio sia per Python che per SQL. Per altre informazioni sui notebook in Azure Data Studio, vedere [Come usare i notebook in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* Pacchetti Python aggiuntivi: gli esempi in questa serie di esercitazioni usano pacchetti Python che potrebbero essere installati o meno.

  Aprire un **prompt dei comandi** e passare al percorso di installazione per la versione di Python usata in Azure Data Studio. Ad esempio: `cd %LocalAppData%\Programs\Python\Python37-32`. Eseguire quindi i comandi seguenti per installare uno di questi pacchetti non ancora installati.

  ```console
  pip install matplotlib
  pip install pandas
  pip install pyodbc
  pip install scipy
  pip install sklearn
  ```

## <a name="restore-the-sample-database"></a>Ripristinare il database di esempio

Il set di dati di esempio usato in questa esercitazione è stato salvato in un file di backup del database con estensione **bak** che è possibile scaricare e usare. Questo set di dati deriva dal set di dati [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp) fornito dal [Transaction Processing Performance Council (TPC)](http://www.tpc.org/).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Se si usa Machine Learning Services in cluster Big Data, vedere l'articolo su come [ripristinare un database nell'istanza master di un cluster Big Data di SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Scaricare il file [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Seguire le istruzioni in [Ripristinare un database da un file di backup](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) in Azure Data Studio usando i dettagli seguenti:

   * Importare i dati dal file **tpcxbb_1gb.bak** scaricato
   * Assegnare al database di destinazione il nome "tpcxbb_1gb"

1. È possibile verificare che il set di dati esista dopo il ripristino del database eseguendo una query sulla tabella **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Scaricare il file [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Seguire le istruzioni in [Ripristinare un database in un'istanza gestita](/azure/sql-database/sql-database-managed-instance-get-started-restore) in SQL Server Management Studio, usando i dati seguenti:

   * Importare i dati dal file **tpcxbb_1gb.bak** scaricato
   * Assegnare al database di destinazione il nome "tpcxbb_1gb"

1. È possibile verificare che il set di dati esista dopo il ripristino del database eseguendo una query sulla tabella **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare con questa esercitazione, eliminare il database tpcxbb_1gb.

## <a name="next-steps"></a>Passaggi successivi

Nella prima parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Ripristinare un database di esempio

Per preparare i dati per il modello di Machine Learning, seguire la seconda parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione su Python: Preparare i dati per eseguire il clustering](python-clustering-model-prepare-data.md)