---
title: Creare un widget di informazioni dettagliate personalizzato
description: Questa esercitazione illustra come creare widget di informazioni dettagliate personalizzati e aggiungerli ai dashboard di server e database in Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 08/26/2020
ms.openlocfilehash: bd4d3c12ea7a56ceddec0ad23f7276a4be4b9f40
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039961"
---
# <a name="tutorial-build-a-custom-insight-widget"></a>Esercitazione: Creare un widget di informazioni dettagliate personalizzato

Questa esercitazione illustra come usare query di informazioni dettagliate per creare widget di informazioni dettagliate personalizzati.

In questa esercitazione verranno illustrate le procedure per:
> [!div class="checklist"]
> * Eseguire una query e visualizzarla in un grafico
> * Creare un widget di informazioni dettagliate personalizzato a partire dal grafico
> * Aggiungere il grafico a un dashboard di server o database
> * Aggiungere dettagli al widget di informazioni dettagliate personalizzato

## <a name="prerequisites"></a>Prerequisiti

Per questa esercitazione è necessario il database di SQL Server o il database SQL di Azure *TutorialDB*. Per creare il database *TutorialDB*, completare uno degli argomenti di avvio rapido seguenti:

- [Connettersi ed eseguire query in SQL Server con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Connettersi ed eseguire query nel database SQL di Azure con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="run-your-own-query-and-view-the-result-in-a-chart-view"></a>Eseguire una query e visualizzare il risultato in un grafico
In questo passaggio si eseguirà uno script SQL per applicare una query alle sessioni attive correnti.

1. Premere **CTRL + N** per aprire un nuovo editor. 

2. Modificare il contesto di connessione in **TutorialDB**.

3. Incollare la query seguente nell'editor di query:

   ```sql
   SELECT count(session_id) as [Active Sessions]
   FROM sys.dm_exec_sessions
   WHERE status = 'running'
   ```

4. Salvare la query nell'editor in un file \*.sql. Per questa esercitazione, salvare lo script come *activeSession.sql*.

5. Premere **F5** per eseguire la query.

6. Quando vengono visualizzati i risultati della query, fare clic su **View as Chart** (Visualizza come grafico) e quindi sulla scheda **Visualizzatore grafico**.

7. Impostare **Tipo di grafico** su **conteggio**. Con queste impostazioni viene generato un grafico di conteggio.

## <a name="add-the-custom-insight-to-the-database-dashboard"></a>Aggiungere informazioni dettagliate personalizzate al dashboard del database

1. Per aprire la configurazione del widget di informazioni dettagliate, fare clic su **Create Insight** (Crea informazioni dettagliate) nella scheda *Visualizzatore grafico*:

   ![configurazione](./media/tutorial-build-custom-insight-sql-server/create-insight.png)
   
2. Copiare la configurazione delle informazioni dettagliate (i dati JSON). 

3. Premere **CTRL + virgola** per aprire *Impostazioni utente*.

4. Digitare *dashboard* in *Impostazioni di ricerca*.

5. Fare clic su **Modifica** per *dashboard.database.widgets*.

   ![impostazioni del dashboard](./media/tutorial-build-custom-insight-sql-server/dashboard-settings.png)

6. Incollare i dati JSON della configurazione di informazioni dettagliate in *dashboard.database.widgets*. Le impostazioni del dashboard del database saranno disposte nel modo seguente:

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql"
                }
            }
        }
    ]
   ```

7. Salvare il file *Impostazioni utente* e aprire il dashboard del database *TutorialDB* per visualizzare il widget delle sessioni attive:

   ![Dashboard delle informazioni dettagliate delle sessioni attive](./media/tutorial-build-custom-insight-sql-server/insight-activesession-dashboard.png)

## <a name="add-details-to-custom-insight"></a>Aggiungere dettagli alle informazioni dettagliate personalizzate

1. Premere **CTRL + N** per aprire un nuovo editor.

2. Modificare il contesto di connessione in **TutorialDB**.

3. Incollare la query seguente nell'editor di query:

   ```sql
    SELECT session_id AS [SID], login_time AS [Login Time], host_name AS [Host Name], program_name AS [Program Name], login_name AS [Login Name]
    FROM sys.dm_exec_sessions
    WHERE status = 'running'
   ```

4. Salvare la query nell'editor in un file \*.sql. Per questa esercitazione, salvare lo script come *activeSessionDetail.sql*.

5. Premere **CTRL + virgola** per aprire *Impostazioni utente*.

6. Modificare il nodo *dashboard.database.widgets* esistente nel file delle impostazioni:

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql",
                    "details": {
                        "queryFile": "{your file folder}/activeSessionDetail.sql",
                        "label": "SID",
                        "value": "Login Name"
                    }
                }
            }
        }
    ]
   ```

7. Salvare il file *Impostazioni utente* e aprire il dashboard del database *TutorialDB*. Fare clic sul pulsante con i puntini di sospensione accanto a *My-Widget* per visualizzare i dettagli del widget:

    ![Informazioni dettagliate sulle sessioni attive](./media/tutorial-build-custom-insight-sql-server/insight-activesession-detail.png)

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> * Eseguire una query e visualizzarla in un grafico
> * Creare un widget di informazioni dettagliate personalizzato a partire dal grafico
> * Aggiungere il grafico a un dashboard di server o database
> * Aggiungere dettagli al widget di informazioni dettagliate personalizzato

Per informazioni su come eseguire il backup e ripristino di un database, completare l'esercitazione successiva:

> [!div class="nextstepaction"]
> [Eseguire il backup e ripristino dei database](tutorial-backup-restore-sql-server.md).
