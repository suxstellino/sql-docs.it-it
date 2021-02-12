---
title: Uso del connettore Apache Spark per SQL Server e SQL Azure
titleSuffix: SQL Server big data clusters
description: Informazioni su come usare il connettore Apache Spark per SQL Server e SQL Azure per le operazioni di lettura e scrittura in SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 29a660452841ee0057485c1a44c8ff30abd6f230
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045591"
---
# <a name="use-the-apache-spark-connector-for-sql-server-and-azure-sql"></a>Usare il connettore Apache Spark per SQL Server e SQL Azure

Il [connettore Apache Spark per SQL Server e Azure SQL](https://github.com/microsoft/sql-spark-connector) è un connettore ad alte prestazioni che consente di usare i dati transazionali in analisi di Big Data e di salvare i risultati per la creazione di query o report ad hoc. Il connettore consente di usare qualsiasi database SQL, in locale o nel cloud, come origine dati di input o sink di dati di output per i processi Spark. Il connettore usa le API di scrittura bulk di SQL Server. I parametri di scrittura bulk possono essere passati come parametri facoltativi dall'utente e vengono passati così come sono dal connettore all'API sottostante. Per altre informazioni sulle operazioni di scrittura bulk, vedere [Uso della copia bulk con il driver JDBC]( ../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions).

Per impostazione predefinita il connettore è incluso nei cluster Big Data di SQL Server.

Altre informazioni sul connettore sono disponibili nel [repository open source](https://github.com/microsoft/sql-spark-connector). Vedere gli [esempi](https://github.com/microsoft/sql-spark-connector/tree/master/samples).

## <a name="write-to-a-new-sql-table"></a>Scrivere in una nuova tabella SQL

>[!CAUTION]
> Nella modalità `overwrite` il connettore elimina prima la tabella se esiste già nel database per impostazione predefinita. Usare questa opzione con cautela per evitare perdite di dati impreviste.
> 
> Nella modalità `overwrite`, se non si usa l'opzione `truncate` in fase di ricreazione della tabella, gli indici vanno persi. Ad esempio, una tabella columnstore diventa un heap. Se si vuole mantenere l'indicizzazione esistente, specificare anche l'opzione `truncate` con valore `true`. Ad esempio `.option("truncate",true)`

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

## <a name="append-to-sql-table"></a>Accodare alla tabella SQL
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

Per impostazione predefinita, questo connettore usa il livello di isolamento READ_COMMITTED quando si esegue l'inserimento bulk nel database. Se si vuole eseguire l'override del livello di isolamento e usarne un altro, usare l'opzione `mssqlIsolationLevel` come illustrato di seguito.
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

## <a name="non-active-directory-mode"></a>Modalità non Active Directory

Nella sicurezza in modalità non Active Directory ogni utente ha un nome utente e una password che devono essere specificati come parametri durante la creazione di un'istanza del connettore per eseguire operazioni di lettura e/o scrittura.

Di seguito è riportato un esempio di creazione di un'istanza del connettore per la modalità non Active Directory. Prima di eseguire lo script, sostituire `?` con il valore dell'account.

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark" 

url = "jdbc:sqlserver://master-p-svc;databaseName=?;"
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("user", ?) \ 
   .option("password",?) 
writer.save() 
```

## <a name="active-directory-mode"></a>Modalità Active Directory

Nella sicurezza in modalità Active Directory, dopo che un utente ha generato un file keytab, l'utente deve specificare `principal` e `keytab` come parametri durante la creazione di un'istanza del connettore.

In questa modalità, il driver carica il file keytab nei rispettivi contenitori degli esecutori. Quindi, gli esecutori usano il nome dell'entità e il keytab per generare un token usato per creare un connettore JDBC per la lettura/scrittura.

Di seguito è riportato un esempio di creazione di un'istanza del connettore per la modalità Active Directory. Prima di eseguire lo script, sostituire `?` con il valore dell'account.

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark"

url = "jdbc:sqlserver://master-p-svc;databaseName=?;integratedSecurity=true;authenticationScheme=JavaKerberos;" 
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("principal", ?) \ 
   .option("keytab", ?)   

writer.save() 
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md)

Commenti o suggerimenti sulle funzionalità per i cluster Big Data di SQL Server? [Lasciare una nota nella pagina per l'invio di commenti e suggerimenti per i cluster Big Data di SQL Server](https://aka.ms/sql-server-bdc-feedback).
