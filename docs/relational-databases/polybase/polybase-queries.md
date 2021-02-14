---
title: Scenari di query di PolyBase | Microsoft Docs
description: Esempi di query che usano la funzionalità PolyBase di SQL Server, tra cui SELECT, JOIN esterno con tabelle locali, importazione/esportazione di dati e nuove viste del catalogo.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: cdcd5d06395d8216fdf3e9c61b8434ab226c9837
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351837"
---
# <a name="polybase-query-scenarios"></a>Scenari di query di PolyBase

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Questo articolo propone alcuni esempi di query che usano la funzionalità [PolyBase](../../relational-databases/polybase/polybase-guide.md) di SQL Server (a partire dalla versione 2016). Prima di usare questi esempi, è necessario installare e configurare PolyBase. Per altre informazioni, vedere la [panoramica di PolyBase](polybase-guide.md).
  
Eseguire istruzioni Transact-SQL su tabelle esterne oppure usare gli strumenti di Business Intelligence per eseguire query su tabelle esterne.
  
## <a name="select-from-external-table"></a>SELECT da tabella esterna  

Una query semplice che restituisce i dati da una tabella esterna definita.  

```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```

Una query semplice che include un predicato.

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>JOIN di tabelle esterne con tabelle locali

```sql
SELECT InsuranceCustomers.FirstName,   
   InsuranceCustomers.LastName,   
   SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  

## <a name="import-data"></a>Importa dati

Importare dati da Hadoop o dall'archiviazione di Azure in SQL Server per l'archivio permanente. Usare SELECT INTO per importare i dati a cui fa riferimento una tabella esterna, per l'archiviazione permanente in SQL Server. Creare un tabella relazionale e quindi creare un indice columnstore sulla tabella in un secondo passaggio.

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
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

## <a name="export-data"></a>Esportazione dei dati

Esportare dati da SQL Server in Hadoop o Archiviazione di Azure. 

Per prima cosa abilitare la funzionalità di esportazione impostando il valore `sp_configure` di 'allow polybase export' su 1. Creare quindi una tabella esterna che punta alla directory di destinazione. L'istruzione CREATE EXTERNAL TABLE crea la directory di destinazione, se non esiste già. Usare quindi INSERT INTO per esportare i dati da una tabella di SQL Server locale a un'origine dati esterna. 

I risultati dell'istruzione SELECT vengono esportati nel percorso specificato usando il formato di file specificato. I file esterni sono denominati *QueryID_date_time_ID.format*, dove *ID* è un identificatore incrementale e *format* è il formato dei dati esportati. Ad esempio, un nome di file potrebbe essere QID776_20160130_182739_0.orc.

> [!NOTE]
> Quando si esportano dati in Hadoop o in Archiviazione BLOB di Azure tramite PolyBase, vengono esportati solo i dati e non i nomi di colonne (metadati) definiti nel comando CREATE EXTERNAL TABLE.

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
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
INSERT INTO dbo.FastCustomers2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>Nuove viste del catalogo

Le nuove viste del catalogo seguenti mostrano le risorse esterne.

```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```

 Per determinare se una tabella è una tabella esterna, usare `is_external`  

```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  

## <a name="next-steps"></a>Passaggi successivi  

Per altre informazioni sulla risoluzione dei problemi, vedere [Risoluzione dei problemi di PolyBase](../../relational-databases/polybase/polybase-troubleshooting.md).
