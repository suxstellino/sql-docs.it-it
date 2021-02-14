---
description: Distribuire un progetto SSIS da Visual Studio Code con Transact-SQL
title: Distribuire un progetto SSIS con Transact-SQL (Visual Studio Code) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d12262ab795551a854dc3c791239cc33b5e1ad68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354286"
---
# <a name="deploy-an-ssis-project-from-visual-studio-code-with-transact-sql"></a>Distribuire un progetto SSIS da Visual Studio Code con Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come usare Visual Studio Code per connettersi al database del catalogo SSIS e quindi usare istruzioni Transact-SQL per distribuire un progetto SSIS nel catalogo SSIS.

Visual Studio Code è un editor di codice per Windows, macOS e Linux che supporta le estensioni, incluse le estensioni `mssql` per la connessione a Microsoft SQL Server, database SQL di Azure o Azure Synapse Analytics. Per altre informazioni su Visual Studio Code, vedere [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi di aver installato la versione più recente di Visual Studio Code e caricato l'estensione `mssql`. Per scaricare questi strumenti, vedere le pagine seguenti:
-   [Download di Visual Studio Code](https://code.visualstudio.com/Download)
-   [Estensione mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>Piattaforme supportate

È possibile usare le informazioni di questa guida introduttiva per distribuire un progetto SSIS nelle piattaforme seguenti:

-   SQL Server in Windows.

Non è possibile usare le informazioni di questa guida introduttiva per distribuire un pacchetto SSIS nel database SQL di Azure. La stored procedure `catalog.deploy_project` prevede che il percorso del file `.ispac` sia nel file system locale. Per altre informazioni sulla distribuzione e l'esecuzione di pacchetti in Azure, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Non è possibile usare le informazioni di questa guida introduttiva per distribuire un pacchetto SSIS a SQL Server in Linux. Per altre informazioni sull'esecuzione di pacchetti in Linux, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="set-language-mode-to-sql-in-vs-code"></a>Impostare la modalità di linguaggio su SQL in Visual Studio Code

Per abilitare i comandi `mssql` e Transact-SQL IntelliSense, impostare la modalità di linguaggio su **SQL** in Visual Studio Code.

1. Aprire Visual Studio Code e quindi aprire una nuova finestra. 

2. Fare clic su **Testo normale** nell'angolo inferiore destro della barra di stato.
 
3. Nel menu a discesa **Seleziona modalità linguaggio** visualizzato selezionare o immettere **SQL** e quindi premere **INVIO** per impostare la modalità del linguaggio su SQL. 

## <a name="supported-authentication-method"></a>Metodo di autenticazione supportata

Vedere [Metodi di autenticazione per la distribuzione](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="connect-to-the-ssis-catalog-database"></a>Connettersi al database del catalogo SSIS

Usare SQL Visual Studio Code per stabilire una connessione al catalogo SSIS.

1. In Visual Studio Code premere **CTRL+MAIUSC+P** (o **F1**) per aprire il riquadro comandi.

2. Digitare **sqlcon** e premere **INVIO**.

3. Premere **INVIO** per iniziare la procedura di **creazione del profilo di connessione**. Questo passaggio consente di creare un profilo di connessione per l'istanza di SQL Server.

4. Seguire le istruzioni per specificare le proprietà di connessione per il nuovo profilo di connessione. Dopo aver specificato ogni valore, premere **INVIO** per continuare. 

   | Impostazione       | Valore consigliato | Altre informazioni |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nome server** | Nome completo del server |  |
   | **Nome database** | **SSISDB** | Il nome del database a cui si effettua la connessione. |
   | **autenticazione** | Account di accesso SQL | |
   | **Nome utente** | Account amministratore del server | Account specificato al momento della creazione del server. |
   | **Password (account di accesso SQL)** | Password per l'account amministratore del server | Password specificata al momento della creazione del server. |
   | **Salvare la password?** | Sì o No | Se non si vuole immettere la password ogni volta, selezionare Sì. |
   | **Immettere un nome per questo profilo** | Un nome di profilo, ad esempio **mySSISServer** | Se si salva un nome di profilo, gli accessi successivi saranno più rapidi. | 

5. Premere il tasto **ESC** per chiudere il messaggio che informa che il profilo è stato creato e connesso.

6. Verificare la connessione nella barra di stato.

## <a name="run-the-t-sql-code"></a>Eseguire il codice T-SQL
Eseguire il codice Transact-SQL seguente per distribuire un progetto SSIS.

1. Nella finestra **Editor** immettere la query seguente nella finestra di query vuota.

2. Aggiornare i valori dei parametri nella stored procedure `catalog.deploy_project` in base al sistema in uso.

3. Premere **CTRL+MAIUSC+E** per eseguire il codice e distribuire il progetto.

```sql
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary = (SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
    @project_name = '<project_name',
    @Project_Stream = @ProjectBinary,
    @operation_id = @operation_id out
```

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per distribuire un pacchetto.
    - [Distribuire un pacchetto SSIS con SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Distribuire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Distribuire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-deploy-cmdline.md)
    - [Distribuire un pacchetto SSIS con PowerShell](ssis-quickstart-deploy-powershell.md)
    - [Distribuire un pacchetto SSIS con C#](./ssis-quickstart-deploy-dotnet.md) 
- Eseguire un pacchetto distribuito. Per eseguire un pacchetto, è possibile scegliere tra diversi strumenti e linguaggi. Per altre informazioni, vedere gli articoli seguenti:
    - [Eseguire un pacchetto SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con PowerShell](ssis-quickstart-run-powershell.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md) 
