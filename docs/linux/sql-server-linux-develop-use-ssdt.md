---
title: Sviluppare e distribuire database do SQL Server per Linux | Microsoft Docs
description: SQL Server Data Tools con Visual Studio è un ambiente avanzato di sviluppo e gestione del ciclo di vita dei database per SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1e924704-e07c-4a8b-b243-8c1dd8cff0d3
ms.openlocfilehash: a7add65dbc62679a6a1183c7a3f6998de7a18cb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273092"
---
# <a name="use-visual-studio-to-create-databases-for-sql-server-on-linux"></a>Usare Visual Studio per creare database per SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

SQL Server Data Tools (SSDT) trasforma Visual Studio in un ambiente avanzato di sviluppo e gestione del ciclo di vita dei database per SQL Server in Linux. È possibile sviluppare, compilare, testare e pubblicare il database da un progetto incluso nel controllo del codice sorgente, esattamente come si sviluppa il codice dell'applicazione.

## <a name="install-visual-studio-and-sql-server-data-tools"></a>Installare Visual Studio e SQL Server Data Tools

1. [Scaricare e installare Visual Studio](https://visualstudio.microsoft.com/downloads/) se non è ancora installato nel computer Windows. Se non si ha una licenza di Visual Studio, è disponibile Visual Studio Community Edition, un IDE gratuito open source con funzionalità complete per studenti e singoli sviluppatori.

2. Durante l'installazione di Visual Studio, impostare l'opzione **Scegliere il tipo di installazione** su **Personalizzata**. Scegliere **Avanti**

3. Selezionare **Microsoft SQL Server Data Tools**, **GIT per Windows** ed **Estensione GitHub per Visual Studio** nell'elenco di selezione delle funzionalità.

   <img src="./media/sql-server-linux-develop-use-ssdt/ssdt-setup.png" alt="ssdt setup" style="width: 400px;"/>

4. Continuare e completare l'installazione di Visual Studio. L'operazione può richiedere alcuni minuti.

## <a name="upgrade-sql-server-data-tools-to-ssdt-170-rc-release"></a>Aggiornare SQL Server Data Tools alla versione SSDT 17.0 RC

SQL Server in Linux è supportato da SSDT 17.0 RC o versione successiva.

* [Scaricare e installare SSDT 17.0 RC2](https://go.microsoft.com/fwlink/?linkid=837939).

## <a name="create-a-new-database-project-in-source-control"></a>Creare un nuovo progetto di database nel controllo del codice sorgente

1. Avviare Visual Studio.

2. Scegliere **Team Explorer** dal menu **Visualizza**. 

3. Fare clic su **Nuovo** nella sezione **Repository Git locale** nella pagina **Connetti**.

   <img src="./media/sql-server-linux-develop-use-ssdt/git-repository.png" alt="Screenshot of the Local Git Repository section with the New option called out." style="width: 300px;"/>

4. Fare clic su **Crea**. Dopo la creazione del repository Git locale, fare doppio clic su **SSDTRepo**.

5. Fare clic su **Nuovo** nella sezione **Soluzioni**. Selezionare **SQL Server** nel nodo **Altre lingue** nella finestra di dialogo **Nuovo progetto**.

   <img src="./media/sql-server-linux-develop-use-ssdt/new-project.png" alt="Screenshot of the Solutions section with the New option and SQL Server option called out." style="width: 480px;"/>

6. Immettere il nome **TutorialDB** e fare clic su **OK** per creare un nuovo progetto di database.

## <a name="create-a-new-table-in-the-database-project"></a>Creare una nuova tabella nel progetto di database

1. Scegliere **Esplora soluzioni** dal menu **Visualizza**.

2. Fare clic con il pulsante destro del mouse su **TutorialDB** in Esplora soluzioni per aprire il menu del progetto di database.

3. Selezionare **Tabella** in **Aggiungi**.

   <img src="./media/sql-server-linux-develop-use-ssdt/create-table.png" alt="create table" style="width: 480px;"/>

4. Usando la finestra di progettazione tabelle, aggiungere due colonne, Name `nvarchar(50)` e Location `nvarchar(50)`, come illustrato nell'immagine. SSDT genera lo script `CREATE TABLE` quando si aggiungono le colonne nella finestra di progettazione.

   <img src="./media/sql-server-linux-develop-use-ssdt/add-columns.png" alt="Screenshot of the table designer with the Name and Location values called out." style="width: 480px;"/>

5. Salvare il file **Table1.sql**.

## <a name="build-and-validate-the-database"></a>Compilare e convalidare il database

1. Aprire il menu del progetto di database in **TutorialDB** e scegliere **Compila**. SSDT compila i file del codice sorgente con estensione sql nel progetto e crea un file di pacchetto di applicazione livello dati, che può essere usato per pubblicare un database nell'istanza di SQL Server in Linux. 

   <img src="./media/sql-server-linux-develop-use-ssdt/build.png" alt="Screenshot showing the TutorialDB with the Build option called out." style="width: 400px;"/>

2. Controllare il messaggio di operazione di compilazione riuscita nella finestra **Output** di Visual Studio. 

## <a name="publish-the-database-to-sql-server-instance-on-linux"></a>Pubblicare il database nell'istanza di SQL Server in Linux

1. Aprire il menu del progetto di database in **TutorialDB** e scegliere **Pubblica**.

2. Fare clic su **Modifica** per selezionare l'istanza di SQL Server in Linux.

   <img src="./media/sql-server-linux-develop-use-ssdt/publish-dialog.png" alt="publish dialog" style="width: 480px;"/>

3. Nella finestra di dialogo della connessione digitare l'indirizzo IP o il nome host dell'istanza di SQL Server in Linux, il nome utente e la password.

   <img src="./media/sql-server-linux-develop-use-ssdt/connection-dialog.png" alt="connection dialog" style="width: 400px;"/>

4. Fare clic sul pulsante **Pubblica** nella finestra di dialogo della pubblicazione.

5. Controllare lo stato della pubblicazione nella finestra **Operazioni con strumenti dati** .

6. Fare clic su **Visualizza risultati** o su **Visualizza script** per vedere i dettagli del risultato della pubblicazione del database in SQL Server in Linux.

   <img src="./media/sql-server-linux-develop-use-ssdt/publish-result.png" alt="publish result" style="width: 480px;"/>

È stato creato un nuovo database nell'istanza di SQL Server in Linux e sono stati illustrati i concetti di base relativi allo sviluppo di un database con un progetto di database incluso nel controllo del codice sorgente.

## <a name="next-steps"></a>Passaggi successivi

Se non si ha familiarità con T-SQL, vedere [Esercitazione: Scrittura di istruzioni Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).

Per altre informazioni sullo sviluppo di un database con SQL Data Tools, vedere gli articoli seguenti.

* [Scaricare e installare Visual Studio](https://www.visualstudio.com/downloads/)
* [Scaricare e installare SSDT](../ssdt/download-sql-server-data-tools-ssdt.md)
* [Documentazione di MSDN su SSDT](/previous-versions/sql/sql-server-data-tools/hh272686(v=vs.103))
* [Esercitazione: Scrittura di istruzioni Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
* [Guida di riferimento a Transact-SQL (Motore di database)](../t-sql/language-reference.md)