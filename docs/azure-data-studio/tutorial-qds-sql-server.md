---
title: Abilitare il widget di esempio per le cinque query più lente
description: Questa esercitazione illustra come abilitare il widget di esempio per le cinque query più lente nel dashboard del database.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu; sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 06/01/2020
ms.openlocfilehash: 6b8632f9ff5c07cb23a296b98a5933639abee4d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048141"
---
# <a name="tutorial-add-the-five-slowest-queries-sample-widget-to-the-database-dashboard"></a>Esercitazione: Aggiungere il widget di esempio per le *cinque query più lente* al dashboard del database

Questa esercitazione illustra il processo di aggiunta di uno dei widget di esempio incorporati di Azure Data Studio al *dashboard del database* per visualizzare rapidamente le cinque query più lente di un database. Si apprenderà anche come visualizzare i dettagli sulle query lente e i piani di query usando le funzionalità di Azure Data Studio. In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Abilitare Query Store in un database
> * Aggiungere un widget di informazioni dettagliate predefinito al dashboard del database
> * Visualizzare i dettagli sulle query più lente del database
> * Visualizzare i piani di esecuzione query per le query lente

Azure Data Studio include diversi widget di informazioni dettagliate predefiniti. Questa esercitazione illustra come aggiungere il widget *query-data-store-db-insight*, ma i passaggi sono fondamentalmente gli stessi per aggiungere qualsiasi widget.

## <a name="prerequisites"></a>Prerequisiti

Per questa esercitazione è necessario il database di SQL Server o il database SQL di Azure *TutorialDB*. Per creare il database *TutorialDB*, completare uno degli argomenti di avvio rapido seguenti:

* [Connettersi ed eseguire query in SQL Server con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)

* [Connettersi ed eseguire query nel database SQL di Azure con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)

## <a name="turn-on-query-store-for-your-database"></a>Attivare Query Store per il database

Il widget in questo esempio richiede l'abilitazione di *Query Store*.

1. Fare clic con il pulsante destro del mouse sul database **TutorialDB** nella barra laterale **SERVER** e scegliere **Nuova query**.

2. Incollare l'istruzione Transact-SQL seguente nell'editor di query e fare clic su **Esegui**:

   ```sql
    ALTER DATABASE TutorialDB SET QUERY_STORE = ON
   ```

## <a name="add-the-slow-queries-widget-to-your-database-dashboard"></a>Aggiungere il widget per le query lente al dashboard del database

Per aggiungere il *widget per le query lente* al dashboard, modificare l'impostazione *dashboard.database.widgets* nel file delle *impostazioni utente*.

1. Aprire le *impostazioni utente* premendo **CTRL+MAIUSC+P** e aprire il *riquadro comandi*.

2. Digitare *impostazioni* nella casella di ricerca e selezionare **Preferenze: Apri impostazioni utente**.

   ![Comando Apri impostazioni utente](./media/tutorial-qds-sql-server/open-user-settings.png)

3. Digitare *dashboard* nella casella di ricerca di impostazioni e individuare **dashboard.database.widgets**, quindi fare clic su *Modifica in settings.json*.

   ![Ricerca impostazioni](./media/tutorial-qds-sql-server/search-settings.png)

4. In settings.json aggiungere il codice seguente:

   ```json
   "dashboard.database.widgets": [
       {
           "name": "slow queries widget",
           "gridItemConfig": {
               "sizex": 2,
               "sizey": 1
           },
           "widget": {
               "query-data-store-db-insight": null
           }
       },
       {
           "name": "Tasks",
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 1
           },
           "widget": {
               "tasks-widget": {}
           }
       },
       {
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 2
           },
           "widget": {
               "explorer-widget": {}
           }
       }
   ]
   ```

5. Premere **CTRL + S** per salvare le **Impostazioni utente** modificate.

6. Aprire il *dashboard del database* passando a **TutorialDB** nella barra laterale **SERVER**, fare clic con il pulsante destro del mouse e scegliere **Gestisci**.

   ![Apertura del dashboard](./media/tutorial-qds-sql-server/insight-open-dashboard.png)

7. Il widget di informazioni dettagliate compare nel dashboard:

   ![Widget QDS](./media/tutorial-qds-sql-server/insight-qds-result.png)

## <a name="view-insight-details-for-more-information"></a>Visualizzare i dettagli delle informazioni dettagliate

1. Per visualizzare informazioni aggiuntive per un widget di informazioni dettagliate, fare clic sui puntini di sospensione ( **...** ) in alto a destra e selezionare **Mostra dettagli**.

2. Per mostrare altri dettagli relativi a un elemento, selezionare un elemento nell'elenco **Dati grafico**.

   ![Finestra di dialogo dei dettagli delle informazioni dettagliate](./media/tutorial-qds-sql-server/insight-details-dialog.png)

3. Chiudere il riquadro **Informazioni dettagliate**.

## <a name="view-the-query-plan"></a>Visualizzare il piano di query

1. Fare clic con il pulsante destro del mouse sul database **TutorialDB**, quindi scegliere *Gestisci*.

2. Dal *widget per le query lente*: per visualizzare informazioni aggiuntive per un widget di informazioni dettagliate, fare clic sui puntini di sospensione ( **...** ) in alto a destra e selezionare **Esegui query**.

    ![Eseguire la query](media/tutorial-qds-sql-server/run-query.png)

3. A questo punto dovrebbe essere visualizzata una nuova finestra di query con i risultati.

    ![Risultati di Esegui query](media/tutorial-qds-sql-server/run-query-results.png)

4. Fare clic su **Spiega**.

   ![Spiegazione delle informazioni dettagliate QDS](./media/tutorial-qds-sql-server/insight-qds-explain.png)

5. Visualizzare il piano di esecuzione della query:

   ![Screenshot che mostra il piano di esecuzione della query.](./media/tutorial-qds-sql-server/showplan.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> * Abilitare Query Store in un database
> * Aggiungere un widget di informazioni dettagliate al dashboard del database
> * Visualizzare i dettagli sulle query più lente del database
> * Visualizzare i piani di esecuzione query per le query lente

Per informazioni su come abilitare il widget di informazioni dettagliate di esempio per lo **spazio utilizzato dalle tabelle**, completare l'esercitazione successiva:

> [!div class="nextstepaction"]
> [Abilitare il widget di informazioni dettagliate di esempio sullo spazio utilizzato dalle tabelle](tutorial-table-space-sql-server.md)