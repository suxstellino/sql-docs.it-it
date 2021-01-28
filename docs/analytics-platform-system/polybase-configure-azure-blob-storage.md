---
title: Usare la polibase per accedere ai dati esterni nell'archivio BLOB di Azure
description: Viene illustrato come usare la polibase in un data warehouse parallelo (APS) per eseguire query sui dati esterni nell'archivio BLOB di Azure.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 59f9e29940947c1afcf3321fe138030a3fb0d5f1
ms.sourcegitcommit: 76c5e10704e3624b538b653cf0352e606b6346d3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98924700"
---
# <a name="configure-polybase-to-access-external-data-in-azure-blob-storage"></a>Configurare PolyBase per l'accesso a dati esterni in Archiviazione BLOB di Azure

L'articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in Archiviazione BLOB di Azure.

> [!NOTE]
> APS supporta attualmente solo l'archiviazione BLOB di Azure con ridondanza locale (con ridondanza locale) standard per utilizzo generico.

## <a name="prerequisites"></a>Prerequisiti

 - Archiviazione BLOB di Azure nella sottoscrizione.
 - Un contenitore creato nell'archivio BLOB di Azure.

### <a name="configure-azure-blob-storage-connectivity"></a>Configurare la connettività di archiviazione BLOB di Azure

Per prima cosa, configurare gli APS per l'uso dell'archiviazione BLOB di Azure.

1. Eseguire [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con 'hadoop connectivity' impostato su un provider di Archiviazione BLOB di Azure. Per trovare il valore per i provider, vedere [Configurazione della connettività di PolyBase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure Blob Storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Riavviare l'area APS usando la pagina stato del servizio nel [Configuration Manager Appliance](launch-the-configuration-manager.md).
  
## <a name="configure-an-external-table"></a>Configurare una tabella esterna

Per eseguire query sui dati nell'archivio BLOB di Azure, è necessario definire una tabella esterna da usare nelle query Transact-SQL. Le procedure seguenti descrivono come configurare la tabella esterna.

1. Creare una chiave master nel database. È necessario crittografare il segreto delle credenziali.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

1. Creare credenziali con ambito database per l'archiviazione BLOB di Azure.

   ```sql
   -- IDENTITY: any string (this is not used for authentication to Azure storage).  
   -- SECRET: your Azure storage account key.  
   CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
   WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';
   ```

1. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION:  Azure account storage account name and blob container name.  
   -- CREDENTIAL: The database scoped credential created above.  
   CREATE EXTERNAL DATA SOURCE AzureStorage with (  
         TYPE = HADOOP,
         LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
         CREDENTIAL = AzureStorageCredential  
   );  
   ```

1. Creare un formato di file esterno con [CREATE EXTERNAL FILE FORMAT](../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Azure Blob Storage (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   -- In this example, the files are pipe (|) delimited
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE)  
   ```

1. Creare una tabella esterna che punta ai dati archiviati in Archiviazione di Azure con [CREATE EXTERNAL TABLE](../t-sql/statements/create-external-table-transact-sql.md). In questo esempio i dati esterni contengono dati di sensori di auto.

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

La query ad hoc seguente unisce i dati relazionali con i dati nell'archiviazione BLOB di Azure. Seleziona i clienti che hanno un ritmo più veloce di 35 mph, associando i dati dei clienti strutturati archiviati in SQL Server con i dati del sensore auto archiviati nell'archivio BLOB di Azure.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
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

La query seguente consente di esportare i dati dagli APS nell'archivio BLOB di Azure. Può essere usato per archiviare dati relazionali nell'archivio BLOB di Azure, pur continuando a eseguire query.

```sql
-- Export data: Move old data to Azure Blob Storage while keeping it query-able via an external table.  
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] 
WITH (  
      LOCATION='/archive/customer/2009',  
      DATA_SOURCE = AzureStorage,  
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

Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](../relational-databases/polybase/polybase-guide.md). 

