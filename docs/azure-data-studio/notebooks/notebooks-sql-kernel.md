---
title: Notebook con kernel SQL in Azure Data Studio
description: Questa esercitazione illustra come è possibile creare ed eseguire un notebook di SQL Server.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray, alayu
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: d2d04be4d571b14445abfb638f070b3666ce9984
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040281"
---
# <a name="create-and-run-a-sql-server-notebook"></a>Creare ed eseguire un notebook di SQL Server

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come creare ed eseguire un notebook in Azure Data Studio con SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- [Azure Data Studio installato](../download-azure-data-studio.md)
- SQL Server installato
  - [Windows](../../database-engine/install-windows/install-sql-server.md)
  - [Linux](../../linux/sql-server-linux-setup.md)

## <a name="create-a--notebook"></a>Creare un notebook

Questa procedura illustra come creare un file di notebook in Azure Data Studio:

1. In Azure Data Studio connettersi all'istanza di SQL Server.

2. Selezionare la connessione in **Connessioni** nella finestra **Server**. Selezionare quindi **Nuovo notebook**.

3. Attendere che il **kernel** e il contesto di destinazione **(Connetti a**) siano popolati. Verificare che **Kernel** sia impostato su **SQL** e impostare **Collega a** per l'istanza di SQL Server (in questo esempio *localhost*).

   ![Imposta Kernel e Connetti a](media/notebooks-sql-kernel/set-kernel-and-attach-to.png)

È possibile salvare il notebook usando il comando **Salva** o **Salva con nome** del menu **File**.

Per aprire un notebook, è possibile usare il comando **Apri file** nel menu **File**, selezionare **Apri file** nella pagina di **benvenuto** o usare il comando **File: Apri** nel riquadro comandi.

## <a name="change-the-sql-connection"></a>Modificare la connessione SQL

Per modificare la connessione SQL per un notebook:

1. Selezionare il menu **Collega a** dalla barra degli strumenti del notebook e quindi selezionare **Cambia connessione**.

   ![Selezionare il menu Associa a sulla barra degli strumenti del notebook](./media/notebooks-sql-kernel/select-attach-to-1.png)

2. A questo punto è possibile selezionare un server di connessione recente oppure immettere i dettagli della nuova connessione per la connessione.

## <a name="run-a-code-cell"></a>Eseguire una cella di codice

È possibile creare celle contenenti codice SQL che è possibile eseguire sul posto facendo clic sul pulsante **Esegui cella** (la freccia rotonda nera) a sinistra della cella. I risultati verranno visualizzati nel notebook al termine dell'esecuzione della cella.

Ad esempio:

1. Aggiungere una nuova cella di codice selezionando il comando **+Codice** sulla barra degli strumenti.

   ![Barra degli strumenti del notebook](media/notebooks-guidance/notebook-toolbar.png)

2. Copiare e incollare l'esempio seguente nella cella e fare clic su **Esegui cella**. In questo esempio viene creato un nuovo database.

   ```sql
   USE master
   GO

   -- Drop the database if it already exists
   IF  EXISTS (
           SELECT name
           FROM sys.databases
           WHERE name = N'TestNotebookDB'
      )
   DROP DATABASE TestNotebookDB
   GO

   -- Create the database
   CREATE DATABASE TestNotebookDB
   GO
   ```

   ![Esegui cella](media/notebooks-sql-kernel/run-notebook-cell.png)

## <a name="save-the-result"></a>Salvare il risultato

Se si esegue uno script che restituisce un risultato, è possibile salvare il risultato in formati diversi usando la barra degli strumenti visualizzata sopra il risultato.

- Salva in formato CSV
- Salva in formato Excel
- Salva in formato JSON
- Salva in formato XML

Il codice seguente, ad esempio, restituisce il risultato [PI](../../t-sql/functions/pi-transact-sql.md).

```sql
SELECT PI() AS PI;
GO
```

![Salvare il risultato](media/notebooks-sql-kernel/run-notebook-cell-2.png)

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui notebook:

- [Come usare i notebook in Azure Data Studio](./notebooks-guidance.md)
- [Creare ed eseguire un notebook Python](././notebooks-python-kernel.md)
- [Eseguire un notebook di esempio con Spark](../../big-data-cluster/notebooks-tutorial-spark.md)