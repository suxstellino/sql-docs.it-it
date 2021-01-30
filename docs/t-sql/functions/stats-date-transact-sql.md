---
description: STATS_DATE (Transact-SQL)
title: STATS_DATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATS_DATE_TSQL
- STATS_DATE
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], last time updated
- STATS_DATE function
- query optimization statistics [SQL Server], last time updated
- last time statistics updated
- stats update date
ms.assetid: f9ec3101-1e41-489d-b519-496a0d6089fb
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ca416d3de8d295cd9f7f535ab2d3b8b65df150f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181253"
---
# <a name="stats_date-transact-sql"></a>STATS_DATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce la data dell'aggiornamento più recente delle statistiche per una tabella o vista indicizzata.  
  
 Per altre informazioni sull'aggiornamento delle statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
STATS_DATE ( object_id , stats_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *object_id*  
 ID della tabella o della vista indicizzata contenente le statistiche.  
  
 *stats_id*  
 ID dell'oggetto statistiche.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce **datetime** in caso di esito positivo. Restituisce **NULL** se un BLOB di statistiche non è stato creato.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile utilizzare funzioni di sistema nell'elenco di selezione, nella clausola WHERE e in tutti i casi in cui è consentita un'espressione.  
 
 La data di aggiornamento delle statistiche viene archiviata nell'[oggetto BLOB di statistiche](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) insieme all'[istogramma](../../relational-databases/statistics/statistics.md#histogram) e al [vettore di densità](../../relational-databases/statistics/statistics.md#density), non nei metadati. Quando non viene letto alcun dato per generare i dati delle statistiche, il BLOB di statistiche non viene creato e la data non è disponibile. È il caso delle statistiche filtrate per le quali il predicato non restituisce alcuna riga o delle nuove tabelle vuote.
 
 Se le statistiche corrispondono a un indice, il valore *stats_id* nella vista del catalogo [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) corrisponde al valore *index_id* nella vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database db_owner o l'autorizzazione per la visualizzazione dei metadati per la tabella o la vista indicizzata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-return-the-dates-of-the-most-recent-statistics-for-a-table"></a>R. Recupero delle date delle statistiche più recenti per una tabella  
 L'esempio seguente restituisce la data dell'aggiornamento più recente di ogni oggetto statistiche nella tabella `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_update_date  
FROM sys.stats   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
 Se le statistiche corrispondono a un indice, il valore *stats_id* nella vista del catalogo [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) corrisponde al valore *index_id* nella vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) e la query seguente restituisce gli stessi risultati della query precedente. Se le statistiche non corrispondono a un indice, vengono restituite nei risultati di sys.stats ma non in quelli di sys.indexes.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-learn-when-a-named-statistics-was-last-updated"></a>B. Informazioni sull'ultimo aggiornamento di determinate statistiche  
 L'esempio seguente crea statistiche sulla colonna LastName della tabella DimCustomer. Viene quindi eseguita una query per visualizzare la data delle statistiche. Le statistiche vengono quindi aggiornate e viene di nuovo eseguita la query per visualizzare la data di aggiornamento.  
  
```sql
--First, create a statistics object  
USE AdventureWorksPDW2012;  
GO  
CREATE STATISTICS Customer_LastName_Stats  
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName)  
WITH SAMPLE 50 PERCENT;  
GO  
  
--Return the date when Customer_LastName_Stats was last updated  
USE AdventureWorksPDW2012;  
GO  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO  
  
--Update Customer_LastName_Stats so it will have a different timestamp in the next query  
GO  
UPDATE STATISTICS dbo.dimCustomer (Customer_LastName_Stats);  
  
--Return the date when Customer_LastName_Stats was last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO    
```  
  
### <a name="c-view-the-date-of-the-last-update-for-all-statistics-on-a-table"></a>C. Visualizzare la data dell'ultimo aggiornamento di tutte le statistiche in una tabella  
 Questo esempio restituisce la data dell'ultimo aggiornamento di ogni oggetto statistica nella tabella DimCustomer.  
  
```sql  
--Return the dates all statistics on the table were last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
 Se le statistiche corrispondono a un indice, il valore *stats_id* nella vista del catalogo [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) corrisponde al valore *index_id* nella vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) e la query seguente restituisce gli stessi risultati della query precedente. Se le statistiche non corrispondono a un indice, vengono restituite nei risultati di sys.stats ma non in quelli di sys.indexes.  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [Statistiche](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
  
  

