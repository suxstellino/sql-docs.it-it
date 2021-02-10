---
title: Usare l'editor Transact-SQL per creare oggetti di database
description: Seguire questa esercitazione per scoprire su come usare l'editor Transact-SQL per eseguire attività di database di base, incluse la creazione e la ricerca di oggetti di database.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/24/2018
ms.openlocfilehash: 1c91b067ff8396968b2d26c57bf5f78d4f5d4181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039931"
---
# <a name="tutorial-use-the-transact-sql-editor-to-create-database-objects---azure-data-studio"></a>Esercitazione: Usare l'editor Transact-SQL per creare oggetti di database - Azure Data Studio

La creazione e l'esecuzione di query, stored procedure, script e così via sono le attività principali dei professionisti dei database. Questa esercitazione illustra le funzionalità principali dell'editor T-SQL per la creazione di oggetti di database.

In questa esercitazione si apprenderà come usare [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] per:
> [!div class="checklist"]
> * Cercare oggetti di database
> * Modificare i dati delle tabelle 
> * Usare i frammenti di codice per scrivere rapidamente T-SQL
> * Visualizzare i dettagli degli oggetti di database usando *Visualizza definizione* e *Vai a definizione*


## <a name="prerequisites"></a>Prerequisiti

Per questa esercitazione è necessario il database di SQL Server o il database SQL di Azure *TutorialDB*. Per creare il database *TutorialDB*, completare uno degli argomenti di avvio rapido seguenti:

- [Connettersi ed eseguire query in SQL Server con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Connettersi ed eseguire query nel database SQL di Azure con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="quickly-locate-a-database-object-and-perform-a-common-task"></a>Individuare rapidamente un oggetto di database ed eseguire un'attività comune

[!INCLUDE[name-sos-short](../includes/name-sos-short.md)] fornisce un widget di ricerca per trovare rapidamente gli oggetti di database. Nell'elenco dei risultati è disponibile un menu di scelta rapida per le attività comuni relative all'oggetto selezionato, ad esempio la *Modifica dati* per una tabella.

1. Aprire la barra laterale SERVER (**CTRL +G**), espandere **Database** e selezionare **TutorialDB**. 

1. Aprire il *dashboard TutorialDB* facendo clic con il pulsante destro del mouse su **TutorialDB** e scegliendo **Gestisci** dal menu di scelta rapida:

   ![menu di scelta rapida - Gestisci](./media/tutorial-sql-editor/insight-open-dashboard.png)

1. Nel dashboard fare clic con il pulsante destro del mouse su **dbo.Customers** (nel widget di ricerca) e selezionare **Modifica dati**.
   
   > [!TIP]
   > Per i database con molti oggetti, usare il widget di ricerca per individuare rapidamente la tabella, la vista e così via.

   ![widget di ricerca rapida](./media/tutorial-sql-editor/quick-search-widget.png)

1. Modificare la colonna **Email** nella prima riga, digitare *orlando0\@adventure-works.com* e premere **Invio** per salvare la modifica.

   ![modifica dati](./media/tutorial-sql-editor/edit-data.png)

## <a name="use-t-sql-snippets-to-create-stored-procedures"></a>Usare frammenti di codice T-SQL per creare stored procedure

Azure Data Studio offre molti frammenti di codice T-SQL predefiniti per la creazione rapida di istruzioni.


1. Aprire un nuovo editor di query premendo **CTRL+N**.

2. Digitare **sql** nell'editor, scorrere verso il basso fino a **sqlCreateStoredProcedure** e premere *TAB* o *INVIO* per caricare il frammento di codice per la creazione di stored procedure.

   ![Screenshot che mostra l'editor di query con codice SQL digitato nell'editor e l'opzione sqlCreateStoredProcedure evidenziata.](./media/tutorial-sql-editor/snippet-list.png)

3. Il frammento di codice per la creazione di stored procedure ha due campi impostati per la modifica rapida, *StoredProcedureName* e *SchemaName*. Selezionare *StoredProcedureName*, fare clic con il pulsante destro del mouse e scegliere **Cambia tutte le occorrenze**. A questo punto, digitare *getCustomer* e tutte le voci *StoredProcedureName* cambiano in *getCustomer*.

   ![Screenshot che mostra l'editor di query con l'opzione Cambia tutte le occorrenze evidenziata.](./media/tutorial-sql-editor/snippet.png)

5. Modificare tutte le occorrenze di *SchemaName* in *dbo*. 
6. Il frammento di codice contiene parametri segnaposto e corpo del testo che devono essere aggiornati. Anche l'istruzione *EXECUTE* contiene testo segnaposto perché non conosce il numero di parametri che la stored procedure avrà. Per questa esercitazione, aggiornare il frammento di codice in modo che abbia un aspetto simile al seguente:

    ```sql
    -- Create a new stored procedure called 'getCustomer' in schema 'dbo'
    -- Drop the stored procedure if it already exists
    IF EXISTS (
    SELECT *
    FROM INFORMATION_SCHEMA.ROUTINES
    WHERE SPECIFIC_SCHEMA = N'dbo'
    AND SPECIFIC_NAME = N'getCustomer'
    )
    DROP PROCEDURE dbo.getCustomer
    GO
    -- Create the stored procedure in the specified schema
    CREATE PROCEDURE dbo.getCustomer
    @ID int
    -- add more stored procedure parameters here
    AS
    -- body of the stored procedure
    SELECT  c.CustomerId, 
    c.Name, 
    c.Location, 
    c.Email
    FROM dbo.Customers c
    WHERE c.CustomerId = @ID
    FOR JSON PATH

    GO
    -- example to execute the stored procedure we just created
    EXECUTE dbo.getCustomer 1
    GO
    ```
    
5. Per creare la stored procedure e avviare un'esecuzione di test, premere **F5**.

La stored procedure viene creata e il riquadro **RISULTATI** mostra il cliente restituito in JSON. Per vedere il formato JSON formattato, fare clic sul record restituito. 


## <a name="use-peek-definition"></a>Usare Visualizza definizione 

Azure Data Studio consente di visualizzare la definizione di un oggetto usando la funzionalità di visualizzazione definizione. Questa sezione crea una seconda stored procedure e usa la visualizzazione definizione per vedere le colonne presenti in una tabella e creare rapidamente il corpo della stored procedure.

1. Aprire un nuovo editor premendo **CTRL+N**. 

2. Digitare *sql* nell'editor, scorrere verso il basso fino a *sqlCreateStoredProcedure* e premere *TAB* o *INVIO* per caricare il frammento di codice per la creazione di stored procedure.
3. Digitare *setCustomer* per *StoredProcedureName* e *dbo* per *SchemaName*

3. Sostituire i segnaposto @param con la definizione di parametro seguente:

   ```sql
   @json_val nvarchar(max)
   ```

4. Sostituire il corpo della stored procedure con il codice seguente:
   ```sql
   INSERT INTO dbo.Customers
   ```

5. Nella riga *INSERT* appena aggiunta, fare clic con il pulsante destro del mouse su **dbo.Customers** e scegliere **Visualizza definizione**.

   ![visualizza definizione](./media/tutorial-sql-editor/peek-definition.png)

6. Compare la definizione della tabella, che consente di vedere rapidamente le colonne presenti nella tabella. Vedere l'elenco delle colonne per completare facilmente le istruzioni per la stored procedure. Completare la creazione dell'istruzione INSERT aggiunta in precedenza per completare il corpo della stored procedure e chiudere la finestra di visualizzazione definizione:

   ```sql
   INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
    )
   ```
7. Eliminare o impostare come commento il comando *EXECUTE* nella parte inferiore della query.
8. L'intera istruzione dovrebbe essere simile al codice seguente:

   ```sql
   -- Create a new stored procedure called 'setCustomer' in schema 'dbo'
   -- Drop the stored procedure if it already exists
   IF EXISTS (
   SELECT *
       FROM INFORMATION_SCHEMA.ROUTINES
       WHERE SPECIFIC_SCHEMA = N'dbo'
       AND SPECIFIC_NAME = N'setCustomer'
   )
   DROP PROCEDURE dbo.setCustomer
   GO
   -- Create the stored procedure in the specified schema
   CREATE PROCEDURE dbo.setCustomer
       @json_val nvarchar(max) 
   AS
       -- body of the stored procedure
       INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
       )
   GO
   ```

8. Per creare la stored procedure *setCustomer*, premere **F5**.

## <a name="use-save-query-results-as-json-to-test-the-setcustomer-stored-procedure"></a>Salvare i risultati della query come JSON per testare la stored procedure setCustomer

La stored procedure *setCustomer* creata nella sezione precedente richiede che i dati JSON vengano passati al parametro *\@json_val*. Questa sezione illustra come ottenere un frammento di JSON formattato correttamente da passare al parametro, in modo da poter testare la stored procedure.

1. Nella barra laterale **SERVER** fare clic con il pulsante destro del mouse sulla tabella *dbo.Customers* e scegliere **Seleziona le prime 1000 righe**.

2. Selezionare la prima riga nella visualizzazione risultati, verificare che sia selezionata l'intera riga (fare clic sul numero 1 nella colonna più a sinistra) e selezionare **Save as JSON** (Salva come JSON).  
3. Modificare la cartella in un percorso semplice da ricordare per poter eliminare il file in un secondo momento, ad esempio il desktop, e fare clic su **Salva**. Viene aperto il file in formato JSON.

   ![salvare come JSON](./media/tutorial-sql-editor/save-as-json.png)

4. Selezionare i dati JSON nell'editor e copiarli.
5. Aprire un nuovo editor premendo **CTRL+N**.
6. I passaggi precedenti mostrano come è possibile ottenere facilmente i dati formattati correttamente per completare la chiamata alla stored procedure *setCustomer*. Si può osservare che il codice seguente usa lo stesso formato JSON con i dettagli dei nuovi clienti, in modo che sia possibile testare la procedura *setCustomer*. L'istruzione include la sintassi per dichiarare il parametro ed eseguire le nuove routine Get e Set. È possibile incollare i dati copiati dalla sezione precedente e modificarli in modo che corrispondano a quelli dell'esempio seguente o semplicemente incollare l'istruzione seguente nell'editor di query.

   ```sql
   -- example to execute the stored procedure we just created
   declare @json nvarchar(max) =
   N'[
       {
           "CustomerId": 5,
           "Name": "Lucy",
           "Location": "Canada",
           "Email": "lucy0@adventure-works.com"
       }
   ]'

   EXECUTE dbo.setCustomer @json_val = @json
   GO

   EXECUTE dbo.getCustomer @ID = 5
   ```

7. Eseguire lo script premendo **F5**. Lo script inserisce un nuovo cliente e restituisce le informazioni del nuovo cliente in formato JSON. Fare clic sul risultato per aprire una visualizzazione formattata.

   ![risultato del test](./media/tutorial-sql-editor/test-result.png)

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> * Cercare rapidamente oggetti dello schema
> * Modificare i dati delle tabelle 
> * Scrivere script T-SQL usando frammenti di codice
> * Visualizzare i dettagli degli oggetti di database usando Visualizza definizione e Vai a definizione


Per informazioni su come abilitare il widget per le **cinque query più lente**, completare l'esercitazione successiva:

> [!div class="nextstepaction"]
> [Abilitare il widget di informazioni dettagliate di esempio sulle query lente](tutorial-qds-sql-server.md)
