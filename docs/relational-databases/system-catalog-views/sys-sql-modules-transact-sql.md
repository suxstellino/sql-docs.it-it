---
description: sys.sql_modules (Transact-SQL)
title: sys.sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_modules_TSQL
- sql_modules
- sql_modules_TSQL
- sys.sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_modules catalog view
ms.assetid: 23d3ccd2-f356-4d89-a2cd-bee381243f99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 58e1f2042f10d1b4f06482fdfb13f07cf5edfa3e
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237706"
---
# <a name="syssql_modules-transact-sql"></a>sys.sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni oggetto che è un modulo definito dal linguaggio SQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , inclusa la funzione scalare definita dall'utente compilata in modo nativo. Agli oggetti di tipo P, RF, V, TR, FN, IF, TF e R è associato un modulo SQL. In questa vista anche agli oggetti predefiniti autonomi, ovvero gli oggetti di tipo D, sono associati a una definizione di modulo SQL. Per una descrizione di questi tipi, vedere la colonna **Type** nella vista del catalogo [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) .  
  
 Per altre informazioni, vedere [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto contenitore. Valore univoco all'interno di un database.|  
|**definizione**|**nvarchar(max)**|Testo SQL che definisce il modulo. Questo valore può essere ottenuto anche usando la funzione incorporata [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .<br /><br /> NULL = Crittografato.|  
|**uses_ansi_nulls**|**bit**|Modulo creato con SET ANSI_NULLS ON.<br /><br /> È sempre = 0 per regole e impostazioni predefinite.|  
|**uses_quoted_identifier**|**bit**|Modulo creato con SET QUOTED_IDENTIFIER ON.|  
|**is_schema_bound**|**bit**|Modulo creato con l'opzione SCHEMABINDING.<br /><br /> Contiene sempre il valore 1 per le stored procedure compilate in modo nativo.|  
|**uses_database_collation**|**bit**|1 = La definizione del modulo associato a uno schema dipende dalle regole di confronto predefinite del database per una corretta valutazione; altrimenti, 0. Tale dipendenza impedisce la modifica delle regole di confronto predefinite del database.|  
|**is_recompiled**|**bit**|Procedura creata con l'opzione WITH RECOMPILE.|  
|**null_on_null_input**|**bit**|Modulo dichiarato per restituire un output NULL per ogni input NULL.|  
|**execute_as_principal_id**|**Int**|ID dell'entità database EXECUTE AS.<br /><br /> Questo valore è NULL per impostazione predefinita e se viene utilizzato EXECUTE AS CALLER.<br /><br /> ID dell'entità specificata se EXECUTE AS SELF o EXECUTE AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|**bit**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] tramite [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].<br /><br /> 0 = non compilata in modo nativo<br /><br /> 1 = compilata in modo nativo<br /><br /> Il valore predefinito è 0.|  
|**is_inlineable**|**bit**|**Si applica a**: [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] e versioni successive.<br/><br />Indica se il modulo è inline o meno. La Incorporabilità è basata sulle condizioni specificate [qui](../user-defined-functions/scalar-udf-inlining.md#inlineable-scalar-udfs-requirements).<br /><br /> 0 = non inline<br /><br /> 1 = è inline. <br /><br /> Per le funzioni definite dall'utente scalari, il valore sarà 1 se la funzione definita dall'utente è inline e 0 in caso contrario. Contiene sempre il valore 1 per funzioni con valori inline e 0 per tutti gli altri tipi di modulo.<br />|  
|**inline_type**|**bit**|**Si applica a**: [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] e versioni successive.<br /><br />Indica se l'incorporamento è attualmente attivato per il modulo. <br /><br />0 = l'incorporamento è disattivato<br /><br /> 1 = l'incorporamento è attivato.<br /><br /> Per le funzioni UDF scalari, il valore sarà 1 se l'incorporamento è attivato (in modo esplicito o implicito). Il valore sarà sempre 1 per funzioni con valori inline e 0 per altri tipi di modulo.<br />|  

  
## <a name="remarks"></a>Commenti  
 L'espressione SQL per un vincolo predefinito, oggetto di tipo D, è disponibile nella vista del catalogo [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md) . L'espressione SQL per un vincolo CHECK, oggetto di tipo C, si trova nella vista del catalogo [sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) .  
  
 Queste informazioni sono descritte anche in [sys.dm_db_uncontained_entities &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti il nome, il tipo e la definizione di ogni modulo del database corrente.  
  
```  
SELECT sm.object_id, OBJECT_NAME(sm.object_id) AS object_name, o.type, o.type_desc, sm.definition  
FROM sys.sql_modules AS sm  
JOIN sys.objects AS o ON sm.object_id = o.object_id  
ORDER BY o.type;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
