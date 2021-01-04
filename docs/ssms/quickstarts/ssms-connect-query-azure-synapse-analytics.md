---
title: Connettersi ed eseguire query in un pool SQL dedicato in Azure Synapse Analytics con SQL Server Management Studio (SSMS)
description: Connettersi a un pool SQL dedicato in Azure Synapse Analytics con SSMS. Creare ed eseguire query in un pool SQL dedicato in Azure Synapse Analytics eseguendo query T-SQL di base in SSMS.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 102eaff45b6cdfb89a159e3de77039c8735395e2
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619293"
---
# <a name="quickstart-connect-and-query-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>Avvio rapido: Connettersi ed eseguire query in un pool SQL dedicato in Azure Synapse Analytics con SQL Server Management Studio (SSMS)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Informazioni introduttive sull'uso di SQL Server Management Studio (SSMS) per connettersi al pool SQL dedicato in Azure Synapse Analytics ed eseguire alcuni comandi Transact-SQL (T-SQL) di base.

> [!div class="checklist"]
> - Connettersi a un pool SQL dedicato in Azure Synapse Analytics
> - Creazione di un database
> - Creare una tabella nel nuovo database
> - Inserire righe nella nuova tabella
> - Eseguire query nella tabella e visualizzare i risultati
> - Usare la tabella della finestra di query per verificare le proprietà di connessione

## <a name="prerequisites"></a>Prerequisiti

Per seguire questo articolo, è necessario SQL Server Management Studio e avere accesso a un'origine dati.

- Installare [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE)

## <a name="connect-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Connettersi a un pool SQL dedicato in Azure Synapse Analytics

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. Avviare SQL Server Management Studio. Quando si esegue SSMS per la prima volta viene visualizzata la finestra di dialogo **Connetti al server**. Se la finestra non si apre è possibile aprirla manualmente selezionando **Esplora oggetti** > **Connetti** > **Motore di database**.

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-object-explorer.png" alt-text="Collegamento Connetti in Esplora oggetti":::

2. Nella finestra **Connetti al server** seguire l'elenco seguente:

    |   Impostazione   |   Valori consigliati   |   Descrizione   |
    |-------------|------------------------|-----------------|
    | **Tipo di server** | Motore di database | In **Tipo di server** selezionare **Motore di database** (in genere l'opzione predefinita). |
    | **Nome server** | Nome completo del server | Per **Nome server** immettere il nome del server Azure Synapse Analytics. |
    | **autenticazione** | Autenticazione di SQL Server | Usare **Autenticazione di SQL Server** per la connessione di Azure Synapse Analytics. </br> </br> Il metodo **Autenticazione di Windows** non è supportato per SQL di Azure. Per altre informazioni, vedere [Autenticazione SQL di Azure](/azure/sql-database/sql-database-security-overview#access-management). |
    | **Accesso** | ID utente dell'account server | ID utente dell'account server usato per creare il server. |
    | **Password** | Password dell'account server | Password dell'account server usato per creare il server. |

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-to-azure-synapse-analytics-object-explorer.png" alt-text="Campo del nome del server per Azure Synapse Analytics":::

3. Dopo aver completato tutti i campi selezionare **Connetti**.

    È anche possibile modificare altre opzioni di connessione selezionando **Opzioni**. Sono esempi di opzioni di connessione il database al quale ci si connette, il valore di timeout della connessione e il protocollo di rete. In questo articolo vengono usati i valori predefiniti per tutte le opzioni.

    Se non sono state configurate le impostazioni del firewall, viene visualizzato un messaggio di richiesta per configurare il firewall. Dopo aver effettuato l'accesso, immettere le informazioni di accesso dell'account Azure e continuare a impostare la regola del firewall. Quindi scegliere **OK**. Si tratta di un'azione da eseguire una volta sola. Dopo aver configurato il firewall, la richiesta del firewall non dovrebbe essere visualizzata.

    :::image type="content" source="media/ssms-connect-query-azure-sql/azure-sql-firewall-sign-in-3.png" alt-text="Nuova regola del firewall per SQL di Azure":::

4. Per verificare l'esito positivo della connessione di Azure Synapse Analytics, espandere ed esplorare gli oggetti all'interno di **Esplora oggetti** in cui vengono visualizzati il nome del server, la versione di SQL Server e il nome utente. Questi oggetti sono diversi a seconda del tipo di server.

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-azure-synapse-analytics.png" alt-text="Connessione a un database di Azure Synapse Analytics":::

## <a name="troubleshoot-connectivity-issues"></a>Risolvere i problemi di connettività

È possibile riscontrare problemi di connessione con Azure Synapse Analytics. Per altre informazioni sulla risoluzione dei problemi di connessione, vedere [Risoluzione dei problemi di connettività](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues).

È possibile prevenire, risolvere i problemi, diagnosticare e mitigare gli errori di connessione e temporanei che si verificano quando si interagisce con il database SQL di Azure o con Istanza gestita di SQL di Azure. Per altre informazioni, vedere [Risolvere gli errori di connessione temporanei](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-connectivity-issues).

## <a name="create-a-database"></a>Creazione di un database

Verrà ora creato un database denominato TutorialDB seguendo questa procedura:

1. Fare clic con il pulsante destro del mouse in Esplora oggetti e scegliere **Nuova query**:

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-query.png" alt-text="Collegamento Nuova query":::

2. Incollare il frammento di codice T-SQL seguente nella finestra di query:

    ```sql
    IF NOT EXISTS (
    SELECT name
    FROM sys.databases
    WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
    ```

3. Eseguire la query selezionando **Esegui** o premendo F5 sulla tastiera.

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/execute.png" alt-text="Comando Esegui":::
  
    Al termine della query il nuovo database TutorialDB viene visualizzato nell'elenco dei database in Esplora oggetti. Se il database non viene visualizzato, fare clic con il pulsante destro del mouse sul nodo **Database** e selezionare **Aggiorna**.

## <a name="create-a-table-in-the-new-database"></a>Creare una tabella nel nuovo database

In questa sezione si crea una tabella nel database TutorialDB appena creato. L'editor di query è ancora nel contesto del database *master*. Cambiare il contesto e impostare la connessione al database *TutorialDB* seguendo questa procedura:

1. Nella casella di riepilogo a discesa dei database selezionare il database desiderato, come indicato di seguito:

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/change-db.png" alt-text="Modificare il database":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-table.png" alt-text="Nuova tabella":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/query-results.png" alt-text="Elenco Risultati":::

    È anche possibile modificare il formato di visualizzazione dei risultati selezionando una delle opzioni seguenti:

   ![Tre opzioni per la visualizzazione dei risultati della query](media/ssms-connect-query-azure-synapse-analytics/results.png)

   - Il primo pulsante visualizza i risultati in **Visualizzazione testo**, come illustrato nell'immagine della sezione successiva.
   - Il pulsante centrale visualizza i risultati in **Visualizzazione griglia**, l'opzione predefinita.
       - Questa opzione viene impostata come predefinita
   - Il terzo pulsante consente di salvare i risultati in un file che per impostazione predefinita ha l'estensione rpt.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Verificare le proprietà di connessione usando la tabella della finestra di query

È possibile trovare informazioni sulle proprietà di connessione tra i risultati della query. Dopo aver eseguito la query specificata nel passaggio precedente, esaminare le proprietà di connessione nella parte inferiore della finestra di query.

- È possibile determinare il server e il database ai quali si è connessi e il nome utente usato.
- È anche possibile visualizzare la durata della query e il numero di righe restituite dalla query eseguita in precedenza.

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connection-properties.png" alt-text="Proprietà di connessione":::

## <a name="additional-tools"></a>Strumenti aggiuntivi

È anche possibile usare [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) per connettersi ed eseguire query su [SQL Server](../../azure-data-studio/quickstart-sql-server.md), su un [database SQL di Azure](../../azure-data-studio/quickstart-sql-database.md) e su [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Passaggi successivi

Il modo migliore per acquisire familiarità con SSMS è la pratica diretta. Questi articoli illustrano le varie funzionalità disponibili in SSMS.

- [Editor di query SQL di Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Scripting](../tutorials/scripting-ssms.md)
- [Uso di modelli in SSMS](../template/templates-ssms.md)
- [Configurazione di SSMS](../tutorials/ssms-configuration.md)
- [Suggerimenti e consigli per l'uso di SSMS](../tutorials/ssms-tricks.md)