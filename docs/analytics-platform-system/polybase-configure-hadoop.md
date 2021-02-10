---
title: 'Accedere ai dati esterni: Hadoop - PolyBase'
description: Viene illustrato come configurare la polibase in parallelo data warehouse per connettersi a Hadoop esterni.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 18bec3d694985e15b1ae2d813fddf2f378c9d78a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049107"
---
# <a name="configure-polybase-in-parallel-data-warehouse-to-access-external-data-in-hadoop"></a>Configurare la polibase in parallelo data warehouse per accedere ai dati esterni in Hadoop

Questo articolo illustra come usare la polibase in un appliance APS per eseguire query sui dati esterni in Hadoop.

## <a name="prerequisites"></a>Prerequisiti

PolyBase supporta due provider di Hadoop, Hortonworks Data Platform (HDP) e Cloudera Distributed Hadoop (CDH). Hadoop segue il modello "principale.secondaria.versione" per le nuove versioni e sono supportate tutte le versioni all'interno di una versione principale e secondaria supportata. Sono supportati i provider Hadoop seguenti:
 - Hortonworks HDP 1.3 su Linux/Windows Server  
 - Hortonworks HDP 2,1-2,6 in Linux
 - Hortonworks HDP 3,0-3,1 in Linux
 - Hortonworks HDP 2.1 - 2.3 su Windows Server  
 - Cloudera CDH 4.3 su Linux  
 - Cloudera CDH 5,1-5,5, 5,9-5,13, 5,15 & 5,16 in Linux

### <a name="configure-hadoop-connectivity"></a>Configurare la connettività Hadoop

Per prima cosa, configurare gli APS per l'uso del provider Hadoop specifico.

1. Eseguire [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con 'hadoop connectivity' e impostare un valore appropriato per il provider. Per trovare il valore per il provider, vedere [Configurazione della connettività di PolyBase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). 

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 and 3.0 - 3.1 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Riavviare l'area APS usando la pagina stato del servizio nel [Configuration Manager Appliance](launch-the-configuration-manager.md).
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Abilita calcolo distribuzione  

Per migliorare le prestazioni delle query, abilitare il calcolo con distribuzione nel cluster Hadoop:  
  
1. Aprire una connessione Desktop remoto al nodo di controllo PDW.

2. Trovare il file **yarn-site.xml** nel nodo di controllo. In genere il percorso è:  

   ```xml  
   C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf\  
   ```  

3. Nel computer Hadoop trovare il file analogo nella directory di configurazione Hadoop. Nel file trovare e copiare il valore della chiave di configurazione yarn.application.classpath.  
  
4. Nel nodo di controllo, nel **file diyarn.site.xml,** trovare la proprietà **Yarn. Application. classpath** . Incollare il valore dal computer Hadoop nell'elemento valore.  
  
5. Per tutte le versioni di CDH 5.X sarà necessario aggiungere i parametri di configurazione mapreduce.application.classpath alla fine del file yarn.site.xml o nel file mapred-site.xml. HortonWorks include queste configurazioni all'interno delle configurazioni yarn.application.classpath. Vedere [Configurazione di PolyBase](../relational-databases/polybase/polybase-configuration.md) per alcuni esempi.

## <a name="example-xml-files-for-cdh-5x-cluster-default-values"></a>File XML di esempio per i valori predefiniti del cluster CDH 5. X

File yarn-site.xml con i parametri di configurazione yarn.application.classpath e mapreduce.application.classpath.

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

Se si sceglie di suddividere le due impostazioni di configurazione nel mapred-site.xml e nella yarn-site.xml, i file sono i seguenti:

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

**mapred-site.xml**

Si noti che è stata aggiunta la proprietà mapreduce.application.classpath. In CDH 5. x sono disponibili i valori di configurazione con la stessa convenzione di denominazione in Ambari.

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
   <property>
     <name>mapred.min.split.size</name>
       <value>1073741824</value>
   </property>
   <property>
     <name>mapreduce.app-submission.cross-platform</name>
     <value>true</value>
   </property>
<property>
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
   </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
     <name>mapreduce.jobhistory.principal</name>
     <value></value>
   </property>
   <property>
     <name>mapreduce.jobhistory.address</name>
     <value></value>
   </property>
-->
</configuration>
```

## <a name="example-xml-files-for-hdp-3x-cluster-default-values"></a>File XML di esempio per i valori predefiniti del cluster HDP 3. X

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
  <property>
     <name>yarn.resourcemanager.connect.max-wait.ms</name>
     <value>40000</value>
  </property>
  <property>
     <name>yarn.resourcemanager.connect.retry-interval.ms</name>
     <value>30000</value>
  </property>
<!-- Applications' Configuration-->
  <property>
    <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
     <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
     <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
     <name>yarn.application.classpath</name>
     <value>$HADOOP_CONF_DIR,/usr/hdp/3.1.0.0-78/hadoop/*,/usr/hdp/3.1.0.0-78/hadoop/lib/*,/usr/hdp/current/hadoop-hdfs-client/*,/usr/hdp/current/hadoop-hdfs-client/lib/*,/usr/hdp/current/hadoop-yarn-client/*,/usr/hdp/current/hadoop-yarn-client/lib/*,/usr/hdp/3.1.0.0-78/hadoop-mapreduce/*,/usr/hdp/3.1.0.0-78/hadoop-yarn/*,/usr/hdp/3.1.0.0-78/hadoop-yarn/lib/*,/usr/hdp/3.1.0.0-78/hadoop-mapreduce/lib/*,/usr/hdp/share/hadoop/common/*,/usr/hdp/share/hadoop/common/lib/*,/usr/hdp/share/hadoop/tools/lib/*</value>
  </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
  <property>
     <name>yarn.resourcemanager.principal</name>
     <value></value>
  </property>
-->
</configuration>
```

## <a name="configure-an-external-table"></a>Configurare una tabella esterna

Per eseguire query sui dati nell'origine dati Hadoop, è necessario definire una tabella esterna da usare in query Transact-SQL. Le procedure seguenti descrivono come configurare la tabella esterna.

1. Creare una chiave master nel database. È necessario crittografare il segreto delle credenziali.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

2. Creare credenziali con ambito database per i cluster Hadoop con protezione Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

3. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION (Required) : Hadoop Name Node IP address and port.  
   -- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
   -- CREDENTIAL (Optional):  the database scoped credential, created above.  
   CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
         TYPE = HADOOP,
         LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',
         RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',
         CREDENTIAL = HadoopUser1
   );  
   ```

4. Creare un formato di file esterno con [CREATE EXTERNAL FILE FORMAT](../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE)  
   ```

5. Creare una tabella esterna che punta ai dati archiviati in Hadoop con [CREATE EXTERNAL TABLE](../t-sql/statements/create-external-table-transact-sql.md). In questo esempio i dati esterni contengono dati di sensori di auto.

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = MyHadoopCluster,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

6. Creare statistiche per una tabella esterna.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>Query PolyBase

PolyBase è adatto per assolvere a una triplice funzione:  
  
- Esecuzione di query ad hoc su tabelle esterne.  
- Importazione di dati.  
- Esportazione di dati.  

Le query seguenti forniscono esempi con dati fittizi di sensori di auto.

### <a name="ad-hoc-queries"></a>Query ad hoc  

La query ad-hoc seguente crea un join relazionale con dati Hadoop. Seleziona i clienti che hanno un ritmo più veloce di 35 mph, associando i dati dei clienti strutturati archiviati in APS con i dati del sensore auto archiviati in Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importazione di dati  

La query seguente importa i dati esterni in APS. In questo esempio vengono importati i dati per i driver veloci in APS per eseguire un'analisi più approfondita. Per migliorare le prestazioni, sfrutta la tecnologia columnstore in APS.  

```sql
CREATE TABLE Fast_Customers
WITH
(CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH (CustomerKey))
AS
SELECT DISTINCT
      Insured_Customers.CustomerKey, Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
```  

### <a name="exporting-data"></a>Esportazione di dati  

La query seguente esporta i dati da APS a Hadoop. Può essere usato per archiviare dati relazionali in Hadoop, mentre è comunque possibile eseguire query su di esso.

```sql
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] 
WITH (  
      LOCATION='/archive/customer/2009',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat
)  
AS
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssdt"></a>Visualizzare oggetti di polibase in SSDT  

In SQL Server Data Tools, le tabelle esterne vengono visualizzate in una cartella separata **tabelle esterne**. Le origini dati esterne e i formati di file esterni si trovano nelle sottocartelle in **External Resources**.  
  
![Oggetti di polibase in SSDT](media/polybase/external-tables-datasource.png)  

## <a name="next-steps"></a>Passaggi successivi

Per le impostazioni di sicurezza di Hadoop, vedere [configurare la sicurezza di Hadoop](polybase-configure-hadoop-security.md).<br>
Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](../relational-databases/polybase/polybase-guide.md). 
 
