---
title: 'Esercitazione: Sviluppare un modello predittivo in R'
titleSuffix: SQL machine learning
description: Nella quarta parte di questa serie di esercitazioni si svilupperanno i dati per eseguire il training di un modello predittivo in R con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 12441de331661e5edbf40ca678963b80a70764a2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101855"
---
# <a name="tutorial-develop-a-predictive-model-in-r-with-sql-machine-learning"></a>Esercitazione: Sviluppare un modello predittivo in R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In questa serie di esercitazioni in quattro parti si useranno R e un modello di Machine Learning in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) oppure in [cluster Big Data](../../big-data-cluster/machine-learning-services.md) per stimare il numero di noleggi di sci.
::: moniker-end
::: moniker range="=sql-server-2017"
In questa serie di esercitazioni in quattro parti si useranno R e un modello di Machine Learning in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) per stimare il numero di noleggi di sci.
::: moniker-end
::: moniker range="=sql-server-2016"
In questa serie di esercitazioni in quattro parti si useranno R e un modello di Machine Learning in [R Services per SQL Server](../r/sql-server-r-services.md) per stimare il numero di noleggi di sci.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In questa serie di esercitazioni in quattro parti si useranno R e un modello di Machine Learning in [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) per stimare il numero di noleggi di sci.
::: moniker-end

Si supponga di essere i proprietari di un'agenzia di noleggio di sci e di voler stimare il numero di noleggi richiesti in una data futura. Queste informazioni consentiranno di preparare correttamente le scorte, il personale e le strutture.

Nella prima parte di questa serie verranno configurati i prerequisiti. Nelle seconda e nella terza parte si svilupperanno alcuni script R in un notebook per preparare i dati ed eseguire il training di un modello di Machine Learning. Nella terza parte verranno quindi eseguiti gli script R nel database mediante stored procedure T-SQL.

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Ripristinare un database di esempio 

Nella [seconda parte](r-predictive-model-prepare-data.md) si apprenderà come caricare i dati da un database in un frame di dati Python e come preparare i dati in R.

Nella [terza parte](r-predictive-model-train.md) si apprenderà come eseguire il training di un modello di Machine Learning in R.

Nelle [quarta parte](r-predictive-model-deploy.md) si apprenderà come archiviare il modello in un database e quindi creare le stored procedure dagli script R sviluppati nella seconda e nella terza parte. Le stored procedure verranno quindi eseguite sul server per eseguire stime basate sui nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* Machine Learning Services per SQL Server - Per installare Machine Learning Services, vedere la [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md) o la [guida all'installazione di Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). È anche possibile [abilitare Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* Machine Learning Services per SQL Server - Per installare Machine Learning Services, vedere la [guida all'installazione di Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
* R Services per SQL Server 2016. Per installare R Services, vedere la [guida all'installazione di Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services per Istanza gestita di SQL di Azure. Per informazioni, vedere [Panoramica di Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) per il ripristino del database di esempio in Istanza gestita di SQL di Azure.
::: moniker-end

* IDE R - Questa esercitazione usa [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* RODBC - Questo driver viene usato negli script R che verranno sviluppati in questa esercitazione. Se non è già installato, eseguirne l'installazione usando il comando R `install.packages("RODBC")`. Per altre informazioni su RODBC, vedere [CRAN - Pacchetto RODBC](https://CRAN.R-project.org/package=RODBC).

* Strumento di query SQL: questa esercitazione presuppone che si usi [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Per altre informazioni, vedere [Come usare i notebook in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

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
* È stato ripristinato un database di esempio

Per preparare i dati per il modello di Machine Learning, seguire la seconda parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Preparare i dati per eseguire il training di un modello predittivo in R](r-predictive-model-prepare-data.md)