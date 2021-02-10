---
title: Abilitare il widget di informazioni dettagliate di esempio sullo spazio usato dalle tabelle
description: Questa esercitazione illustra come abilitare il widget di informazioni dettagliate di esempio sullo spazio usato dalle tabelle nel dashboard del database Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/10/2019
ms.openlocfilehash: 128b9e0b63cf006577250afe04e6a28c92379497
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048081"
---
# <a name="tutorial-enable-the-table-space-usage-sample-insight-widget-using-azure-data-studio"></a>Esercitazione: Abilitare il widget di informazioni dettagliate di esempio sull'utilizzo dello spazio da parte delle tabelle tramite Azure Data Studio

Questa esercitazione illustra come abilitare un widget di informazioni dettagliate nel dashboard del database, offrendo una vista immediata dell'uso dello spazio da parte di tutte le tabelle di un database. In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Attivare rapidamente un widget di informazioni dettagliate usando un widget di informazioni dettagliate predefinito
> * Visualizzare i dettagli relativi allo spazio usato dalle tabelle
> * Filtrare i dati e visualizzare i dettagli dell'etichetta in un grafico di informazioni dettagliate

## <a name="prerequisites"></a>Prerequisiti

Per questa esercitazione è necessario il database di SQL Server o il database SQL di Azure *TutorialDB*. Per creare il database *TutorialDB*, completare uno degli argomenti di avvio rapido seguenti:

* [Connettersi ed eseguire query in SQL Server con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
* [Connettersi ed eseguire query nel database SQL di Azure con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)

## <a name="turn-on-a-management-insight-on-azure-data-studios-database-dashboard"></a>Attivare le informazioni dettagliate sulla gestione nel dashboard del database di Azure Data Studio

Azure Data Studio include un widget di esempio predefinito per monitorare lo spazio usato dalle tabelle in un database.

1. Aprire le *impostazioni utente* premendo **CTRL+MAIUSC+P** e aprire il *riquadro comandi*.

2. Digitare *impostazioni* nella casella di ricerca e selezionare **Preferenze: Apri impostazioni utente**.

3. Digitare *dashboard* nella casella di input Ricerca impostazioni e individuare **dashboard.database.widgets**.

4. Per personalizzare le impostazioni di **dashboard.database.widgets**, è necessario modificare la voce **dashboard.database.widgets** nella sezione **IMPOSTAZIONI UTENTE**.

   ![Screenshot che mostra la sezione IMPOSTAZIONI UTENTE con la sezione Dashboard > Database Widgets evidenziata.](media/tutorial-table-space-sql-server/search-settings.png)

   Se nella sezione **IMPOSTAZIONI UTENTE** non è presente la voce **dashboard.database.widgets**, posizionare il puntatore del mouse sul testo **dashboard.database.widgets** nella colonna IMPOSTAZIONI PREDEFINITE, fare clic sull'icona a forma di *ingranaggio* che compare a sinistra del testo e quindi fare clic su **Copy as Setting JSON** (Copia come JSON impostazione). Se il popup indica **Sostituisci nelle impostazioni**, non fare clic su di esso. Passare alla colonna **IMPOSTAZIONI UTENTE** a destra, individuare la sezione **dashboard.database.widgets** e procedere al passaggio successivo.

5. Nella sezione **dashboard.database.widgets** aggiungere le righe seguenti:

   ```json
        {
            "name": "Space Used by Tables",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "table-space-db-insight": null
            }
        },
    ```

   La sezione **dashboard.database.widgets** dovrebbe essere simile all'immagine seguente:

    ![Screenshot del file settings.json con il primo oggetto della matrice dashboard.database.widgets.](./media/tutorial-table-space-sql-server/insight-table-space.png)

6. Premere **CTRL + S** per salvare le impostazioni.

7. Aprire il dashboard del database facendo clic con il pulsante destro del mouse su **TutorialDB** e scegliere **Gestisci**.

8. Visualizzare il widget di informazioni dettagliate sullo *spazio usato dalle tabelle*, come illustrato nell'immagine seguente:

   ![Widget](./media/tutorial-table-space-sql-server/insight-table-space-result.png)

## <a name="working-with-the-insight-chart"></a>Uso del grafico di informazioni dettagliate

Il grafico di informazioni dettagliate di Azure Data Studio fornisce dettagli relativi ai filtri e al passaggio del mouse. Per provare questa procedura:

1. Fare clic e attivare la legenda *row_count* sul grafico. Azure Data Studio visualizza e nasconde la serie di dati tramite l'attivazione e la disattivazione di una legenda.

2. Passare il puntatore del mouse sul grafico. Azure Data Studio visualizza altre informazioni sull'etichetta della serie di dati e il relativo valore, come illustrato nello screenshot seguente.

   ![attivazione e legenda grafico](./media/tutorial-table-space-sql-server/insight-table-space-toggle.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> * Attivare rapidamente un widget di informazioni dettagliate usando un widget di informazioni dettagliate predefinito.
> * Visualizzare i dettagli relativi allo spazio usato dalle tabelle.
> * Filtrare i dati e visualizzare i dettagli dell'etichetta in un grafico di informazioni dettagliate

Per informazioni su come creare un widget di informazioni dettagliate personalizzato, completare l'esercitazione seguente:

> [!div class="nextstepaction"]
> [Creare un widget di informazioni dettagliate personalizzato](tutorial-build-custom-insight-sql-server.md)
