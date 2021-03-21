---
description: sys.indexes (Transact-SQL)
title: sys. Indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.indexes
- indexes
- sys.indexes_TSQL
- indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes catalog view
ms.assetid: 066bd9ac-6554-4297-88fe-d740de1f94a8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0279de0234345366a9fa2b71199aecae999a193
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754711"
---
# <a name="sysindexes-transact-sql"></a>sys.indexes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni indice o heap di un oggetto in formato tabella, come una tabella, una vista o una funzione con valori di tabella.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto a cui appartiene l'indice.|  
|**nome**|**sysname**|Nome dell'indice. il **nome** è univoco solo all'interno dell'oggetto.<br /><br /> NULL = Heap|  
|**index_id**|**int**|ID dell'indice. **index_id** è univoco solo all'interno dell'oggetto.<br /><br /> 0 = heap<br /><br /> 1 o 5 = indice cluster (albero B, columnstore)<br /><br /> > 1 e <> 5 = indice non cluster|  
|**type**|**tinyint**|Tipo di indice:<br /><br /> 0 = heap<br /><br /> 1 = Cluster<br /><br /> 2 = Non cluster<br /><br /> 3 = XML<br /><br /> 4 = Spaziale<br /><br /> 5 = indice columnstore cluster. **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> 6 = indice columnstore non cluster. **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> 7 = indice hash non cluster. **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.|  
|**type_desc**|**nvarchar(60)**|Descrizione del tipo di indice:<br /><br /> HEAP<br /><br /> CLUSTERED<br /><br /> NONCLUSTERED<br /><br /> XML<br /><br /> SPATIAL<br /><br /> COLUMNStore CLUSTER: **si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> COLUMNStore non CLUSTER: **si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> HASH non CLUSTER: gli indici HASH non CLUSTER sono supportati solo nelle tabelle ottimizzate per la memoria. Nella vista sys.hash_indexes vengono mostrati gli indici hash correnti e le proprietà hash. Per ulteriori informazioni, vedere [sys.hash_indexes &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md). **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.|  
|**is_unique**|**bit**|1 = Indice univoco.<br /><br /> 0 = Indice non univoco.<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**data_space_id**|**int**|ID dello spazio dati per l'indice. Lo spazio dati può essere un filegroup o uno schema di partizione.<br /><br /> 0 = **object_id** è una funzione con valori di tabella o un indice in memoria.|  
|**ignore_dup_key**|**bit**|1 = IGNORE_DUP_KEY è ON.<br /><br /> 0 = IGNORE_DUP_KEY è OFF.|  
|**is_primary_key**|**bit**|1 = L'indice fa parte di un vincolo PRIMARY KEY.<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**is_unique_constraint**|**bit**|1 = L'indice fa parte di un vincolo UNIQUE.<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**fill_factor**|**tinyint**|> 0 = percentuale di FILLFACTOR utilizzata al momento della creazione o della ricompilazione dell'indice.<br /><br /> 0 = Valore predefinito<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**is_padded**|**bit**|1 = PADINDEX è ON.<br /><br /> 0 = PADINDEX è OFF.<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**is_disabled**|**bit**|1 = L'indice è disabilitato.<br /><br /> 0 = L'indice non è disabilitato.|  
|**is_hypothetical**|**bit**|1 = L'indice è ipotetico e non può essere utilizzato direttamente come percorso di accesso ai dati. Gli indici ipotetici contengono le statistiche a livello di colonna.<br /><br /> 0 = L'indice non è ipotetico.|  
|**allow_row_locks**|**bit**|1 = L'indice consente blocchi di riga.<br /><br /> 0 = L'indice non consente blocchi di riga.<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**allow_page_locks**|**bit**|1 = L'indice consente blocchi di pagina.<br /><br /> 0 = L'indice non consente blocchi di pagina.<br /><br /> Sempre 0 per gli indici columnstore cluster.|  
|**has_filter**|**bit**|1 = L'indice dispone di un filtro e contiene solo righe che soddisfanno la definizione del filtro.<br /><br /> 0 = L'indice non dispone di un filtro.|  
|**filter_definition**|**nvarchar(max)**|Espressione per il subset di righe incluso nell'indice filtrato.<br /><br /> NULL per heap, indice non filtrato o autorizzazioni insufficienti per la tabella.|  
|**auto_created**|**bit**|1 = l'indice è stato creato dall'ottimizzazione automatica.<br /><br />0 = l'indice è stato creato dall'utente.
|**optimize_for_sequential_key**|**bit**|1 = nell'indice è abilitata l'ottimizzazione dell'inserimento dell'ultima pagina.<br><br>0 = valore predefinito. Nell'indice è stata disabilitata l'ottimizzazione dell'inserimento dell'ultima pagina.|

> [!NOTE]
> Il bit **optimize_for_sequential_key** è supportato solo nelle versioni SQL Server 2019 CTP 3,1 e versioni successive.
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti tutti gli indici per la tabella `Production.Product` nel [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] database.  
  
```  
  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('Production.Product');  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.key_constraints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
