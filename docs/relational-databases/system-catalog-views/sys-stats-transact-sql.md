---
description: sys.stats (Transact-SQL)
title: sys. stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.stats
- stats_TSQL
- sys.stats_TSQL
- stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.stats catalog view
ms.assetid: 42605c80-126f-460a-befb-a0b7482fae6a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 05f4df0c3fa0514fb2c393ded5acc677852a8221
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200851"
---
# <a name="sysstats-transact-sql"></a>sys.stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni oggetto statistiche presente per le tabelle, gli indici e le viste indicizzate nel database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ogni indice avrà una riga di statistiche corrispondente con lo stesso nome e lo stesso ID (**index_id**  =  **stats_id**), ma non tutte le righe delle statistiche hanno un indice corrispondente.  
  
 La vista del catalogo [sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) fornisce informazioni sulle statistiche per ogni colonna nel database. Per altre informazioni sulle statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto a cui appartengono le statistiche.|  
|**nome**|**sysname**|Nome delle statistiche. Valore univoco all'interno dell'oggetto.|  
|**stats_id**|**int**|ID delle statistiche. Valore univoco all'interno dell'oggetto.<br /><br />Se le statistiche corrispondono a un indice, il valore *stats_id* corrisponde al valore *index_id* nella vista del catalogo [sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) .|  
|**auto_created**|**bit**|Indica se le statistiche sono state create automaticamente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = le statistiche non sono state create automaticamente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = le statistiche sono state create automaticamente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**user_created**|**bit**|Indica se le statistiche sono state create da un utente.<br /><br /> 0 = le statistiche non sono state create da un utente.<br /><br /> 1 = le statistiche sono state create da un utente.|  
|**no_recompute**|**bit**|Indica se le statistiche sono state create con l'opzione **NORECOMPUTE** .<br /><br /> 0 = le statistiche non sono state create con l'opzione **NORECOMPUTE** .<br /><br /> 1 = le statistiche sono state create con l'opzione **NORECOMPUTE** .|  
|**has_filter**|**bit**|0 = le statistiche non dispongono di un filtro e vengono calcolate in tutte le righe.<br /><br /> 1 = le statistiche dispongono di un filtro e vengono calcolate solo in righe che soddisfanno la definizione del filtro.|  
|**filter_definition**|**nvarchar(max)**|Espressione per il subset di righe incluso nelle statistiche filtrate.<br /><br /> NULL = statistiche non filtrate.|  
|**is_temporary**|**bit**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Indica se le statistiche sono temporanee. Le statistiche temporanee supportano database secondari di [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] abilitati per l'accesso in sola lettura.<br /><br /> 0 = le statistiche non sono temporanee.<br /><br /> 1 = le statistiche sono temporanee.|  
|**is_incremental**|**bit**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Indica se le statistiche sono create come statistiche incrementali.<br /><br /> 0 = le statistiche non sono incrementali.<br /><br /> 1 = le statistiche sono incrementali.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti vengono restituite tutte le statistiche e colonne delle statistiche per la tabella `HumanResources.Employee`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('HumanResources.Employee');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Statistiche](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)
 

 
