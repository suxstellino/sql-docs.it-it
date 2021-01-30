---
description: sys.pdw_nodes_pdw_physical_databases (Transact-SQL)
title: sys.pdw_nodes_pdw_physical_databases (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 70e0939d-4d97-4ae0-ba16-934e0a80e718
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 9eebddd7409cdd3429883aaed6de3fb0fb688618
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212080"
---
# <a name="syspdw_nodes_pdw_physical_databases-transact-sql"></a>sys.pdw_nodes_pdw_physical_databases (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contiene una riga per ogni database fisico in un nodo di calcolo. Aggregare le informazioni fisiche del database per ottenere informazioni dettagliate sui database. Per combinare le informazioni, unire le `sys.pdw_nodes_pdw_physical_databases` alle `sys.pdw_database_mappings` `sys.databases` tabelle e.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID oggetto per il database. Si noti che questo valore non corrisponde a un database_id nella vista [&#41;Transact-SQL &#40;sys. databases ](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .|  
|physical_name|**sysname**|Nome fisico del database nei nodi della shell/calcolo. Questo valore è uguale a un valore nella colonna physical_name della vista [sys.pdw_database_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-database-mappings-transact-sql.md) .|  
|pdw_node_id|**int**|ID numerico univoco associato al nodo.|  
  
## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-returning"></a>R. Restituzione  
 La query seguente restituisce il nome e l'ID di ogni database nel database master e il nome del database corrispondente in ogni nodo di calcolo.  
  
```  
SELECT D.database_id AS DBID_in_master, D.name AS UserDatabaseName,   
PD.pdw_node_id AS NodeID, DM.physical_name AS PhysDBName   
FROM sys.databases AS D  
JOIN sys.pdw_database_mappings AS DM  
    ON D.database_id = DM.database_id  
JOIN sys.pdw_nodes_pdw_physical_databases AS PD  
    ON DM.physical_name = PD.physical_name  
ORDER BY D.database_id, PD.pdw_node_ID;  
```  
  
### <a name="b-using-syspdw_nodes_pdw_physical_databases-to-gather-detailed-object-information"></a>B. Utilizzo di sys.pdw_nodes_pdw_physical_databases per raccogliere informazioni dettagliate sugli oggetti  
 Nella query seguente vengono illustrate le informazioni sugli indici e sono incluse informazioni utili sul database a cui appartengono gli oggetti del database.  
  
```  
SELECT D.name AS UserDatabaseName, D.database_id AS DBIDinMaster,  
DM.physical_name AS PhysDBName, PD.pdw_node_id AS NodeID,   
IU.object_id, IU.index_id, IU.user_seeks, IU.user_scans, IU.user_lookups, IU.user_updates  
FROM sys.databases AS D  
JOIN sys.pdw_database_mappings AS DM  
    ON D.database_id = DM.database_id  
JOIN sys.pdw_nodes_pdw_physical_databases AS PD  
    ON DM.physical_name = PD.physical_name  
JOIN sys.dm_pdw_nodes_db_index_usage_stats AS IU  
    ON PD.database_id = IU.database_id  
ORDER BY D.database_id, IU.object_id, IU.index_id, PD.pdw_node_ID;  
```  
  
### <a name="c-using-syspdw_nodes_pdw_physical_databases-to-determine-the-encryption-state"></a>C. Utilizzo di sys.pdw_nodes_pdw_physical_databases per determinare lo stato di crittografia  
 La query seguente fornisce lo stato di crittografia del database AdventureWorksPDW2012.  
  
```  
WITH dek_encryption_state AS   
(  
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id, encryption_state  
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek  
        INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map  
            ON dek.database_id = node_db_map.database_id AND dek.pdw_node_id = node_db_map.pdw_node_id  
        LEFT JOIN sys.pdw_database_mappings AS db_map  
            ON node_db_map .physical_name = db_map.physical_name  
        INNER JOIN sys.dm_pdw_nodes AS nodes  
            ON nodes.pdw_node_id = dek.pdw_node_id  
    WHERE dek.encryptor_thumbprint <> 0x  
)  
SELECT TOP 1 encryption_state  
       FROM  dek_encryption_state  
       WHERE dek_encryption_state.database_id = DB_ID('AdventureWorksPDW2012 ')  
       ORDER BY (CASE encryption_state WHEN 3 THEN -1 ELSE encryption_state END) DESC;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.pdw_database_mappings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-database-mappings-transact-sql.md)  
  
  

