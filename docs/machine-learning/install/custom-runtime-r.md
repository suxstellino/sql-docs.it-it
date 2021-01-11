---
title: Installare il runtime personalizzato di R
description: Informazioni su come installare un runtime personalizzato di R per SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 9029dd87712ee7128504a9b495b699c9c58e000d
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804344"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Installare un runtime personalizzato di R per SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive come installare un runtime personalizzato per l'esecuzione di script R con SQL Server. Il runtime personalizzato usa la tecnologia di estensione del linguaggio basata su un framework di estendibilità per l'esecuzione di codice esterno. Il runtime personalizzato per R può essere usato negli scenari seguenti:

+ Un'installazione di SQL Server con framework di estendibilità.

+ Un'installazione di Machine Learning Services con SQL Server 2019. L'estensione del linguaggio può essere usata con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) dopo aver completato alcuni passaggi di configurazione aggiuntivi.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Questo articolo descrive come installare un runtime personalizzato per R in Windows. Per eseguire l'installazione in Linux, vedere [Installare un runtime personalizzato di R per SQL Server in Linux](custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="pre-install-checklist"></a>Elenco di controllo preliminare all'installazione

Prima di installare un runtime personalizzato di R, installare quanto segue:

+ [SQL Server 2019 per Windows (aggiornamento cumulativo 3 o versione successiva)](../../database-engine/install-windows/install-sql-server.md).

+ [Estensioni del linguaggio di SQL Server in Windows con il framework di estendibilità](../../language-extensions/install/windows-java.md).

+ [R versione 3.3 o successiva](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-windows"></a>Aggiungere le estensioni del linguaggio di SQL Server per Windows

> [!NOTE]
> Se Machine Learning Services è installato in SQL Server 2019, il framework di estendibilità per le estensioni del linguaggio con il servizio Launchpad è già installato ed è possibile ignorare questo passaggio.

Le estensioni del linguaggio usano il framework di estendibilità per l'esecuzione di codice esterno. L'esecuzione del codice è isolata dai processi del motore di base, ma completamente integrata con l'esecuzione delle query di SQL Server.

1. Avviare l'Installazione guidata di SQL Server 2019.

1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

    ![Installazione di SQL Server 2019 CU3 o versione successiva](../install/media/2019setup-installation-page-mlsvcs.png)

1. Nella pagina **Selezione funzionalità** selezionare queste opzioni:

    - **Servizi motore di database**

        Per usare le estensioni del linguaggio con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza predefinita oppure un'istanza denominata.

    - **Machine Learning Services ed estensioni del linguaggio**

        Selezionare **Machine Learning Services ed estensioni del linguaggio**. Non è necessario selezionare R.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Installazione delle estensioni del linguaggio di SQL Server 2019.":::

1. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.

    + Servizi motore di database
    + Machine Learning Services ed estensioni del linguaggio

1. Dopo che l'installazione è completata, riavviare il computer, se richiesto. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="install-r"></a>Installare R

> [!NOTE]
>Per SQL Machine Learning Services, R è già installato nella cartella **R_SERVICES** dell'istanza di SQL Server. Ad esempio: "C:\Programmi\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\R_SERVICES". Se si vuole continuare a usare questo percorso come R_HOME, andare al passaggio successivo dell'installazione di Rccp. In caso contrario, se si vuole usare un runtime diverso di R, continuare qui per installarlo.

Installare [R (3.3 o versione successiva)](https://cran.r-project.org/bin/windows/base/) e prendere nota del percorso in cui viene installato. Questo percorso corrisponde a **R_HOME**. Ad esempio, come illustrato di seguito, R_HOME è "C:\Programmi\R\R-4.0.2"

![Installare R personalizzato](../install/media/custom-r-installation.png)

> [!NOTE]
>Nelle istruzioni seguenti,% R_HOME% è il percorso dell'installazione di R e deve essere sostituito con tale valore.

## <a name="install-rcpp-package"></a>Installare il pacchetto Rcpp

+ Individuare l'eseguibile di R in %R_HOME%\bin. Per impostazione predefinita, si trova in `C:\Program Files\R\R-4.0.2\bin`.

+ Avviare R da un prompt dei comandi con *privilegi elevati*:

```CMD
%R_HOME%\bin\R.exe
```

In questo prompt di R con *privilegi elevati* (%R_HOME%\bin\R.exe), eseguire lo script seguente per installare il pacchetto Rcpp nella cartella %R_HOME%\library.

```R
install.packages("Rcpp", lib="%R_HOME%/library");
```

## <a name="update-the-system-environment-variables"></a>Aggiornare le variabili di ambiente del sistema

1. Aggiungere o modificare **R_HOME** come variabile di ambiente del sistema.
    + Nella casella di ricerca di Windows digitare "ambiente" e selezionare **Modifica le variabili di ambiente relative al sistema**.
    + Nella scheda **Avanzate** selezionare **Variabili di ambiente**.

    + In **Variabili di sistema** selezionare **Nuovo** per creare R_HOME.
    Per modificarla, selezionare **Modifica**. Modificare il valore in modo che punti al percorso di installazione di R personalizzato.

    ![Creare la variabile di ambiente del sistema R_HOME.](../install/media/sys-env-r-home.png)

2. Aggiornare la variabile di ambiente **PATH**.
    Aggiungere il percorso di **R.dll** alla variabile di ambiente **PATH** del sistema. Selezionare **PATH**, quindi **Modifica** e aggiungere `%R_HOME%\bin\x64` all'elenco dei percorsi.

    ![Aggiunta alla variabile di ambiente del sistema PATH.](../install/media/sys-env-path-r.png)

3. Selezionare **OK** per chiudere le finestre rimanenti.

    In alternativa, per impostare queste variabili di ambiente da un prompt dei comandi *con privilegi elevati*, eseguire i comandi seguenti. Assicurarsi di usare il percorso di installazione di R personalizzato.

```CMD
setx /m R_HOME "path\to\installation\of\R"
setx /m PATH "path\to\installation\of\R\bin\x64;%PATH%"
```

## <a name="grant-access-to-the-custom-r-installation-folder"></a>Concedere l'accesso alla cartella di installazione personalizzata di R

> [!NOTE]
>Se R è stato installato nel percorso predefinito **C:\Programmi\R\R-version**, è possibile ignorare questo passaggio.

Eseguire i comandi **icacls** da un nuovo prompt dei comandi *con privilegi elevati* per concedere l'accesso in lettura ed esecuzione a **nome utente del servizio Launchpad di SQL Server** e al SID **S-1-15-2-1** (**ALL APPLICATION PACKAGES**). Il nome utente del servizio Launchpad è nel formato `NT Service\MSSQLLAUNCHPAD$INSTANCENAME` dove `INSTANCENAME` è il nome dell'istanza di SQL Server.

I comandi concederanno l'accesso in modo ricorsivo a tutti i file e le cartelle nel percorso di directory specificato.

Aggiungere il nome dell'istanza a `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`). In questo esempio `INSTANCENAME` è l'istanza predefinita `MSSQLSERVER`.

1. Concedere le autorizzazioni a **nome utente del servizio Launchpad di SQL Server**

    ```cmd
    icacls "%R_HOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T
    ```
2. Concedere le autorizzazioni al **SID S-1-15-2-1**

    ```cmd
    icacls "%R_HOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.


```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

In alternativa, fare clic con il pulsante destro del mouse sul servizio Launchpad di SQL Server nell'app **Servizi** del sistema e scegliere il comando **Riavvia**. Oppure usare [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md) per riavviare il servizio.

## <a name="download-r-language-extension"></a>Scaricare l'estensione del linguaggio R

Scaricare il [file ZIP contenente l'estensione del linguaggio R per Windows](https://github.com/microsoft/sql-server-language-extensions/releases). È consigliabile usare la versione di rilascio in ambiente di produzione. Usare la versione di debug in ambiente di sviluppo o test poiché offre informazioni dettagliate sulla registrazione per esaminare eventuali errori.

## <a name="register-external-language"></a>Registrare il linguaggio esterno

Registrare questa estensione del linguaggio R con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) per ogni database in cui si vuole usarla. Usare [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) per connettersi a SQL Server ed eseguire il comando T-SQL seguente.
Modificare il percorso in questa istruzione in modo che corrisponda al percorso del file ZIP dell'estensione del linguaggio scaricato (R-lang-extension.zip).

> [!NOTE]
>**R** è una parola riservata. Usare un nome diverso per il linguaggio esterno, ad esempio "mioR".

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

È possibile installare SQL Server in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) e Ubuntu. Per altre informazioni, vedere [la sezione delle piattaforme supportate nelle linee guida per l'installazione di SQL Server in Linux](../../linux/sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> Questo articolo descrive come installare un runtime personalizzato per R in Linux. Per eseguire l'installazione in Windows, vedere [Installare un runtime personalizzato di R per SQL Server in Windows](custom-runtime-r.md?view=sql-server-ver15&preserve-view=true).

## <a name="pre-install-checklist"></a>Elenco di controllo preliminare all'installazione

Prima di installare un runtime personalizzato di R, installare quanto segue:

+ [SQL Server 2019 per Linux (aggiornamento cumulativo 3 o versione successiva)](../../linux/sql-server-linux-setup.md).
Prima di installare SQL Server in Linux, è necessario configurare un repository Microsoft. Per altre informazioni, vedere [Configurazione dei repository](../../linux/sql-server-linux-change-repo.md).

+ [Estensioni del linguaggio di SQL Server in Linux con il framework di estendibilità](../../linux/sql-server-linux-setup-language-extensions-java.md).

+ [R versione 3.3 o successiva](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-linux"></a>Aggiungere le estensioni del linguaggio di SQL Server per Linux

> [!NOTE]
> Se Machine Learning Services è installato in SQL Server 2019, il pacchetto **mssql-server-extensibility** per le estensioni del linguaggio è già installato ed è possibile ignorare questo passaggio.

Le estensioni del linguaggio usano il framework di estendibilità per l'esecuzione di codice esterno. L'esecuzione del codice è isolata dai processi del motore di base, ma completamente integrata con l'esecuzione delle query di SQL Server.

Usare i comandi seguenti per installare le estensioni del linguaggio, a seconda della versione di Linux.

### <a name="ubuntu"></a>Ubuntu
> [!Tip]
> Se possibile, eseguire `sudo apt-get update` per aggiornare i pacchetti nel sistema prima dell'installazione. Ubuntu potrebbe non avere l'opzione di trasporto https apt. Per installarla, usare `sudo apt-get install apt-transport-https`.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### <a name="red-hat"></a>Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### <a name="suse"></a>SUSE
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Installare R

>[!NOTE]
>Per SQL Machine Learning Services, R è già installato in `/opt/microsoft/ropen/3.5.2/lib64/R`. Se si vuole continuare a usare questo percorso come R_HOME, andare al passaggio successivo dell'**installazione di Rccp**. 

Se si vuole usare un runtime diverso di R, prima di continuare con l'installazione di una nuova versione è necessario rimuovere `microsoft-r-open-mro`. Esempio per Ubuntu:

```bash
sudo apt remove microsoft-r-open-mro-3.5.2
```

Seguire queste [istruzioni](https://cran.r-project.org/bin/linux/) per completare l'installazione di R (3.3 o versione successiva) per la piattaforma Linux corrispondente. Per impostazione predefinita, R viene installato in **/usr/lib/R**. Questo percorso corrisponde a **R_HOME**. Se si installa R in un percorso diverso, prendere nota del percorso come R_HOME.

Istruzioni di esempio per Ubuntu. Modificare l'URL del repository seguente per la versione di R.

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
#
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
sudo apt-get update

# Install R runtime.
#
sudo apt-get -y install r-base-core
```

## <a name="install-rcpp-package"></a>Installare il pacchetto Rcpp

Nelle istruzioni seguenti ${R_HOME} è il percorso dell'installazione di R. 

+ Individuare il file binario di R in ${R_HOME}/bin. Per impostazione predefinita, è in **/usr/lib/R/bin**.

+ Avviare R

  ```bash
  sudo ${R_HOME}/bin/R
  ```

+ In questo prompt di R con *privilegi elevati* (${R_HOME}/bin/R), eseguire lo script seguente per installare il pacchetto **Rcpp** nella cartella ${R_HOME}/library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="using-a-custom-installation-of-r"></a>Uso di un'installazione personalizzata di R

> [!NOTE]
>Se R è stato installato nel percorso predefinito **/usr/lib/R**, è possibile ignorare questa sezione.

### <a name="update-the-environment-variables"></a>Aggiornare le variabili di ambiente

1. Modificare il servizio mssql-launchpadd per aggiungere la variabile di ambiente R_HOME al file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

    + Inserire il testo seguente nel file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` che si apre. Impostare il valore di R_HOME sul percorso di installazione di R personalizzato.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

    + Salvare e chiudere.

2. Assicurarsi che **libR.so** possa essere caricato.

    + Creare un file custom-r.conf in **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

    + Nel file che si apre aggiungere il percorso di **libR.so** dall'installazione di R personalizzata.

    ```vi editor
    /path/to/installation/of/R/lib
    ```

    + Salvare e chiudere il nuovo file.

    + Eseguire `ldconfig` e verificare che sia possibile caricare **libR.so** eseguendo il comando seguente e verificando che tutte le librerie dipendenti siano disponibili.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Concedere l'accesso alla cartella di installazione personalizzata di R

Impostare l'opzione `datadirectories` nella sezione extensibility del file /var/opt/mssql/mssql.conf sull'installazione personalizzata di R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Riavviare il servizio mssql-launchpadd

> [!NOTE]
>Se R è stato installato nel percorso predefinito **/usr/lib/R**, è possibile ignorare questo passaggio.

```bash
sudo systemctl restart mssql-launchpadd
```

## <a name="download-r-language-extension"></a>Scaricare l'estensione del linguaggio R

Scaricare il [file ZIP contenente l'estensione del linguaggio R per Linux](https://github.com/microsoft/sql-server-language-extensions/releases). È consigliabile usare la versione di rilascio in ambiente di produzione. Usare la versione di debug in ambiente di sviluppo o test poiché offre informazioni dettagliate sulla registrazione per esaminare eventuali errori.

## <a name="register-external-language"></a>Registrare il linguaggio esterno

Registrare questa estensione del linguaggio R con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) per ogni database in cui si vuole usarla. Usare [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) per connettersi a SQL Server ed eseguire il comando T-SQL seguente. 
Modificare il percorso in questa istruzione in modo che corrisponda al percorso del file ZIP dell'estensione del linguaggio scaricato (r-lang-extension.zip).


> [!NOTE]
>**R** è una parola riservata. Usare un nome diverso per il linguaggio esterno, ad esempio "mioR".

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>Abilitare l'esecuzione di script esterni in SQL Server

Uno script esterno in R può essere eseguito tramite la stored procedure [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) eseguita su SQL Server. 

Per abilitare gli script esterni, eseguire i comandi SQL seguenti usando [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) connesso a SQL Server.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="verify-language-extension-installation"></a>Verificare l'installazione dell'estensione del linguaggio

Questo script SQL verifica la corretta installazione dell'estensione del linguaggio R personalizzata. L'output di questo script deve visualizzare R_HOME, il percorso di R e la versione del runtime di R personalizzato. Conferma che lo script usa il runtime personalizzato.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>Verificare i parametri e i set di dati di tipi di dati diversi

Questo script verifica diversi tipi di dati per i set di dati e i parametri di input/output.

```sql
DECLARE @sumVal INT = 12;
DECLARE @charVal VARCHAR(30) = N'Hello';
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(sumVal);
print(charVal);
sumVal <- sumVal + 300;
OutputDataSet <- InputDataSet;'
    ,@input_data_1 = N'select 1, cast(1.4 as real), ''Hi'', cast(''1'' as bit)'
    ,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
    ,@sumVal = @sumVal OUTPUT
    ,@charVal =  @charVal
WITH RESULT SETS ((intCol INT, doubleCol REAL, charCol CHAR(2), logicalCol BIT));
PRINT @sumVal;
```

## <a name="see-also"></a>Vedere anche

+ [Framework di estendibilità in SQL Server](../concepts/extensibility-framework.md)
+ [Panoramica delle estensioni del linguaggio](../../language-extensions/language-extensions-overview.md)