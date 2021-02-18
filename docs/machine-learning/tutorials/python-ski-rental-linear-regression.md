---
title: 'Esercitazione su Python: Noleggi di sci'
titleSuffix: SQL machine learning
description: In questa serie di esercitazioni in quattro parti si creerà un modello di regressione lineare in Python per stimare i noleggi di sci con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: bd650e17959f5f40bf35c700eb671e6748f1d78f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351485"
---
# <a name="python-tutorial-predict-ski-rental-with-linear-regression-with-sql-machine-learning"></a>Esercitazione su Python: Stimare il noleggio di sci con la regressione lineare con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In questa serie di esercitazioni in quattro parti si useranno Python e la regressione lineare in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) oppure in [cluster Big Data](../../big-data-cluster/machine-learning-services.md) per stimare il numero di noleggi di sci. Questa esercitazione usa un [notebook Python in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).
::: moniker-end
::: moniker range="=sql-server-2017"
In questa serie di esercitazioni in quattro parti si useranno Python e la regressione lineare in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) per stimare il numero di noleggi di sci. Questa esercitazione usa un [notebook Python in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In questa serie di esercitazioni in quattro parti si useranno Python e la regressione lineare in [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) per stimare il numero di noleggi di sci. Questa esercitazione usa un [notebook Python in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).
::: moniker-end

Si supponga di essere i proprietari di un'agenzia di noleggio di sci e di voler stimare il numero di noleggi richiesti in una data futura. Queste informazioni consentiranno di preparare correttamente le scorte, il personale e le strutture.

Nella prima parte di questa serie verranno configurati i prerequisiti. Nelle seconda e nella terza parte si svilupperanno alcuni script Python in un notebook per preparare i dati ed eseguire il training di un modello di Machine Learning. Nella terza parte verranno quindi eseguiti gli script Python nel database mediante stored procedure T-SQL.

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Importare un database di esempio

Nella [seconda parte](python-ski-rental-linear-regression-prepare-data.md) si apprenderà come caricare i dati da un database in un frame di dati Python e come preparare i dati in Python.

Nella [terza parte](python-ski-rental-linear-regression-train-model.md) si apprenderà come eseguire il training di un modello di regressione lineare in Python.

Nelle [quarta parte](python-ski-rental-linear-regression-deploy-model.md) si apprenderà come archiviare il modello in un database e quindi creare le stored procedure dagli script Python sviluppati nella seconda e nella terza parte. Le stored procedure verranno quindi eseguite sul server per eseguire stime basate sui nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* Machine Learning Services per SQL Server - Per installare Machine Learning Services, vedere la [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md) o la [guida all'installazione di Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). È anche possibile [abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* Machine Learning Services per SQL Server - Per installare Machine Learning Services, vedere la [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services di Istanza gestita di SQL di Azure - Per informazioni, vedere [Panoramica di Machine Learning Services di Istanza gestita SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) per il ripristino del database di esempio in Istanza gestita di SQL di Azure.
::: moniker-end

* IDE Python: questa esercitazione usa un notebook Python in [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Per altre informazioni, vedere [Come usare i notebook in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* Strumento di query SQL: questa esercitazione presuppone che si usi [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* Pacchetti Python aggiuntivi: gli esempi in questa serie di esercitazioni usano i seguenti pacchetti Python, che potrebbero essere installati o meno per impostazione predefinita:

  * pandas
  * pyodbc
  * sklearn

  Per installare questi pacchetti:
  1. Nel notebook di Azure Data Studio selezionare **Gestisci pacchetti**.
  2. Nel riquadro **Gestisci pacchetti** selezionare la scheda **Aggiungi nuovo**.
  3. Per ognuno dei seguenti pacchetti immettere il nome del pacchetto, fare clic su **Cerca**, quindi fare clic su **Installa**.

  In alternativa è possibile aprire un **prompt dei comandi**, passare al percorso di installazione della versione di Python usata in Azure Data Studio (ad esempio `cd %LocalAppData%\Programs\Python\Python37-32`), quindi eseguire `pip install` per ogni pacchetto.

## <a name="restore-the-sample-database"></a>Ripristinare il database di esempio

Il database di esempio usato in questa esercitazione è stato salvato in un file di backup del database con estensione **bak** che è possibile scaricare e usare.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Se si usa Machine Learning Services in cluster Big Data, vedere l'articolo su come [ripristinare un database nell'istanza master di un cluster Big Data di SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Scaricare il file [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Seguire le istruzioni in [Ripristinare un database da un file di backup](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) in Azure Data Studio usando i dettagli seguenti:

   * Importare i dati dal file **TutorialDB.bak** scaricato
   * Assegnare al database di destinazione il nome "TutorialDB"

1. È possibile verificare che il database ripristinato esista eseguendo una query sulla tabella **dbo.rental_data**:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Scaricare il file [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Seguire le istruzioni in [Ripristinare un database in un'istanza gestita](/azure/sql-database/sql-database-managed-instance-get-started-restore) in SQL Server Management Studio, usando i dati seguenti:

   * Importare i dati dal file **TutorialDB.bak** scaricato
   * Assegnare al database di destinazione il nome "TutorialDB"

1. È possibile verificare che il database ripristinato esista eseguendo una query sulla tabella **dbo.rental_data**:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare con questa esercitazione, eliminare il database TutorialDB.

## <a name="next-steps"></a>Passaggi successivi

Nella prima parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Sono stati installati i prerequisiti
* Importare un database di esempio

Per preparare i dati dal database TutorialDB, seguire la seconda parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione su Python: Preparare i dati per eseguire il training di un modello di regressione lineare](python-ski-rental-linear-regression-prepare-data.md)