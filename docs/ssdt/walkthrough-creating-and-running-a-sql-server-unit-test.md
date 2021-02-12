---
title: Creazione ed esecuzione di uno unit test di SQL Server
description: Informazioni su come creare un unit test di SQL Server. Procedura dettagliata per la configurazione di un test che rileva un errore in una stored procedure.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 992c1d8e-3729-438b-9ef4-cd103e28f145
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6b0eaf98f4d10c08548e898c261a489ad71f0ff4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016731"
---
# <a name="walkthrough-creating-and-running-a-sql-server-unit-test"></a>Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server

In questa procedura dettagliata viene creato uno unit test di SQL Server tramite cui viene verificato il comportamento di diverse stored procedure. Vengono creati unit test di SQL Server per semplificare l'identificazione di eventuali difetti del codice che potrebbero causare il comportamento non corretto dell'applicazione. È possibile eseguire test dell'applicazione e unit test di SQL Server come parte di un gruppo di test automatizzato.  
  
Questa procedura dettagliata prevede l'esecuzione delle attività seguenti:  
  
-   [Creare uno script contenente uno schema del database](#CreateScript)  
  
-   [Creare un progetto di database e importare lo schema](#CreateProjectAndImport)  
  
-   [Distribuire il progetto di database in un ambiente di sviluppo isolato](#DeployDBProj)  
  
-   [Creare unit test di SQL Server](#CreateDBUnitTests)  
  
-   [Definire la logica del test](#DefineTestLogic)  
  
-   [Eseguire unit test di SQL Server](#RunTests)  
  
-   [Aggiungere uno unit test negativo](#NegativeTest)  
  
Dopo il rilevamento di un errore in una stored procedure mediante uno degli unit test, è possibile correggere l'errore ed eseguire nuovamente il test.  
  
## <a name="prerequisites"></a>Prerequisiti  
Per completare questa procedura dettagliata, è necessario essere in grado di connettersi a un server di database (o database LocalDB) per il quale si dispone delle autorizzazioni per la creazione e distribuzione di un database. Per altre informazioni, vedere [Autorizzazioni necessarie per le funzionalità di database di Visual Studio](/previous-versions/visualstudio/visual-studio-2010/aa833413(v=vs.100)).  
  
## <a name="create-a-script-that-contains-a-database-schema"></a><a name="CreateScript"></a>Creare uno script contenente uno schema del database  
  
#### <a name="to-create-a-script-from-which-you-can-import-a-schema"></a>Per creare uno script da cui sia possibile importare uno schema  
  
1.  Scegliere **Nuovo** dal menu **File**, quindi fare clic su **File**.  
  
    Verrà visualizzata la finestra di dialogo **Nuovo file** .  
  
2.  Se non è già evidenziato, selezionare **Generale** nell'elenco **Categorie** .  
  
3.  Nell'elenco **Modelli** fare clic su **File SQL**, quindi scegliere **Apri**.  
  
    Verrà aperto l'editor Transact\-SQL.  
  
4.  Copiare il codice Transact\-SQL seguente e incollarlo nell'editor Transact\-SQL.  
  
    ```  
    PRINT N'Creating Sales...';  
    GO  
    CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
    GO  
    PRINT N'Creating Sales.Customer...';  
    GO  
    CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Orders...';  
    GO  
    CREATE TABLE [Sales].[Orders] (  
        [CustomerID] INT      NOT NULL,  
        [OrderID]    INT      IDENTITY (1, 1) NOT NULL,  
        [OrderDate]  DATETIME NOT NULL,  
        [FilledDate] DATETIME NULL,  
        [Status]     CHAR (1) NOT NULL,  
        [Amount]     INT      NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDOrders...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDOrders] DEFAULT 0 FOR [YTDOrders];  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDSales...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDSales] DEFAULT 0 FOR [YTDSales];  
    GO  
    PRINT N'Creating Sales.Def_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_OrderDate] DEFAULT GetDate() FOR [OrderDate];  
    GO  
    PRINT N'Creating Sales.Def_Orders_Status...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_Status] DEFAULT 'O' FOR [Status];  
    GO  
    PRINT N'Creating Sales.PK_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [PK_Customer_CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.PK_Orders_OrderID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [PK_Orders_OrderID] PRIMARY KEY CLUSTERED ([OrderID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.FK_Orders_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [FK_Orders_Customer_CustID] FOREIGN KEY ([CustomerID]) REFERENCES [Sales].[Customer] ([CustomerID]) ON DELETE NO ACTION ON UPDATE NO ACTION;  
    GO  
    PRINT N'Creating Sales.CK_Orders_FilledDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_FilledDate] CHECK ((FilledDate >= OrderDate) AND (FilledDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.CK_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_OrderDate] CHECK ((OrderDate > '01/01/2005') and (OrderDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.uspCancelOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'X'  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspFillOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspFillOrder]  
    @OrderID INT, @FilledDate DATETIME  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'F',  
           [FilledDate] = @FilledDate  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspNewCustomer...';  
    GO  
    CREATE PROCEDURE [Sales].[uspNewCustomer]  
    @CustomerName NVARCHAR (40)  
    AS  
    BEGIN  
    INSERT INTO [Sales].[Customer] (CustomerName) VALUES (@CustomerName);  
    RETURN SCOPE_IDENTITY()  
    END  
    GO  
    PRINT N'Creating Sales.uspPlaceNewOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspPlaceNewOrder]  
    @CustomerID INT, @Amount INT, @OrderDate DATETIME, @Status CHAR (1)='O'  
    AS  
    BEGIN  
    DECLARE @RC INT  
    BEGIN TRANSACTION  
    INSERT INTO [Sales].[Orders] (CustomerID, OrderDate, FilledDate, Status, Amount)   
         VALUES (@CustomerID, @OrderDate, NULL, @Status, @Amount)  
    SELECT @RC = SCOPE_IDENTITY();  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders + @Amount  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    RETURN @RC  
    END  
    GO  
    CREATE PROCEDURE [Sales].[uspShowOrderDetails]  
    @CustomerID INT=0  
    AS  
    BEGIN  
    SELECT [C].[CustomerName], CONVERT(date, [O].[OrderDate]), CONVERT(date, [O].[FilledDate]), [O].[Status], [O].[Amount]  
      FROM [Sales].[Customer] AS C  
      INNER JOIN [Sales].[Orders] AS O  
         ON [O].[CustomerID] = [C].[CustomerID]  
      WHERE [C].[CustomerID] = @CustomerID  
    END  
    GO  
    ```  
  
5.  Salvare il file. Prendere nota del percorso poiché sarà necessario utilizzare questo script nella procedura successiva.  
  
6.  Scegliere **Chiudi soluzione** dal menu **File**.  
  
    Successivamente è possibile creare un progetto di database e importare lo schema dallo script creato.  
  
## <a name="create-a-database-project-and-import-a-schema"></a><a name="CreateProjectAndImport"></a>Creare un progetto di database e importare uno schema  
  
#### <a name="to-create-a-database-project"></a>Per creare un progetto di database  
  
1.  Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.  
  
    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .  
  
2.  In **Modelli installati** selezionare il nodo **SQL Server** e quindi selezionare **Progetto di database di SQL Server**.  
  
3.  In **Nome** digitare **SimpleUnitTestDB**.  
  
4.  Se non è già selezionata, selezionare la casella di controllo **Crea directory per soluzione** .  
  
5.  Se non è già deselezionata, deselezionare la casella di controllo **Aggiungi al controllo del codice sorgente** , quindi fare clic su **OK**.  
  
    Il progetto di database viene creato e visualizzato in **Esplora soluzioni**. Successivamente è possibile importare lo schema del database da uno script.  
  
#### <a name="to-import-a-database-schema-from-a-script"></a>Per importare uno schema del database da uno script  
  
1.  Scegliere **Importa** dal menu **Progetto** e quindi selezionare **Script (\*.sql)** .  
  
2.  Fare clic su **Avanti** dopo aver letto la pagina di benvenuto.  
  
3.  Fare clic su **Sfoglia** e accedere alla directory in cui è stato salvato il file con estensione SQL.  
  
4.  Fare doppio clic sul file con estensione SQL e fare clic su **Fine**.  
  
    Lo script viene importato e gli oggetti in esso definiti vengono aggiunti al progetto di database.  
  
5.  Esaminare il riepilogo, quindi fare clic su **Fine** per completare l'operazione.  
  
    > [!NOTE]  
    > Nella stored procedure Sales.uspFillOrder è incluso un errore di codifica intenzionale che verrà individuato e corretto in un secondo momento in questa procedura.  
  
#### <a name="to-examine-the-resulting-project"></a>Per esaminare il progetto risultante  
  
1.  In **Esplora soluzioni** esaminare i file di script che sono stati importati nel progetto.  
  
2.  In **Esplora oggetti di SQL Server** esaminare il database nel nodo Progetti.  
  
## <a name="deploying-to-localdb"></a><a name="DeployDBProj"></a>Distribuzione nel database locale (LocalDB)  
Per impostazione predefinita, quando si preme F5 il database viene distribuito (o pubblicato) in un database LocalDB. È possibile modificare il percorso del database accedendo alla scheda Debug della pagina delle proprietà del progetto e cambiando la stringa di connessione.  
  
## <a name="create-sql-server-unit-tests"></a><a name="CreateDBUnitTests"></a>Creare unit test di SQL Server  
  
#### <a name="to-create-a-sql-server-unit-test-for-the-stored-procedures"></a>Per creare uno unit test di SQL Server per le stored procedure  
  
1.  In **Esplora oggetti di SQL Server** espandere il nodo dei progetti per **SimpleUnitTestDB**, quindi espandere **Programmabilità** e il nodo **Stored procedure**.  
  
2.  Fare clic con il pulsante destro del mouse su una delle stored procedure e selezionare **Crea unit test** per visualizzare la finestra di dialogo **Crea unit test**.  
  
3.  Selezionare le caselle di controllo delle cinque stored procedure: **Sales.uspCancelOrder**, **Sales.uspFillOrder**, **Sales.uspNewCustomer**, **Sales.uspPlaceNewOrder** e **Sales.uspShowOrderDetails**.  
  
4.  Nell'elenco a discesa **Progetto** selezionare **Crea nuovo progetto di test Visual C#** .  
  
5.  Accettare i nomi predefiniti per il progetto e la classe e fare clic su **OK**.  
  
6.  In **Esegui unit test utilizzando la connessione dati seguente** della finestra di dialogo relativa alla configurazione di test specificare una connessione al database distribuito precedentemente in questa procedura dettagliata. Ad esempio, se venisse usato il percorso di distribuzione predefinito, ossia il database LocalDB, si farebbe clic su **Nuova connessione** e si specificherebbe **(LocalDB)\Projects**. Successivamente, scegliere il nome del database. Quindi fare clic su OK per chiudere la finestra di dialogo **Proprietà connessione** .  
  
    > [!NOTE]  
    > Se è necessario testare viste o stored procedure con autorizzazioni limitate, è in genere consigliabile specificare la connessione appropriata in questo passaggio. Per convalidare il test è quindi opportuno specificare la connessione secondaria con autorizzazioni più ampie. Se si dispone di una connessione secondaria, è consigliabile aggiungere il relativo utente al progetto di database e creare un account di accesso per l'utente in questione nello script pre-distribuzione.  
  
7.  Nella sezione **Distribuzione** della finestra di dialogo relativa alla configurazione di test selezionare la casella di controllo **Distribuisci automaticamente il progetto di database prima dell'esecuzione degli unit test** .  
  
8.  In **Progetto di database** fare clic su **SimpleUnitTestDB.sqlproj**.  
  
9. In **Configurazione distribuzione** fare clic su **Debug**.  
  
    È anche possibile generare dati di test come parte degli unit test di SQL Server. Per questa procedura dettagliata, tale passaggio verrà ignorato in quanto i dati verranno creati dai test.  
  
10. Fare clic su **OK**.  
  
    Il progetto di test viene compilato e viene visualizzata la finestra di progettazione unit test di SQL Server. Successivamente sarà possibile aggiornare la logica del test nello script Transact\-SQL degli unit test.  
  
## <a name="define-test-logic"></a><a name="DefineTestLogic"></a>Definire la logica del test  
In questo database molto semplice sono contenute due tabelle, Customer e Order. L'aggiornamento del database si effettua utilizzando le stored procedure seguenti:  
  
-   uspNewCustomer: tramite questa stored procedure viene aggiunto un record alla tabella Customer in cui le colonne YTDOrders e YTDSales del cliente vengono impostate su zero.  
  
-   uspPlaceNewOrder: tramite questa stored procedure viene aggiunto un record alla tabella Orders per il cliente specificato e viene aggiornato il valore di YTDOrders nel record corrispondente della tabella Customer.  
  
-   uspFillOrder: tramite questa stored procedure viene aggiornato un record nella tabella Orders modificando lo stato da "O" a "F" e viene aumentato l'importo di YTDSales nel record corrispondente della tabella Customer.  
  
-   uspCancelOrder: tramite questa stored procedure viene aggiornato un record nella tabella Orders modificando lo stato da "O" a "X" e viene diminuito l'importo di YTDOrders nel record corrispondente della tabella Customer.  
  
-   uspShowOrderDetails: tramite questa stored procedure viene creato un join della tabella Orders alla tabella Custom e vengono visualizzati i record di un cliente specifico.  
  
> [!NOTE]  
> Questo esempio illustra come creare uno unit test di SQL Server semplice. In un database reale è possibile sommare gli importi totali di tutti gli ordini con stato "O" o "F" per un cliente specifico. Nelle stored procedure in questa procedura dettagliata non sono inoltre incluse funzioni di gestione degli errori. Ad esempio, non vengono impedite chiamate a uspFillOrder per un ordine già compilato.  
  
Per i test, si presuppone che il database venga avviato in uno stato pulito. Verranno creati test tramite cui vengono verificate le condizioni seguenti:  
  
-   uspNewCustomer: viene verificato che nella tabella Customer è contenuta una riga dopo l'esecuzione della stored procedure.  
  
-   uspPlaceNewOrder: per il cliente il cui CustomerID è 1, viene effettuato un ordine di 100 dollari US. Viene verificato che l'importo di YTDOrders per il cliente in questione sia 100 e che l'importo di YTDSales sia zero.  
  
-   uspFillOrder: per il cliente il cui CustomerID è 1, viene effettuato un ordine di 50 dollari US. Viene compilato l'ordine in questione. Viene verificato che gli importi di YTDSales e YTDOrders siano entrambi pari a 50.  
  
-   uspShowOrderDetails: per il cliente il cui CustomerID è 1, vengono effettuati ordini di 100, 50 e 5 dollari US. Viene verificato che tramite uspShowOrderDetails venga restituito il numero corretto di colonne e che il checksum del set di risultati sia quello previsto.  
  
> [!NOTE]  
> Per un set completo di unit test di SQL Server, è in genere consigliabile verificare che le altre colonne siano impostate correttamente. Per evitare di rendere eccessivamente complessa questa procedura dettagliata, non viene descritto come verificare il comportamento di uspCancelOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspnewcustomer"></a>Per scrivere lo unit test di SQL Server per uspNewCustomer  
  
1.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspNewCustomerTest** e verificare che **Test** sia evidenziato nell'elenco adiacente.  
  
    Dopo aver eseguito il passaggio precedente, è possibile creare lo script di test per l'azione di test nello unit test.  
  
2.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspNewCustomer  
    DECLARE @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @CustomerName = 'Fictitious Customer';  
  
    EXECUTE @RC = [Sales].[uspNewCustomer] @CustomerName;  
  
    SELECT * FROM [Sales].[Customer];  
    ```  
  
3.  Nel riquadro **Condizioni di test** fare clic sulla la condizione di test Senza risultati e quindi fare clic sull'icona **Elimina condizione di test** (X rossa).  
  
4.  Nel riquadro **Condizioni di test** fare clic su **Conteggio righe** nell'elenco e quindi fare clic sull'icona **Aggiungi condizione di test** (segno + verde).  
  
5.  Aprire la finestra **Proprietà** (selezionare la condizione di test e premere F4) e impostare la proprietà **Conteggio righe** su 1.  
  
6.  Scegliere **Salva tutti** dal menu **File**.  
  
    Successivamente è possibile definire la logica dello unit test per uspPlaceNewOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspplaceneworder"></a>Per scrivere lo unit test di SQL Server per uspPlaceNewOrder  
  
1.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspPlaceNewOrderTest** e verificare che **Test** sia evidenziato nell'elenco adiacente.  
  
    Dopo aver eseguito questo passaggio, è possibile creare lo script di test per l'azione di test nello unit test.  
  
2.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspPlaceNewOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDOrders] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC  
    ```  
  
3.  Nel riquadro **Condizioni di test** selezionare la condizione di test Senza risultati e fare clic su **Elimina condizione di test**.  
  
4.  Nel riquadro **Condizioni di test** selezionare **Valore scalare** nell'elenco, quindi fare clic su **Aggiungi condizione di test**.  
  
5.  Nella finestra **Proprietà** impostare la proprietà **Valore previsto** su 100.  
  
6.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspPlaceNewOrderTest** e verificare che **Pre-test** sia evidenziato nell'elenco adiacente.  
  
    Dopo aver eseguito questo passaggio, è possibile specificare istruzioni tramite cui viene assegnato ai dati lo stato necessario per eseguire il test. Per questo esempio, è necessario creare il record del cliente prima di poter effettuare un ordine.  
  
7.  Fare clic su **Fare clic qui per creare** per creare uno script di pre-test.  
  
8.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    -- Add a customer for this test with the name 'Fictitious Customer'  
    DECLARE @NewCustomerID AS INT, @CustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
       @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
    ```  
  
9. Scegliere **Salva tutti** dal menu **File**.  
  
    Successivamente è possibile creare lo unit test per uspFillOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspfillorder"></a>Per scrivere lo unit test di SQL Server per uspFillOrder  
  
1.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspFillOrderTest** e verificare che **Test** sia evidenziato nell'elenco adiacente.  
  
    Dopo aver eseguito questo passaggio, è possibile creare lo script di test per l'azione di test nello unit test.  
  
2.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDSales] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC;  
    ```  
  
3.  Nel riquadro **Condizioni di test** selezionare la condizione di test Senza risultati e fare clic su **Elimina condizione di test**.  
  
4.  Nel riquadro **Condizioni di test** selezionare **Valore scalare** nell'elenco, quindi fare clic su **Aggiungi condizione di test**.  
  
5.  Nella finestra **Proprietà** impostare la proprietà **Valore previsto** su 100.  
  
6.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspFillOrderTest** e verificare che **Pre-test** sia evidenziato nell'elenco adiacente. Dopo aver eseguito questo passaggio, è possibile specificare istruzioni tramite cui viene assegnato ai dati lo stato necessario per eseguire il test. Per questo esempio, è necessario creare il record del cliente prima di poter effettuare un ordine.  
  
7.  Fare clic su **Fare clic qui per creare** per creare uno script di pre-test.  
  
8.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
9. Scegliere **Salva tutti** dal menu **File**.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspshoworderdetails"></a>Per scrivere lo unit test di SQL Server per uspShowOrderDetails  
  
1.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspShowOrderDetailsTest** e verificare che **Test** sia evidenziato nell'elenco adiacente.  
  
    Dopo aver eseguito questo passaggio, è possibile creare lo script di test per l'azione di test nello unit test.  
  
2.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  Nel riquadro **Condizioni di test** selezionare la condizione di test Senza risultati e fare clic su **Elimina condizione di test**.  
  
4.  Nel riquadro **Condizioni di test** selezionare **Schema previsto** nell'elenco, quindi fare clic su **Aggiungi condizione di test**.  
  
5.  Nella finestra **Proprietà** fare clic sul pulsante Sfoglia ( **...** ) nella proprietà **Configurazione**.  
  
6.  Nella finestra di dialogo **Configurazione per expectedSchemaCondition1** specificare una connessione al database. Ad esempio, se venisse usato il percorso di distribuzione predefinito, ossia il database LocalDB, si farebbe clic su **Nuova connessione** e si specificherebbe **(LocalDB)\Projects**. Successivamente, scegliere il nome del database.  
  
7.  Fare clic su **Recupera**. Se necessario, fare clic su **Recupera** finché non vengono visualizzati i dati.  
  
    Il corpo \- dello unit test viene eseguito e lo schema risultante viene visualizzato nella finestra di dialogo. Poiché il codice di pre-test non è stato eseguito, non vengono restituiti dati. Dal momento che si sta eseguendo solo la verifica dello schema e non dei dati, l'operazione è corretta.  
  
8.  Fare clic su **OK**.  
  
    Lo schema previsto viene archiviato con la condizione di test.  
  
9. Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspShowOrderDetailsTest** e verificare che **Pre-test** sia evidenziato nell'elenco adiacente. Dopo aver eseguito questo passaggio, è possibile specificare istruzioni tramite cui viene assegnato ai dati lo stato necessario per eseguire il test. Per questo esempio, è necessario creare il record del cliente prima di poter effettuare un ordine.  
  
10. Fare clic su **Fare clic qui per creare** per creare uno script di pre-test.  
  
11. Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'FictitiousCustomer'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
12. Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspShowOrderDetailsTest** e su **Test** nell'elenco adiacente.  
  
    Questa operazione è necessaria perché si desidera applicare la condizione di checksum al test, non al pre-test.  
  
13. Nel riquadro **Condizioni di test** selezionare **Checksum di dati** nell'elenco, quindi fare clic su **Aggiungi condizione di test**.  
  
14. Nella finestra **Proprietà** fare clic sul pulsante Sfoglia ( **...** ) nella proprietà **Configurazione**.  
  
15. Nella finestra di dialogo **Configurazione per checksumCondition1** specificare una connessione al database.  
  
16. Sostituire il codice Transact\-SQL nella finestra di dialogo (sotto il pulsante **Modifica connessione**) con il seguente:  
  
    ```  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @FilledDate AS DATETIME;  
    DECLARE @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
    Tramite questo codice viene combinato il codice Transact\-SQL del pre-test con il codice Transact\-SQL del test stesso. È necessario il codice di entrambi affinché vengano restituiti gli stessi risultati che si otterranno al momento dell'esecuzione del test.  
  
17. Fare clic su **Recupera**. Se necessario, fare clic su **Recupera** finché non vengono visualizzati i dati.  
  
    Il codice Transact\-SQL specificato viene eseguito e viene calcolato un checksum per i dati restituiti.  
  
18. Fare clic su **OK**.  
  
    Il checksum calcolato viene archiviato con la condizione di test. Il checksum previsto viene visualizzato nella colonna relativa al valore della condizione di test Checksum di dati.  
  
19. Scegliere **Salva tutti** dal menu **File**.  
  
    A questo punto, è possibile eseguire i test.  
  
## <a name="run-sql-server-unit-tests"></a><a name="RunTests"></a>Eseguire unit test di SQL Server  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>Per eseguire unit test di SQL Server  
  
1.  Scegliere **Finestre** dal menu **Test** e quindi fare clic su **Visualizzazione test** in Visual Studio 2010 o su **Esplora test** in Visual Studio 2012.  
  
2.  Nella finestra **Visualizzazione test** (Visual Studio 2010) fare clic su **Aggiorna** sulla barra degli strumenti per aggiornare l'elenco dei test. Per visualizzare l'elenco di test in **Esplora test** (Visual Studio 2012), compilare la soluzione.  
  
    Nella finestra **Visualizzazione test** o **Esplora test** sono elencati i test creati precedentemente in questa procedura dettagliata e a cui sono state aggiunte condizioni di test e istruzioni Transact\-SQL. Il test denominato TestMethod1 è vuoto e non viene utilizzato in questa procedura dettagliata.  
  
3.  Fare clic con il pulsante destro del mouse su **Sales_uspNewCustomerTest** e scegliere **Esegui selezione**.  
  
    Visual Studio usa il contesto autorizzato specificato dall'utente per connettersi al database e applicare il piano di generazione dati. Visual Studio passa quindi al contesto di esecuzione prima di eseguire lo script Transact\-SQL nel test. Infine, Visual Studio valuta i risultati dello script Transact\-SQL rispetto a quelli specificati nella condizione di test. L'esito della valutazione, positivo o negativo, viene visualizzato nella finestra **Risultati test**.  
  
4.  I risultati vengono visualizzati nella finestra **Risultati test** .  
  
    Il test viene superato, vale a dire che in seguito all'esecuzione dell'istruzione **SELECT** viene restituita una riga.  
  
5.  Ripetere il passaggio 3 per i test Sales_uspPlaceNewOrderTest, Sales_uspFillOrderTest e Sales_uspShowOrderDetailsTest. I risultati saranno simili ai seguenti:  
  
    |Test|Risultato previsto|  
    |--------|-------------------|  
    |Sales_uspPlaceNewOrderTest|Pass|  
    |Sales_uspShowOrderDetailsTest|Pass|  
    |Sales_uspFillOrderTest|Non superato con l'errore seguente: "Condizione ScalarValueCondition (scalarValueCondition2) non riuscita: ResultSet 1 Riga 1 Colonna 1: valori non corrispondenti, '-100' effettivi, '100' previsti". Questo errore si verifica perché la definizione della stored procedure contiene un errore secondario.|  
  
    Successivamente sarà possibile correggere l'errore ed eseguire nuovamente il test.  
  
#### <a name="to-correct-the-error-in-salesuspfillorder"></a>Per correggere l'errore in Sales.uspFillOrder  
  
1.  Nel nodo dei progetti di **Esplora oggetti di SQL Server** per il database, fare doppio clic sulla stored procedure **uspFillOrder** per aprire la relativa definizione nell'editor Transact\-SQL.  
  
2.  Nella definizione individuare l'istruzione Transact\-SQL seguente:  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
3.  Modificare la clausola SET nell'istruzione affinché corrisponda all'istruzione seguente:  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales + @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
4.  Scegliere **Salva uspFillOrder.sql** dal menu **File**.  
  
5.  In **Visualizzazione test** fare clic con il pulsante destro del mouse su **Sales_uspFillOrderTest** e scegliere **Esegui selezione**.  
  
    Il test viene superato.  
  
## <a name="add-a-negative-unit-test"></a><a name="NegativeTest"></a>Aggiungere uno unit test negativo  
È possibile creare un test negativo per verificare che un test effettivamente non venga superato quando è previsto che abbia esito negativo. Ad esempio, se si tenta di annullare un ordine già compilato, l'esito del test deve essere negativo. In questa parte della procedura dettagliata verrà creato uno unit test negativo per la stored procedure Sales.uspCancelOrder.  
  
Per creare e verificare un test negativo, è necessario effettuare le attività seguenti:  
  
-   Aggiornare la stored procedure per testare le condizioni di errore  
  
-   Definire un nuovo unit test  
  
-   Modificare il codice per lo unit test affinché venga indicato l'esito negativo previsto  
  
-   Eseguire lo unit test  
  
#### <a name="to-update-the-stored-procedure"></a>Per aggiornare la stored procedure  
  
1.  Nel nodo dei progetti di **Esplora oggetti di SQL Server** per il database SimpleUnitTestDB espandere i nodi Programmabilità e Stored procedure, quindi fare doppio clic su uspCancelOrder.  
  
2.  Nell'editor Transact\-SQL aggiornare la definizione della stored procedure in modo che corrisponda al codice seguente:  
  
    ```  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
        DECLARE @Delta INT, @CustomerID INT, @PriorStatus CHAR(1)  
        BEGIN TRANSACTION  
            BEGIN TRY  
                IF (NOT EXISTS(SELECT [CustomerID] from [Sales].[Orders] WHERE [OrderID] = @OrderID))  
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR( 'That order does not exist.', -- Message text  
                               16, -- severity  
                                1 -- state  
                            ) WITH LOG;  
                END  
  
                SELECT @Delta = [Amount], @CustomerID = [CustomerID], @PriorStatus = [Status]  
                 FROM [Sales].[Orders] WHERE [OrderID] = @OrderID  
  
                IF @PriorStatus <> 'O'   
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR ( 'You can only cancel open orders.', -- Message text  
                                16, -- Severity  
                                1 -- State  
                                ) WITH LOG;  
                END  
                ELSE  
                BEGIN  
                    -- If we make it to here, then we can cancel the order. Update the status to 'X' first...  
                    UPDATE [Sales].[Orders]  
                       SET [Status] = 'X'  
                    WHERE [OrderID] = @OrderID  
                    -- and then remove the amount from the YTDOrders for the customer  
                    UPDATE [Sales].[Customer]  
                           SET  
                               YTDOrders = YTDOrders - @Delta  
                    WHERE [CustomerID] = @CustomerID  
                    COMMIT TRANSACTION  
                    RETURN 1; -- indicate success  
                END  
            END TRY  
            BEGIN CATCH  
                DECLARE @ErrorMessage NVARCHAR(4000);  
                DECLARE @ErrorSeverity INT;  
                DECLARE @ErrorState INT;  
  
                SELECT @ErrorMessage = ERROR_MESSAGE(),  
                       @ErrorSeverity = ERROR_SEVERITY(),  
                       @ErrorState = ERROR_STATE();  
  
                ROLLBACK TRANSACTION  
                -- Use RAISERROR inside the CATCH block to return  
                -- error information about the original error that  
                -- caused execution to jump to the CATCH block.  
                RAISERROR (@ErrorMessage, -- Mesasge text  
                           @ErrorSeverity, -- Severity  
                           @ErrorState -- State  
                          );  
                RETURN 0; -- indicate failure  
            END CATCH;  
    END  
    ```  
  
3.  Scegliere **Salva uspCancelOrder.sql** dal menu **File**.  
  
4.  Premere F5 per distribuire **SimpleUnitTestDB**.  
  
    Vengono distribuiti gli aggiornamenti nella stored procedure uspCancelOrder. Non sono stati modificati altri oggetti, pertanto viene aggiornata solo la stored procedure in questione.  
  
    Successivamente è possibile definire lo unit test associato per questa stored procedure.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspcancelorder"></a>Per scrivere lo unit test di SQL Server per uspCancelOrder  
  
1.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspCancelOrderTest** e verificare che **Test** sia evidenziato nell'elenco adiacente.  
  
    Dopo aver eseguito questo passaggio, è possibile creare lo script di test per l'azione di test nello unit test.  
  
2.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- try to cancel an order for that customer that has already been filled  
    EXECUTE @RC = [Sales].[uspCancelOrder] @OrderID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  Nel riquadro **Condizioni di test** selezionare la condizione di test Senza risultati, quindi fare clic sull'icona **Elimina condizione di test** .  
  
4.  Nel riquadro **Condizioni di test** selezionare **Valore scalare** nell'elenco, quindi fare clic sull'icona **Aggiungi condizione di test** .  
  
5.  Nella finestra **Proprietà** impostare la proprietà **Valore previsto** su 0.  
  
6.  Nella barra di navigazione della finestra di progettazione unit test di SQL Server fare clic su **Sales_uspCancelOrderTest** e verificare che **Pre-test** sia evidenziato nell'elenco adiacente. Dopo aver eseguito questo passaggio, è possibile specificare istruzioni tramite cui viene assegnato ai dati lo stato necessario per eseguire il test. Per questo esempio, è necessario creare il record del cliente prima di poter effettuare un ordine.  
  
7.  Fare clic su **Fare clic qui per creare** per creare uno script di pre-test.  
  
8.  Aggiornare le istruzioni Transact\-SQL nell'editor Transact\-SQL in modo che corrispondano a quelle seguenti:  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @FilledDate AS DATETIME, @Status AS CHAR (1), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
       @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
       @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @OrderID = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- fill the order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    COMMIT TRANSACTION  
    ```  
  
9. Scegliere **Salva tutti** dal menu **File**.  
  
    A questo punto, è possibile eseguire i test.  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>Per eseguire unit test di SQL Server  
  
1.  In **Visualizzazione test** fare clic con il pulsante destro del mouse su **Sales_uspCancelOrderTest** e scegliere **Esegui selezione**.  
  
2.  I risultati vengono visualizzati nella finestra **Risultati test** .  
  
    Il test non viene superato e viene visualizzato l'errore seguente:  
  
    **Il metodo di test TestProject1.SqlServerUnitTests1.Sales_uspCancelOrderTest ha generato un'eccezione: System.Data.SqlClient.SqlException: è possibile annullare solo ordini aperti.**  
  
    Successivamente è possibile modificare il codice per indicare che l'eccezione è prevista.  
  
#### <a name="to-modify-the-code-for-the-unit-test"></a>Per modificare il codice per lo unit test  
  
1.  In **Esplora soluzioni** espandere **TestProject1**, fare clic con il pulsante destro del mouse su **SqlServerUnitTests1.cs** e scegliere **Visualizza codice**.  
  
2.  Nell'editor del codice passare al metodo Sales_uspCancelOrderTest. Modificare gli attributi del metodo affinché corrispondano al codice seguente:  
  
    ```  
    [TestMethod(), ExpectedSqlException(Severity=16, MatchFirstError=false, State=1)]  
    public void Sales_uspCancelOrderTest()  
    ```  
  
    È possibile specificare che è prevista un'eccezione specifica. Facoltativamente è possibile indicare un numero di errore specifico. Se non si aggiunge questo attributo, l'esito dello unit test sarà negativo e nella finestra Risultati test verrà visualizzato un messaggio.  
  
    > [!IMPORTANT]  
    > Attualmente, Visual Studio 2012 non supporta l'attributo ExpectedSqlException. Per informazioni sulla risoluzione di questo problema, vedere [Impossibile eseguire lo unit test del database "Errore previsto"](https://social.msdn.microsoft.com/Forums/en-US/e74e06ad-e3c9-4cb0-97ad-a6f235a52345/unable-to-run-quotexpected-failurequot-database-unit-test).  
  
3.  Scegliere Salva SqlServerUnitTests1.cs dal menu File.  
  
    Successivamente è possibile eseguire di nuovo lo unit test per verificare che abbia esito negativo come previsto.  
  
#### <a name="to-re-run-the-sql-server-unit-tests"></a>Per eseguire di nuovo gli unit test di SQL Server  
  
1.  In **Visualizzazione test** fare clic con il pulsante destro del mouse su **Sales_uspCancelOrderTest** e scegliere **Esegui selezione**.  
  
2.  I risultati vengono visualizzati nella finestra **Risultati test** .  
  
    Il test viene superato, pertanto l'esito della stored procedure è negativo come previsto.  
  
## <a name="next-steps"></a>Passaggi successivi  
In un progetto tipico verrebbero definiti ulteriori unit test per verificare il corretto funzionamento di tutti gli oggetti di database critici. Una volta completato il set di test, questi ultimi verrebbero archiviati nel controllo delle versioni affinché possano essere condivisi con il team.  
  
Dopo aver stabilito una linea di base, è possibile creare e modificare oggetti di database, quindi creare test associati per verificare se una modifica può compromettere il comportamento previsto.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Procedura: Creare uno unit test di SQL Server vuoto](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
