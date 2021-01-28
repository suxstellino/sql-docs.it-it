---
title: Installare pacchetti R con sqlmlutils
description: Informazioni su come usare sqlmlutils per installare nuovi pacchetti R in un'istanza di Machine Learning Services per SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/15/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: bdd8189559bc3de1659e4874f80f8862dc341b1d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596280"
---
# <a name="install-r-packages-with-sqlmlutils"></a>Installare pacchetti R con sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Questo articolo descrive come usare le funzioni nel pacchetto [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) per installare pacchetti R in un'istanza di [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) e in [cluster Big Data](../../big-data-cluster/machine-learning-services.md). I pacchetti installati possono essere usati negli script R in esecuzione nel database usando l'istruzione T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

> [!NOTE]
> Il pacchetto **sqlmlutils** descritto in questo articolo viene usato per aggiungere pacchetti R a SQL Server 2019 o versione successiva. Per SQL Server 2017 e versioni precedenti, vedere [Installare i pacchetti con gli strumenti R](./install-r-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Questo articolo descrive come usare le funzioni nel pacchetto [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) per installare pacchetti R in un'istanza di [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). I pacchetti installati possono essere usati negli script R in esecuzione nel database usando l'istruzione T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

## <a name="prerequisites"></a>Prerequisiti

- Installare [R](https://www.r-project.org) e [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) nel computer client usato per connettersi a SQL Server. È possibile usare qualsiasi IDE di R per l'esecuzione di script, ma in questo articolo si presuppone che venga usato RStudio.

  La versione di R nel computer client deve corrispondere alla versione di R nel server e i pacchetti installati devono essere conformi alla versione di R disponibile.
  Per informazioni sulla versione di R inclusa con ogni versione di SQL Server, vedere [Versioni di Python e R](../sql-server-machine-learning-services.md#versions).
  
  Per verificare la versione di R in un'istanza particolare di SQL Server, usare il comando T-SQL seguente.

  ```sql
  EXECUTE sp_execute_external_script @language = N'R'
   , @script = N'print(R.version)'
  ```

- Installare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) nel computer client usato per connettersi a SQL Server. È possibile usare altri strumenti di query o gestione di database, ma in questo articolo si presuppone che venga usato Azure Data Studio.

### <a name="other-considerations"></a>Altre considerazioni

- L'installazione del pacchetto è specifica dell'istanza di SQL, del database e dell'utente indicati nelle informazioni di connessione specificate per **sqlmlutils**. Per usare il pacchetto in più istanze o database SQL oppure per utenti diversi, è necessario installare il pacchetto per ognuno di essi. L'eccezione è che se il pacchetto viene installato da un membro di `dbo`, il pacchetto è *pubblico* ed è condiviso con tutti gli utenti. Se un utente installa una versione più recente di un pacchetto pubblico, tale pacchetto non sarà interessato, ma l'utente avrà accesso alla versione più recente.

- Lo script R eseguito in SQL Server può usare solo i pacchetti installati nella libreria dell'istanza predefinita. SQL Server non è in grado di caricare pacchetti da librerie esterne, anche se si trovano nello stesso computer, incluse le librerie R installate con altri prodotti Microsoft.

- In un ambiente di SQL Server con protezione avanzata, è consigliabile evitare i tipi di pacchetti seguenti:
  - Pacchetti che richiedono l'accesso alla rete
  - Pacchetti che richiedono l'accesso al file system con privilegi elevati
  - Pacchetti usati per lo sviluppo Web o altre attività che non traggono vantaggio dall'esecuzione all'interno di SQL Server

## <a name="install-sqlmlutils-on-the-client-computer"></a>Installare sqlmlutils nel computer client

Prima di usare **sqlmlutils**, è necessario installarlo nel computer client usato per connettersi a SQL Server.

Il pacchetto **sqlmlutils** dipende dal pacchetto **odbc** e **odbc** dipende da una serie di altri pacchetti. Le procedure seguenti installano tutti questi pacchetti nell'ordine corretto.

### <a name="install-sqlmlutils-online"></a>Installare sqlmlutils online

Se il computer client dispone di accesso a Internet, è possibile scaricare e installare **sqlmlutils** e i relativi pacchetti dipendenti online.

1. Scaricare la versione più recente del file **sqlmlutils** (`.zip` per Windows, `.tar.gz` per Linux) da https://github.com/Microsoft/sqlmlutils/tree/master/R/dist al computer client. Non espandere il file.

1. Aprire un **prompt dei comandi** ed eseguire i comandi seguenti per installare i pacchetti **odbc** e **sqlmlutils**. Sostituire il percorso del file **sqlmlutils** scaricato. Il pacchetto **odbc** viene trovato online e installato.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

### <a name="install-sqlmlutils-offline"></a>Installare sqlmlutils offline

Se il computer client non dispone di una connessione Internet, è necessario scaricare i pacchetti **odbc** e **sqlmlutils** in anticipo usando un computer con accesso a Internet. È quindi possibile copiare i file in una cartella del computer client e installare i pacchetti offline.

Il pacchetto **odbc** include alcuni pacchetti dipendenti e quindi l'identificazione di tutte le dipendenze per un pacchetto diventa complicata. È consigliabile usare [**miniCRAN**](https://andrie.github.io/miniCRAN/) in modo da creare per il pacchetto una cartella di repository locale che includa tutti i pacchetti dipendenti.
Per altre informazioni, vedere [Creare un repository di pacchetti R locale usando miniCRAN](create-a-local-package-repository-using-minicran.md).

Il pacchetto **sqlmlutils** è costituito da un singolo file che è possibile copiare nel computer client e installare.

In un computer con accesso a Internet:

1. Installare **miniCRAN**. Per informazioni dettagliate, vedere [Installare miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

1. In RStudio eseguire lo script R seguente per creare un repository locale del pacchetto **odbc**. In questo esempio si presuppone che il repository venga creato nella cartella `odbc`.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   Per il valore di `Rversion`, usare la versione di R installata in SQL Server. Per verificare la versione installata, usare il comando T-SQL seguente.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Scaricare la versione più recente del file **sqlmlutils** (`.zip` per Windows, `.tar.gz` per Linux) da [https://github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist). Non espandere il file.

1. Copiare l'intera cartella del repository **odbc** e il file **sqlmlutils** nel computer client.

Nel computer client usato per connettersi a SQL Server:

1. Aprire un prompt dei comandi.

1. Eseguire i comandi seguenti per installare **odbc** e quindi **sqlmlutils**. Sostituire i percorsi completi della cartella del repository **odbc** e del file **sqlmlutils** copiati nel computer.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

## <a name="add-an-r-package-on-sql-server"></a>Aggiungere un pacchetto R in SQL Server

Nell'esempio seguente si aggiungerà il pacchetto [**glue**](https://cran.r-project.org/web/packages/glue/) in SQL Server.

### <a name="add-the-package-online"></a>Aggiungere il pacchetto online

Se il computer client usato per connettersi a SQL Server dispone di accesso a Internet, è possibile usare **sqlmlutils** per trovare il pacchetto **glue** e le eventuali dipendenze su Internet, quindi installare il pacchetto in un'istanza di SQL Server in modalità remota.

1. Nel computer client aprire RStudio e creare un nuovo file di **script R**.

1. Usare lo script R seguente per installare il pacchetto **glue** usando **sqlmlutils**. Sostituire le informazioni di connessione del database di SQL Server personalizzate.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server   = "server",
     database = "database",
     uid      = "username",
     pwd      = "password")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > L'ambito (**scope**) può essere **PUBLIC** o **PRIVATE**. L'ambito pubblico è utile all'amministratore del database per installare i pacchetti che possono essere usati da tutti gli utenti. L'ambito privato consente di limitare la disponibilità del pacchetto all'utente che lo ha installato. Se non si specifica l'ambito, il valore predefinito è **PRIVATE**.

### <a name="add-the-package-offline"></a>Aggiungere il pacchetto offline

Se il computer client non dispone di una connessione Internet, è possibile usare **miniCRAN** per scaricare il pacchetto **glue** usando un computer con accesso a Internet. Copiare quindi il pacchetto nel computer client in cui è possibile installare il pacchetto offline.
Per informazioni sull'installazione di **miniCRAN**, vedere [Installare miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

In un computer con accesso a Internet:

1. Eseguire lo script R seguente per creare un repository locale per **glue**. Questo esempio crea la cartella del repository in `c:\downloads\glue`.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   Per il valore di `Rversion`, usare la versione di R installata in SQL Server. Per verificare la versione installata, usare il comando T-SQL seguente.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Copiare l'intera cartella del repository di **glue** (`c:\downloads\glue`) nel computer client. Ad esempio, copiarla nella cartella `c:\temp\packages\glue`.

Nel computer client:

1. Aprire RStudio e creare un nuovo file di **script R**.

1. Usare lo script R seguente per installare il pacchetto **glue** usando **sqlmlutils**. Usare le proprie informazioni di connessione al database di SQL Server. Se non si usa l'autenticazione di Windows, aggiungere i parametri `uid` e `pwd`.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > L'ambito (**scope**) può essere **PUBLIC** o **PRIVATE**. L'ambito pubblico è utile all'amministratore del database per installare i pacchetti che possono essere usati da tutti gli utenti. L'ambito privato consente di limitare la disponibilità del pacchetto all'utente che lo ha installato. Se non si specifica l'ambito, il valore predefinito è **PRIVATE**.

## <a name="use-the-package"></a>Usare il pacchetto

Dopo aver installato il pacchetto **glue**, è possibile usarlo in uno script R in SQL Server con il comando T-SQL **sp_execute_external_script**.

1. Aprire Azure Data Studio e connettersi al database di SQL Server.

1. Eseguire il comando seguente:

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **Risultati**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>Rimuovere il pacchetto

Se si vuole rimuovere il pacchetto **glue**, eseguire lo script R seguente. Usare la stessa variabile **connection** definita in precedenza.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="more-sqlmlutils-functions"></a>Altre funzioni sqlmlutils

Il pacchetto **sqlmlutils** contiene una serie di funzioni per la gestione dei pacchetti R e per la creazione, la gestione e l'esecuzione di stored procedure e query in un'istanza di SQL Server. Per informazioni dettagliate, vedere il [file README di sqlmlutils R](https://github.com/microsoft/sqlmlutils/tree/master/R).

Per informazioni su qualsiasi funzione di **sqlmlutils**, usare la funzione **help** di R o **?** . Ad esempio:

```R
library(sqlmlutils)
help("sql_install.packages")
```

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni sui pacchetti R installati, vedere [Ottenere informazioni sui pacchetti R](r-package-information.md)
- Per informazioni sull'uso di pacchetti R, vedere [Suggerimenti per l'uso di pacchetti R](tips-for-using-r-packages.md)
- Per informazioni sull'installazione dei pacchetti Python, vedere [Installare pacchetti Python con pip](install-additional-python-packages-on-sql-server.md)
- Per altre informazioni su Machine Learning Services per SQL Server, vedere [Che cos'è Machine Learning Services per SQL Server (Python e R)?](../sql-server-machine-learning-services.md)