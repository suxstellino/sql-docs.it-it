---
title: Usare l'estensione mssql di Visual Studio Code
description: Usare l'estensione mssql per Visual Studio Code per modificare ed eseguire script Transact-SQL per SQL Server in Linux.
ms.topic: conceptual
ms.prod: sql
ms.technology: tools-other
ms.assetid: 9766ee75-32d3-4045-82a6-4c7968bdbaa6
author: markingmyname
ms.author: maghan
ms.date: 10/28/2019
ms.openlocfilehash: fdb3d5e695824117def89347da883a0a7a5d7a9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340547"
---
# <a name="use-visual-studio-code-to-create-and-run-transact-sql-scripts"></a>Usare Visual Studio Code per creare ed eseguire script Transact-SQL

[!INCLUDE [SQL Server - Linux](../../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come usare l'estensione **mssql** per Visual Studio Code per sviluppare i database di SQL Server. Poiché Visual Studio Code è multipiattaforma, è possibile usare l'estensione **mssql** in Linux, macOS e Windows.

## <a name="install-and-start-visual-studio-code"></a>Installare e avviare Visual Studio Code

Visual Studio Code è un editor di codice grafico multipiattaforma che supporta le estensioni.

1. [Scaricare e installare Visual Studio Code](https://code.visualstudio.com/) nel computer.

2. Avviare Visual Studio Code.

    >[!NOTE]
    >Se Visual Studio Code non viene avviato quando si è connessi tramite una sessione desktop remoto xrdp, vedere [VS Code not working on Ubuntu when connected using XRDP](https://github.com/Microsoft/vscode/issues/3451) (VS Code non funziona in Ubuntu quando si è connessi tramite XRDP).

## <a name="install-the-mssql-extension"></a>Installare l'estensione mssql

L'[estensione mssql per Visual Studio Code](https://aka.ms/mssql-marketplace) consente di connettersi a SQL Server, eseguire query con Transact-SQL (T-SQL) e visualizzare i risultati.

1. In Visual Studio Code selezionare **Visualizza** > **Riquadro comandi** oppure premere **CTRL**+**MAIUSC**+**P** o premere **F1** per aprire il **Riquadro comandi**.

2. Nel **Riquadro comandi** selezionare **Estensioni: Installa estensioni** dal menu a discesa.

3. Nel riquadro **Estensioni** digitare *mssql*.

4. Selezionare l'estensione **SQL Server (mssql)** e quindi selezionare **Installa**.

   ![Installare l'estensione mssql](./media/sql-server-develop-use-vscode/vscode-extension.png)

5. Al termine dell'installazione, selezionare **Ricarica** per abilitare l'estensione.

## <a name="create-or-open-a-sql-file"></a>Creare o aprire un file SQL

L'estensione mssql abilita i comandi mssql e T-SQL IntelliSense nell'editor di codice quando la modalità di linguaggio è impostata su **SQL**.

1. Selezionare **File** > **Nuovo file** o premere **CTRL**+**N**. Per impostazione predefinita, Visual Studio Code apre un nuovo file di testo normale. 

2. Selezionare **Testo normale** nella barra di stato inferiore o premere **CTRL**+**K** > **M** e selezionare **SQL** dal menu a discesa dei linguaggi. 

   ![Modalità di linguaggio SQL](./media/sql-server-develop-use-vscode/vscode-language-mode.png)

   > [!NOTE]
   > Se è la prima volta che si usa l'estensione, l'estensione installa gli strumenti di SQL Server di supporto.

Se si apre un file esistente con estensione file *sql*, la modalità di linguaggio viene automaticamente impostata su SQL.  

## <a name="connect-to-sql-server"></a>Connessione a SQL Server

Seguire questa procedura per creare un profilo di connessione e connettersi a un'istanza di SQL Server.

1. Premere **CTRL**+**MAIUSC**+**P** o **F1** per aprire il **Riquadro comandi**. 

2. Digitare *sql* per visualizzare i comandi mssql o digitare *sqlcon* e quindi selezionare **MS SQL: Connect** dal menu a discesa.

   ![Comandi mssql](./media/sql-server-develop-use-vscode/vscode-commands.png)

   >[!NOTE]
   >Per poter eseguire i comandi mssql, un file SQL, ad esempio il file SQL vuoto creato, deve avere lo stato attivo nell'editor di codice.

3. Selezionare il comando **MS SQL: Manage Connection Profiles**.

4. Selezionare quindi **Crea** per creare un nuovo profilo di connessione per SQL Server.

5. Seguire le istruzioni per specificare le proprietà per il nuovo profilo di connessione. Dopo aver specificato ogni valore, premere **INVIO** per continuare.

   | Proprietà di connessione | Descrizione |
   |---|---|
   | **Server name or ADO connection string** (Nome del server o stringa di connessione ADO) | Specificare il nome dell'istanza di SQL Server. Usare *localhost* per connettersi a un'istanza di SQL Server nel computer locale. Per connettersi a un'istanza di SQL Server remota, immettere il nome o l'indirizzo IP dell'istanza di SQL Server di destinazione. Per connettersi a un contenitore di SQL Server, specificare l'indirizzo IP del computer host del contenitore. Se è necessario specificare una porta, usare una virgola per separarla dal nome. Per un server in ascolto sulla porta 1401, ad esempio, immettere `<servername or IP>,1401`.<br/><br/>In alternativa, è possibile immettere qui la stringa di connessione ADO per il database. |
   | **Nome di database** (facoltativo) | Database che si vuole usare. Per connettersi al database predefinito, non specificare un nome di database. |
   | **Tipo di autenticazione** | Scegliere **Integrata** o **Account di accesso SQL**. |
   | **Nome utente** | Se si è selezionato **Account di accesso SQL**, immettere il nome di un utente con accesso a un database nel server. |
   | **Password** | Immettere la password per l'utente specificato. |
   | **Salva password** | Premere **INVIO** per selezionare **Sì** e salvare la password. Selezionare **No** per fare in modo che la password venga chiesta ogni volta che si usa il profilo di connessione. |
   | **Nome profilo** (facoltativo) | Digitare un nome per il profilo di connessione, ad esempio *localhost profile*. |

   Dopo aver immesso tutti i valori e aver selezionato **Invia**, Visual Studio Code crea il profilo di connessione e si connette a SQL Server.

   > [!TIP]
   > Se la connessione non riesce, provare a diagnosticare il problema dal messaggio di errore nel pannello **Output** di Visual Studio Code. Per aprire il pannello **Output**, selezionare **Visualizza** > **Output**. Rivedere anche i [consigli per la risoluzione dei problemi di connessione](../../linux/sql-server-linux-troubleshooting-guide.md).

6. Verificare la connessione nella barra di stato inferiore.

   ![Stato della connessione](./media/sql-server-develop-use-vscode/vscode-connection-status.png)

In alternativa ai passaggi precedenti, è anche possibile creare ed eliminare i profili di connessione nel file delle impostazioni utente (*settings.json*). Per aprire il file delle impostazioni, selezionare **File** > **Preferenze** > **Impostazioni**. Per altre informazioni, vedere [Manage connection profiles](https://github.com/Microsoft/vscode-mssql/wiki/manage-connection-profiles) (Gestire i profili di connessione).

## <a name="create-a-sql-database"></a>Creare un database SQL

1. Nel nuovo file SQL avviato in precedenza, digitare *sql* per visualizzare un elenco di frammenti di codice modificabili.

   ![Frammenti SQL](./media/sql-server-develop-use-vscode/vscode-sql-snippets.png)

2. Selezionare **sqlCreateDatabase**.

3. Nel frammento digitare `TutorialDB` per sostituire "DatabaseName":

   ```sql
   -- Create a new database called 'TutorialDB'
   -- Connect to the 'master' database to run this snippet
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

4. Premere **CTRL**+**MAIUSC**+**E** per eseguire i comandi Transact-SQL. Visualizzare i risultati nella finestra di query.

    ![Creare i messaggi del database](./media/sql-server-develop-use-vscode/vscode-create-database-messages.png)

    > [!TIP]
    > È possibile personalizzare la combinazione di tasti per i comandi mssql. Vedere [Customize shortcuts](https://github.com/Microsoft/vscode-mssql/wiki/customize-shortcuts) (Personalizzare le scelte rapide da tastiera).

## <a name="create-a-table"></a>Creare una tabella

1. Eliminare il contenuto della finestra dell'editor di codice.

2. Premere **CTRL**+**MAIUSC**+**P** o **F1** per aprire il **Riquadro comandi**.

3. Digitare *sql* per visualizzare i comandi mssql o digitare *sqluse* e quindi selezionare il comando **MS SQL: Use Database**.

4. Selezionare il nuovo database **TutorialDB**.

   ![Usare il database](./media/sql-server-develop-use-vscode/vscode-use-database.png)

5. Nell'editor di codice digitare *sql* per visualizzare i frammenti, selezionare **sqlCreateTable** e quindi premere **INVIO**.

6. Nel frammento digitare `Employees` come nome della tabella.

7. Premere **TAB** per passare al campo successivo e quindi digitare `dbo` come nome dello schema.

8. Sostituire le definizioni di colonna con le colonne seguenti:

   ```sql
   EmployeesId INT NOT NULL PRIMARY KEY,
   Name [NVARCHAR](50)  NOT NULL,
   Location [NVARCHAR](50)  NOT NULL
   ```

9. Premere **CTRL**+**MAIUSC**+**E** per creare la tabella.

## <a name="insert-and-query"></a>Inserimento e query

1. Aggiungere le istruzioni seguenti per inserire quattro righe nella tabella **Employees**.

   ```sql
   -- Insert rows into table 'Employees'
   INSERT INTO Employees
      ([EmployeesId],[Name],[Location])
   VALUES
      ( 1, N'Jared', N'Australia'),
      ( 2, N'Nikita', N'India'),
      ( 3, N'Tom', N'Germany'),
      ( 4, N'Jake', N'United States')
   GO
   -- Query the total count of employees
   SELECT COUNT(*) as EmployeeCount FROM dbo.Employees;
   -- Query all employee information
   SELECT e.EmployeesId, e.Name, e.Location
   FROM dbo.Employees as e
   GO
   ```

   Durante la digitazione, T-SQL IntelliSense consente di completare le istruzioni:

   ![T-SQL IntelliSense](./media/sql-server-develop-use-vscode/vscode-intellisense.png)

   > [!TIP]
   > L'estensione mssql ha anche i comandi per la creazione di istruzioni INSERT e SELECT. Questi comandi non sono stati usati nell'esempio precedente.

2. Premere **CTRL**+**MAIUSC**+**E** per eseguire i comandi. I due set di risultati vengono visualizzati nella finestra **Risultati**.

   ![Risultati](./media/sql-server-develop-use-vscode/vscode-result-grid.png)

## <a name="view-and-save-the-result"></a>Visualizzare e salvare il risultato

1. Selezionare **Visualizza** > **Editor Layout** (Layout dell'editor) > **Flip Layout** (Capovolgi il layout) per passare a un layout con divisione orizzontale o verticale.

2. Selezionare le intestazione dei pannelli **Risultati** e **Messaggi** per comprimere ed espandere i pannelli.

   ![Attivare/Disattivare le intestazioni](./media/sql-server-develop-use-vscode/vscode-toggle-messages-pannel.png)

   > [!TIP]
   > È possibile personalizzare il comportamento predefinito dell'estensione mssql. Vedere [Customize extension options](https://github.com/Microsoft/vscode-mssql/wiki/customize-options) (Personalizzare le opzioni dell'estensione).

3. Selezionare l'icona per ingrandire la griglia sulla seconda griglia dei risultati per ingrandirli.

   ![Ingrandire la griglia](./media/sql-server-develop-use-vscode/vscode-maximize-grid.png)

   > [!NOTE]
   > L'icona di ingrandimento viene visualizzata quando lo script T-SQL genera due o più griglie di risultati.

4. Aprire il menu di scelta rapida della griglia facendo clic con il pulsante destro del mouse sulla griglia.

   ![Menu di scelta rapida](./media/sql-server-develop-use-vscode/vscode-grid-context-menu.png)

5. Selezionare **Seleziona tutto**.

6. Aprire di nuovo il menu di scelta rapida della griglia e selezionare **Save as JSON** (Salva come JSON) per salvare il risultato in un file *JSON*.

7. Specificare un nome per il file JSON.

8. Verificare che il file JSON venga salvato e aperto in Visual Studio Code.

   ![Salvare come JSON](./media/sql-server-develop-use-vscode/vscode-save-as-json.png)

Se è necessario salvare ed eseguire gli script SQL in un secondo momento, per l'amministrazione o un progetto di sviluppo più grande, salvare gli script con l'estensione *sql*.

## <a name="next-steps"></a>Passaggi successivi

Se non si ha familiarità con T-SQL, vedere [Esercitazione: Scrittura di istruzioni Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md) e [Guida di riferimento a Transact-SQL (Motore di database)](../../t-sql/language-reference.md).

Per altre informazioni sull'uso o sul contributo all'estensione mssql, vedere la [wiki di progetto dell'estensione mssql](https://github.com/Microsoft/vscode-mssql/wiki).

Per altre informazioni sull'uso di Visual Studio Code, vedere la [documentazione di Visual Studio Code](https://code.visualstudio.com/docs).