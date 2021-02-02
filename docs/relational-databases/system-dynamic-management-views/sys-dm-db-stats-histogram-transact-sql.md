---
description: sys.dm_db_stats_histogram (Transact-SQL)
title: sys.dm_db_stats_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_stats_histogram
- sys.dm_db_stats_histogram_TSQL
- dm_db_stats_histogram
- dm_db_stats_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_stats_histogram dynamic management function
ms.assetid: 1897fd4a-8d51-461e-8ef2-c60be9e563f2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5225498eda8ccba4b32b8889062282a6c7d9f3cd
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236068"
---
# <a name="sysdm_db_stats_histogram-transact-sql"></a>sys.dm_db_stats_histogram (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Restituisce l'istogramma delle statistiche per l'oggetto di database specificato (tabella o vista indicizzata) nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database corrente. Simile a `DBCC SHOW_STATISTICS WITH HISTOGRAM`.

> [!NOTE] 
> Questa DMF è disponibile a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU2

## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_db_stats_histogram (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Argomenti  
 *object_id*  
 ID dell'oggetto nel database corrente per il quale sono richieste le proprietà di una delle relative statistiche. *object_id* è di **tipo int**.  
  
 *stats_id*  
 ID delle statistiche per l'oggetto *object_id* specificato. L'ID delle statistiche può essere ottenuto dalla DMV [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* è di tipo **int**.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id |**int**|ID dell'oggetto (tabella o vista indicizzata) per cui restituire le proprietà dell'oggetto statistiche.|  
|stats_id |**int**|ID dell'oggetto statistiche. Univoco all'interno della tabella o della vista indicizzata. Per altre informazioni, vedere [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|  
|step_number |**int** |Numero di passaggi nell'istogramma. |
|range_high_key |**sql_variant** |Valore di colonna pari al limite superiore per un intervallo dell'istogramma. Il valore di colonna viene denominato anche valore chiave.|
|range_rows |**real** |Numero stimato di righe il cui valore di colonna è compreso in un intervallo dell'istogramma, escluso il limite superiore. |
|equal_rows |**real** |Numero stimato di righe il cui valore di colonna è uguale al limite superiore dell'intervallo dell'istogramma. |
|distinct_range_rows |**bigint** |Numero stimato di righe con un valore distinct di colonna compreso in un intervallo dell'istogramma, escluso il limite superiore. |
|average_range_rows |**real** |Numero medio di righe con valori di colonna duplicati in un intervallo dell'istogramma, escluso il limite superiore ( `RANGE_ROWS / DISTINCT_RANGE_ROWS` per `DISTINCT_RANGE_ROWS > 0` ). |
  
 ## <a name="remarks"></a>Commenti  
 
 Il ResultSet per `sys.dm_db_stats_histogram` restituisce informazioni simili a `DBCC SHOW_STATISTICS WITH HISTOGRAM` e include anche `object_id` , `stats_id` e `step_number` .

 Poiché la colonna `range_high_key` è un tipo di dati sql_variant, potrebbe essere necessario utilizzare `CAST` o `CONVERT` se un predicato esegue il confronto con una costante non stringa.

### <a name="histogram"></a>Istogramma
  
 Un istogramma misura la frequenza di occorrenza per ogni valore distinct in un set di dati. Query Optimizer calcola un istogramma nei valori di colonna nella prima colonna chiave dell'oggetto statistiche, selezionando i valori di colonna tramite il campionamento statistico delle righe o un'analisi completa di tutte le righe della tabella o della vista. Se l'istogramma viene creato da un set campionato di righe, i totali archiviati per numero di righe e numero di valori distinct sono stime e non è necessario che siano numeri interi.  
  
 Per creare l'istogramma, Query Optimizer ordina i valori di colonna, calcola il numero di valori che corrispondono a ogni valore distinct di colonna, quindi aggrega i valori di colonna in un massimo di 200 intervalli contigui dell'istogramma. Ogni intervallo comprende un insieme di valori di colonna seguiti da un valore di colonna pari al limite superiore. Nell'insieme sono inclusi tutti i possibili valori di colonna compresi tra i valori limite, esclusi questi ultimi. Il minore tra i valori di colonna ordinati costituisce il limite superiore per il primo intervallo dell'istogramma.  
  
 Nel diagramma seguente viene illustrato un istogramma con sei intervalli. L'area a sinistra del primo valore limite superiore è il primo intervallo.  
  
 ![Istogramma](../../relational-databases/system-dynamic-management-views/media/histogram-2.svg "Istogramma")  
  
 Per ogni intervallo dell'istogramma:  
  
-   La riga in grassetto rappresenta il valore limite superiore (*range_high_key*) e il relativo numero di occorrenze (*equal_rows*).  
  
-   L'area a tinta unita a sinistra di *range_high_key* rappresenta l'intervallo dei valori di colonna e il numero medio di occorrenze di ogni valore (*average_range_rows*) di colonna. Il valore *average_range_rows* per il primo passaggio dell'istogramma è sempre 0.  
  
-   Le linee punteggiate rappresentano i valori campionati usati per stimare il numero complessivo dei valori distinti nell'intervallo (*distinct_range_rows*) e il numero complessivo dei valori nell'intervallo (*range_rows*). Query Optimizer usa *range_rows* e *distinct_range_rows* per calcolare *average_range_rows* e non archivia i valori campionati.  
  
 Query Optimizer definisce gli intervalli dell'istogramma in base al relativo significato statistico e utilizza un algoritmo per il calcolo della differenza massima per ridurre al minimo il numero di intervalli nell'istogramma, aumentando contemporaneamente la differenza tra i valori limite. Il numero massimo di intervalli è 200. Il numero di intervalli dell'istogramma può essere minore del numero di valori distinct, anche per le colonne con un numero di punti limite inferiore a 200. A una colonna con 100 valori distinct, ad esempio, può essere associato un istogramma con un numero di punti limite inferiore a 100.  
  
## <a name="permissions"></a>Autorizzazioni  

L'utente deve avere autorizzazioni di selezione per le colonne delle statistiche o essere proprietario della tabella o membro del ruolo predefinito del server `sysadmin`, del ruolo predefinito del database `db_owner` o del ruolo predefinito del database `db_ddladmin`.

## <a name="examples"></a>Esempi  

### <a name="a-simple-example"></a>R. Esempio semplice    
Nell'esempio seguente viene creata e popolata una tabella semplice. Quindi crea le statistiche sulla `Country_Name` colonna.

```sql
CREATE TABLE Country
(Country_ID int IDENTITY PRIMARY KEY,
Country_Name varchar(120) NOT NULL);
INSERT Country (Country_Name) VALUES ('Canada'), ('Denmark'), ('Iceland'), ('Peru');

CREATE STATISTICS Country_Stats  
    ON Country (Country_Name) ;  
```   
La chiave primaria occupa il `stat_id` numero 1, quindi chiamare `sys.dm_db_stats_histogram` per il `stat_id` numero 2, per restituire l'istogramma delle statistiche per la `Country` tabella.    
```sql     
SELECT * FROM sys.dm_db_stats_histogram(OBJECT_ID('Country'), 2);
```

### <a name="b-useful-query"></a>B. Query utile:   
```sql  
SELECT hist.step_number, hist.range_high_key, hist.range_rows, 
    hist.equal_rows, hist.distinct_range_rows, hist.average_range_rows
FROM sys.stats AS s
CROSS APPLY sys.dm_db_stats_histogram(s.[object_id], s.stats_id) AS hist
WHERE s.[name] = N'<statistic_name>';
```

### <a name="c-useful-query"></a>C. Query utile:
Nell'esempio seguente viene selezionata la tabella `Country` con un predicato sulla colonna `Country_Name` .

```sql  
SELECT * FROM Country 
WHERE Country_Name = 'Canada';
```

Nell'esempio seguente vengono esaminate le statistiche create in precedenza sulla tabella `Country` e la colonna `Country_Name` per il passaggio dell'istogramma corrispondente al predicato nella query precedente.

```sql  
SELECT ss.name, ss.stats_id, shr.steps, shr.rows, shr.rows_sampled, 
    shr.modification_counter, shr.last_updated, sh.range_rows, sh.equal_rows
FROM sys.stats ss
INNER JOIN sys.stats_columns sc 
    ON ss.stats_id = sc.stats_id AND ss.object_id = sc.object_id
INNER JOIN sys.all_columns ac 
    ON ac.column_id = sc.column_id AND ac.object_id = sc.object_id
CROSS APPLY sys.dm_db_stats_properties(ss.object_id, ss.stats_id) shr
CROSS APPLY sys.dm_db_stats_histogram(ss.object_id, ss.stats_id) sh
WHERE ss.[object_id] = OBJECT_ID('Country') 
    AND ac.name = 'Country_Name'
    AND sh.range_high_key = CAST('Canada' AS CHAR(8));
```
  
## <a name="see-also"></a>Vedere anche  
[DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
[Funzioni e viste a gestione dinamica relative agli oggetti (Transact-SQL)](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)  
[sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)  
