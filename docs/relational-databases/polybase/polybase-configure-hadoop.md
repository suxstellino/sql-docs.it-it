---
title: 'Accedere a dati esterni: Hadoop - PolyBase'
description: L'articolo usa PolyBase in un'SQL Server con Hardoop. La tecnologia PolyBase è adatta per le query ad hoc di tabelle esterne e per l'importazione/esportazione di dati.
ms.date: 12/13/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019
ms.openlocfilehash: 3b56ae68b582916debfdaf4696823422940887f3
ms.sourcegitcommit: 554497d604e0c63c055bf6d572d92fdadb027dbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107571374"
---
# <a name="configure-polybase-to-access-external-data-in-hadoop"></a>Configurare PolyBase per l'accesso a dati esterni in Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

L'articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in Hadoop.

## <a name="prerequisites"></a>Prerequisiti

- Se PolyBase non è stato installato, vedere [Installazione di PolyBase](polybase-installation.md). Nell'articolo sull'installazione vengono illustrati i prerequisiti.

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

- A partire da SQL Server 2019, è anche necessario [abilitare la funzionalità PolyBase](polybase-installation.md#enable).

::: moniker-end

- PolyBase supporta due provider di Hadoop, Hortonworks Data Platform (HDP) e Cloudera Distributed Hadoop (CDH). Hadoop segue il modello "principale.secondaria.versione" per le nuove versioni e sono supportate tutte le versioni all'interno di una versione principale e secondaria supportata. Sono supportati i provider Hadoop seguenti:

  - Hortonworks HDP 1.3, 2.1-2.6, 3.0 in Linux
  - Hortonworks HDP 1.3, 2.1-2.3, 3.1 <sup>*</sup> in Windows Server
  - Cloudera CDH 4.3, 5.1 - 5.5, 5.9 - 5.13 in Linux

   <sup>*</sup> Hortonworks HDB 3.1 richiede SQL Server 2019 CU9 (15.0.4102) o versione successiva.

> [!NOTE]
> PolyBase supporta le zone di crittografia Hadoop a partire da SQL Server 2016 SP1 CU7 e SQL Server 2017 CU3. Se si usano gruppi [di scalabilità orizzontale PolyBase,](polybase-scale-out-groups.md)anche tutti i nodi di calcolo devono essere in una compilazione che include il supporto per le zone di crittografia Hadoop.

### <a name="configure-hadoop-connectivity"></a>Configurare la connettività Hadoop

Configurare prima di tutto SQL Server PolyBase per usare il provider di Hadoop specifico.

1. Eseguire [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con 'hadoop connectivity' e impostare un valore appropriato per il provider. Per trovare il valore per il provider, vedere [Configurazione della connettività di PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). Per impostazione predefinita, l'opzione hadoop connectivity è impostata su 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. È necessario riavviare SQL Server tramite **services.msc**. Il riavvio di SQL Server comporta il riavvio di questi servizi:  

   - Servizio spostamento dati di PolyBase per SQL Server  
   - Motore di PolyBase per SQL Server  
  
   ![arrestare e avviare servizi PolyBase in services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "arrestare e avviare servizi PolyBase in services.msc")  
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Abilitare il calcolo pushdown  

Per migliorare le prestazioni delle query, abilitare il calcolo con distribuzione nel cluster Hadoop:  
  
1. Trovare il file **yarn-site.xml** nel percorso di installazione di SQL Server. In genere il percorso è:  

   ```xml  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf\  
   ```  

1. Nel computer Hadoop trovare il file analogo nella directory di configurazione Hadoop. Nel file trovare e copiare il valore della chiave di configurazione yarn.application.classpath.  
  
1. Nel computer SQL Server, nel **fileyarn-site.xml trovare** la **proprietà yarn.application.classpath.** Incollare il valore dal computer Hadoop nell'elemento valore.  
  
1. Per tutte le versioni di CDH 5.X sarà necessario aggiungere i parametri di configurazione mapreduce.application.classpath alla fine del file yarn-site.xml o nel file mapred-site.xml. HortonWorks include queste configurazioni all'interno delle configurazioni yarn.application.classpath. Vedere [Configurazione di PolyBase](../../relational-databases/polybase/polybase-configuration.md) per alcuni esempi.

## <a name="configure-an-external-table"></a>Configurare una tabella esterna

Per eseguire query sui dati nell'origine dati Hadoop, è necessario definire una tabella esterna da usare in query Transact-SQL. Le procedure seguenti descrivono come configurare la tabella esterna.

1. Creare una chiave master nel database, se non esiste già. Questo passaggio è necessario per crittografare il segreto delle credenziali.

     ```sql
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';  
     ```
    ## <a name="arguments"></a>Argomenti
    PASSWORD ='password'

    Password utilizzata per crittografare la chiave master nel database. password deve soddisfare i requisiti per i criteri password di Windows del computer che esegue l'hosting dell'istanza di SQL Server.
1. Creare credenziali con ambito database per i cluster Hadoop con protezione Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

2. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

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

3. Creare un formato di file esterno con [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))
   ```

4. Creare una tabella esterna che punta ai dati archiviati in Hadoop con [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). In questo esempio i dati esterni contengono dati di sensori di auto.

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

5. Creare statistiche per una tabella esterna.

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

La query ad-hoc seguente crea un join relazionale con dati Hadoop. Seleziona i clienti che guidano a velocità superiori a 35 miglia/h, unendo in join i dati dei clienti strutturati archiviati in SQL Server con i dati dei sensori di auto archiviati in Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importazione di dati  

La query seguente importa dati esterni in SQL Server. Questo esempio importa i dati relativi agli autisti che guidano veloce in SQL Server per eseguire un'analisi più dettagliata. Per migliorare le prestazioni, sfrutta la tecnologia columnstore.  

```sql
SELECT DISTINCT
      Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```  

### <a name="exporting-data"></a>Esportazione di dati  

La query seguente esporta i dati da SQL Server in Hadoop. A tale scopo, è prima di tutto necessario abilitare l'esportazione di PolyBase. Creare poi una tabella esterna per la destinazione prima dell'esportazione dei dati.

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
      [FirstName] char(25) NOT NULL,
      [LastName] char(25) NOT NULL,
      [YearlyIncome] float NULL,
      [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
      LOCATION='/old_data/2009/customerdata',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat,  
      REJECT_TYPE = VALUE,  
      REJECT_VALUE = 0  
);  

-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssms"></a>Visualizzare gli oggetti PolyBase in SQL Server Management Studio  

In SQL Server Management Studio, le tabelle esterne vengono visualizzate in una cartella separata **Tabelle esterne**. Le origini dati esterne e i formati di file esterni si trovano nelle sottocartelle in **External Resources**.  
  
![Oggetti PolyBase in SQL Server Management Studio](media/polybase-management.png)  

## <a name="next-steps"></a>Passaggi successivi

Per esplorare altri modi per usare e monitorare PolyBase, vedere gli articoli seguenti:

[Gruppi con scalabilità orizzontale di PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
[Risoluzione dei problemi di PolyBase](polybase-troubleshooting.md).  
