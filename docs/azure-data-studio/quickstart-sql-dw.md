---
title: Connettersi ed eseguire query con Azure Synapse Analytics
description: Questo argomento di avvio rapido descrive la connessione a un pool SQL dedicato in Azure Synapse Analytics tramite Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: maghan, jrasnick
ms.custom: seodec18; seo-lt-2019
ms.date: 10/15/2020
ms.openlocfilehash: 91e91aeb69db1d3ff3f357289f0156cfcf739bab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040072"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Avvio rapido: Usare Azure Data Studio per connettersi ed eseguire query sui dati con un pool SQL dedicato in Azure Synapse Analytics

Questo argomento di avvio rapido descrive la connessione a un pool SQL dedicato in Azure Synapse Analytics tramite Azure Data Studio.

## <a name="prerequisites"></a>Prerequisiti
Per completare questo argomento di avvio rapido, è necessario disporre di Azure Data Studio e di un pool SQL dedicato in Azure Synapse Analytics.

- [Installare Azure Data Studio](./download-azure-data-studio.md).

Se non si dispone già di un pool SQL dedicato, vedere [Creare un pool SQL dedicato](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

Annotare il nome del server e le credenziali di accesso.


## <a name="connect-to-your-dedicated-sql-pool"></a>Connettersi al pool SQL dedicato

Usare Azure Data Studio per stabilire una connessione al server di Azure Synapse Analytics.

1. La prima volta che si esegue Azure Data Studio viene visualizzata la pagina **Connessione**. Se la pagina **Connessione** non viene visualizzata, selezionare **Aggiungi connessione** o l'icona **Nuova connessione** nella barra laterale **SERVER**:
   
   ![Screenshot della pagina Connessione con l'icona Nuova connessione evidenziata.](media/quickstart-sql-dw/new-connection-icon.png)

2. Questo articolo usa l'account di *accesso SQL*, ma è supportata anche l'*autenticazione di Windows*. Compilare i campi seguenti usando il nome server, il nome utente e la password relativi al *server SQL di Azure*:

   |   Impostazione    | Valore consigliato | Descrizione |
   |--------------|-----------------|-------------| 
   | **Nome server** | Nome completo del server | Ad esempio, il nome dovrebbe essere simile al seguente: **sqlpoolservername.database.windows.net**. |
   | **autenticazione** | Account di accesso SQL| In questa esercitazione viene usata l'autenticazione SQL. |
   | **Nome utente** | Account amministratore del server | Si tratta dell'account specificato al momento della creazione del server. |
   | **Password (account di accesso SQL)** | Password per l'account amministratore del server | Si tratta della password specificata al momento della creazione del server. |
   | **Salvare la password?** | Sì o No | Se non si vuole immettere ogni volta la password, selezionare Sì. |
   | **Nome database** | *lasciare vuoto* | Il nome del database a cui si effettua la connessione. |
   | **Gruppo di server** | Selezionare <Default> | È possibile impostare questo campo su uno specifico gruppo di server precedentemente creato. | 

3. Se nel server non è presente una regola del firewall che consente la connessione di Azure Data Studio, viene visualizzato il modulo **Crea nuova regola del firewall**. Completare il modulo per creare una nuova regola del firewall. Per informazioni dettagliate, vedere [Regole del firewall](/azure/sql-database/sql-database-firewall-configure).

4. Dopo il completamento della connessione, il server si apre nella barra laterale *Server*.

## <a name="create-a-database-in-your-dedicated-sql-pool"></a>Creare un database nel pool SQL dedicato

1. Fare clic con il pulsante destro del mouse in Esplora oggetti del server e scegliere **Nuova query**.

2. Incollare il frammento di codice seguente nell'editor di query e selezionare **Esegui**:

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```

## <a name="create-a-table"></a>Creare una tabella

Si vuole creare una tabella nel database *TutorialDB*, ma l'editor di query è ancora connesso al database *master*. 

1. Modificare il contesto di connessione in **TutorialDB**:

2. Incollare il frammento di codice seguente nell'editor di query e selezionare **Esegui**:

   > [!NOTE]
   > È possibile aggiungerlo alla query esistente nell'editor oppure sovrascriverla. Si noti che quando si fa clic su **Esegui** viene eseguita solo la query selezionata. Se non è selezionata alcuna query, quando si fa clic su **Esegui** vengono eseguite tutte le query nell'editor.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-table.png" alt-text="Creare una tabella nel database TutorialDB":::


## <a name="insert-rows"></a>Inserire righe

1. Incollare il frammento di codice seguente nell'editor di query e selezionare **Esegui**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-rows.png" alt-text="Creare righe nella tabella":::

## <a name="view-the-result"></a>Visualizzare il risultato

1. Incollare il frammento di codice seguente nell'editor di query e selezionare **Esegui**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

2. Vengono visualizzati i risultati della query:

    :::image type="content" source="media/quickstart-sql-dw/view-results.png" alt-text="View the results":::


## <a name="clean-up-resources"></a>Pulire le risorse

Se non si prevede di continuare a usare i database di esempio creati in questo articolo, [eliminare il gruppo di risorse](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal#clean-up-resources).

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni, vedere [Connettersi a Synapse SQL con Azure Data Studio](/azure/synapse-analytics/sql/get-started-azure-data-studio).

Ora che si è stabilita la connessione ad Azure Synapse Analytics e si è eseguita una query, effettuare l'[esercitazione sull'editor di codice](tutorial-sql-editor.md).