---
title: 'Gestione: Notebook di Azure Data Studio'
titleSuffix: SQL Server Big Data Clusters
description: Usare un notebook di Azure Data Studio per gestire e risolvere problemi relativi a un cluster Big Data di SQL Server.
author: yualan
ms.author: alanyu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a526f4d1da472a8d5bae9c5bceb296978d24986b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235705"
---
# <a name="manage-sql-server-big-data-clusters-with-azure-data-studio-notebooks"></a>Gestire cluster Big Data di SQL Server con notebook di Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] fornisce un'estensione per Azure Data Studio contenente alcuni notebook. Un notebook fornisce codice e documentazione che è possibile usare in Azure Data Studio per gestire cluster Big Data di SQL Server 2019.

Originariamente implementati come progetto open source, i [notebook](../azure-data-studio/notebooks/notebooks-guidance.md) sono stati successivamente integrati in [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md). È possibile usare markdown per testo nelle celle di testo e uno dei kernel disponibili per scrivere codice nelle celle di codice.

È possibile usare notebook per distribuire cluster Big Data per [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

Oltre ai notebook, è possibile visualizzare anche una raccolta di notebook, denominata Jupyter Book. Un Jupyter Book costituisce un sommario per la consultazione di una raccolta di notebook, in modo da poter trovare facilmente il notebook necessario, ad esempio per risolvere un problema relativo a SQL Server o per visualizzare lo stato di un cluster.

## <a name="prerequisites"></a>Prerequisiti

Per l'apertura di un notebook sono necessari i prerequisiti seguenti:

* La versione più recente di [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
* L'estensione di [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], installata in Azure Data Studio

Oltre a questi prerequisiti, per distribuire cluster Big Data di SQL Server 2019, sono necessari anche:

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli)

## <a name="access-troubleshooting-notebooks"></a>Accedere ai notebook per la risoluzione dei problemi

Sono disponibili tre modi per accedere ai notebook per la risoluzione dei problemi.

### <a name="command-palette"></a>Riquadro comandi

1. Selezionare **Visualizza** > **Riquadro comandi**.

2. Immettere **Jupyter Books: SQL Server 2019 Guide**.

Si aprirà il viewlet dei Jupyter Book con il Book contenente il notebook per la risoluzione dei problemi relativi ai cluster Big Data di SQL Server.

### <a name="sql-master-dashboard"></a>Dashboard master di SQL

1. Dopo aver installato Azure Data Studio Insiders, connettersi all'istanza di un cluster Big Data di SQL Server.

2. Dopo aver eseguito la connessione all'istanza, fare clic con il pulsante destro del mouse sul nome del server in **CONNESSIONI** e selezionare **Gestisci**.

3. Nel dashboard selezionare **SQL Server Big Data Cluster** (Cluster Big Data di SQL Server). Selezionare **SQL Server 2019 guide** (Guida a SQL Server 2019) per aprire il Jupyter Book contenente i notebook necessari.
    ![Notebook Jupyter nel dashboard](media/manage-notebooks/jupyter-book-button.png)

4. Selezionare il notebook relativo all'attività da completare.

### <a name="controller-dashboard"></a>Dashboard del controller

1. Nella visualizzazione **Connessioni** espandere **SQL Server Big Data Clusters** (Cluster Big Data per SQL Server).

2. Aggiungere i dettagli dell'endpoint del controller.

3. Dopo aver eseguito la connessione al controller, fare clic con il pulsante destro del mouse sull'endpoint e selezionare **Gestisci**.

4. Al termine del caricamento del dashboard, selezionare **Risoluzione problemi** per aprire le guide alla risoluzione dei problemi del Jupyter Book.

## <a name="use-troubleshooting-notebooks"></a>Usare i notebook per la risoluzione dei problemi

1. Nel sommario del Jupyter Book trovare la guida alla risoluzione dei problemi necessaria.

2. I notebook sono ottimizzati, pertanto è sufficiente selezionare **Run Cells** (Esegui celle). Questa azione esegue singolarmente ogni cella del notebook fino al suo completamento.

3. Se viene rilevato un errore, il Jupyter Book suggerisce un notebook che è possibile eseguire per correggere l'errore. Seguire la procedura consigliata ed eseguire nuovamente il notebook.

## <a name="change-the-big-data-cluster"></a>Modificare il cluster Big Data

Per modificare il cluster Big Data di SQL Server per un notebook:

1. Fare clic sul menu **Associa a** sulla barra degli strumenti del notebook.

   ![Fare clic sul menu "Associa a" sulla barra degli strumenti del notebook](./media/notebooks-how-to-manage/select-attach-to-1.png)

2. Scegliere un server dal menu **Associa a**.

   ![Selezionare un server dal menu Associa a.](./media/notebooks-how-to-manage/select-attach-to-2.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui notebook in Azure Data Studio, vedere [Come usare i notebook in SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).