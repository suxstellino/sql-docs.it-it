---
title: Modello di applicazione - partizionamento di tabelle con ottimizzazione per la memoria
description: Informazioni sul modello di progettazione dell'applicazione OLTP in memoria che archivia i dati attivi correnti in una tabella ottimizzata per la memoria e i dati meno recenti in una tabella partizionata.
ms.custom: seo-dt-2019,issue-PR=4700-14820
ms.date: 05/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 3f867763-a8e6-413a-b015-20e9672cc4d1
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f7cda5a657e6efd7ea6d9607d03eb8185455afba
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489648"
---
# <a name="application-pattern-for-partitioning-memory-optimized-tables"></a>Modello di applicazione per il partizionamento di tabelle con ottimizzazione per la memoria

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[hek_2](../../includes/hek-2-md.md)] supporta un modello di progettazione di applicazioni che gestisce le risorse per le prestazioni per dati relativamente correnti. Questo modello può essere applicato quando i dati correnti vengono letti o aggiornati molto più spesso rispetto ai dati meno recenti. In questo caso, i dati correnti sono detti *attivi* o *ad accesso frequente* e i dati meno recenti sono detti *ad accesso sporadico*.

Il concetto è archiviare i dati *ad accesso frequente* in una tabella ottimizzata per la memoria. Su base settimanale o mensile, i dati meno recenti che rientrano nella categoria dei dati *ad accesso sporadico* vengono spostati in una tabella partizionata. I dati nella tabella partizionata vengono archiviati su disco o in un'altra unità disco rigido, non in memoria.

In genere, questa progettazione usa una chiave **datetime** per consentire al processo di spostamento di distinguere in modo efficiente tra dati ad accesso frequente e quelli ad accesso sporadico.

## <a name="advanced-partitioning"></a>Partizionamento avanzato

In questa progettazione è previsto l'uso di una tabella partizionata che include anche una partizione ottimizzata per la memoria. Per il corretto funzionamento di questa progettazione, è necessario assicurarsi che tutte le tabelle condividano uno schema comune. Nell'esempio di codice riportato più avanti in questo articolo viene illustrata la tecnica.

Si presume che i nuovi dati siano ad accesso frequente per definizione. I dati ad accesso frequente vengono inseriti e aggiornati nella tabella ottimizzata per la memoria. I dati ad accesso sporadico vengono mantenuti nella tabella partizionata tradizionale. Periodicamente, una stored procedure aggiunge una nuova partizione. La partizione contiene i dati ad accesso sporadico più recenti che sono stati spostati fuori dalla tabella ottimizzata per la memoria.

Se un'operazione richiede solo dati ad accesso frequente, può usare le stored procedure compilate in modo nativo per accedere ai dati. Per unire in join la tabella ottimizzata per la memoria con la tabella partizionata, le operazioni che potrebbero accedere a dati ad accesso frequente o sporadico devono usare istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretate.

### <a name="add-a-partition"></a>Aggiungere una partizione

I dati che sono diventati di recente ad accesso sporadico devono essere spostati nella tabella partizionata. I passaggi per questo scambio di partizione periodico sono i seguenti:

1. Per i dati nella tabella ottimizzata per la memoria, determinare il valore datetime che rappresenta il limite o il valore di cambio data tra dati ad accesso frequente e dati diventati di recente ad accesso sporadico.
2. Inserire i nuovi dati ad accesso sporadico, dalla tabella OLTP in memoria in una tabella *cold\_staging*.
3. Eliminare gli stessi dati ad accesso sporadico dalla tabella ottimizzata per la memoria.
4. Scambiare la tabella cold\_staging in una partizione.
5. Aggiungere la partizione.

#### <a name="maintenance-window"></a>Finestra di manutenzione

Uno dei passaggi precedenti consiste nell'eliminare i dati diventati di recente ad accesso sporadico dalla tabella ottimizzata per la memoria. Esiste un intervallo di tempo tra questa eliminazione e il passaggio finale che aggiunge la nuova partizione. Durante questo intervallo, le applicazioni che tentano di leggere i dati diventati di recente ad accesso sporadico riscontreranno errori.

Per un esempio correlato, vedere [Partizionamento a livello di applicazione](../../relational-databases/in-memory-oltp/application-level-partitioning.md).

## <a name="code-sample"></a>Codice di esempio

L'esempio Transact-SQL seguente viene presentato in una serie di blocchi di codice più piccoli, solo per la facilità di presentazione. È possibile aggiungerli tutti in un unico grande blocco di codice per i test.

Nel suo complesso, l'esempio T-SQL illustra come usare una tabella ottimizzata per la memoria con una tabella basata su disco partizionata.

Nelle prime fasi dell'esempio T-SQL viene creato il database, quindi vengono creati gli oggetti, come le tabelle nel database. Nelle fasi successive viene illustrato come spostare i dati da una tabella ottimizzata per la memoria in una tabella partizionata.

### <a name="create-a-database"></a>Creazione di un database

Questa sezione dell'esempio T-SQL crea un database di prova. Il database è configurato in modo da supportare sia tabelle ottimizzate per la memoria che tabelle partizionate.

```sql
CREATE DATABASE PartitionSample;
GO

-- Add a FileGroup, enabled for In-Memory OLTP.
-- Change file path as needed.

ALTER DATABASE PartitionSample
    ADD FILEGROUP PartitionSample_mod
    CONTAINS MEMORY_OPTIMIZED_DATA;

ALTER DATABASE PartitionSample
    ADD FILE(
        NAME = 'PartitionSample_mod',
        FILENAME = 'c:\data\PartitionSample_mod')
    TO FILEGROUP PartitionSample_mod;
GO
```

### <a name="create-a-memory-optimized-table-for-hot-data"></a>Creare una tabella ottimizzata per la memoria per i dati ad accesso frequente

Questa sezione crea la tabella ottimizzata per la memoria che contiene i dati più recenti, in gran parte dati ancora ad accesso frequente.

```sql
USE PartitionSample;
GO

-- Create a memory-optimized table for the HOT Sales Order data.
-- Notice the index that uses datetime2.

CREATE TABLE dbo.SalesOrders_hot (
   so_id INT IDENTITY PRIMARY KEY NONCLUSTERED,
   cust_id INT NOT NULL,
   so_date DATETIME2 NOT NULL INDEX ix_date NONCLUSTERED,
   so_total MONEY NOT NULL,
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc)
) WITH (MEMORY_OPTIMIZED=ON);
GO
```

### <a name="create-a-partitioned-table-for-cold-data"></a>Creare una tabella partizionata per i dati ad accesso sporadico

In questa sezione viene creata la tabella partizionata che contiene i dati ad accesso sporadico.

```sql
-- Create a partition and table for the COLD Sales Order data.
-- Notice the index that uses datetime2.

CREATE PARTITION FUNCTION [ByDatePF](datetime2) AS RANGE RIGHT
   FOR VALUES();
GO

CREATE PARTITION SCHEME [ByDateRange]
   AS PARTITION [ByDatePF]
   ALL TO ([PRIMARY]);
GO

CREATE TABLE dbo.SalesOrders_cold (
   so_id INT NOT NULL,
   cust_id INT NOT NULL,
   so_date DATETIME2 NOT NULL,
   so_total MONEY NOT NULL,
   CONSTRAINT PK_SalesOrders_cold PRIMARY KEY (so_id, so_date),
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc)
) ON [ByDateRange](so_date);
GO
```

### <a name="create-a-table-to-store-cold-data-during-move"></a>Creare una tabella per archiviare i dati ad accesso sporadico durante lo spostamento

In questa sezione viene creata la tabella cold\_staging. Viene anche creata una vista che unisce i dati ad accesso frequente e sporadico dalle due tabelle.

```sql
-- A table used to briefly stage the newly cold data, during moves to a partition.

CREATE TABLE dbo.SalesOrders_cold_staging (
   so_id INT NOT NULL,
   cust_id INT NOT NULL,
   so_date datetime2 NOT NULL,
   so_total MONEY NOT NULL,
   CONSTRAINT PK_SalesOrders_cold_staging PRIMARY KEY (so_id, so_date),
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc),
   CONSTRAINT CHK_SalesOrders_cold_staging CHECK (so_date >= '1900-01-01')
);
GO

-- A view, for retrieving the aggregation of hot plus cold data.

CREATE VIEW dbo.SalesOrders
AS SELECT so_id,
          cust_id,
          so_date,
          so_total,
          1 AS 'is_hot'
       FROM dbo.SalesOrders_hot
   UNION ALL
   SELECT so_id,
          cust_id,
          so_date,
          so_total,
          0 AS 'is_cold'
       FROM dbo.SalesOrders_cold;
GO
```

### <a name="create-the-stored-procedure"></a>Creare la stored procedure

In questa sezione viene creata la stored procedure da eseguire periodicamente. La stored procedure consente di spostare i dati diventati ad accesso sporadico di recente dalla tabella ottimizzata per la memoria alla tabella partizionata.

```sql
-- A stored procedure to move all newly cold sales orders data
-- to its staging location.

CREATE PROCEDURE dbo.usp_SalesOrdersOffloadToCold @splitdate datetime2
   AS
   BEGIN
      BEGIN TRANSACTION;

      -- Insert the cold data as a temporary heap.
      INSERT INTO dbo.SalesOrders_cold_staging WITH (TABLOCKX)
      SELECT so_id , cust_id , so_date , so_total
         FROM dbo.SalesOrders_hot WITH (serializable)
         WHERE so_date <= @splitdate;

      -- Delete the moved data from the hot table.
      DELETE FROM dbo.SalesOrders_hot WITH (SERIALIZABLE)
         WHERE so_date <= @splitdate;

      -- Update the partition function, and switch in the new partition.
      ALTER PARTITION SCHEME [ByDateRange] NEXT USED [PRIMARY];

      DECLARE @p INT = (
        SELECT MAX(partition_number)
            FROM sys.partitions
            WHERE object_id = OBJECT_ID('dbo.SalesOrders_cold'));

      EXEC sp_executesql
        N'ALTER TABLE dbo.SalesOrders_cold_staging
            SWITCH TO dbo.SalesOrders_cold partition @i',
        N'@i int',
        @i = @p;

      ALTER PARTITION FUNCTION [ByDatePF]()
      SPLIT RANGE( @splitdate);

      -- Modify a constraint on the cold_staging table, to align with new partition.
      ALTER TABLE dbo.SalesOrders_cold_staging
         DROP CONSTRAINT CHK_SalesOrders_cold_staging;

      DECLARE @s nvarchar( 100) = CONVERT( nvarchar( 100) , @splitdate , 121);
      DECLARE @sql nvarchar( 1000) = N'alter table dbo.SalesOrders_cold_staging 
         add constraint CHK_SalesOrders_cold_staging check (so_date > ''' + @s + ''')';
      PRINT @sql;
      EXEC sp_executesql @sql;

      COMMIT;
END;
GO
```

### <a name="prepare-sample-data-and-demo-the-stored-procedure"></a>Preparare i dati di esempio ed eseguire la stored procedure

Questa sezione genera e inserisce dati di esempio, quindi esegue la stored procedure a titolo dimostrativo.

```sql
-- Insert sample values into the hot table.
INSERT INTO dbo.SalesOrders_hot VALUES(1,SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(1, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(1, SYSDATETIME(), 1);
GO

-- Verify that the hot data is in the table, by selecting from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Treat all data in the hot table as cold data:
-- Run the stored procedure, to move (offload) all sales orders to date to cold storage.
DECLARE @t datetime2 = SYSDATETIME();
EXEC dbo.usp_SalesOrdersOffloadToCold @t;

-- Again, read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Retrieve the name of every partition.
SELECT OBJECT_NAME( object_id) , * FROM sys.dm_db_partition_stats ps
   WHERE object_id = OBJECT_ID( 'dbo.SalesOrders_cold');

-- Insert more data into the hot table.
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO

-- Read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Again, run the stored procedure, to move all sales orders to date to cold storage.
DECLARE @t datetime2 = SYSDATETIME();
EXEC dbo.usp_SalesOrdersOffloadToCold @t;

-- Read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Again, retrieve the name of every partition.
-- The stored procedure can modify the partitions.
SELECT OBJECT_NAME( object_id) , partition_number , row_count
  FROM sys.dm_db_partition_stats ps
  WHERE object_id = OBJECT_ID( 'dbo.SalesOrders_cold')
    AND index_id = 1;
```

### <a name="drop-all-the-demo-objects"></a>Eliminare tutti gli oggetti dimostrativi

Ricordarsi di rimuovere il database di test dimostrativo dal sistema di test.

```sql
-- You must first leave the context of the PartitionSample database.

-- USE <A-Database-Name-Here>;
GO

DROP DATABASE PartitionSample;
GO
```

## <a name="see-also"></a>Vedere anche

[Tabelle ottimizzate per la memoria](./sample-database-for-in-memory-oltp.md)