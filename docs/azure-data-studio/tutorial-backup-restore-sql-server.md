---
title: Eseguire il backup e ripristinare un database
description: Seguire questa esercitazione per scoprire come eseguire il backup e il ripristino di database usando Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 11/04/2019
ms.openlocfilehash: 851fbdf950656f9edfba76e3d5a0aae54e8e3d99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048171"
---
# <a name="tutorial-back-up-and-restore-databases-using-azure-data-studio"></a>Esercitazione: Eseguire il backup e il ripristino di database con Azure Data Studio

In questa esercitazione si apprenderà come usare Azure Data Studio per:
> [!div class="checklist"]
> * Eseguire il backup di un database.
> * Visualizzare lo stato dell'operazione di backup.
> * Generare lo script usato per eseguire il backup.
> * Ripristinare un database.
> * Visualizzare lo stato dell'attività di ripristino.

## <a name="prerequisites"></a>Prerequisiti

Per questa esercitazione è necessario il database *TutorialDB* di SQL Server. Per creare il database TutorialDB, completare l'avvio rapido seguente:

* [Usare Azure Data Studio per connettersi a SQL Server ed eseguire query](quickstart-sql-server.md)

Questa esercitazione richiede la connessione a un database di SQL Server. Azure Data Studio non esegue il backup e ripristino di database SQL di Azure poiché in essi sono già previsti backup automatici. Per altre informazioni, vedere [Informazioni sui backup automatici del database SQL](/azure/sql-database/sql-database-automated-backups).

## <a name="back-up-a-database"></a>Eseguire il backup del database

1. Aprire il dashboard del database TutorialDB aprendo la barra laterale **SERVER**. Selezionare quindi **CTRL+G**, espandere **Database**, fare clic con il pulsante destro del mouse su **TutorialDB** e scegliere **Gestisci**.

1. Aprire la finestra di dialogo **Backup database**  selezionando **Backup** nel widget **Attività**.

   ![Screenshot che mostra il widget Attività.](./media/tutorial-backup-restore-sql-server/tasks.png)

1. In questa esercitazione vengono usate le opzioni di backup predefinite, quindi selezionare **Backup**.

   ![Screenshot che mostra la finestra di dialogo Backup.](./media/tutorial-backup-restore-sql-server/backup-dialog.png)

Dopo aver selezionato **Backup**, la finestra di dialogo **Backup database** scompare e viene avviato il processo di backup.

## <a name="view-the-backup-status-and-the-backup-script"></a>Visualizzare lo stato e lo script di backup

1. Viene visualizzato il riquadro **Cronologia attività**. In caso contrario, selezionare **CTRL+T** per aprirlo.

   ![Screenshot che mostra il riquadro Cronologia attività.](./media/tutorial-backup-restore-sql-server/task-history.png)

1. Per visualizzare lo script di backup nell'editor, fare clic con il pulsante destro del mouse su **Backup di database riuscito** e scegliere **Script**.

   ![Screenshot che mostra lo script di backup.](./media/tutorial-backup-restore-sql-server/task-script.png)

## <a name="restore-a-database-from-a-backup-file"></a>Ripristinare un database da un file di backup.

1. Aprire la barra laterale **SERVER** selezionando **CTRL+G**. Fare quindi clic con il pulsante destro del mouse sul server e scegliere **Gestisci**.

1. Aprire la finestra di dialogo **Ripristina database**  selezionando **Ripristina** nel widget **Attività**.

   ![Screenshot che mostra l'attività di ripristino.](media/tutorial-backup-restore-sql-server/tasks-restore.png)

1. Nella casella **Ripristina da** selezionare **File di backup**.

1. Selezionare i puntini di sospensione (...) nella casella **Percorso file di backup** e selezionare il file di backup più recente per *TutorialDB*.

1. Immettere **TutorialDB_Restored** nella casella **Database di destinazione** nella sezione **Destinazione** per ripristinare il file di backup in un nuovo database. Selezionare quindi **Ripristina**.

   ![Screenshot che mostra il ripristino del backup.](./media/tutorial-backup-restore-sql-server/restore.png)

1. Per visualizzare lo stato dell'operazione di ripristino, premere **CTRL+T** per aprire **Cronologia attività**.

   ![Screenshot che mostra la cronologia delle attività per il ripristino.](./media/tutorial-backup-restore-sql-server/task-history-restore.png)