---
description: sys.memory_optimized_tables_internal_attributes (Transact-SQL)
title: sys.memory_optimized_tables_internal_attributes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.memory_optimized_tables_internal_attributes
- sys.memory_optimized_tables_internal_attributes_TSQL
- memory_optimized_tables_internal_attributes
- memory_optimized_tables_internal_attributes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.memory_optimized_tables_internal_attributes catalog view
ms.assetid: 78ef5807-0504-4de8-9a01-ede6c03c7ff1
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29d087c2501bd4296c3c60b9cefb4ca28be93fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191328"
---
# <a name="sysmemory_optimized_tables_internal_attributes-transact-sql"></a>sys.memory_optimized_tables_internal_attributes (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Contiene una riga per ogni tabella interna ottimizzata per la memoria usata per archiviare tabelle utente ottimizzate per la memoria. Ogni tabella utente corrisponde a una o più tabelle interne. Una singola tabella viene usata per l'archivio dati principale. Tabelle interne aggiuntive vengono usate per supportare funzionalità come l'archiviazione temporale, dell'indice columnstore e all'esterno di righe (LOB) per le tabelle ottimizzate per la memoria.
 
| Nome colonna  | Tipo di dati  | Descrizione |
| :------ |:----------| :-----|
|object_id  |**int**|       ID della tabella utente. Le tabelle interne ottimizzate per la memoria presenti per supportare una tabella utente (ad esempio l'archiviazione all'esterno di righe o le righe eliminate in caso di combinazioni Hk/Columnstore) hanno lo stesso valore di object_id dell'elemento padre. |
|xtp_object_id  |**bigint**|    ID di oggetto OLTP in memoria corrispondente alla tabella interna ottimizzata per la memoria usata per supportare la tabella utente. È univoco all'interno del database e può cambiare nel corso della durata dell'oggetto. 
|tipo|  **int** |   Tipo di tabella interna.<br/><br/> 0 => DELETED_ROWS_TABLE <br/> 1 => USER_TABLE <br/> 2 => DICTIONARIES_TABLE<br/>3 => SEGMENTS_TABLE<br/>4 => ROW_GROUPS_INFO_TABLE<br/>5 => INTERNAL OFF-ROW DATA TABLE<br/>252 => INTERNAL_TEMPORAL_HISTORY_TABLE | 
|type_desc| **nvarchar(60)**|   Descrizione del tipo<br/><br/>DELETED_ROWS_TABLE -> Tabella interna che tiene traccia delle righe eliminate per un indice columnstore.<br/>USER_TABLE -> Tabella contenente i dati utente all'interno di righe.<br/>DICTIONARIES_TABLE -> Dizionari per un indice columnstore.<br/>SEGMENTS_TABLE -> Segmenti compressi per un indice columnstore.<br/>ROW_GROUPS_INFO_TABLE -> Metadati relativi ai gruppi di righe compressi di un indice columnstore.<br/>INTERNAL OFF-ROW DATA TABLE -> Tabella interna usata per l'archiviazione di una colonna all'esterno di righe. In questo caso, minor_id riflette column_id.<br/>INTERNAL_TEMPORAL_HISTORY_TABLE -> Parte finale, ad accesso frequente, della tabella di cronologia basata su disco. Le righe inserite nella cronologia vengono inserite prima in questa tabella ottimizzata per la memoria interna. Viene eseguita un'attività in background che sposta in modo asincrono le righe da questa tabella interna alla tabella di cronologia basata su disco. |
|minor_id|  **int**|    0 indica un utente o una tabella interna<br/><br/>Un valore diverso da 0 indica l'ID di una colonna archiviata all'esterno di righe. Si unisce a column_id in sys.columns.<br/><br/>Ogni colonna archiviata all'esterno di righe ha una riga corrispondente in questa vista di sistema.|

## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-all-columns-that-are-stored-off-row"></a>R. Restituzione di tutte le colonne archiviate all'esterno di righe

Lo script T-SQL seguente illustra una tabella con più colonne non LOB di grandi dimensioni e una singola colonna LOB:

```Transact-SQL
CREATE TABLE dbo.LargeTableSample
(
      Id   int IDENTITY PRIMARY KEY NONCLUSTERED,
      C1   nvarchar(4000),
      C2   nvarchar(4000),
      C3   nvarchar(4000),
      C4   nvarchar(4000),
      Misc nvarchar(max)
) WITH (MEMORY_OPTIMIZED = ON);
GO
```

La query seguente mostra tutte le colonne che vengono archiviate all'esterno di righe, con le relative dimensioni. Una dimensione di -1 indica una colonna LOB. Tutte le colonne LOB vengono archiviate all'esterno di righe.

```Transact-SQL
SELECT 
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table', 
  c.name AS 'column', 
  c.max_length
FROM sys.memory_optimized_tables_internal_attributes moa
     JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
     JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="b-returning-memory-consumption-of-all-columns-that-are-stored-off-row"></a>B. Restituzione dell'utilizzo di memoria di tutte le colonne archiviate all'esterno di righe

Per ottenere altre informazioni sull'utilizzo di memoria delle colonne all'esterno di righe, è possibile usare la query seguente, che mostra l'utilizzo di memoria di tutte le tabelle interne e dei relativi indici usati per archiviare colonne all'esterno di righe:

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  c.name AS 'column',
  c.max_length,
  mc.memory_consumer_desc,
  mc.index_id,
  mc.allocated_bytes,
  mc.used_bytes
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="c-returning-memory-consumption-of-columnstore-indexes-on-memory-optimized-tables"></a>C. Restituzione dell'utilizzo di memoria degli indici columnstore in tabelle ottimizzate per la memoria

Usare la query seguente per mostrare il consumo di memoria degli indici columnstore nelle tabelle ottimizzate per la memoria:

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  SUM(mc.allocated_bytes) / 1024 as [allocated_kb],
  SUM(mc.used_bytes) / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
GROUP BY o.schema_id, moa.object_id, i.name;
```

Usare la query seguente per suddividere il consumo di memoria tra le strutture interne usate per gli indici columnstore nelle tabelle ottimizzate per la memoria:

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  moa.type_desc AS 'internal table',
  mc.index_id AS 'index',
  mc.memory_consumer_desc,
  mc.allocated_bytes / 1024 as [allocated_kb],
  mc.used_bytes / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
```


