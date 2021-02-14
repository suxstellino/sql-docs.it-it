---
title: Usare RevoScaleR per installare i pacchetti R
description: Informazioni su come usare le funzioni RevoScaleR per installare i pacchetti R in SQL Server con Machine Learning Services o R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 99750753f7b74070ecb3bc8217d72b91255a0962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082612"
---
# <a name="use-revoscaler-to-install-r-packages"></a>Usare RevoScaleR per installare i pacchetti R

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Questo articolo descrive come usare le funzioni [RevoScaleR](../r/ref-r-revoscaler.md) versione 9.0.1 e per installare i pacchetti R in SQL Server con Machine Learning Services o R Services. Le funzioni RevoScaleR possono essere usate da utenti non amministratori in remoto per installare i pacchetti in SQL Server senza accesso diretto al server.

::: moniker range="=sql-server-2016"
> [!NOTE]
> I clienti di SQL Server R Services devono eseguire un [aggiornamento del componente](../install/upgrade-r-and-python.md) per ottenere le funzioni di gestione dei pacchetti RevoScaleR. Per istruzioni su come recuperare la versione e il contenuto dei pacchetti, vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md).
::: moniker-end

## <a name="revoscaler-functions-for-package-management"></a>Funzioni RevoScaleR per la gestione dei pacchetti

La tabella seguente descrive le funzioni usate per l'installazione e la gestione dei pacchetti R.

| Funzione | Descrizione |
|----------|-------------|
| [rxSqlLibPaths](/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) | Determina il percorso della libreria dell'istanza in SQL Server remoto. |
| [rxFindPackage](/machine-learning-server/r-reference/revoscaler/rxfindpackage) | Ottiene il percorso per uno o più pacchetti in SQL Server remoto. |
| [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) | Chiama la funzione da un client R remoto per installare i pacchetti in un contesto di calcolo di SQL Server, da un repository specificato o leggendo i pacchetti compressi salvati in locale. La funzione controlla la presenza di dipendenze e verifica se è possibile installare eventuali pacchetti correlati in SQL Server, analogamente a come vengono installati i pacchetti R nel contesto di calcolo locale. Per usare questa opzione, è necessario abilitare la gestione dei pacchetti nel server e nel database. Gli ambienti client e server devono avere la stessa versione di RevoScaleR. |
| [rxInstalledPackages](/machine-learning-server/r-reference/revoscaler/rxinstalledpackages) | Ottiene un elenco dei pacchetti installati nel contesto di calcolo specificato. |
| [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) | Copia le informazioni relative a una libreria di pacchetti tra il file system e il database, per il contesto di calcolo specificato. |
| [rxRemovePackages](/machine-learning-server/r-reference/revoscaler/rxremovepackages) | Rimuove i pacchetti da un contesto di calcolo specificato. Calcola anche le dipendenze e verifica che i pacchetti che non sono più usati da altri pacchetti in SQL Server vengano rimossi per liberare le risorse. |

## <a name="prerequisites"></a>Prerequisites

+ Gestione remota abilitata in SQL Server. Per altre informazioni, vedere [Abilitare la gestione remota dei pacchetti R in SQL Server](r-package-how-to-enable-or-disable.md).

+ Gli ambienti client e server devono avere la stessa versione di RevoScaleR. Per altre informazioni, vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md).

+ Si dispone dell'autorizzazione per connettersi al server e a un database e per eseguire i comandi R. È necessario essere un membro di un ruolo del database che consente di installare i pacchetti nell'istanza e nel database specificati.

  + I pacchetti in un **ambito condiviso** possono essere installati dagli utenti appartenenti al ruolo `rpkgs-shared` in un database specificato. Tutti gli utenti in questo ruolo possono disinstallare i pacchetti condivisi.

  + I pacchetti in un **ambito privato** possono essere installati da qualsiasi utente appartenente al ruolo `rpkgs-private` in un database. Gli utenti possono tuttavia visualizzare e disinstallare solo i propri pacchetti.

  + I proprietari del database possono usare sia i pacchetti condivisi sia i pacchetti privati.

## <a name="client-connections"></a>Connessioni client

Una workstation client può essere [Microsoft R Client](/machine-learning-server/r-client/install-on-windows) o [Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) nella stessa rete. I data scientist usano spesso la versione gratuita Developer Edition.

Quando si chiamano le funzioni di gestione dei pacchetti da un client R remoto, è necessario prima creare un oggetto contesto di calcolo usando la funzione [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver). Successivamente, per ogni funzione di gestione dei pacchetti usata, è necessario passare il contesto di calcolo come argomento.

L'identità utente viene in genere specificata durante l'impostazione del contesto di calcolo. Se non si specificano un nome utente e una password durante la creazione del contesto di calcolo, viene usata l'identità dell'utente che esegue il codice R.

1. Da una riga di comando R, definire una stringa di connessione per l'istanza e il database.
2. Usare il costruttore [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) per definire un contesto di calcolo SQL Server usando la stringa di connessione.

    ```R
    sqlcc <- RxInSqlServer(connectionString = myConnString, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```
3. Creare un elenco dei pacchetti da installare e salvare l'elenco in una variabile di stringa.

    ```R
    packageList <- c("e1071", "mice")
    ```

4. Chiamare [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) e passare il contesto di calcolo e la variabile di stringa contenente i nomi dei pacchetti.

    ```R
    rxInstallPackages(pkgs = packageList, verbose = TRUE, computeContext = sqlcc)
    ```

    Se necessari, vengono installati anche i pacchetti dipendenti, presumendo che nel client sia disponibile una connessione Internet.
    
    I pacchetti vengono installati usando le credenziali dell'utente che esegue la connessione, nell'ambito predefinito per tale utente.

## <a name="call-package-management-functions-in-stored-procedures"></a>Chiamare le funzioni di gestione dei pacchetti nelle stored procedure

È possibile eseguire le funzioni di gestione dei pacchetti in `sp_execute_external_script`. In questo caso la funzione viene eseguita usando il contesto di sicurezza del chiamante della stored procedure.

## <a name="examples"></a>Esempi

Questa sezione illustra esempi su come usare queste funzioni da un client remoto quando ci si connette a un'istanza o a un database di SQL Server come contesto di calcolo.

Per tutti gli esempi, è necessario specificare una stringa di connessione o un contesto di calcolo che richiede una stringa di connessione. Questo esempio descrive un modo per creare un contesto di calcolo per SQL Server:

```R
instance_name <- "computer-name/instance-name";
database_name <- "TestDB";
sqlWait= TRUE;
sqlConsoleOutput <- TRUE;
connString <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
sqlcc <- RxInSqlServer(connectionString = connString, wait = sqlWait, consoleOutput = sqlConsoleOutput, numTasks = 4);
```

A seconda della posizione in cui si trova il server e del modello di sicurezza, potrebbe essere necessario specificare un dominio e una subnet nella stringa di connessione oppure usare un accesso di SQL. Ad esempio:

```R
connStr <- "Driver=SQL Server;Server=myserver.financeweb.contoso.com;Database=Finance;Uid=RUser1;Pwd=RUserPassword"
```

### <a name="get-package-path-on-a-remote-sql-server-compute-context"></a>Ottenere il percorso del pacchetto in un contesto di calcolo di SQL Server remoto

Questo esempio ottiene il percorso del pacchetto **RevoScaleR** nel contesto di calcolo `sqlcc`.

```R
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlcc)
print(sqlPackagePaths)
```

**Risultati**

[1] "C:/Programmi/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library/RevoScaleR"

> [!TIP]
> Se è stata abilitata l'opzione per visualizzare l'output della console SQL, è possibile che vengano visualizzati messaggi di stato dalla funzione che precede l'istruzione `print`. Dopo aver concluso il test del codice, impostare `consoleOutput` su FALSE nel costruttore del contesto di calcolo per eliminare i messaggi.

### <a name="get-locations-for-multiple-packages"></a>Ottenere i percorsi per più pacchetti

Questo esempio ottiene i percorsi per i pacchetti **RevoScaleR** e **lattice** nel contesto di calcolo `sqlcc`. Per ottenere informazioni su più pacchetti, passare un vettore di stringa contenente i nomi dei pacchetti.

```R
packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"), computeContext = sqlcc)
print(packagePaths)
```

### <a name="get-package-versions-on-a-remote-compute-context"></a>Ottenere le versioni dei pacchetti in un contesto di calcolo remoto

Eseguire questo comando da una console R per ottenere il numero di build e i numeri di versione per i pacchetti installati nel contesto di calcolo *sqlServer*.

```R
sqlPackages <- rxInstalledPackages(fields = c("Package", "Version", "Built"), computeContext = sqlServer)
```

### <a name="install-a-package-on-sql-server"></a>Installare un pacchetto in SQL Server

Questo esempio installa il pacchetto **forecast** e le relative dipendenze nel contesto di calcolo.

  ```R
  pkgs <- c("forecast")
  rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="remove-a-package-from-sql-server"></a>Rimuovere un pacchetto da SQL Server

Questo esempio rimuove il pacchetto **forecast** e le relative dipendenze dal contesto di calcolo.

  ```R
  pkgs <- c("forecast")
  rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="synchronize-packages-between-database-and-file-system"></a>Sincronizzare i pacchetti tra database e file system

L'esempio seguente controlla il database **TestDB** e determina se tutti i pacchetti sono installati nel file system. Se alcuni pacchetti risultano mancanti, vengono installati nel file system.

```R
# Instantiate the compute context
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )

# Synchronize the packages in the file system for all scopes and users
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

La sincronizzazione dei pacchetti funziona in base al database e agli utenti. Per altre informazioni, vedere [Sincronizzazione dei pacchetti R per SQL Server](package-install-uninstall-and-sync.md).

### <a name="use-a-stored-procedure-to-list-packages-in-sql-server"></a>Usare una stored procedure per elencare i pacchetti in SQL Server

Eseguire questo comando da Management Studio o da un altro strumento che supporta T-SQL per ottenere un elenco dei pacchetti installati nell'istanza corrente, usando `rxInstalledPackages` in una stored procedure.

```sql
EXEC sp_execute_external_script 
  @language=N'R', 
  @script=N'
    myPackages <- rxInstalledPackages();
    OutputDataSet <- as.data.frame(myPackages);
    '
```

È possibile usare la funzione `rxSqlLibPaths` per determinare la libreria attiva usata da Machine Learning Services di SQL Server. Questo script può restituire solo il percorso della libreria per il server corrente. 

```sql
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));
    print(.libPaths());
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;
```

## <a name="see-also"></a>Vedere anche

+ [Abilitare la gestione remota dei pacchetti R](r-package-how-to-enable-or-disable.md)
+ [Sincronizzare i pacchetti R](package-install-uninstall-and-sync.md)
+ [Suggerimenti per l'uso di pacchetti R](tips-for-using-r-packages.md)
+ [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md)