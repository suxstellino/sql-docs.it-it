---
title: Connettersi ed eseguire query in un'istanza di SQL Server in una macchina virtuale di Azure con SQL Server Management Studio (SSMS)
description: Connettersi a un'istanza di SQL Server in una macchina virtuale di Azure con SSMS. Creare ed eseguire query in un'istanza di SQL Server in una macchina virtuale di Azure usando query T-SQL di base in SSMS.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 29c39caf6885ee974c62ed153df982b435c72c95
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619323"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>Avvio rapido: Connettersi ed eseguire query in un'istanza di SQL Server in una macchina virtuale di Azure con SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Informazioni introduttive sull'uso di SQL Server Management Studio (SSMS) per connettersi all'istanza di SQL Server in una macchina virtuale di Azure ed eseguire alcuni comandi Transact-SQL (T-SQL) di base.

> [!div class="checklist"]
> - Connessione a un'istanza di SQL Server
> - Creazione di un database
> - Creare una tabella nel nuovo database
> - Inserire righe nella nuova tabella
> - Eseguire query nella tabella e visualizzare i risultati
> - Usare la tabella della finestra di query per verificare le proprietà di connessione
## <a name="prerequisites"></a>Prerequisiti

Per seguire questo articolo, è necessario SQL Server Management Studio e avere accesso a un'origine dati.

- Installare [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [SQL Server in una macchina virtuale di Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE).

## <a name="connect-to-sql-virtual-machines"></a>Connettersi a macchine virtuali SQL

La procedura seguente illustra come creare un'etichetta DNS facoltativa per la macchina virtuale di Azure e quindi come connettersi con SQL Server Management Studio (SSMS).

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>Configurare un'etichetta DNS per l'indirizzo IP pubblico

Per connettersi al motore di database di SQL Server da Internet, prendere in considerazione la possibilità di configurare un'etichetta DNS per l'indirizzo IP pubblico. È possibile connettersi tramite l'indirizzo IP, ma l'etichetta DNS crea un record A che è più facile da identificare e astrae l'indirizzo IP pubblico sottostante.

> [!NOTE]
> Le etichette DNS non sono necessarie se si intende connettersi solo all'istanza di SQL Server presente nella stessa rete virtuale o solo in locale.

1. Creare un'etichetta DNS selezionando **Macchine virtuali** nel portale. Selezionare la propria macchina virtuale di SQL Server per visualizzarne le proprietà.

2. Nella panoramica della macchina virtuale selezionare l'**indirizzo IP pubblico**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="Indirizzo IP pubblico":::

3. Nelle proprietà dell'indirizzo IP pubblico espandere **Configurazione**.

4. Immettere un nome per l'etichetta DNS. Questo nome è un record A che consente di connettersi direttamente alla macchina virtuale di SQL Server usando il nome, anziché tramite l'indirizzo IP.

5. Fare clic sul pulsante **Salva**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="Etichetta DNS":::

### <a name="connect"></a>Connessione

1. Avviare SQL Server Management Studio. Quando si esegue SSMS per la prima volta viene visualizzata la finestra di dialogo **Connetti al server**. Se la finestra non si apre è possibile aprirla manualmente selezionando **Esplora oggetti** > **Connetti** > **Motore di database**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="Collegamento Connetti in Esplora oggetti":::

2. Viene visualizzata la finestra di dialogo **Connetti al server** . Immettere le seguenti informazioni:

    |   Impostazione   |   Valori consigliati   |   Descrizione   |
    |--------------|-----------------------|-----------------|
    | **Tipo di server** | Motore di database | In **Tipo di server** selezionare **Motore di database** (in genere l'opzione predefinita). |
    | **Nome server** | Nome completo del server | In **Nome server** immettere il nome della macchina virtuale SQL di Azure in uso. Per la connessione è anche possibile usare l'indirizzo IP della macchina virtuale SQL di Azure. | 
    | **autenticazione** | Autenticazione di SQL Server | Usare **Autenticazione di SQL Server** per la connessione della macchina virtuale SQL di Azure. Inoltre, se è disponibile un ambiente Azure Active Directory configurato, è possibile usare una qualsiasi delle opzioni di Azure Active Directory. </br> </br> Il metodo **Autenticazione di Windows** non è supportato per la macchina virtuale SQL di Azure. Per altre informazioni, vedere [Autenticazione SQL di Azure](/azure/sql-database/sql-database-security-overview#access-management).|
    | **Accesso** | ID utente dell'account server | ID utente dell'account server usato per creare il server. Un account di accesso è obbligatorio quando si usa l'**autenticazione di SQL Server**. |
    | **Password** | Password dell'account server | Password dell'account server usato per creare il server. Una password è obbligatoria quando si usa l'**autenticazione di SQL Server**. |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="Campo del nome del server per le macchine virtuali SQL":::

3. Dopo aver completato tutti i campi selezionare **Connetti**.

    È anche possibile modificare altre opzioni di connessione selezionando **Opzioni**. Sono esempi di opzioni di connessione il database al quale ci si connette, il valore di timeout della connessione e il protocollo di rete. In questo articolo vengono usati i valori predefiniti per tutte le opzioni.

4. Per verificare l'esito positivo della connessione dell'istanza di SQL Server nella macchina virtuale di Azure, espandere ed esplorare gli oggetti all'interno di **Esplora oggetti** in cui vengono visualizzati il nome del server, la versione di SQL Server e il nome utente. Questi oggetti sono diversi a seconda del tipo di server.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="Connessione della macchina virtuale SQL di Azure":::

## <a name="troubleshoot-connectivity-issues"></a>Risolvere i problemi di connettività

Benché il portale offra opzioni per la configurazione automatica della connettività, è utile sapere come configurarla manualmente. Anche l'identificazione dei requisiti può semplificare la risoluzione dei problemi.

La tabella seguente elenca i requisiti per la connessione a SQL Server in una macchina virtuale di Azure.

| Requisito | Descrizione |
|---|---|
| [Abilitare la modalità di autenticazione di SQL Server](/sql/database-engine/configure-windows/change-server-authentication-mode#use-ssms) | L'autenticazione di SQL Server è necessaria per connettersi alla macchina virtuale in remoto, a meno che non sia stato configurato Active Directory in una rete virtuale. |
| [Creare un account di accesso SQL](/sql/relational-databases/security/authentication-access/create-a-login) | Se si usa l'autenticazione SQL, è necessario un account di accesso SQL con un nome utente e una password, che abbia anche le autorizzazioni per il database di destinazione. |
| Abilitare il protocollo TCP/IP | SQL Server deve consentire le connessioni tramite TCP. |
| [Abilitare una regola del firewall per la porta di SQL Server](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | Il firewall nella macchina virtuale deve consentire il traffico in ingresso sulla porta di SQL Server (porta predefinita: 1433). |
| [Creare una regola del gruppo di sicurezza di rete per la porta TCP 1433](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#create-a-security-rule) | Consentire alla macchina virtuale di ricevere il traffico sulla porta di SQL Server (porta predefinita: 1433) se si vuole connettersi tramite Internet. Questo non è un requisito per le connessioni locali e solo a reti virtuali. Questo passaggio è necessario solo nel portale di Azure. |

> [!TIP]
> I passaggi indicati nella tabella precedente vengono eseguiti automaticamente durante la configurazione della connettività nel portale. Usare questi passaggi solo per confermare la configurazione o per configurare manualmente la connettività per SQL Server.

## <a name="create-a-database"></a>Creazione di un database

Creare un database denominato TutorialDB seguendo questa procedura:

1. Fare clic con il pulsante destro del mouse in Esplora oggetti e scegliere **Nuova query**:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="Collegamento Nuova query":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="Comando Esegui":::
  
    Al termine della query il nuovo database TutorialDB viene visualizzato nell'elenco dei database in Esplora oggetti. Se il database non viene visualizzato, fare clic con il pulsante destro del mouse sul nodo **Database** e selezionare **Aggiorna**.

## <a name="create-a-table-in-the-new-database"></a>Creare una tabella nel nuovo database

In questa sezione si crea una tabella nel database TutorialDB appena creato. L'editor di query è ancora nel contesto del database *master*. Cambiare il contesto e impostare la connessione al database *TutorialDB* seguendo questa procedura:

1. Nella casella di riepilogo a discesa dei database selezionare il database desiderato, come indicato di seguito:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="Modificare il database":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="Nuova tabella":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="Elenco Risultati":::

    È anche possibile modificare il formato di visualizzazione dei risultati selezionando una delle opzioni seguenti:

   ![Tre opzioni per la visualizzazione dei risultati della query](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - Il primo pulsante visualizza i risultati in **Visualizzazione testo**, come illustrato nell'immagine della sezione successiva.
   - Il pulsante centrale visualizza i risultati in **Visualizzazione griglia**, l'opzione predefinita.
       - Questa opzione viene impostata come predefinita
   - Il terzo pulsante consente di salvare i risultati in un file che per impostazione predefinita ha l'estensione rpt.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Verificare le proprietà di connessione usando la tabella della finestra di query

È possibile trovare informazioni sulle proprietà di connessione tra i risultati della query. Dopo aver eseguito la query specificata nel passaggio precedente, esaminare le proprietà di connessione nella parte inferiore della finestra di query.

- È possibile determinare il server e il database ai quali si è connessi e il nome utente usato.
- È anche possibile visualizzare la durata della query e il numero di righe restituite dalla query eseguita in precedenza.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="Proprietà di connessione":::

## <a name="additional-tools"></a>Strumenti aggiuntivi

È anche possibile usare [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) per connettersi ed eseguire query su [SQL Server](../../azure-data-studio/quickstart-sql-server.md), su un [database SQL di Azure](../../azure-data-studio/quickstart-sql-database.md) e su [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Passaggi successivi

Il modo migliore per acquisire familiarità con SSMS è la pratica diretta. Questi articoli illustrano le varie funzionalità disponibili in SSMS.

- [Editor di query SQL di Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Scripting](../tutorials/scripting-ssms.md)
- [Uso di modelli in SSMS](../template/templates-ssms.md)
- [Configurazione di SSMS](../tutorials/ssms-configuration.md)
- [Suggerimenti e consigli per l'uso di SSMS](../tutorials/ssms-tricks.md)