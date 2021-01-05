---
title: 'Avvio rapido: Connettersi ed eseguire query in SQL Server'
description: Eseguire una guida di avvio rapido che illustra come usare Azure Data Studio per connettersi a SQL Server e quindi usare istruzioni Transact-SQL (T-SQL) per creare un database.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 08/02/2019
ms.openlocfilehash: 6a637791eac7858e905f766126ca93d9277b65c1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637627"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-sql-server"></a>Avvio rapido: Usare Azure Data Studio per connettersi a SQL Server ed eseguire query

Questo avvio rapido illustra come usare Azure Data Studio per connettersi a SQL Server e quindi usare istruzioni Transact-SQL (T-SQL) per creare il database *TutorialDB* usato nelle esercitazioni di Azure Data Studio.

## <a name="prerequisites"></a>Prerequisiti

Per completare questo avvio rapido, sono necessari Azure Data Studio e l'accesso a SQL Server.

- [Installare Azure Data Studio](./download-azure-data-studio.md).

Se non si ha accesso a SQL Server, selezionare la piattaforma dai collegamenti seguenti. Assicurarsi di conoscere l'account di accesso e la password SQL:

- [Windows - Scaricare SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)
- [macOS - Scaricare SQL Server 2017 in Docker](../linux/quickstart-install-connect-docker.md)
- [Linux - Scaricare SQL Server 2017 Developer Edition](../linux/sql-server-linux-overview.md#install): è sufficiente seguire la procedura per *Creare ed eseguire query sui dati*.

## <a name="connect-to-a-sql-server"></a>Connettersi a SQL Server

1. Avviare **Azure Data Studio**.

2. La prima volta che si esegue Azure Data Studio viene visualizzata la pagina di **introduzione**. Se la pagina di **introduzione** non viene visualizzata, selezionare **Help** > **Introduzione**. Selezionare **Nuova connessione** per aprire il riquadro **Connessione**:

   ![Icona Nuova connessione](media/quickstart-sql-server/new-connection-icon.png)

3. Questo articolo usa l'account di *accesso SQL*, ma è supportata anche l'*autenticazione di Windows*. Compilare i campi come indicato di seguito:

   - **Nome server:** Immettere il nome del server qui. Ad esempio localhost.
   - **Tipo di autenticazione:** Account di accesso SQL
   - **Nome utente:** Nome utente di SQL Server
   - **Password:** Password di SQL Server
   - **Nome database:** \<Default\>
   - **Gruppo di server:** \<Default\>

   ![Nuova schermata di connessione](media/quickstart-sql-server/new-connection-screen.png)

## <a name="create-a-database"></a>Creazione di un database

Questa procedura consente di creare un database denominato **TutorialDB**:

1. Fare clic con il pulsante destro del mouse sul server, **localhost**, e scegliere **Nuova query**.

2. Incollare il frammento di codice seguente nella finestra di query e quindi selezionare **Esegui**.

    ```sql
    USE master
    GO
    IF NOT EXISTS (
     SELECT name
     FROM sys.databases
     WHERE name = N'TutorialDB'
    )
     CREATE DATABASE [TutorialDB];
    GO
    IF SERVERPROPERTY('ProductVersion') > '12'
     ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON;
    GO
    ```

   Dopo il completamento della query, nell'elenco di database viene incluso anche il nuovo database **TutorialDB**. Se il database non è visualizzato, fare clic con il pulsante destro del mouse sul nodo **Database** e selezionare **Aggiorna**.

   ![Creazione del database](media/quickstart-sql-server/create-database.png)

## <a name="create-a-table"></a>Creare una tabella

Si vuole creare una tabella nel database *TutorialDB*, ma l'editor di query è ancora connesso al database *master*.

1. Modificare il contesto di connessione in **TutorialDB**:

   ![Modifica del contesto](media/quickstart-sql-server/change-context.png)

2. Incollare il frammento di codice seguente nella finestra di query e fare clic su **Esegui**:

   > [!NOTE]
   > È possibile aggiungerlo alla query esistente nell'editor oppure sovrascriverla. Facendo clic su **Esegui** viene eseguita solo la query evidenziata. Se nessuna query risulta selezionata, facendo clic su **Esegui** vengono eseguite tutte le query nell'editor.

    ```sql
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
     DROP TABLE dbo.Customers;
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
     CustomerId int NOT NULL PRIMARY KEY, -- primary key column
     Name nvarchar(50) NOT NULL,
     Location nvarchar(50) NOT NULL,
     Email nvarchar(50) NOT NULL
    );
    GO
    ```

Dopo il completamento della query, nell'elenco di tabelle viene inclusa anche la nuova tabella **Clienti**. Potrebbe essere necessario fare clic con il pulsante destro del mouse sul nodo **TutorialDB > Tabelle** e selezionare **Aggiorna**.

## <a name="insert-rows"></a>Inserire righe

- Incollare il frammento di codice seguente nella finestra di query e fare clic su **Esegui**:

    ```sql
    -- Insert rows into table 'Customers'
    INSERT INTO dbo.Customers
     ([CustomerId], [Name], [Location], [Email])
    VALUES
     ( 1, N'Orlando', N'Australia', N''),
     ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
     ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
     ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
    GO
    ```

## <a name="view-the-data-returned-by-a-query"></a>Visualizzare i dati restituiti da una query

 - Incollare il frammento di codice seguente nella finestra di query e fare clic su **Esegui**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

   ![Selezione dei risultati](media/quickstart-sql-server/select-results.png)

## <a name="next-steps"></a>Passaggi successivi

Ora che è stata effettuata la connessione a SQL Server ed è stata eseguita una query, proseguire con l'[esercitazione sull'editor di codice](tutorial-sql-editor.md).