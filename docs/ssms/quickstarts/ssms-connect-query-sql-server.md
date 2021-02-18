---
title: Connettersi a un'istanza di SQL Server ed eseguire query con SQL Server Management Studio (SSMS)
description: Connettersi a un'istanza di SQL Server in SSMS. Creare ed eseguire query in un database di SQL Server in SSMS che esegue query T-SQL di base.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 92160da1da48fc107be98354250e4c580cb51155
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350589"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-using-sql-server-management-studio-ssms"></a>Avvio rapido: Connettersi a un'istanza di SQL Server ed eseguire query con SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Informazioni introduttive sull'uso di SQL Server Management Studio (SSMS) per connettersi a un'istanza di SQL Server ed eseguire alcuni comandi Transact-SQL (T-SQL) di base.

Questo articolo illustra come seguire i passaggi seguenti:

> [!div class="checklist"]
> - Connessione a un'istanza di SQL Server
> - Creazione di un database
> - Creare una tabella nel nuovo database
> - Inserire righe nella nuova tabella
> - Eseguire query nella tabella e visualizzare i risultati
> - Usare la tabella della finestra di query per verificare le proprietà di connessione

## <a name="prerequisites"></a>Prerequisiti

- [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md) installato.
- Un'[istanza di SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) installata e configurata.

## <a name="connect-to-a-sql-server-instance"></a>Connessione a un'istanza di SQL Server

1. Avviare SQL Server Management Studio. Quando si esegue SSMS per la prima volta viene visualizzata la finestra di dialogo **Connetti al server**. Se la finestra non si apre è possibile aprirla manualmente selezionando **Esplora oggetti** > **Connetti** > **Motore di database**.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-object-explorer.png" alt-text="Collegamento Connetti in Esplora oggetti":::

2. Viene visualizzata la finestra di dialogo **Connetti al server** . Immettere le seguenti informazioni:

    |   Impostazione   |   Valori consigliati   |   Descrizione   |
    |--------------|-----------------------|-----------------|
    | **Tipo di server** | Motore di database | In **Tipo di server** selezionare **Motore di database** (in genere l'opzione predefinita). |
    | **Nome server** | Nome completo del server | Per **Nome server** immettere il nome dell'istanza di SQL Server (è anche possibile usare *localhost* come nome del server se ci si connette in locale). Se non si usa l'istanza predefinita- ***MSSQLSERVER*** , è necessario immettere il nome del server e il nome dell'istanza. </br> </br> Se non si sa come determinare il nome dell'istanza di SQL Server, vedere [Suggerimenti e consigli per l'uso di SSMS](../tutorials/ssms-tricks.md#find-sql-server-instance-name). |
    | **autenticazione** | Autenticazione di Windows </br> </br> Autenticazione di SQL Server | L'autenticazione di Windows viene impostata come predefinita. </br> </br> È anche possibile usare l'**autenticazione di SQL Server** per la connessione. Tuttavia, se si seleziona **Autenticazione di SQL Server**, sono necessari un nome utente e una password. </br> </br> Per altre informazioni sui tipi di autenticazione, vedere [Connetti al server (motore di database)](../f1-help/connect-to-server-database-engine.md). |
    | **Accesso** | ID utente dell'account server | ID utente dell'account server usato per accedere al server. Un account di accesso è obbligatorio quando si usa l'**autenticazione di SQL Server**. |
    | **Password** | Password dell'account server | Password dell'account server usato per accedere al server. Una password è obbligatoria quando si usa l'**autenticazione di SQL Server**. |

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-to-sql-server-object-explorer.png" alt-text="Campo del nome del server per SQL Server":::

3. Dopo aver completato tutti i campi selezionare **Connetti**.

    È anche possibile modificare altre opzioni di connessione selezionando **Opzioni**. Sono esempi di opzioni di connessione il database al quale ci si connette, il valore di timeout della connessione e il protocollo di rete. In questo articolo vengono usati i valori predefiniti per tutti i campi.

4. Per verificare l'esito positivo della connessione a SQL Server, espandere ed esplorare gli oggetti all'interno di **Esplora oggetti** in cui vengono visualizzati il nome del server, la versione di SQL Server e il nome utente. Questi oggetti sono diversi a seconda del tipo di server.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-on-prem.png" alt-text="Connessione a un server locale":::

## <a name="troubleshoot-connectivity-issues"></a>Risolvere i problemi di connettività

Per esaminare le tecniche di risoluzione dei problemi da usare quando non è possibile connettersi a un'istanza del motore di database di SQL Server in un singolo server, vedere [Risolvere i problemi di connessione al motore di database di SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).

## <a name="create-a-database"></a>Creazione di un database

Verrà ora creato un database denominato TutorialDB seguendo questa procedura:

1. Fare clic con il pulsante destro del mouse in Esplora oggetti e scegliere **Nuova query**:

   :::image type="content" source="media/ssms-connect-query-sql-server/new-query.png" alt-text="Collegamento Nuova query":::

2. Incollare il frammento di codice T-SQL seguente nella finestra di query:

    ```sql
    USE master
    GO
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
   ```

3. Eseguire la query selezionando **Esegui** o premendo F5 sulla tastiera.

   :::image type="content" source="media/ssms-connect-query-sql-server/execute.png" alt-text="Comando Esegui":::
  
    Al termine della query il nuovo database TutorialDB viene visualizzato nell'elenco dei database in Esplora oggetti. Se il database non viene visualizzato, fare clic con il pulsante destro del mouse sul nodo **Database** e selezionare **Aggiorna**.

## <a name="create-a-table-in-the-new-database"></a>Creare una tabella nel nuovo database

In questa sezione si crea una tabella nel database TutorialDB appena creato. L'editor di query è ancora nel contesto del database *master*. Cambiare il contesto e impostare la connessione al database *TutorialDB* seguendo questa procedura:

1. Nella casella di riepilogo a discesa dei database selezionare il database desiderato, come indicato di seguito:

   :::image type="content" source="media/ssms-connect-query-sql-server/change-db.png" alt-text="Modificare il database":::

2. Incollare il frammento di codice T-SQL seguente nella finestra di query:

    ```sql
    USE [TutorialDB]
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
    DROP TABLE dbo.Customers
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
       CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
       Name      [NVARCHAR](50)  NOT NULL,
       Location  [NVARCHAR](50)  NOT NULL,
       Email     [NVARCHAR](50)  NOT NULL
    );
    GO
    ```

3. Eseguire la query selezionando **Esegui** o premendo F5 sulla tastiera.

Al termine della query la nuova tabella Customers viene visualizzata nell'elenco delle tabelle in Esplora oggetti. Se la tabella non viene visualizzata, fare clic con il pulsante destro del mouse sul nodo **TutorialDB** > **Tabelle** in Esplora oggetti e scegliere **Aggiorna**.

   :::image type="content" source="media/ssms-connect-query-sql-server/new-table.png" alt-text="Nuova tabella":::

## <a name="insert-rows-into-the-new-table"></a>Inserire righe nella nuova tabella

Ora si inseriranno alcune righe nella tabella Customers creata. Incollare il frammento di codice T-SQL seguente nella finestra di query e selezionare **Esegui**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>Eseguire query nella tabella e visualizzare i risultati

I risultati di una query vengono visualizzati sotto la finestra di testo della query. Per eseguire una query sulla tabella Customers e visualizzare le righe inserite, seguire questa procedura:

1. Incollare il frammento di codice T-SQL seguente nella finestra di query e selezionare **Esegui**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    I risultati della query vengono visualizzati al di sotto dell'area in cui è stato immesso il testo.

   :::image type="content" source="media/ssms-connect-query-sql-server/query-results.png" alt-text="Elenco Risultati":::

    È anche possibile modificare il formato di visualizzazione dei risultati selezionando una delle opzioni seguenti:

   ![Tre opzioni per la visualizzazione dei risultati della query](media/ssms-connect-query-sql-server/results.png)

   - Il primo pulsante visualizza i risultati in **Visualizzazione testo**, come illustrato nell'immagine della sezione successiva.
   - Il pulsante centrale visualizza i risultati in **Visualizzazione griglia**, l'opzione predefinita.
       - Questa opzione viene impostata come predefinita
   - Il terzo pulsante consente di salvare i risultati in un file che per impostazione predefinita ha l'estensione rpt.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Verificare le proprietà di connessione usando la tabella della finestra di query

È possibile trovare informazioni sulle proprietà di connessione tra i risultati della query. Dopo aver eseguito la query specificata nel passaggio precedente, esaminare le proprietà di connessione nella parte inferiore della finestra di query.

- È possibile determinare il server e il database ai quali si è connessi e il nome utente usato.
- È anche possibile visualizzare la durata della query e il numero di righe restituite dalla query eseguita in precedenza.

   :::image type="content" source="media/ssms-connect-query-sql-server/connection-properties.png" alt-text="Proprietà di connessione":::

## <a name="additional-tools"></a>Strumenti aggiuntivi

È anche possibile usare [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) per connettersi ed eseguire query su [SQL Server](../../azure-data-studio/quickstart-sql-server.md), su un [database SQL di Azure](../../azure-data-studio/quickstart-sql-database.md) e su [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Passaggi successivi

Il modo migliore per acquisire familiarità con SSMS è la pratica diretta. Questi articoli illustrano le varie funzionalità disponibili in SSMS.

- [Editor di query SQL di Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Scripting](../tutorials/scripting-ssms.md)
- [Uso di modelli in SSMS](../template/templates-ssms.md)
- [Configurazione di SSMS](../tutorials/ssms-configuration.md)
- [Suggerimenti e consigli per l'uso di SSMS](../tutorials/ssms-tricks.md)