---
title: Creare frammenti di codice riutilizzabili
description: Informazioni su come creare e usare frammenti di codice SQL di Azure Data Studio, che semplificano la creazione di database e oggetti di database.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: e2ea3a950a548cae6ad23f40055acc09b2e9534f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048691"
---
# <a name="create-and-use-code-snippets-to-quickly-create-transact-sql-t-sql-scripts-in-azure-data-studio"></a>Creare e usare frammenti di codice per creare rapidamente script Transact-SQL (T-SQL) in Azure Data Studio

In Azure Data Studio i frammenti di codice sono modelli che consentono di creare facilmente database e oggetti di database. 

In Azure Data Studio sono disponibili vari frammenti di codice T-SQL che consentono di generare rapidamente la sintassi corretta. 

È possibile creare anche frammenti di codice definiti dall'utente.

## <a name="using-built-in-t-sql-code-snippets"></a>Uso di frammenti di codice T-SQL predefiniti

1. Per accedere ai frammenti di codice disponibili, digitare *sql* nell'editor di query per aprire l'elenco:

   ![frammenti](media/code-snippets/sql-snippets.png)

2. Selezionare il frammento di codice che si vuole usare, da cui verrà generato lo script T-SQL. Selezionare, ad esempio, *sqlCreateTable*:

   ![Creazione di frammenti di codice di tabella](media/code-snippets/create-table.png)

3. Aggiornare i campi evidenziati con i valori specifici appropriati. Sostituire, ad esempio, *TableName* e *Schema* con i valori relativi al database in uso:

   ![Tabella da frammento](media/code-snippets/table-from-snippet.png)

   Se il campo che si vuole modificare non è più evidenziato (questo errore si verifica quando si sposta il cursore all'interno dell'editor), fare clic con il pulsante destro del mouse sulla parola che si vuole modificare e selezionare **Modifica tutte le occorrenze**:

   ![Modifica tutto](media/code-snippets/change-all.png)

4. Aggiornare o aggiungere gli script T-SQL necessari per il frammento di codice selezionato. Aggiornare, ad esempio, *Column1* e *Column2* e aggiungere altre colonne.

## <a name="creating-sql-code-snippets"></a>Creazione di frammenti di codice T-SQL

È possibile definire frammenti di codice personalizzati. Per aprire il file del frammento di codice SQL da modificare:

1. Aprire il *Riquadro comandi* (**MAIUSC + CTRL + P**), digitare *snip* e selezionare **Preferenze: Apri frammenti di codice utente**:

   ![Frammenti utente](media/code-snippets/user-snippets.png)

2. Selezionare **SQL**:

   > [!NOTE]
   > Azure Data Studio eredita la funzionalità dei frammenti di codice da Visual Studio Code ed è per questo motivo che questo articolo analizza nello specifico l'uso di frammenti di codice SQL. Per informazioni più dettagliate, vedere [Creazione di frammenti di codice personalizzati](https://code.visualstudio.com/docs/editor/userdefinedsnippets) nella documentazione di Visual Studio Code. 

   ![Selezione di SQL](media/code-snippets/select-sql.png)

3. Incollare il codice seguente in *sql.json*:

    ```sql
    {
     "Select top 5": {
    "prefix": "sqlSelectTop5",
    "body": "SELECT TOP 5 * FROM ${1:TableName}",
    "description": "User-defined snippet example 1"
    },
    "Create Table snippet":{
    "prefix": "sqlCreateTable2",
    "body": [
    "-- Create a new table called '${1:TableName}' in schema '${2:SchemaName}'",
    "-- Drop the table if it already exists",
    "IF OBJECT_ID('$2.$1', 'U') IS NOT NULL",
    "DROP TABLE $2.$1",
    "GO",
    "-- Create the table in the specified schema",
    "CREATE TABLE $2.$1",
    "(",
    "$1Id INT NOT NULL PRIMARY KEY, -- primary key column",
    "Column1 [NVARCHAR](50) NOT NULL,",
    "Column2 [NVARCHAR](50) NOT NULL",
    "-- specify more columns here",
    ");",
    "GO"
    ],
       "description": "User-defined snippet example 2"
       }
       }
    ```

4. Salvare il file sql.json.

5. Aprire una nuova finestra dell'editor di query facendo clic su **CTRL + N**.

6. Digitare **sql** per visualizzare i due frammenti di codice utente appena aggiunti: *sqlCreateTable2* e *sqlSelectTop5*.

Selezionare uno dei nuovi frammenti di codice e provarlo.

## <a name="next-steps"></a>Passaggi successivi

Per informazioni sull'editor SQL, vedere [Esercitazione sull'editor di codice](tutorial-sql-editor.md).
