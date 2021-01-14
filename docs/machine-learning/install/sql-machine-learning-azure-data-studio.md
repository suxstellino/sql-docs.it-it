---
title: Notebook di Azure Data Studio (Python, R)
description: Informazioni su come eseguire gli script Python e R in un notebook in Azure Data Studio con SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/09/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: b1e3d81e0229d028b9fab6a4521ee11cb20ce1b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091550"
---
# <a name="run-python-and-r-scripts-in-azure-data-studio-notebooks-with-sql-server-machine-learning-services"></a>Eseguire script Python e R in notebook di Azure Data Studio con SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Informazioni su come eseguire script Python e R in notebook di [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Azure Data Studio è uno strumento per database multipiattaforma.

## <a name="prerequisites"></a>Prerequisiti

- [Scaricare e installare Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) nel computer in uso. Azure Data Studio è multipiattaforma e può essere eseguito in Windows, macOS e Linux.

- Un server con Machine Learning Services per SQL Server installato e abilitato. È possibile usare Machine Learning Services in cluster Windows, Linux o Big Data:

  - [Installare Machine Learning Services per SQL Server in Windows](sql-machine-learning-services-windows-install.md).
  - [Installare Machine Learning Services per SQL Server in Linux](../../linux/sql-server-linux-setup-machine-learning.md).
  - [Eseguire script Python e R con Machine Learning Services in cluster Big Data di SQL Server](../../big-data-cluster/machine-learning-services.md).

## <a name="create-a-sql-notebook"></a>Creare un notebook SQL

> [!IMPORTANT]
> Machine Learning Services viene eseguito come parte di SQL Server. Pertanto, è necessario usare un kernel SQL e non un kernel Python.

È possibile usare Machine Learning Services in Azure Data Studio con un notebook SQL. Per creare un nuovo notebook, seguire questa procedura:

1. Fare clic su **File** e **Nuovo notebook** per creare un nuovo notebook. Il notebook userà per impostazione predefinita il **kernel SQL**.

1. Fare clic su **Collega a** e **Cambia connessione**. 

    > [!div class="mx-imgBorder"]
    > ![Modificare la connessione per il notebook SQL di Azure Data Studio](media/ads-attach-to-connection.png)
    
1. Connettersi a un'istanza di SQL Server nuova o esistente. È possibile:

    1. Scegliere una connessione esistente in **Connessioni recenti** o **Connessioni salvate**.

    1. Creare una nuova connessione in **Dettagli connessione**. Inserire i dettagli della connessione per SQL Server e il database.

    > [!div class="mx-imgBorder"]
    > ![Dettagli della connessione per il notebook SQL di Azure Data Studio](media/ads-connection-details.png)  

## <a name="run-python-or-r-scripts"></a>Eseguire script Python o R

I notebook SQL sono costituiti da celle di codice e di testo. Le celle di codice vengono usate per eseguire script Python o R tramite la stored procedure [sp_execute_external_scripts](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Le celle di testo possono essere usate per documentare il codice nel notebook.

### <a name="run-a-python-script"></a>Eseguire uno script Python

Per eseguire uno script Python, seguire questa procedura:

1. Fare clic su **+ Codice** per aggiungere una cella di codice.

    > [!div class="mx-imgBorder"]
    > ![Aggiungere un blocco di codice per notebook SQL di Azure Data Studio](media/ads-add-code.png)  

1. Immettere lo script seguente nella cella di codice:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Fare clic su **Esegui cella** (la freccia rotonda nera) oppure premere **F5** per eseguire la singola cella.

    > [!div class="mx-imgBorder"]
    > ![Eseguire codice Python per notebook SQL di Azure Data Studio](media/ads-run-python.png)  

1. Il risultato verrà visualizzato nella cella di codice.

    > [!div class="mx-imgBorder"]
    > ![Output del codice Python per notebook SQL di Azure Data Studio](media/ads-run-python-output.png)  

### <a name="run-an-r-script"></a>Eseguire uno script R

Per eseguire uno script R, seguire questa procedura:

1. Fare clic su **+ Codice** per aggiungere una cella di codice.

    > [!div class="mx-imgBorder"]
    > ![Aggiungere un blocco di codice per notebook SQL di Azure Data Studio](media/ads-add-code.png)  

1. Immettere lo script seguente nella cella di codice:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Fare clic su **Esegui cella** (la freccia rotonda nera) oppure premere **F5** per eseguire la singola cella.

    > [!div class="mx-imgBorder"]
    > ![Eseguire codice R per notebook SQL di Azure Data Studio](media/ads-run-r.png)  

1. Il risultato verrà visualizzato nella cella di codice.

    > [!div class="mx-imgBorder"]
    > ![Output del codice R per notebook SQL di Azure Data Studio](media/ads-run-r-output.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Come usare i notebook in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md)
- [Creare ed eseguire un notebook di SQL Server](../../azure-data-studio/notebooks/notebooks-sql-kernel.md)
- [Avvio rapido: Eseguire semplici script Python con Machine Learning Services per SQL Server](../tutorials/quickstart-python-create-script.md)
- [Avvio rapido: Eseguire semplici script R con Machine Learning Services per SQL Server](../tutorials/quickstart-r-create-script.md)