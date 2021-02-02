---
description: Indici columnstore - Data warehouse
title: Indici columnstore - Data warehouse | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 21fd153b-116d-47fc-a926-f1528299a391
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cf8e82f8ebaa0a8b5b30cc4ffd460f7db7d4d121
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234865"
---
# <a name="columnstore-indexes---data-warehouse"></a>Indici columnstore - Data warehouse
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gli indici columnstore, in combinazione con il partizionamento, sono essenziali per la creazione di un data warehouse [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="whats-new"></a>Novità  
 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] offre prestazioni migliori degli indici columnstore grazie all'introduzione delle funzionalità seguenti:  
  
-   Supporto in AlwaysOn dell'esecuzione di query su un indice columnstore in una replica secondaria leggibile.  
-   Supporto in MARS (Multiple Active Result Sets) degli indici columnstore.  
-   La nuova vista a gestione dinamica [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md) offre informazioni sulla risoluzione dei problemi di prestazioni a livello di gruppo di righe.  
-   Possibilità di eseguire query a thread singolo in modalità batch su indici columnstore. In precedenza era possibile eseguire in modalità batch solo query a thread multipli.  
-   L'operatore `SORT` viene eseguito in modalità batch.  
-   È possibile eseguire più operazioni `DISTINCT` in modalità batch.  
-   Window Aggregates viene ora eseguito in modalità batch per il livello di compatibilità database 130 e i livelli superiori.  
-   Distribuzione dell'aggregazione per un'elaborazione efficiente delle aggregazioni. Supportata in tutti i livelli di compatibilità del database.  
-   Distribuzione dei predicati stringa per un'elaborazione efficiente dei predicati stringa. Supportata in tutti i livelli di compatibilità del database.  
-   Isolamento dello snapshot per il livello di compatibilità del database 130 e i livelli superiori.  
  
## <a name="improve-performance-by-combining-nonclustered-and-columnstore-indexes"></a>Migliorare le prestazioni con una combinazione di indici non cluster e columnstore  
 A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile definire indici non cluster in un indice columnstore cluster.   
  
### <a name="example-improve-efficiency-of-table-seeks-with-a-nonclustered-index"></a>Esempio: migliorare l'efficienza del posizionamento all'interno delle tabelle con un indice non cluster  
 Per migliorare l'efficienza del posizionamento all'interno delle tabelle in un data warehouse, è possibile creare un indice non cluster progettato per l'esecuzione di query le cui prestazioni vengono ottimizzate dal posizionamento all'interno delle tabelle. Le query che cercano valori corrispondenti o restituiscono un intervallo di valori ridotto garantiscono prestazioni migliori con un indice albero B che con un indice columnstore. Tali query non richiedono l'analisi completa della tabella eseguita dall'indice columnstore e restituiscono il risultato corretto più velocemente eseguendo una ricerca binaria tramite un indice albero B.  
  
```sql  
--BASIC EXAMPLE: Create a nonclustered index on a columnstore table.  
  
--Create the table  
CREATE TABLE t_account (  
    AccountKey int NOT NULL,  
    AccountDescription nvarchar (50),  
    AccountType nvarchar(50),  
    UnitSold int  
);  
GO  
  
--Store the table as a columnstore.  
CREATE CLUSTERED COLUMNSTORE INDEX taccount_cci ON t_account;  
GO  
  
--Add a nonclustered index.  
CREATE UNIQUE INDEX taccount_nc1 ON t_account (AccountKey);  
```  
  
### <a name="example-use-a-nonclustered-index-to-enforce-a-primary-key-constraint-on-a-columnstore-table"></a>Esempio: usare un indice non cluster per imporre un vincolo di chiave primaria su un indice columnstore.  
 Da progettazione, una tabella columnstore non consente vincoli di chiave primaria. È ora possibile imporre un vincolo di chiave primaria su una tabella columnstore tramite un indice non cluster. Una chiave primaria è equivalente a un vincolo UNIQUE su una colonna non NULL e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa un vincolo UNIQUE come indice non cluster. Combinando questi fatti, l'esempio seguente definisce un vincolo UNIQUE sulla colonna non NULL accountkey. Il risultato è un indice non cluster che impone un vincolo di chiave primaria come vincolo UNIQUE su una colonna non NULL.  
  
 La tabella viene quindi convertita in un indice columnstore cluster. Durante la conversione l'indice non cluster diventa permanente. Il risultato è un indice columnstore cluster con un indice non cluster che impone un vincolo di chiave primaria. Poiché qualsiasi aggiornamento o inserimento nella tabella columnstore influirà anche sull'indice non cluster, tutte le operazioni che violano il vincolo UNIQUE e la caratteristica non NULL causeranno l'esito negativo dell'intera operazione.  
  
 Il risultato è un indice columnstore con un indice non cluster che impone un vincolo di chiave primaria su entrambi gli indici.  
  
```sql
--EXAMPLE: Enforce a primary key constraint on a columnstore table.   
  
--Create a rowstore table with a unique constraint.  
--The unique constraint is implemented as a nonclustered index.  
CREATE TABLE t_account (  
    AccountKey int NOT NULL,  
    AccountDescription nvarchar (50),  
    AccountType nvarchar(50),  
    UnitSold int,  
  
    CONSTRAINT uniq_account UNIQUE (AccountKey)  
);  
  
--Store the table as a columnstore.   
--The unique constraint is preserved as a nonclustered index on the columnstore table.  
CREATE CLUSTERED COLUMNSTORE INDEX t_account_cci ON t_account  
  
--By using the previous two steps, every row in the table meets the UNIQUE constraint  
--on a non-NULL column.  
--This has the same end-result as having a primary key constraint  
--All updates and inserts must meet the unique constraint on the nonclustered index or they will fail.  
  
--If desired, add a foreign key constraint on AccountKey.  
  
ALTER TABLE [dbo].[t_account]  
WITH CHECK ADD FOREIGN KEY([AccountKey]) REFERENCES my_dimension(Accountkey); 
```  
  
### <a name="improve-performance-by-enabling-row-level-and-row-group-level-locking"></a>Migliorare le prestazioni abilitando il blocco a livello di riga e a livello di gruppo di righe  
 Per integrare l'indice non cluster in un indice columnstore, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] offre la funzionalità di blocco granulare per le operazioni di selezione, aggiornamento ed eliminazione. È possibile eseguire query con blocco a livello di riga per operazioni di index seek su un indice non cluster e con blocco a livello di gruppo di righe per operazioni di analisi completa di tabelle sull'indice columnstore. Usare questa funzionalità per ottenere una concorrenza più elevata in lettura e scrittura con l'uso appropriato del blocco a livello di riga e a livello di gruppo di righe.  
  
```sql  
--Granular locking example  
--Store table t_account as a columnstore table.  
CREATE CLUSTERED COLUMNSTORE INDEX taccount_cci ON t_account  
GO  
  
--Add a nonclustered index for use with this example  
CREATE UNIQUE INDEX taccount_nc1 ON t_account (AccountKey);  
GO  
  
--Look at locking with access through the nonclustered index  
SET TRANSACTION ISOLATION LEVEL repeatable read;  
GO  
  
BEGIN TRAN  
    -- The query plan chooses a seek operation on the nonclustered index  
    -- and takes the row lock  
    SELECT * FROM t_account WHERE AccountKey = 100;  
END TRAN  
```  
  
### <a name="snapshot-isolation-and-read-committed-snapshot-isolations"></a>Isolamento dello snapshot e isolamento dello snapshot Read Committed  
 Usare l'isolamento dello snapshot per garantire la coerenza transazionale e l'isolamento dello snapshot Read Committed per garantire la coerenza a livello di istruzione per le query su indici columnstore. Questo consente di eseguire le query senza bloccare le scritture di dati. Questo comportamento, oltre a non causare blocchi, riduce anche in modo significativo la probabilità di deadlock per le transazioni complesse. Per altre informazioni, vedere [Isolamento dello Snapshot in SQL Server](https://msdn.microsoft.com/library/tcbchxcb\(v=vs.110\).aspx) in MSDN.  
  
## <a name="see-also"></a>Vedere anche  
 [Indici columnstore - Linee guida per la progettazione](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Indici columnstore - Linee guida per il caricamento di dati](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Prestazioni delle query per gli indici columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)    
 [Architettura degli indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index) 
  
