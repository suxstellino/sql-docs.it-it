---
title: 'Accedere ai dati esterni: Archiviazione BLOB di Azure - PolyBase'
description: L'articolo usa PolyBase su un'istanza di SQL Server con Archiviazione BLOB di Azure. La tecnologia PolyBase è adatta per le query ad hoc di tabelle esterne e per l'importazione/esportazione di dati.
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 5363cabdce211884ae9e0f998fcde3056700d7a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073645"
---
# <a name="configure-polybase-to-access-external-data-in-azure-blob-storage"></a>Configurare PolyBase per l'accesso a dati esterni in Archiviazione BLOB di Azure

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L'articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in Archiviazione BLOB di Azure.

## <a name="prerequisites"></a>Prerequisiti

Se PolyBase non è stato installato, vedere [Installazione di PolyBase](polybase-installation.md). Nell'articolo sull'installazione vengono illustrati i prerequisiti.

### <a name="configure-azure-blob-storage-connectivity"></a>Configurare la connettività ad Archiviazione BLOB di Azure

Configurare prima di tutto SQL Server PolyBase per usare Archiviazione BLOB di Azure.

1. Eseguire [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con 'hadoop connectivity' impostato su un provider di Archiviazione BLOB di Azure. Per trovare il valore per i provider, vedere [Configurazione della connettività di PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). Per impostazione predefinita, l'opzione hadoop connectivity è impostata su 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Riavviare SQL Server tramite **services.msc**. Il riavvio di SQL Server comporta il riavvio di questi servizi:  

   - Servizio spostamento dati di PolyBase per SQL Server  
   - Motore di PolyBase per SQL Server  
  
   ![arrestare e avviare servizi PolyBase in services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "arrestare e avviare servizi PolyBase in services.msc")  
  
## <a name="configure-an-external-table"></a>Configurare una tabella esterna

Per eseguire query sui dati nell'origine dati Hadoop, è necessario definire una tabella esterna da usare in query Transact-SQL. Le procedure seguenti descrivono come configurare la tabella esterna.

1. Creare una chiave master nel database. La chiave master è necessaria per crittografare il segreto delle credenziali.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

1. Creare credenziali con ambito database per Archiviazione BLOB di Azure.

   ```sql
   -- IDENTITY: any string (this is not used for authentication to Azure storage).  
   -- SECRET: your Azure storage account key.  
   CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
   WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';
   ```

1. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION:  Azure account storage account name and blob container name.  
   -- CREDENTIAL: The database scoped credential created above.  
   CREATE EXTERNAL DATA SOURCE AzureStorage with (  
         TYPE = HADOOP,
         LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
         CREDENTIAL = AzureStorageCredential  
   );  
   ```

1. Creare un formato di file esterno con [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))  
   ```

1. Creare una tabella esterna che punta ai dati archiviati in Archiviazione di Azure con [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). In questo esempio i dati esterni contengono dati di sensori di auto.

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
         DATA_SOURCE = AzureStorage,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

1. Creare statistiche per una tabella esterna.

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

La query seguente esporta i dati da SQL Server in Archiviazione BLOB di Azure. Abilitare prima l'esportazione di PolyBase. Creare poi una tabella esterna per la destinazione prima dell'esportazione dei dati.

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

Con questo metodo, l'esportazione di PolyBase può creare più file.

## <a name="view-polybase-objects-in-ssms"></a>Visualizzare gli oggetti PolyBase in SQL Server Management Studio  

In SQL Server Management Studio, le tabelle esterne vengono visualizzate in una cartella separata **Tabelle esterne**. Le origini dati esterne e i formati di file esterni si trovano nelle sottocartelle in **External Resources**.  
  
![Oggetti PolyBase in SQL Server Management Studio](media/polybase-management.png)  

## <a name="next-steps"></a>Passaggi successivi

Per esplorare altri modi per usare e monitorare PolyBase, vedere gli articoli seguenti:

[Gruppi con scalabilità orizzontale di PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
[Risoluzione dei problemi di PolyBase](polybase-troubleshooting.md).  
