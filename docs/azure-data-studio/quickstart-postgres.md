---
title: 'Avvio rapido: Connettersi ed eseguire query in PostgreSQL'
description: Eseguire una guida di avvio rapido che illustra come usare Azure Data Studio per connettersi a PostgreSQL e quindi usare istruzioni SQL per creare ed eseguire query in un database.
ms.custom: seodec18
ms.date: 09/18/2019
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: sunilagarwal
ms.author: sunila
ms.openlocfilehash: dad9f6348b3df9d7589be739af5ba7c748d37b6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040083"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-postgresql"></a>Avvio rapido: Usare Azure Data Studio per connettersi a PostgreSQL ed eseguire query

Questa guida di avvio rapido illustra come usare Azure Data Studio per connettersi a PostgreSQL e quindi usare istruzioni SQL per creare il database *tutorialdb* ed eseguire query su di esso.

## <a name="prerequisites"></a>Prerequisiti

Per completare questo avvio rapido sono necessari Azure Data Studio, l'estensione PostgreSQL per Azure Data Studio e l'accesso a un server PostgreSQL.

- [Installare Azure Data Studio](./download-azure-data-studio.md).
- [Installare l'estensione PostgreSQL per Azure Data Studio](./extensions/postgres-extension.md).
- [Installare PostgreSQL](https://www.postgresql.org/download/). In alternativa, è possibile creare un database Postgres nel cloud con il comando [az postgres up](/azure/postgresql/quickstart-create-server-up-azure-cli). 

## <a name="connect-to-postgresql"></a>Connettersi a PostgreSQL

1. Avviare **Azure Data Studio**.

2. La prima volta che si avvia Azure Data Studio viene visualizzata la finestra di dialogo **Connessione**. Se la finestra di dialogo **Connessione** non si apre, fare clic sull'icona **Nuova connessione** nella pagina **SERVER**:

   ![Icona Nuova connessione](media/quickstart-postgresql/new-connection-icon.png)

3. Nel modulo che viene visualizzato, passare a **Tipo di connessione** e scegliere **PostgreSQL** nell'elenco a discesa.


4. Compilare i campi rimanenti usando il nome server, il nome utente e la password relativi al server PostgreSQL. 

   ![Nuova schermata di connessione](media/quickstart-postgresql/new-connection-screen.png)  

   | Impostazione       | Valore di esempio | Descrizione |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nome server** | localhost | Nome completo del server |
   | **Nome utente** | postgres | Nome utente con cui si desidera eseguire l'accesso. |
   | **Password (account di accesso SQL)** | *password* | Password dell'account con cui si esegue l'accesso. |
   | **Password** | *Controllo* | Selezionare questa casella se non si vuole immettere la password ogni volta che ci si connette. |
   | **Nome database** | \<Default\> | Compilare questo campo se si vuole che nella connessione sia specificato un database. |
   | **Gruppo di server** | \<Default\> | Questa opzione consente di assegnare la connessione a uno specifico gruppo di server precedentemente creato. | 
   | **Nome (facoltativo)** | *lasciare vuoto* | Questa opzione consente di specificare un nome descrittivo per il server. | 

5. Selezionare **Connetti**. 

Dopo il completamento della connessione, il server si apre nella barra laterale **SERVER**.


## <a name="create-a-database"></a>Creazione di un database

Questa procedura consente di creare un database denominato **tutorialdb**:

1. Fare clic con il pulsante destro del mouse sul server PostgreSQL nella barra laterale **SERVER** e scegliere **Nuova query**.

2. Incollare questa istruzione SQL nell'editor di query visualizzato.

   ```sql
   CREATE DATABASE tutorialdb;
   ```

3. Sulla barra degli strumenti selezionare **Esegui** per eseguire la query. Nel riquadro **MESSAGGI** vengono visualizzate alcune notifiche che informano l'utente sullo stato di avanzamento della query.

>[!TIP]
> Per eseguire l'istruzione è possibile premere **F5** sulla tastiera anziché usare l'opzione **Esegui**.

Dopo il completamento della query, fare clic con il pulsante destro del mouse su **Database** e scegliere **Aggiorna** per visualizzare la voce **tutorialdb** nell'elenco presente sotto il nodo **Database**.


## <a name="create-a-table"></a>Creare una tabella

 Questa procedura consente di creare una tabella in **tutorialdb**:

1. Modificare il contesto di connessione in **tutorialdb** usando l'elenco a discesa disponibile nell'editor di query. 

   ![Modifica del contesto](media/quickstart-postgresql/change-context.png)

2. Incollare l'istruzione SQL seguente nell'editor di query e fare clic su **Esegui**. 

   > [!NOTE]
   > È possibile aggiungerla o sovrascrivere la query esistente nell'editor. Facendo clic su **Esegui** viene eseguita solo la query evidenziata. Se nessuna query risulta evidenziata, facendo clic su **Esegui** vengono eseguite tutte le query nell'editor.

   ```sql
   -- Drop the table if it already exists
   DROP TABLE IF EXISTS customers;
   -- Create a new table called 'customers'
   CREATE TABLE customers(
       customer_id SERIAL PRIMARY KEY,
       name VARCHAR (50) NOT NULL,
       location VARCHAR (50) NOT NULL,
       email VARCHAR (50) NOT NULL
   );
   ```

## <a name="insert-rows"></a>Inserire righe

Incollare il frammento di codice seguente nella finestra di query e fare clic su **Esegui**:

   ```sql
   -- Insert rows into table 'customers'
   INSERT INTO customers
       (customer_id, name, location, email)
    VALUES
      ( 1, 'Orlando', 'Australia', ''),
      ( 2, 'Keith', 'India', 'keith0@adventure-works.com'),
      ( 3, 'Donna', 'Germany', 'donna0@adventure-works.com'),
      ( 4, 'Janet', 'United States','janet1@adventure-works.com');
   ```

## <a name="query-the-data"></a>Eseguire una query sui dati

1. Incollare il frammento di codice seguente nell'editor di query e fare clic su **Esegui**:
   
   ```sql
   -- Select rows from table 'customers'
   SELECT * FROM customers; 
   ```

2. Vengono visualizzati i risultati della query:

   ![Visualizzazione dei risultati](media/quickstart-postgresql/view-results.png)

## <a name="next-steps"></a>Passaggi successivi

Informazioni sugli [scenari disponibili per Postgres in Azure Data Studio](./extensions/postgres-extension.md).