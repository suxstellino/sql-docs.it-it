---
description: Eseguire un pacchetto SSIS da Visual Studio Code con Transact-SQL
title: Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c911778dbf9321a09fb901806e50f9fae501507a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336311"
---
# <a name="run-an-ssis-package-from-visual-studio-code-with-transact-sql"></a>Eseguire un pacchetto SSIS da Visual Studio Code con Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come usare Visual Studio Code per connettersi al database del catalogo SSIS e quindi usare istruzioni Transact-SQL per eseguire un pacchetto SSIS archiviato nel catalogo SSIS.

Visual Studio Code è un editor di codice per Windows, macOS e Linux che supporta le estensioni, incluse le estensioni `mssql` per la connessione a Microsoft SQL Server, database SQL di Azure o Azure Synapse Analytics. Per altre informazioni su Visual Studio Code, vedere [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Prerequisites

Prima di iniziare, assicurarsi di aver installato la versione più recente di Visual Studio Code e caricato l'estensione `mssql`. Per scaricare questi strumenti, vedere le pagine seguenti:
-   [Download di Visual Studio Code](https://code.visualstudio.com/Download)
-   [Estensione mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>Piattaforme supportate

È possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS nelle piattaforme seguenti:

-   SQL Server in Windows.

-   Database SQL di Azure. Per altre informazioni sulla distribuzione e l'esecuzione di pacchetti in Azure, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Non è possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS in Linux. Per altre informazioni sull'esecuzione di pacchetti in Linux, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="set-language-mode-to-sql-in-vs-code"></a>Impostare la modalità di linguaggio su SQL in Visual Studio Code

Per abilitare i comandi `mssql` e Transact-SQL IntelliSense, la modalità di linguaggio è impostata su **SQL** in Visual Studio Code.

1. Aprire Visual Studio Code e quindi aprire una nuova finestra. 

2. Fare clic su **Testo normale** nell'angolo inferiore destro della barra di stato.

3. Nel menu a discesa **Seleziona modalità linguaggio** visualizzato selezionare o immettere **SQL** e quindi premere **INVIO** per impostare la modalità del linguaggio su SQL. 

## <a name="for-azure-sql-database-get-the-connection-info"></a>Ottenere le informazioni di connessione per il database SQL di Azure

Per eseguire il pacchetto nel database SQL di Azure, ottenere le informazioni di connessione necessarie per connettersi al database del catalogo SSIS (SSISDB). Nelle procedure che seguono sono necessari il nome completo del server e le informazioni di accesso.

1. Accedere al [Portale di Azure](https://portal.azure.com/).
2. Selezionare **Database SQL** nel menu a sinistra e quindi il database SSISDB nella pagina **Database SQL**. 
3. Nella pagina **Panoramica** del database controllare il nome completo del server. Passare il mouse sul nome del server per visualizzare l'opzione **Fare clic per copiare**. 
4. Se si dimenticano le informazioni di accesso del server di database SQL di Azure, passare alla pagina del server di database SQL per visualizzare il nome amministratore del server. Se necessario, è possibile reimpostare la password.

## <a name="connect-to-the-ssis-catalog-database"></a>Connettersi al database del catalogo SSIS

Usare SQL Visual Studio Code per stabilire una connessione al catalogo SSIS.

> [!IMPORTANT]
> Prima di continuare, verificare che siano disponibili le informazioni relative a server, database e accesso. Se si sposta lo stato attivo da Visual Studio Code quando si inizia a immettere le informazioni sul profilo di connessione, è necessario riavviare la creazione del profilo di connessione.

1. In Visual Studio Code premere **CTRL+MAIUSC+P** (o **F1**) per aprire il riquadro comandi.

2. Digitare **sqlcon** e premere **INVIO**.

3. Premere **INVIO** per iniziare la procedura di **creazione del profilo di connessione**. Questo passaggio consente di creare un profilo di connessione per l'istanza di SQL Server.

4. Seguire le istruzioni per specificare le proprietà di connessione per il nuovo profilo di connessione. Dopo aver specificato ogni valore, premere **INVIO** per continuare. 

   | Impostazione       | Valore consigliato | Altre informazioni |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nome server** | Nome completo del server | Se si sta eseguendo la connessione a un server di database SQL di Azure, il nome è nel formato `<server_name>.database.windows.net`. |
   | **Nome database** | **SSISDB** | Il nome del database a cui si effettua la connessione. |
   | **autenticazione** | Account di accesso SQL | Con l'autenticazione di SQL Server è possibile connettersi a SQL Server o al database SQL di Azure. Se ci si connette a un server di database SQL di Azure, non è possibile usare l'autenticazione di Windows. |
   | **Nome utente** | Account amministratore del server | Account specificato al momento della creazione del server. |
   | **Password (account di accesso SQL)** | Password per l'account amministratore del server | Password specificata al momento della creazione del server. |
   | **Salvare la password?** | Sì o No | Se non si vuole immettere la password ogni volta, selezionare Sì. |
   | **Immettere un nome per questo profilo** | Un nome di profilo, ad esempio **mySSISServer** | Se si salva un nome di profilo, gli accessi successivi saranno più rapidi. | 

5. Premere il tasto **ESC** per chiudere il messaggio che informa che il profilo è stato creato e connesso.

6. Verificare la connessione nella barra di stato.

## <a name="run-the-t-sql-code"></a>Eseguire il codice T-SQL
Eseguire il codice Transact-SQL seguente per eseguire un pacchetto SSIS.

1. Nella finestra **Editor** immettere la query seguente nella finestra di query vuota. Questo è il codice generato dall'opzione **Script** nella finestra di dialogo **Esegui pacchetto** di SSMS.

2. Aggiornare i valori dei parametri nella stored procedure `catalog.create_execution` in base al sistema in uso.

3. Premere **CTRL+MAIUSC+E** per eseguire il codice ed eseguire il pacchetto.

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per eseguire un pacchetto.
    - [Eseguire un pacchetto SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con PowerShell](ssis-quickstart-run-powershell.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md) 
