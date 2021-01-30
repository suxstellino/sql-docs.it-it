---
description: sys.pdw_nodes_column_store_row_groups (Transact-SQL)
title: sys.pdw_nodes_column_store_row_groups (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 08/05/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 17a4c925-d4b5-46ee-9cd6-044f714e6f0e
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: d4b9ea267b0dbf23ff304dc7632a2fdc742d37dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186577"
---
# <a name="syspdw_nodes_column_store_row_groups-transact-sql"></a>sys.pdw_nodes_column_store_row_groups (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Fornisce informazioni sugli indici columnstore cluster in base a ogni segmento, in modo da consentire all'amministratore di prendere decisioni di gestione del sistema in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . **sys.pdw_nodes_column_store_row_groups** include una colonna per il numero totale di righe archiviate fisicamente (incluse quelle contrassegnate come eliminate) e una colonna per il numero di righe contrassegnate come eliminate. Utilizzare **sys.pdw_nodes_column_store_row_groups** per determinare quali gruppi di righe hanno una percentuale elevata di righe eliminate e devono essere ricompilati.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID della tabella sottostante. Si tratta della tabella fisica nel nodo di calcolo, non della object_id per la tabella logica sul nodo di controllo. Ad esempio, object_id non corrisponde al object_id in sys. Tables.<br /><br /> Per eseguire il join con sys. Tables, utilizzare sys.pdw_index_mappings.|  
|**index_id**|**int**|ID dell'indice columnstore cluster nella tabella *object_id* .|  
|**partition_number**|**int**|ID della partizione della tabella che include *row_group_id* di gruppi di righe. È possibile utilizzare *partition_number* per aggiungere questa DMV a sys. partitions.|  
|**row_group_id**|**int**|ID di questo gruppo di righe. Univoco all'interno della partizione.|  
|**dellta_store_hobt_id**|**bigint**|Hobt_id per i gruppi di righe delta o NULL se il tipo del gruppo di righe non è delta. Un gruppo di righe delta è un gruppo di righe di lettura/scrittura che accetta nuovi record. Lo stato di un gruppo di righe Delta è **aperto** . Un gruppo di righe delta presenta ancora il formato rowstore e non è stato compresso nel formato columnstore.|  
|**state**|**tinyint**|Numero ID associato a state_description.<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED|  
|**state_desccription**|**nvarchar(60)**|Descrizione dello stato persistente del gruppo di righe:<br /><br /> APRIRE: un gruppo di righe di lettura/scrittura che accetta nuovi record. Un gruppo di righe aperto presenta ancora il formato rowstore e non è stato compresso nel formato columnstore.<br /><br /> CLOSED: gruppo di righe compilato ma non ancora compresso dal processo del motore di Tuple.<br /><br /> COMPRESSO: gruppo di righe che è stato riempito e compresso.|  
|**total_rows**|**bigint**|Righe totali archiviate fisicamente nel gruppo di righe. È possibile che alcune siano state eliminate, ma risultano comunque archiviate. Il numero massimo di righe in un gruppo di righe è 1.048.576 (esadecimale FFFFF).|  
|**deleted_rows**|**bigint**|Numero di righe archiviate fisicamente nel gruppo di righe contrassegnate per l'eliminazione.<br /><br /> Sempre 0 per i gruppi di righe DELTA.|  
|**size_in_bytes**|**int**|Dimensioni combinate, in byte, di tutte le pagine di questo gruppo di righe. Questa dimensione non include le dimensioni necessarie per archiviare i metadati o i dizionari condivisi.|  
|**pdw_node_id**|**int**|ID univoco di un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|  
|**distribution_id**|**int**|ID univoco della distribuzione.|
  
## <a name="remarks"></a>Commenti  
 Restituisce una riga per ogni gruppo di righe columnstore per ogni tabella che dispone di un indice columnstore cluster o non cluster.  
  
 Utilizzare **sys.pdw_nodes_column_store_row_groups** per determinare il numero di righe incluse nel gruppo di righe e le dimensioni del gruppo di righe.  
  
 Quando il numero delle righe eliminate in un gruppo di righe diventa una percentuale elevata delle righe totali, la tabella diventa meno efficiente. Ricompilare l'indice columnstore per ridurre le dimensioni della tabella, riducendo le operazioni di I/O del disco necessarie per leggere la tabella. Per ricompilare l'indice columnstore, utilizzare l'opzione **Rebuild** dell'istruzione **alter index** .  
  
 Il columnstore aggiornabile inserisce prima i nuovi dati in un rowgroup **aperto** , in formato rowstore, ed è anche definito tabella Delta.  Quando un rowgroup aperto è pieno, il relativo stato diventa **chiuso**. Un rowgroup chiuso viene compresso nel formato columnstore dal motore di tuple e lo stato passa a **compresso**.  Il processo tuple-mover è un processo in background che si riattiva periodicamente per verificare se esistano gruppi di righe chiusi pronti per essere compressi in un gruppo di righe columnstore.  Il processo tuple-mover dealloca i gruppi di righe in cui sono state eliminate tutte le righe. RowGroups deallocati sono contrassegnati come **RItirati**. Per eseguire immediatamente il motore di tuple, utilizzare l'opzione **REorganize** dell'istruzione **alter index** .  
  
 Quando un gruppo di righe columnstore risulta riempito, viene compresso e non accetta più nuove righe. Quando vengono eliminate righe da un gruppo compresso, vengono contrassegnate come eliminate, ma risultano ancora presenti. Gli aggiornamenti a un gruppo compresso vengono implementati come un'eliminazione dal gruppo compresso e come un inserimento in un gruppo aperto.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **VIEW SERVER STATE**.  
  
## <a name="examples-sssdw-and-sspdw"></a>Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente viene unito il **sys.pdw_nodes_column_store_row_groups** tabella ad altre tabelle di sistema per restituire informazioni su tabelle specifiche. La colonna `PercentFull` calcolata è una stima dell'efficienza del gruppo di righe. Per trovare informazioni su una singola tabella, rimuovere i trattini dei commenti davanti alla clausola WHERE e specificare un nome di tabella.  
  
```sql
SELECT IndexMap.object_id,   
  object_name(IndexMap.object_id) AS LogicalTableName,   
  i.name AS LogicalIndexName, IndexMap.index_id, NI.type_desc,   
  IndexMap.physical_name AS PhyIndexNameFromIMap,   
  CSRowGroups.*,  
  100*(ISNULL(deleted_rows,0))/total_rows AS PercentDeletedRows   
FROM sys.tables AS t  
JOIN sys.indexes AS i  
    ON t.object_id = i.object_id  
JOIN sys.pdw_index_mappings AS IndexMap  
    ON i.object_id = IndexMap.object_id  
    AND i.index_id = IndexMap.index_id  
JOIN sys.pdw_nodes_indexes AS NI  
    ON IndexMap.physical_name = NI.name  
    AND IndexMap.index_id = NI.index_id  
JOIN sys.pdw_nodes_column_store_row_groups AS CSRowGroups  
    ON CSRowGroups.object_id = NI.object_id   
    AND CSRowGroups.pdw_node_id = NI.pdw_node_id  
    AND CSRowGroups.distribution_id = NI.distribution_id
    AND CSRowGroups.index_id = NI.index_id      
WHERE total_rows > 0
--WHERE t.name = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, IndexMap.physical_name, pdw_node_id;  
```  

Nell' [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] esempio seguente vengono conteggiate le righe per partizione per gli archivi di colonne del cluster, nonché il numero di righe in gruppi di righe aperti, chiusi o compressi:  

```sql
SELECT
    s.name AS [Schema Name]
    ,t.name AS [Table Name]
    ,rg.partition_number AS [Partition Number]
    ,SUM(rg.total_rows) AS [Total Rows]
    ,SUM(CASE WHEN rg.State = 1 THEN rg.Total_rows Else 0 END) AS [Rows in OPEN Row Groups]
    ,SUM(CASE WHEN rg.State = 2 THEN rg.Total_Rows ELSE 0 END) AS [Rows in Closed Row Groups]
    ,SUM(CASE WHEN rg.State = 3 THEN rg.Total_Rows ELSE 0 END) AS [Rows in COMPRESSED Row Groups]
FROM sys.pdw_nodes_column_store_row_groups rg
  JOIN sys.pdw_nodes_tables pt
    ON rg.object_id = pt.object_id
    AND rg.pdw_node_id = pt.pdw_node_id
    AND pt.distribution_id = rg.distribution_id
  JOIN sys.pdw_table_mappings tm
    ON pt.name = tm.physical_name
  INNER JOIN sys.tables t
    ON tm.object_id = t.object_id
  INNER JOIN sys.schemas s
    ON t.schema_id = s.schema_id
GROUP BY s.name, t.name, rg.partition_number
ORDER BY 1, 2
```
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [CREARE un indice COLUMNStore &#40;&#41;Transact-SQL ](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_dictionaries &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)  
  
  
