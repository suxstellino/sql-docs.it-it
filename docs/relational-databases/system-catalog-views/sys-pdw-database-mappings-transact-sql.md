---
description: sys.pdw_database_mappings (Transact-SQL)
title: sys.pdw_database_mappings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 4ae2c71e-dd56-41ea-a16b-64936175b459
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 52881d6590abd115565e2d35bc109a99460eb72d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180800"
---
# <a name="syspdw_database_mappings-transact-sql"></a>sys.pdw_database_mappings (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Esegue il mapping del **database_id** di database al nome fisico usato nei nodi di calcolo e fornisce l' **ID entità** del proprietario del database nel sistema. Aggiungere **sys.pdw_database_mappings** a **sys. databases** e **sys.pdw_nodes_pdw_physical_databases**.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|physical_name|**nvarchar (36)**|Nome fisico del database nei nodi di calcolo.<br /><br /> **physical_name** e **database_id** formano la chiave per questa visualizzazione.||  
|database_id|**int**|ID oggetto per il database. Vedere [sys. databases &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).<br /><br /> **physical_name** e **database_id** formano la chiave per questa visualizzazione.||  
  
## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente viene eseguito il join sys.pdw_database_mappings ad altre tabelle di sistema per mostrare la modalità di mapping dei database.  
  
```  
SELECT DB.database_id, DB.name, Map.*, Phys.*   
FROM sys.databases AS DB  
JOIN sys.pdw_database_mappings AS Map  
    ON DB.database_id = Map.database_id  
JOIN sys.pdw_nodes_pdw_physical_databases AS Phys  
    ON Map.physical_name = Phys.physical_name  
ORDER BY DB.database_id, Phys.pdw_node_id;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.pdw_index_mappings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-index-mappings-transact-sql.md)   
 [sys.pdw_table_mappings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-table-mappings-transact-sql.md)   
 [sys.pdw_nodes_pdw_physical_databases &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-pdw-physical-databases-transact-sql.md)  
  
  

