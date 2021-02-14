---
title: Introduzione alle funzionalità relative alle prestazioni di SQL Server in Linux
description: Questo articolo fornisce un'introduzione alle funzionalità per le prestazioni di SQL Server per gli utenti Linux che non hanno familiarità con SQL Server. Molti di questi esempi funzionano su tutte le piattaforme, ma il contesto di questo articolo è Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 60036d26-4797-4872-9a9e-3552841c61be
ms.openlocfilehash: 24794910e53f730b16975714c514eb717b41ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350959"
---
# <a name="walkthrough-for-the-performance-features-of-sql-server-on-linux"></a>Procedura dettagliata per le funzionalità per le prestazioni di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Le attività seguenti illustrano alcune delle funzionalità per le prestazioni per gli utenti Linux che non hanno familiarità con SQL Server. Queste attività non sono univoche o specifiche di Linux, ma offrono un'idea delle aree da approfondire. In ogni esempio viene fornito un collegamento alla documentazione dettagliata dell'area.

> [!NOTE]
> Gli esempi seguenti usano il database di esempio AdventureWorks. Per istruzioni su come ottenere e installare questo database di esempio, vedere [Ripristinare un database di SQL Server da Windows a Linux](sql-server-linux-migrate-restore-database.md).

## <a name="create-a-columnstore-index"></a>Creare un indice columnstore
L'indice columnstore è una tecnologia per l'archiviazione e l'esecuzione di query su grandi archivi di dati in un formato a colonne, detto columnstore.  

1. Aggiungere un indice columnstore alla tabella SalesOrderDetail eseguendo i comandi Transact-SQL seguenti:

   ```sql
   CREATE NONCLUSTERED COLUMNSTORE INDEX [IX_SalesOrderDetail_ColumnStore]
      ON Sales.SalesOrderDetail
      (UnitPrice, OrderQty, ProductID)
   GO
   ```

2. Eseguire la query seguente che usa l'indice columnstore per analizzare la tabella:

   ```sql
   SELECT ProductID, SUM(UnitPrice) SumUnitPrice, AVG(UnitPrice) AvgUnitPrice,
      SUM(OrderQty) SumOrderQty, AVG(OrderQty) AvgOrderQty
   FROM Sales.SalesOrderDetail
      GROUP BY ProductID
      ORDER BY ProductID
   ```

3. Per verificare che sia stato usato l'indice columnstore, cercare object_id per l'indice columnstore e controllare che venga visualizzato nelle statistiche di utilizzo per la tabella SalesOrderDetail:

   ```sql
   SELECT * FROM sys.indexes WHERE name = 'IX_SalesOrderDetail_ColumnStore'
   GO

   SELECT * 
   FROM sys.dm_db_index_usage_stats
      WHERE database_id = DB_ID('AdventureWorks')
      AND object_id = OBJECT_ID('AdventureWorks.Sales.SalesOrderDetail');
   ```
   
## <a name="use-in-memory-oltp"></a>Usare OLTP in memoria
SQL Server fornisce funzionalità OLTP in memoria che possono migliorare notevolmente le prestazioni dei sistemi di applicazioni.  Questa sezione della guida alla valutazione illustra i passaggi per creare una tabella ottimizzata per la memoria archiviata in memoria e una stored procedure compilata in modo nativo in grado di accedere alla tabella senza che sia necessario compilarla o interpretarla.

### <a name="configure-database-for-in-memory-oltp"></a>Configurare il database per OLTP in memoria
1. Per usare OLTP in memoria, è consigliabile impostare il database su un livello di compatibilità pari almeno a 130.  Usare la query seguente per controllare il livello di compatibilità corrente di AdventureWorks:  

   ```sql
   USE AdventureWorks
   GO
   SELECT d.compatibility_level
   FROM sys.databases as d
     WHERE d.name = Db_Name();
   GO
   ```
   
   Se necessario, aggiornare il livello impostandolo su 130:

   ```sql
   ALTER DATABASE CURRENT
   SET COMPATIBILITY_LEVEL = 130;
   GO
   ```

2. Quando una transazione coinvolge sia una tabella basata su disco che una tabella ottimizzata per la memoria, è essenziale che la parte ottimizzata per la memoria sia operativa al livello di isolamento della transazione denominato SNAPSHOT.  Per applicare in modo affidabile questo livello per le tabelle ottimizzate per la memoria in una transazione tra contenitori, eseguire quanto segue:

   ```sql
   ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
   GO
   ```

3. Prima di creare una tabella ottimizzata per la memoria, è necessario creare un FILEGROUP ottimizzato per la memoria e un contenitore per i file di dati:

   ```sql
   ALTER DATABASE AdventureWorks ADD FILEGROUP AdventureWorks_mod CONTAINS memory_optimized_data
   GO  
   ALTER DATABASE AdventureWorks ADD FILE (NAME='AdventureWorks_mod', FILENAME='/var/opt/mssql/data/AdventureWorks_mod') TO FILEGROUP AdventureWorks_mod
   GO
   ```

### <a name="create-a-memory-optimized-table"></a>Creare una tabella ottimizzata per la memoria
L'archivio primario per le tabelle ottimizzate per la memoria è la memoria principale, quindi, a differenza di quanto avviene con le tabelle basate su disco, i dati non devono essere letti dal disco nei buffer di memoria.  Per creare una tabella ottimizzata per la memoria, usare la clausola MEMORY_OPTIMIZED = ON.

1. Eseguire la query seguente per creare la tabella ottimizzata per la memoria dbo.ShoppingCart.  Per impostazione predefinita, i dati verranno salvati in modo permanente su disco per motivi di durabilità. Si noti che la DURABILITÀ può anche essere impostata per salvare in modo permanente solo lo schema. 

   ```sql
   CREATE TABLE dbo.ShoppingCart ( 
   ShoppingCartId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED,
   UserId INT NOT NULL INDEX ix_UserId NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000), 
   CreatedDate DATETIME2 NOT NULL, 
   TotalPrice MONEY
   ) WITH (MEMORY_OPTIMIZED=ON) 
   GO
   ```

2. Inserire alcuni record nella tabella:

   ```sql
   INSERT dbo.ShoppingCart VALUES (8798, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (23, SYSDATETIME(), 45.4) 
   INSERT dbo.ShoppingCart VALUES (80, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (342, SYSDATETIME(), 65.4) 
   ```

### <a name="natively-compiled-stored-procedure"></a>Stored procedure compilata in modo nativo
SQL Server supporta le stored procedure compilate in modo nativo che accedono alle tabelle ottimizzate per la memoria. Le istruzioni T-SQL vengono compilate nel codice del computer e archiviate come DLL native, consentendo un accesso ai dati più veloce e un'esecuzione delle query più efficiente rispetto a T-SQL tradizionale.   Le stored procedure che sono contrassegnate con NATIVE_COMPILATION vengono compilate in modo nativo. 

1. Eseguire lo script seguente per creare una stored procedure compilata in modo nativo che inserisce un numero elevato di record nella tabella ShoppingCart:


   ```sql
   CREATE PROCEDURE dbo.usp_InsertSampleCarts @InsertCount int 
    WITH NATIVE_COMPILATION, SCHEMABINDING AS 
   BEGIN ATOMIC 
   WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

   DECLARE @i int = 0

   WHILE @i < @InsertCount 
    BEGIN 
     INSERT INTO dbo.ShoppingCart VALUES (1, SYSDATETIME() , NULL) 
     SET @i += 1 
    END
   END 
   ```
2. Inserire 1 milione di righe:

   ```sql
   EXEC usp_InsertSampleCarts 1000000 
   ```

3. Verificare che le righe siano state inserite:

   ```sql
   SELECT COUNT(*) FROM dbo.ShoppingCart 
   ```

### <a name="learn-more-about-in-memory-oltp"></a>Altre informazioni su OLTP in memoria
Per altre informazioni su OLTP in memoria, vedere gli argomenti seguenti:

- [Avvio rapido 1: Tecnologie OLTP in memoria per migliorare le prestazioni di Transact-SQL](../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)
- [Migrazione a OLTP in memoria](../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)
- [Tabella temporanea più rapida e variabile di tabella tramite l'ottimizzazione per la memoria](../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)
- [Monitorare e risolvere i problemi relativi all'utilizzo della memoria](../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)
- [OLTP in memoria (ottimizzazione in memoria)](../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)

## <a name="use-query-store"></a>Usare Query Store
Query Store raccoglie informazioni dettagliate sulle prestazioni relative a query, piani di esecuzione e statistiche di runtime.

Query Store non è attivo per impostazione predefinita e può essere abilitato con ALTER DATABASE:

   ```sql
   ALTER DATABASE AdventureWorks SET QUERY_STORE = ON;
   ```

Eseguire la query seguente per restituire le informazioni sulle query e sui piani inclusi nell'archivio query: 

   ```sql
   SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
   FROM sys.query_store_plan AS Pl
      JOIN sys.query_store_query AS Qry
         ON Pl.query_id = Qry.query_id
      JOIN sys.query_store_query_text AS Txt
         ON Qry.query_text_id = Txt.query_text_id ;
   ```

## <a name="query-dynamic-management-views"></a>Eseguire query sulle nuove viste a gestione dinamica
Le viste a gestione dinamica restituiscono informazioni sullo stato del server che possono essere usate per monitorare l'integrità di un'istanza del server, diagnosticare i problemi e ottimizzare le prestazioni.

Per eseguire una query sulla vista a gestione dinamica dm_os_wait stats:

   ```sql
   SELECT wait_type, wait_time_ms
   FROM sys.dm_os_wait_stats;
   ```