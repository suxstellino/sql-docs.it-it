---
title: Installare l'estensione del linguaggio Java in Windows
titleSuffix: SQL Server Language Extensions
description: Informazioni su come installare la funzionalità dell'estensione del linguaggio Java di SQL Server in Windows.
author: dphansen
ms.author: davidph
ms.date: 11/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 6974a44210f008807de5498d70083530d9b40757
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096868"
---
# <a name="install-sql-server-java-language-extension-on-windows"></a>Installare l'estensione del linguaggio Java di SQL Server in Windows

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Informazioni su come installare il componente dell'[estensione del linguaggio Java](../java-overview.md) per SQL Server in Windows. L'estensione del linguaggio Java è inclusa nelle [estensioni del linguaggio di SQL Server](../language-extensions-overview.md).

> [!NOTE]
> Questo articolo riguarda l'installazione dell'estensione del linguaggio Java per SQL Server in Windows. Per Linux, vedere [Installare l'estensione del linguaggio Java di SQL Server in Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).

<a name="prerequisites"></a>

## <a name="pre-install-checklist"></a>Elenco di controllo preliminare all'installazione

+ Per installare il supporto per l'estensione del linguaggio Java, occorre installare SQL Server 2019.

+ È necessaria un'istanza del motore di database. Le funzionalità dell'estensione del linguaggio Java non possono essere installate da sole, ma possono essere aggiunte in modo incrementale a un'istanza esistente.

+ Per assicurare la continuità aziendale, per le estensioni del linguaggio è disponibile il supporto di [Gruppi di disponibilità AlwaysOn](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). È necessario installare le estensioni del linguaggio e configurare i pacchetti in ogni nodo.

+ L'installazione dell'estensione del linguaggio Java è supportata in un cluster di failover in SQL Server 2019.

+ Non installare le estensioni del linguaggio di SQL Server o l'estensione del linguaggio Java in un controller di dominio. La parte del programma di installazione relativa alle estensioni del linguaggio avrà esito negativo.

+ Le estensioni del linguaggio e [Machine Learning Services](../../machine-learning/index.yml) vengono installati per impostazione predefinita nei cluster Big Data di SQL Server. Se si usano cluster Big Data, non è necessario seguire la procedura descritta in questo articolo. Per altre informazioni, vedere [Usare Machine Learning Services (Python e R) in cluster Big Data](../../big-data-cluster/machine-learning-services.md).

> [!IMPORTANT]
> Al termine dell'installazione, assicurarsi di completare i passaggi di post-configurazione descritti in questo articolo, tra cui l'abilitazione di SQL Server per l'uso di codice esterno e l'aggiunta degli account necessari per SQL Server per eseguire il codice Java per conto dell'utente. Per completare le modifiche alla configurazione è in genere necessario riavviare l'istanza o riavviare il servizio Launchpad.

<a name="java-jre-jdk"></a>

## <a name="java-jre-or-jdk"></a>JRE o JDK Java

Esistono due metodi per installare e usare Java con SQL Server:

1. Usare il runtime Java predefinito, Zulu Open JRE versione 11.0.3. Questo runtime è supportato e incluso nell'installazione di SQL Server.

1. Usare la distribuzione Java preferita invece del runtime Java predefinito.

    La versione attualmente supportata in Windows è Java 11. Java Runtime Environment (JRE) è il requisito minimo, ma Java Development Kit (JDK) è utile se sono necessari il compilatore Java e i pacchetti di sviluppo. Poiché JDK è tutto compreso, se lo si installa non è richiesto JRE. In Windows, se possibile, è consigliabile installare JDK nella cartella `/Program Files/` predefinita. In caso contrario, è necessaria una configurazione aggiuntiva per concedere autorizzazioni ai file eseguibili. Per altre informazioni, vedere la sezione [Concedere autorizzazioni (Windows)](#perms-nonwindows) in questo documento.

    > [!NOTE]
    > Poiché Java è compatibile con le versioni precedenti, eventuali versioni precedenti potrebbero funzionare. Tuttavia, la versione supportata e testata per SQL Server 2019 è Java 11.

## <a name="get-the-installation-media"></a>Ottenere il supporto di installazione

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>Esecuzione del programma di installazione

Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura ed esecuzione relative a tale condivisione.

1. Avviare l'Installazione guidata di SQL Server 2019.
  
1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

    ![installazione di SQL Server 2019](../media/sql-install.png) 

1. Nella pagina **Selezione funzionalità** selezionare queste opzioni:
  
    - **Servizi motore di database**
  
        Per usare le estensioni del linguaggio con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza predefinita oppure un'istanza denominata.
  
    - **Machine Learning Services ed estensioni del linguaggio**
  
        Questa opzione consente di installare il componente Estensioni del linguaggio che supporta l'esecuzione di codice Java.

        - Se si vuole installare il runtime Java predefinito, Zulu Open JRE 11.0.3, selezionare **Machine Learning Services ed estensioni del linguaggio** e **Java**.

        - Se si vuole usare un runtime Java personalizzato, selezionare **Machine Learning Services ed estensioni del linguaggio**. Non selezionare **Java**.

        Se si vuole usare R e Python, vedere [Installare SQL Server Machine Learning Services in Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).

    ![Opzioni delle funzionalità per le estensioni del linguaggio](../media/sql-install-feature-selection.png)

1. Se si sceglie **Java** nel passaggio precedente per installare il runtime Java predefinito, verrà visualizzata la pagina **Percorso di installazione di Java**.

    Selezionare **Installa Open JRE 11.0.3 incluso in questa installazione**.

    ![Scegliere il percorso di installazione di Java](../media/sql-install-openjdk.png)

    > [!NOTE]
    > L'opzione **Specificare il percorso di una versione diversa installata in questo computer** non viene usata per le estensioni del linguaggio.

1. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.
  
    + Servizi motore di database
    + Machine Learning Services ed estensioni del linguaggio

    Si noti la posizione della cartella nel percorso `..\Setup Bootstrap\Log` in cui sono archiviati i file di configurazione. Al termine dell'installazione, è possibile esaminare i componenti installati nel file Summary.

6. Dopo che l'installazione è completata, riavviare il computer, se richiesto. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="add-the-jre_home-variable"></a>Aggiungere la variabile JRE_HOME

`JRE_HOME` è una variabile di ambiente di sistema che specifica il percorso dell'interprete Java. In questo passaggio creare una variabile di ambiente di sistema in Windows.

1. Trovare e copiare il percorso della home directory di JRE.

    Ad esempio, il percorso della home directory di JRE per il runtime Java predefinito Zulu JRE 11.0.3 è `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Binn\AZUL-OpenJDK-JRE\`.

    A seconda del percorso di installazione di SQL Server o se si sceglie un altro runtime Java, il percorso di JDK o JRE potrebbe essere diverso da quello dell'esempio precedente. Anche se JDK è installato, spesso si ottiene una sottocartella JRE come parte di tale installazione. In tal caso, puntare alla cartella JRE. L'estensione Java tenterà di caricare `jvm.dll` dal percorso `%JRE_HOME%\bin\server`.

1. Nel Pannello di controllo aprire **Sistema e sicurezza**, aprire **Sistema** e selezionare **Proprietà del sistema avanzate**.

1. Selezionare **Variabili di ambiente**.

1. Creare una nuova variabile di sistema per `JRE_HOME` con il valore del percorso di JDK/JRE (disponibile nel passaggio 1).

1. Riavviare [Launchpad](../concepts/extensibility-framework.md#launchpad).

    1. Aprire [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

    1. In Servizi di SQL Server fare clic con il pulsante destro del mouse su Launchpad di SQL Server e selezionare **Riavvia**.

<a name="perms-nonwindows"></a>

## <a name="grant-access-to-non-default-jre-folder"></a>Concedere l'accesso alla cartella JRE non predefinita

Se non è stato installato il runtime predefinito Zulu Open JRE incluso in SQL Server e JDK o JRE non sono stati installati nei file di programma, è necessario eseguire la procedura seguente. Eseguire i comandi **icacls** da una riga *con privilegi elevati* per concedere l'accesso agli account **SQLRUsergroup** e del servizio SQL Server (in **ALL_APPLICATION_PACKAGES**) per l'accesso a JRE. I comandi concederanno l'accesso in modo ricorsivo a tutti i file e le cartelle nel percorso di directory specificato.

1. Assegnare autorizzazioni a SQLRUserGroup

    Per un'istanza denominata, aggiungere il nome dell'istanza a SQLRUsergroup (ad esempio, `SQLRUsergroupINSTANCENAME`).

    ```cmd
    icacls "<PATH to JRE>" /grant "SQLRUsergroup":(OI)(CI)RX /T
    ```
    
    È possibile ignorare questo passaggio se JDK/JRE è stato installato nella cartella predefinita nei file di programma in Windows.

1. Assegnare autorizzazioni ad AppContainer

    ```cmd
    icacls “<PATH to JRE>” /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    > [!NOTE]
    > Il comando precedente concede le autorizzazioni al SID del computer **S-1-15-2-1**, che equivale a **ALL APPLICATION PACKAGES** (TUTTI I PACCHETTI APPLICAZIONI) in una versione di Windows in inglese. In alternativa è possibile usare `icacls "<PATH to JRE>" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in una versione di Windows in inglese.

## <a name="restart-the-service"></a>Riavviare il servizio.

Al termine dell'installazione, riavviare il motore di database prima di continuare con il passaggio successivo, abilitando l'esecuzione degli script.

Il riavvio del servizio ha l'effetto di riavviare automaticamente anche il servizio Launchpad di SQL Server correlato.

Per riavviare il servizio è possibile eseguire il comando **Riavvia** del menu di scelta rapida per l'istanza in SSMS oppure usare il pannello **Servizi** del Pannello di controllo o [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="enable-script-execution"></a>Abilitare l'esecuzione di script

1. Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

1. Connettersi all'istanza di in cui sono state installate le estensioni del linguaggio, fare clic su **Nuova query** per aprire una finestra di query ed eseguire il comando seguente:

    ```sql
    sp_configure
    ```

    A questo punto, il valore della proprietà `external scripts enabled` deve essere **0**. La funzionalità è disattivata per impostazione predefinita e deve essere abilitata esplicitamente da un amministratore per poter eseguire il codice Java.

1. Per abilitare la funzionalità per script esterni, eseguire l'istruzione seguente:

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```

    Se questa funzionalità è già stata abilitata per Machine Learning Services, non ripetere la configurazione per le estensioni del linguaggio. La piattaforma di estendibilità sottostante supporta entrambi.

<a name="register_external_language"></a>

## <a name="register-external-language"></a>Registrare il linguaggio esterno

Per ogni database in cui si vogliono usare le estensioni del linguaggio, è necessario registrare il linguaggio esterno con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

L'esempio seguente aggiunge un linguaggio esterno denominato Java a un database in SQL Server in Windows.

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

Per altre informazioni, vedere [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="verify-installation"></a>Verificare l'installazione

Verificare lo stato di installazione dell'istanza nei log di installazione.

Usare la procedura seguente per verificare che tutti i componenti usati per avviare lo script esterno siano in esecuzione.

1. In SQL Server Management Studio o Azure Data Studio aprire una nuova finestra di query ed eseguire il comando seguente:

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    **run_value** è ora impostato su 1.

1. Aprire il pannello **Servizi** o Gestione configurazione SQL Server e verificare che il **servizio Launchpad di SQL Server** sia in esecuzione. Dovrebbe essere in esecuzione un servizio per ogni istanza del motore di database in cui sono installate le estensioni del linguaggio. Per altre informazioni sul servizio, vedere [Framework di estendibilità](../concepts/extensibility-framework.md).

## <a name="additional-configuration"></a>Configurazione aggiuntiva

Se la procedura di verifica ha avuto esito positivo, è possibile eseguire il codice Java da SQL Server Management Studio, Azure Data Studio, Visual Studio Code o qualsiasi altro client in grado di inviare istruzioni T-SQL al server.

Se durante l'esecuzione del comando è stato restituito un errore, rivedere i passaggi di configurazione aggiuntivi in questa sezione. Potrebbe essere necessario eseguire specifiche configurazioni aggiuntive per il servizio o il database.

A livello di istanza, la configurazione aggiuntiva può includere:

+ [Configurare il firewall per SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
+ [Abilitare protocolli di rete aggiuntivi](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
+ [Abilitare connessioni remote](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
+ [Creare un account di accesso per SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

<a name="bkmk_configureAccounts"></a>
<a name="permissions-external-script"></a>

Nel database potrebbero essere necessari gli aggiornamenti di configurazione seguenti:

+ [Concedere agli utenti l'autorizzazione per SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)
+ [Concedere agli utenti l'autorizzazione per eseguire un linguaggio specifico](../../t-sql/statements/create-external-language-transact-sql.md#permissions)

> [!NOTE]
> La necessità di una configurazione aggiuntiva dipende dallo schema di sicurezza, dal percorso in cui è stato installato SQL Server e dalla modalità presumibilmente adottata dagli utenti per connettersi al database ed eseguire script esterni.

## <a name="suggested-optimizations"></a>Ottimizzazioni suggerite

Ora che tutto il sistema funziona, può essere necessario ottimizzare il server per supportare l'estensione del linguaggio Java.

### <a name="optimize-the-server-for-java-language-extension"></a>Ottimizzare il server per l'estensione del linguaggio Java

Le impostazioni predefinite per il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consentono di ottimizzare il bilanciamento del server per un'ampia gamma di servizi supportati dal motore di database, che possono includere processi di estrazione, trasformazione e caricamento (ETL), servizi di controllo e creazione di report e applicazioni che usano dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di conseguenza, con le impostazioni predefinite, talvolta alle risorse per le estensioni del linguaggio vengono applicate restrizioni o limitazioni, in particolare nel caso di operazioni che richiedono molta memoria.

Per assicurarsi che i processi delle estensioni del linguaggio siano considerati prioritari e dispongano delle risorse appropriate, è consigliabile usare la funzionalità Resource Governor di SQL Server per configurare un pool di risorse esterne. Può inoltre essere opportuno modificare la quantità di memoria allocata al motore di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aumentare il numero di account eseguiti con il servizio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

+ Per configurare un pool di risorse per la gestione delle risorse esterne, vedere [Creare un pool di risorse esterne](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
+ Per modificare la quantità di memoria riservata per il database, vedere [Opzioni di configurazione server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
Se si usa l'edizione Standard e non è disponibile Resource Governor, per gestire le risorse del server è possibile usare le viste a gestione dinamica (DMV) e gli eventi estesi, oltre che il monitoraggio eventi di Windows.

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori Java possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di Java con SQL Server. Per il passaggio successivo, vedere il collegamento seguente:

+ [Esercitazione: Espressioni regolari con Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)