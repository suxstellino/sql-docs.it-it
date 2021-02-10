---
title: Installare l'estensione del linguaggio Java in Linux
titleSuffix: SQL Server Language Extensions
description: Informazioni su come installare l'estensione del linguaggio Java di SQL Server in Red Hat, Ubuntu e SUSE Linux.
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 11/12/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a98eed7109a45958864e85822bbb69de4767a85d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073932"
---
# <a name="install-sql-server-java-language-extension-on-linux"></a>Installare l'estensione del linguaggio Java di SQL Server in Linux

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

Informazioni su come installare il componente dell'[estensione del linguaggio Java](../language-extensions/java-overview.md) per SQL Server in Linux. L'estensione del linguaggio Java fa parte delle [estensioni del linguaggio di SQL Server](../language-extensions/language-extensions-overview.md) ed è un componente aggiuntivo per il motore di database. 

Anche se è possibile [installare il motore di database e le estensioni del linguaggio simultaneamente](#install-all), è consigliabile installare e configurare prima il motore di database di SQL Server per poter risolvere eventuali problemi prima di aggiungere altri componenti.

## <a name="prerequisites"></a>Prerequisiti

+ La versione di Linux deve essere [supportata da SQL Server](sql-server-linux-release-notes-2019.md#supported-platforms), ma non include il motore Docker. Le versioni supportate includono:

   + [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)
   + [SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
   + [Ubuntu](quickstart-install-connect-ubuntu.md)

+ È necessario avere uno strumento per eseguire i comandi T-SQL. Un editor di query è necessario per la configurazione e la convalida successiva all'installazione. È consigliabile usare [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md?view=sql-server-2017&preserve-view=true#get-azure-data-studio-for-linux), scaricabile gratuitamente, che viene eseguito in Linux.

+ Il percorso del pacchetto per le estensioni Java si trova nei repository di origine di Linux per SQL Server. Se i repository di origine per l'installazione del motore di database sono già stati configurati, è possibile eseguire i comandi di installazione del pacchetto **mssql-server-extensibility-java** usando la stessa registrazione dei repository.

+ Le estensioni del linguaggio sono supportate anche nei contenitori Linux. Non vengono forniti contenitori predefiniti con le estensioni del linguaggio, ma è possibile crearne uno dai contenitori di SQL Server usando [un modello di esempio disponibile in GitHub](https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/examples/mssql-mlservices).

+ Le estensioni del linguaggio e [Machine Learning Services](../machine-learning/index.yml) vengono installati per impostazione predefinita nei cluster Big Data di SQL Server. Se si usano cluster Big Data, non è necessario seguire la procedura descritta in questo articolo. Per altre informazioni, vedere [Usare Machine Learning Services (Python e R) in cluster Big Data](../big-data-cluster/machine-learning-services.md).

## <a name="package-list"></a>Elenco di pacchetti

In un dispositivo connesso a Internet i pacchetti vengono scaricati e installati in modo indipendente dal motore di database usando il programma di installazione del pacchetto per ogni sistema operativo. Nella tabella seguente vengono descritti tutti i pacchetti disponibili.

| Nome del pacchetto | Applicabile a | Descrizione |
|--------------|----------|-------------|
|mssql-server-extensibility  | Tutte le lingue | Framework di estendibilità usato per l'estensione del linguaggio Java |
|mssql-server-extensibility-java | Java | Framework di estendibilità usato per l'estensione del linguaggio Java che include un runtime Java supportato |

<a name="RHEL"></a>

## <a name="install-java-language-extension"></a>Installare l'estensione del linguaggio Java

Per installare le estensioni del linguaggio e Java in Linux, installare **mssql-server-extensibility-java**. Quando si installa **mssql-server-extensibility-java**, il pacchetto installa automaticamente JRE 11 se non è già installato. Verrà anche aggiunto il percorso JVM a una variabile di ambiente denominata JRE_HOME.

> [!Note]
> In un server connesso a Internet, durante l'installazione del pacchetto principale, vengono scaricate e installate le dipendenze del pacchetto. Se il server non è connesso a Internet, vedere altri dettagli sull'[installazione offline](#offline-install).

### <a name="redhat-install-command"></a>Comando di installazione di RedHat

È possibile installare le estensioni del linguaggio per Java in RedHat usando il comando seguente.

> [!Tip]
> Se possibile, eseguire `yum clean all` per aggiornare i pacchetti nel sistema prima dell'installazione.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility-java
```

<a name="ubuntu"></a>

### <a name="ubuntu-install-command"></a>Comando di installazione di Ubuntu

È possibile installare le estensioni del linguaggio per Java in Ubuntu usando il comando seguente.

> [!Tip]
> Se possibile, eseguire `apt-get update` per aggiornare i pacchetti nel sistema prima dell'installazione. Inoltre, alcune immagini Docker di Ubuntu potrebbero non avere l'opzione di trasporto apt HTTPS. Per installarla, usare `apt-get install apt-transport-https`.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility-java
```

<a name="suse"></a>

### <a name="suse-install-command"></a>Comando di installazione di SUSE

È possibile installare le estensioni del linguaggio per Java in SUSE usando il comando seguente.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility-java
```

## <a name="post-install-config-required"></a>Configurazione successiva all'installazione (obbligatoria)

1. Concedere le autorizzazioni in Linux

    Non è necessario eseguire questo passaggio se si usano librerie esterne. Il modo consigliato per lavorare è usare le librerie esterne. Per la creazione di una libreria esterna dal file JAR, vedere [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md)

    Se non si usano librerie esterne, è necessario fornire a SQL Server le autorizzazioni per eseguire le classi Java nel file JAR.

    Per concedere l'accesso in lettura e in esecuzione a un file JAR, eseguire il comando **chmod** seguente sul file JAR. Si consiglia di inserire sempre i file di classe in un file JAR quando si lavora con SQL Server. Per la creazione di un file JAR, vedere [Come creare un file JAR](../language-extensions/how-to/create-a-java-jar-file-from-class-files.md).

    ```cmd
    chmod ug+rx <MyJarFile.jar>
    ```

    È anche necessario assegnare alle autorizzazioni mssql_satellite il file JAR da leggere/eseguire.

    ```cmd
    chown mssql_satellite:mssql_satellite <MyJarFile.jar>
    ```

    La configurazione aggiuntiva viene eseguita principalmente tramite lo [strumento mssql-conf](sql-server-linux-configure-mssql-conf.md).

2. Aggiungere l'account utente mssql usato per eseguire il servizio SQL Server. Questa operazione è necessaria se la configurazione non è stata eseguita in precedenza.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

3. Abilitare l'accesso alla rete in uscita. L'accesso alla rete in uscita è disabilitato per impostazione predefinita. Per abilitare le richieste in uscita, impostare la proprietà booleana "outboundnetworkaccess" usando lo strumento mssql-conf. Per altre informazioni, vedere [Configurare SQL Server in Linux con mssql conf](sql-server-linux-configure-mssql-conf.md#mlservices-outbound-access).

   ```bash
   # Run as SUDO or root
   # Enable outbound requests over the network
   sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1
   ```

4. Riavviare il servizio Launchpad di SQL Server e l'istanza del motore di database per leggere i valori aggiornati dal file INI. Un messaggio di riavvio avverte ogni volta che viene modificata un'impostazione relativa all'estendibilità.  

   ```bash
   systemctl restart mssql-launchpadd

   systemctl restart mssql-server.service
   ```

5. Abilitare l'esecuzione di script esterni usando Azure Data Studio o un altro strumento come SQL Server Management Studio (solo Windows) che esegue Transact-SQL.

   ```bash
   EXEC sp_configure 'external scripts enabled', 1
   RECONFIGURE WITH OVERRIDE
   ```

6. Riavviare nuovamente il servizio `mssql-launchpadd`.

7. Per ogni database in cui si vogliono usare le estensioni del linguaggio, è necessario registrare il linguaggio esterno con [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md).

## <a name="register-external-language"></a>Registrare il linguaggio esterno

Per ogni database in cui si vogliono usare le estensioni del linguaggio, è necessario registrare il linguaggio esterno con [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md).

L'esempio seguente aggiunge un linguaggio esterno denominato Java a un database in SQL Server in Linux.

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'/opt/mssql-extensibility/lib/java-lang-extension.tar.gz', 
    FILE_NAME = 'javaextension.so', 
    ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"/opt/mssql/lib/zulu-jre-11"}')
```
Per l'estensione Java, la variabile di ambiente "JRE_HOME" viene usata per determinare il percorso da cui trovare e inizializzare JVM.

L'istruzione DDL CREATE EXTERNAL LANGUAGE include un parametro (ENVIRONMENT_VARIABLES) per impostare le variabili di ambiente in modo specifico per il processo che ospita l'estensione. Si tratta del metodo consigliato e più efficace per impostare le variabili di ambiente richieste dalle estensioni di linguaggio esterno.

Per altre informazioni, vedere [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md).

## <a name="verify-installation"></a>Verificare l'installazione

L'integrazione delle funzionalità Java non include librerie, ma è possibile eseguire `grep -r JRE_HOME /etc` per verificare la creazione della variabile di ambiente JAVA_HOME.

Per convalidare l'installazione, eseguire uno script T-SQL che esegue una stored procedure di sistema richiamando Java. Per questa attività sarà necessario uno strumento di query. Azure Data Studio è una scelta ottimale. Altri strumenti di uso comune, ad esempio SQL Server Management Studio o PowerShell, sono solo per Windows. Se si ha un computer Windows con questi strumenti, usarlo per connettersi all'installazione Linux del motore di database.

<a name="install-all"></a>

## <a name="full-install-of-sql-server-and-java-language-extension"></a>Installazione completa di SQL Server e dell'estensione del linguaggio Java

È possibile installare e configurare il motore di database e l'estensione del linguaggio Java in una sola procedura aggiungendo i pacchetti e i parametri Java in un comando che installa il motore di database.

1. Specificare una riga di comando che includa il motore di database, oltre alle funzionalità delle estensioni del linguaggio.

    È possibile aggiungere l'estendibilità Java a un'installazione del motore di database.

    ```bash
    sudo yum install -y mssql-server mssql-server-extensibility-java 
    ```

1. Accettare i contratti di licenza e completare la configurazione successiva all'installazione. Per questa attività usare lo strumento **mssql-conf**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf setup
    ```

    Verrà richiesto di accettare il contratto di licenza per il motore di database, scegliere un'edizione e impostare la password dell'amministratore. 

1. Se richiesto, riavviare il servizio.

    ```bash
    sudo systemctl restart mssql-server.service
    ```

## <a name="unattended-installation"></a>Installazione automatica

Usare l'[installazione automatica](./sql-server-linux-setup.md#unattended) per il motore di database e aggiungere i pacchetti per **mssql-server-extensibility-java**.

<a name="offline-install"></a>

## <a name="offline-installation"></a>Installazione offline

Per la procedura di installazione dei pacchetti, seguire le istruzioni contenute in [Installazione offline](sql-server-linux-setup.md#offline). Trovare il sito di download e quindi scaricare i pacchetti specifici usando l'elenco di pacchetti riportato di seguito.

> [!Tip]
> Molti degli strumenti di gestione dei pacchetti forniscono comandi che consentono di determinare le dipendenze dei pacchetti. Per yum, usare `sudo yum deplist [package]`. Per Ubuntu, usare `sudo apt-get install --reinstall --download-only [package name]` seguito da `dpkg -I [package name].deb`.

#### <a name="download-site"></a>Sito di download

È possibile scaricare i pacchetti da [https://packages.microsoft.com/](https://packages.microsoft.com/). Tutti i pacchetti per Java si trovano nella stessa posizione del pacchetto del motore di database.

#### <a name="redhat7-paths"></a>Percorsi RedHat/7

|Pacchetto|Percorso download|
|--|----|
| Pacchetti mssql/extensibility-java | [https://packages.microsoft.com/rhel/7/mssql-server-2019/](https://packages.microsoft.com/rhel/7/mssql-server-2019/) |

#### <a name="ubuntu1604-paths"></a>Percorsi Ubuntu/16.04

|Pacchetto|Percorso download|
|--|----|
| Pacchetti mssql/extensibility-java | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/) |

#### <a name="suse12-paths"></a>Percorsi SUSE/12

|Pacchetto|Percorso download|
|--|----|
| Pacchetti mssql/extensibility-java | [https://packages.microsoft.com/sles/12/mssql-server-2019/](https://packages.microsoft.com/sles/12/mssql-server-2019/) |

#### <a name="package-list"></a>Elenco di pacchetti
A seconda delle estensioni che si vogliono usare, scaricare i pacchetti necessari per una linguaggio specifico. I nomi dei file esatti includono le informazioni sulla piattaforma nel suffisso, ma i nomi dei file riportati di seguito dovrebbero essere sufficienti per determinare quali file ottenere.

```
# Core packages
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# Java
mssql-server-extensibility-java-15.0.1000
```

## <a name="limitations"></a>Limitazioni

L'autenticazione implicita attualmente non è disponibile in Linux. Ciò significa che non è possibile connettersi al server da Java in esecuzione per accedere ai dati o ad altre risorse.

### <a name="resource-governance"></a>Governance delle risorse

Esiste parità tra Linux e Windows per la [governance delle risorse](../t-sql/statements/create-external-resource-pool-transact-sql.md) per i pool di risorse esterni, ma le statistiche per [sys.dm_resource_governor_external_resource_pools](../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) hanno attualmente unità diverse in Linux.

| Nome colonna   | Descrizione | Valore in Linux |
|---------------|--------------|---------------|
|peak_memory_kb | Quantità massima di memoria usata per il pool di risorse. | In Linux questa statistica è originata dal sottosistema di memoria CGroups, dove il valore è memory.max_usage_in_bytes |
|write_io_count | Il totale degli I/O di scrittura generati dalla reimpostazione delle statistiche di Resource Governor. | In Linux questa statistica è originata dal sottosistema blkio di CGroups, dove il valore nella riga di scrittura è blkio.throttle.io_serviced |
|read_io_count | Il totale degli I/O di lettura generati dalla reimpostazione delle statistiche di Resource Governor. | In Linux questa statistica è originata dal sottosistema blkio di CGroups, dove il valore nella riga di lettura è blkio.throttle.io_serviced |
|total_cpu_kernel_ms | Tempo del kernel dell'utente della CPU cumulativo, espresso in millisecondi, dalla reimpostazione delle statistiche di Resource Governor. | In Linux questa statistica è originata dal sottosistema cpuacct di CGroups, dove il valore nella riga dell'utente è cpuacct.stat |  
|total_cpu_user_ms | Tempo dell'utente della CPU cumulativo, espresso in millisecondi, dalla reimpostazione delle statistiche di Resource Governor.| In Linux questa statistica è originata dal sottosistema cpuacct di CGroups, dove il valore nella riga di sistema è cpuacct.stat |
|active_processes_count | Numero di processi esterni in esecuzione al momento della richiesta.| In Linux questa statistica è originata dal sottosistema pids CGroups, dove il valore è pids.current |

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori Java possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di Java con SQL Server. Per il passaggio successivo, vedere i collegamenti seguenti:

+ [Esercitazione: Espressioni regolari con Java](../language-extensions/tutorials/search-for-string-using-regular-expressions-in-java.md)