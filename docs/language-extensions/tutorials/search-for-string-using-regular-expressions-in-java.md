---
title: 'Esercitazione: Ricerca di stringhe regex in Java'
description: Questa esercitazione illustra come usare le estensioni del linguaggio di SQL Server ed eseguire codice Java per la ricerca di una stringa con espressioni regolari (regex).
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 6502be49b2076e8f6d89bdd88695f24b65dc79ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075192"
---
# <a name="tutorial-search-for-a-string-using-regular-expressions-regex-in-java"></a>Esercitazione: cercare una stringa usando espressioni regolari (regex) in Java
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come usare le [estensioni del linguaggio di SQL Server](../language-extensions-overview.md) per creare una classe Java che riceve due colonne (ID e testo) da SQL Server e un'espressione regolare (regex) come parametro di input. La classe restituisce due colonne (ID e testo) a SQL Server.

Per un determinato testo nella colonna di testo inviata alla classe Java, il codice controlla se l'espressione regolare specificata viene soddisfatta e restituisce tale testo insieme all'ID originale.

Il codice di esempio usa un'espressione regolare che controlla se un testo contiene la parola "Java" o "java".

## <a name="prerequisites"></a>Prerequisiti

+ Istanza del motore di database di SQL Server 2019 con il framework di estendibilità e l'estensione di programmazione Java [in Windows](../install/windows-java.md) o [in Linux](../../linux/sql-server-linux-setup-language-extensions-java.md). Per altre informazioni, vedere [Estensione del linguaggio in SQL Server 2019](../language-extensions-overview.md). Per altre informazioni sui requisiti di codifica, vedere [Come chiamare Java in SQL Server](../how-to/call-java-from-sql.md).

+ SQL Server Management Studio o Azure Data Studio per l'esecuzione di T-SQL.

+ Java SE Development Kit (JDK) 8 o JRE 8 in Windows o Linux.

+ File **mssql-java-lang-extension.jar** disponibile in [Microsoft Java Extensibility SDK per Microsoft SQL Server](../how-to/extensibility-sdk-java-sql-server.md).

Per questa esercitazione è sufficiente la compilazione da riga di comando tramite **javac**.

## <a name="create-sample-data"></a>Creare dati di esempio

Per prima cosa, creare un nuovo database e popolare una tabella **testdata** con le colonne **ID** e **text**.

```sql
CREATE DATABASE javatest
GO

USE javatest
GO

CREATE TABLE testdata (
    id int NOT NULL,
    "text" nvarchar(100) NOT NULL
)
GO

-- Insert data into test table
INSERT INTO testdata(id, "text") VALUES (1, 'This sentence contains java')
INSERT INTO testdata(id, "text") VALUES (2, 'This sentence does not')
INSERT INTO testdata(id, "text") VALUES (3, 'I love Java!')
GO
```

## <a name="create-the-main-class"></a>Creare la classe principale

In questo passaggio creare un file di classe denominato **RegexSample.java** e copiare il codice Java seguente nel file.

Questa classe principale importa l'SDK, il che significa che il file con estensione jar scaricato nel passaggio 1 deve essere individuabile da questa classe.

```java
package pkg;

import com.microsoft.sqlserver.javalangextension.PrimitiveDataset;
import com.microsoft.sqlserver.javalangextension.AbstractSqlServerExtensionExecutor;
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.ListIterator;
import java.util.regex.*;

public class RegexSample extends AbstractSqlServerExtensionExecutor {
    private Pattern expr;

    public RegexSample() {
        // Setup the expected extension version, and class to use for input and output dataset
        executorExtensionVersion = SQLSERVER_JAVA_LANG_EXTENSION_V1;
        executorInputDatasetClassName = PrimitiveDataset.class.getName();
        executorOutputDatasetClassName = PrimitiveDataset.class.getName();
    }
    
    public PrimitiveDataset execute(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Validate the input parameters and input column schema
        validateInput(input, params);

        int[] inIds = input.getIntColumn(0);
        String[] inValues = input.getStringColumn(1);
        int rowCount = inValues.length;

        String regexExpr = (String)params.get("regexExpr");
        expr = Pattern.compile(regexExpr);

        System.out.println("regex expression: " + regexExpr);

        // Lists to store the output data
        LinkedList<Integer> outIds = new LinkedList<Integer>();
        LinkedList<String> outValues = new LinkedList<String>();

        // Evaluate each row
        for(int i = 0; i < rowCount; i++) {
            if (check(inValues[i])) {
                outIds.add(inIds[i]);
                outValues.add(inValues[i]);
            }
        }

        int outputRowCount = outValues.size();

        int[] idOutputCol = new int[outputRowCount];
        String[] valueOutputCol = new String[outputRowCount];

        // Convert the list of output columns to arrays
        outValues.toArray(valueOutputCol);

        ListIterator<Integer> it = outIds.listIterator(0);
        int rowId = 0;

        System.out.println("Output data:");
        while (it.hasNext()) {
            idOutputCol[rowId] = it.next().intValue();

            System.out.println("ID: " + idOutputCol[rowId] + " Value: " + valueOutputCol[rowId]);
            rowId++;
        }

        // Construct the output dataset
        PrimitiveDataset output = new PrimitiveDataset();

        output.addColumnMetadata(0, "ID", java.sql.Types.INTEGER, 0, 0);
        output.addColumnMetadata(1, "Text", java.sql.Types.NVARCHAR, 0, 0);

        output.addIntColumn(0, idOutputCol, null);
        output.addStringColumn(1, valueOutputCol);

        return output;
    }

    private void validateInput(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Check for the regex expression input parameter
        if (params.get("regexExpr") == null) {
            throw new IllegalArgumentException("Input parameter 'regexExpr' is not found");
        }

        // The expected input schema should be at least 2 columns, (INTEGER, STRING)
        if (input.getColumnCount() < 2) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }

        // Check that the input column types are expected
        if (input.getColumnType(0) != java.sql.Types.INTEGER &&
                (input.getColumnType(1) != java.sql.Types.VARCHAR && input.getColumnType(1) == java.sql.Types.NVARCHAR )) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }
    }

    private boolean check(String text) {
        Matcher m = expr.matcher(text);

        return m.find();
    }
}
```

## <a name="compile-and-create-a-jar-file"></a>Compilare e creare un file con estensione jar

Assemblare le classi e le dipendenze in un file `.jar`. La maggior parte degli ambienti IDE Java (ad esempio, Eclipse o IntelliJ) supporta la generazione di file `.jar` quando si crea o si compila il progetto. Assegnare al file `.jar` il nome **regex.jar**.

Se non si usa un ambiente IDE Java, è possibile creare manualmente un file `.jar`. Per altre informazioni, vedere [Come creare un file con estensione jar di Java dai file di classe](../how-to/create-a-java-jar-file-from-class-files.md).

> [!NOTE]
> In questa esercitazione vengono usati i pacchetti. La riga `package pkg;` nella parte superiore della classe garantisce che il codice compilato venga salvato in una sottocartella denominata **pkg**. Se si usa un IDE, il codice compilato viene salvato automaticamente in questa cartella. Se si usa **javac** per compilare manualmente le classi, è necessario inserire il codice compilato nella cartella **pkg**.

## <a name="create-external-language"></a>Creare un linguaggio esterno

È necessario creare un linguaggio esterno nel database. Il linguaggio esterno è un oggetto con ambito di database, il che significa che è necessario creare linguaggi esterni come Java per ogni database in cui si vuole usarlo.

### <a name="create-external-language-on-windows"></a>Creare un linguaggio esterno in Windows

Se si usa Windows, attenersi alla procedura seguente per creare un linguaggio esterno per Java.

1. Creare un file ZIP contenente l'estensione.

    Come parte del programma di installazione di SQL Server in Windows, il file **ZIP** dell'estensione Java è installato in questo percorso: `[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip`. Questo file ZIP contiene **javaextension.dll**.

2. Creare un linguaggio esterno Java dal file ZIP:

    ```sql
    CREATE EXTERNAL LANGUAGE Java
    FROM
    (CONTENT = N'[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip', FILE_NAME = 'javaextension.dll',
    ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
    GO
    ```

### <a name="create-external-language-on-linux"></a>Creare un linguaggio esterno in Linux

Come parte del programma di installazione, il file **.tar.gz** dell'estensione viene salvato nel percorso seguente: `/opt/mssql-extensibility/lib/java-lang-extension.tar.gz`.

Per creare un linguaggio esterno Java, eseguire l'istruzione T-SQL seguente in Linux:

```sql
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'/opt/mssql-extensibility/lib/java-lang-extension.tar.gz', file_name = 'javaextension.so',
ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
GO
```

### <a name="permissions-to-execute-external-language"></a>Autorizzazioni per eseguire il linguaggio esterno

Per eseguire il codice Java, un utente deve disporre dell'autorizzazione per l'esecuzione di script esterni in quel linguaggio specifico.

Per altre informazioni, vedere [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="create-external-libraries"></a>Creare librerie esterne

Usare [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) per creare una libreria esterna per i file `.jar`. SQL Server avrà accesso ai file `.jar` e non è necessario impostare autorizzazioni speciali per il **classpath**.

In questo esempio si creeranno due librerie esterne, una per l'SDK e l'altra per il codice Java RegEx.

1. Il file con estensione jar dell'SDK **mssql-java-lang-extension.jar** è installato come parte di SQL Server 2019 in Windows e Linux.

    + Percorso di installazione predefinito in Windows: **[home directory installazione istanza]\MSSQL\Binn\mssql-java-lang-extension.jar**

    + Percorso di installazione predefinito in Linux: **/opt/mssql/lib/mssql-java-lang-extension.jar**

    Il codice è anche open source ed è disponibile nel [repository GitHub delle estensioni del linguaggio di SQL Server](https://github.com/microsoft/sql-server-language-extensions). Per altre informazioni, vedere [Microsoft Extensibility SDK per Java per Microsoft SQL Server](../how-to/extensibility-sdk-java-sql-server.md).

2. Creare una libreria esterna per l'SDK.

    ```sql
    CREATE EXTERNAL LIBRARY sdk
    FROM (CONTENT = '<OS specific path from above>/mssql-java-lang-extension.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

3. Creare una libreria esterna per il codice RegEx.

    ```sql
    CREATE EXTERNAL LIBRARY regex
    FROM (CONTENT = '<path>/regex.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

## <a name="set-permissions"></a>Impostare autorizzazioni

> [!NOTE]
> Ignorare questo passaggio se si usano librerie esterne nel passaggio precedente. Il metodo consigliato consiste nel creare una libreria esterna dal file `.jar`.

Se non si vogliono usare librerie esterne, sarà necessario impostare le autorizzazioni richieste. L'esecuzione dello script ha esito positivo solo se le identità del processo hanno accesso al codice. Per altre informazioni sull'impostazione di autorizzazioni, vedere la [guida all'installazione](../install/windows-java.md).

### <a name="on-linux"></a>In Linux

Concedere autorizzazioni di lettura/esecuzione per il classpath all'utente **mssql_satellite**.

### <a name="on-windows"></a>In Windows

Concedere autorizzazioni di lettura ed esecuzione a **SQLRUserGroup** e l'ID di sicurezza per **Tutti i pacchetti applicazioni** per la cartella contenente il codice Java compilato.

L'intero albero deve avere le autorizzazioni, dal padre di livello radice all'ultima sottocartella.

1. Fare clic con il pulsante destro del mouse sulla cartella, ad esempio `C:\myJavaCode`, e quindi scegliere **Proprietà** > **Sicurezza**.
2. Fare clic su **Modifica**.
3. Scegliere **Aggiungi**.
4. In **Seleziona utenti, computer, account servizio o gruppi**:
   1. Fare clic su **Tipi di oggetto** e assicurarsi che siano selezionati i *principi di sicurezza predefiniti* e i *gruppi*.
   2. Fare clic su **Percorsi** per selezionare il nome del computer locale nella parte superiore dell'elenco.
5. Immettere **SQLRUserGroup**, verificare il nome e fare clic su OK per aggiungere il gruppo.
6. Immettere **TUTTI I PACCHETTI APPLICAZIONI**, verificare il nome e fare clic su OK per aggiungerlo. 
    Se il nome non viene risolto, rivedere il passaggio relativo ai percorsi. L'ID di sicurezza è locale per il computer in uso.

Verificare che entrambe le identità di sicurezza dispongano delle autorizzazioni **Lettura ed esecuzione** per la cartella e la sottocartella **pkg**.

<a name="call-method"></a>

## <a name="call-the-java-class"></a>Chiamare la classe Java

Creare una stored procedure che chiama `sp_execute_external_script` per chiamare il codice Java da SQL Server. Nel parametro **script** definire quale `package.class` si vuole chiamare. Nel codice riportato di seguito la classe appartiene a un pacchetto denominato **pkg** e a un file di classe denominato **RegexSample.java**.

> [!NOTE]
> Il codice non definisce il metodo da chiamare. Per impostazione predefinita, verrà chiamato il metodo **execute**. Ciò significa che è necessario seguire l'interfaccia dell'SDK e implementare un metodo execute nella classe Java, se si vuole essere in grado di chiamare la classe da SQL Server.

La stored procedure accetta una query di input (set di dati di input) e un'espressione regolare e restituisce le righe che hanno soddisfatto l'espressione regolare specificata. Viene usata un'espressione regolare `[Jj]ava` che controlla se un testo contiene la parola **Java** o **java**.

```sql
CREATE OR ALTER PROCEDURE [dbo].[java_regex] @expr nvarchar(200), @query nvarchar(400)
AS
BEGIN
--Call the Java program by giving the package.className in @script
--The method invoked in the Java code is always the "execute" method
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'pkg.RegexSample'
, @input_data_1 = @query
, @params = N'@regexExpr nvarchar(200)'
, @regexExpr = @expr
with result sets ((ID int, text nvarchar(100)));
END
GO

--Now execute the above stored procedure and provide the regular expression and an input query
EXECUTE [dbo].[java_regex] N'[Jj]ava', N'SELECT id, text FROM testdata'
GO
```

### <a name="results"></a>Risultati

Dopo aver eseguito la chiamata, si dovrebbe ottenere un set di risultati con due delle righe.

![Risultati dell'esempio Java](../media/java/java-sample-results.png "Risultati dell'esempio")

### <a name="if-you-get-an-error"></a>Se si riceve un errore

+ Quando si compilano le classi, la sottocartella **pkg** deve contenere il codice compilato per tutte e tre le classi.

+ Se non si usano librerie esterne, controllare le autorizzazioni per *ogni* cartella, dalla sottocartella **radice** a **pkg**, per assicurarsi che le identità di sicurezza che eseguono il processo esterno dispongano delle autorizzazioni per la lettura e l'esecuzione del codice.

## <a name="next-steps"></a>Passaggi successivi

+ [Come chiamare Java in SQL Server](../how-to/call-java-from-sql.md)
+ [Estensioni Java in SQL Server](../language-extensions-overview.md)
+ [Tipi di dati Java e SQL Server](../how-to/java-to-sql-data-types.md)