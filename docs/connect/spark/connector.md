---
title: Connettore Apache Spark per SQL Server
description: Informazioni su come usare il connettore Apache Spark per SQL Server.
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: 15a4bdb66ea87f85337540adfec4b5e175b1c55b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058126"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Connettore Apache Spark: SQL Server e Azure SQL

Il connettore Apache Spark per SQL Server e Azure SQL è un connettore ad alte prestazioni che consente di usare i dati transazionali in analisi dei Big Data e di salvare i risultati per la creazione di query o report ad hoc. Il connettore consente di usare qualsiasi database SQL, in locale o nel cloud, come origine dati di input o sink di dati di output per i processi Spark.

Questa libreria contiene il codice sorgente per il connettore Apache Spark per SQL Server e Azure SQL.

[Apache Spark](https://spark.apache.org/) è un motore di analisi unificato per l'elaborazione di dati su larga scala.

È possibile ottenere il connettore da [Maven](https://search.maven.org/search?q=spark-mssql-connector). Per importare il connettore, usare la coordinata `com.microsoft.azure:spark-mssql-connector:<latest version>`. È anche possibile creare il connettore dall'origine o scaricare il file con estensione jar dalla sezione Release in GitHub. Per informazioni aggiornate sul connettore, vedere il [repository GitHub del connettore SQL Spark](https://github.com/microsoft/sql-spark-connector).

## <a name="supported-features"></a>Funzionalità supportate

* Supporto di tutte le associazioni Spark (Scala, Python, R)
* Supporto di autenticazione di base e keytab Active Directory (AD)
* Supporto della scrittura di `dataframe` riordinato
* Supporto della scrittura in Istanza singola e Pool di dati di SQL Server in cluster Big Data di SQL Server
* Supporto di Reliable Connector per Istanza singola di SQL Server

| Componente                            | Versioni supportate              |
|--------------------------------------|---------------------------------|
| Apache Spark                         | 2.4.5 (Spark 3.0 non è supportato) |
| Scala                                | 2,11                            |
| Microsoft JDBC Driver per SQL Server | 8.2                             |
| Microsoft SQL Server                 | SQL Server 2008 o versione successiva        |
| Database SQL di Azure                  | Supportato                       |

> [!NOTE]
> L'uso di Azure Synapse Analytics non è stato testato con questo connettore. È possibile che funzioni correttamente, ma potrebbero verificarsi conseguenze impreviste.

### <a name="supported-options"></a>Opzioni supportate
Il connettore Apache Spark per SQL Server e Azure SQL supporta le opzioni definite qui: [JDBC per origine dati SQL](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

Sono supportate anche le opzioni seguenti:

| Opzione | Predefinito | Descrizione |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` o `NO_DUPLICATES`. `NO_DUPLICATES` implementa un inserimento affidabile negli scenari di riavvio dell'executor |
| `dataPoolDataSource` | `none` | `none` indica che il valore non è impostato e che il connettore deve scrivere in una singola istanza di SQL Server. Impostare questo valore sul nome dell'origine dati per scrivere in una tabella del pool di dati in cluster Big Data|
| `isolationLevel` | `READ_COMMITTED` | Specificare il livello di isolamento |
| `tableLock` | `false` | Implementa un inserimento con l'opzione `TABLOCK` per migliorare le prestazioni di scrittura |
| `schemaCheckEnabled` | `true` | Disabilita la verifica rigida dello schema della tabella SQL e del frame di dati quando è impostata su false |

Altre [opzioni di copia bulk](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions) possono essere impostate come opzioni in `dataframe` e verranno passate alle API `bulkcopy` durante la scrittura

## <a name="performance-comparison"></a>Confronto delle prestazioni

Il connettore Apache Spark per SQL Server e Azure SQL è 15 volte più veloce del connettore JDBC generico per la scrittura in SQL Server. Le prestazioni variano a seconda del tipo, del volume di dati, delle opzioni usate e possono far registrare variazioni da un'esecuzione all'altra. I risultati di prestazioni seguenti corrispondono al tempo impiegato per sovrascrivere una tabella SQL con 143,9 milioni di righe in un `dataframe` Spark. Il `dataframe` Spark viene costruito leggendo la tabella HDFS `store_sales` generata con il [benchmark TPCDS Spark](https://github.com/databricks/spark-sql-perf). Il tempo per la lettura da `store_sales` a `dataframe` non è incluso. I risultati corrispondono a una media calcolata su tre esecuzioni.

| Tipo connettore | Opzioni | Descrizione |  Tempo di scrittura |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | Impostazione predefinita | Connettore JDBC generico con opzioni predefinite |  1385 secondi |
| `sql-spark-connector` | `BEST_EFFORT` | `sql-spark-connector` a massimo sforzo con opzioni predefinite |580 secondi |
| `sql-spark-connector` | `NO_DUPLICATES ` | `sql-spark-connector` affidabile | 709 secondi |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | `sql-spark-connector` a massimo sforzo con blocco di tabella abilitato | 72 secondi |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| `sql-spark-connector` affidabile con blocco di tabella abilitato| 198 secondi |

File di configurazione

- Configurazione Spark: num_executors = 20, executor_memory = '1664 m', executor_cores = 2
- Configurazione generatore dati: scale_factor=50, partitioned_tables=true
- File di dati `store_sales` con 143.997.590 righe

Ambiente

- [Cluster Big Data di SQL Server](../../big-data-cluster/release-notes-big-data-cluster.md) CU5
- `master` + 6 nodi
- Ogni nodo con server gen 5, 512 GB di RAM, 4 TB di NVM per nodo, NIC 10 GB

## <a name="commonly-faced-issues"></a>Problemi comuni

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

Questo problema si verifica quando si usa una versione precedente del driver MSSQL (ora incluso in questo connettore) nell'ambiente Hadoop in uso. Se si stava usando il connettore Azure SQL precedente e sono stati installati manualmente driver nel cluster per la compatibilità con Azure Active Directory, sarà necessario rimuovere tali driver.

Passaggi per risolvere il problema:

1. Se si usa un ambiente Hadoop generico, controllare e rimuovere il file MSSQL con estensione jar: `rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar`. Se si usa Databricks, aggiungere uno script di inizializzazione globale o cluster per rimuovere le versioni precedenti del driver MSSQL dalla cartella `/databricks/jars` o aggiungere la riga seguente a uno script esistente: `rm /databricks/jars/*mssql*`
2. Aggiungere i pacchetti `adal4j` e `mssql`. Ad esempio è possibile usare Maven, ma qualsiasi modalità dovrebbe funzionare. 

   > [!CAUTION]
   > Non installare il connettore Spark SQL in questo modo.

1. Aggiungere la classe driver alla configurazione della connessione. Ad esempio:

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

Per altre informazioni e spiegazioni, vedere la pagina relativa alla risoluzione di [https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339).

## <a name="get-started"></a>Introduzione

Il connettore Apache Spark per SQL Server e Azure SQL si basa sull'API Spark DataSourceV1 e sull'API SQL Server Bulk e usa la stessa interfaccia del connettore Spark SQL JDBC incorporato. Questa integrazione consente di integrare facilmente il connettore ed eseguire la migrazione dei processi Spark esistenti semplicemente aggiornando il parametro format con `com.microsoft.sqlserver.jdbc.spark`.

Per includere il connettore nei progetti, scaricare questo repository e compilare il file con estensione jar usando SBT.

## <a name="write-to-a-new-sql-table"></a>Scrivere in una nuova tabella SQL

> [!WARNING]
> La modalità `overwrite` elimina innanzitutto la tabella se esiste già nel database per impostazione predefinita. Usare questa opzione con cautela per evitare perdite di dati impreviste.

Quando si usa la modalità `overwrite`, se non si usa l'opzione `truncate` quando si ricrea la tabella, gli indici vanno perduti. Una tabella columnstore diventa ora un heap. Se si vuole mantenere l'indicizzazione esistente, specificare anche l'opzione `truncate` con valore true. Ad esempio: `.option("truncate","true")`.

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

### <a name="append-to-sql-table"></a>Accodare alla tabella SQL

```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>Specificare il livello di isolamento

Per impostazione predefinita, questo connettore usa il livello di isolamento `READ_COMMITTED` quando si esegue l'inserimento bulk nel database. Se si vuole eseguire l'override del livello di isolamento, usare l'opzione `mssqlIsolationLevel` come illustrato di seguito.

```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>Leggere dalla tabella SQL

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="azure-active-directory-authentication"></a>Autenticazione di Azure Active Directory

### <a name="python-example-with-service-principal"></a>Esempio di Python con entità servizio

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>Esempio di Python con password di Active Directory

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

Per eseguire l'autenticazione con Active Directory, è necessario installare una dipendenza obbligatoria.

Per **Scala** è necessario installare l'artefatto `_com.microsoft.aad.adal4j_`.

Per **Python** è necessario installare la libreria `_adal_`.  Questa operazione è disponibile tramite PIP.

Per esempi, vedere [Notebook di esempio](https://github.com/microsoft/sql-spark-connector/tree/master/samples).

## <a name="support"></a>Supporto

Il connettore Apache Spark per Azure SQL e SQL Server è un progetto open source. Questo connettore non dispone di supporto tecnico Microsoft. Per problemi o domande sul connettore, creare una voce nel repository del progetto. La community del connettore è attiva ed elabora le domande inoltrate.

## <a name="next-steps"></a>Passaggi successivi

Visitare il [repository GitHub del connettore Spark SQL](https://github.com/microsoft/sql-spark-connector).

Per informazioni sui livelli di isolamento, vedere [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).
