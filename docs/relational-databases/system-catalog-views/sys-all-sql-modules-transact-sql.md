---
description: sys.all_sql_modules (Transact-SQL)
title: sys.all_sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- all_sql_modules_TSQL
- sys.all_sql_modules
- all_sql_modules
- sys.all_sql_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_sql_modules catalog view
ms.assetid: 7477a3fe-afb3-44c8-bb2c-c6e1d9bdee6f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 627cafe9f1ab44ceacbe6476f5c4c912f63f006d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092540"
---
# <a name="sysall_sql_modules-transact-sql"></a>sys.all_sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce l'Unione di **sys.sql_modules** e **sys.system_sql_modules**.  
  
 La vista restituisce una riga per ogni funzione scalare definita dall'utente e compilata in modo nativo. Per altre informazioni, vedere [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto contenitore. Valore univoco all'interno di un database.|  
|**definizione**|**nvarchar(max)**|Testo SQL che definisce il modulo.<br /><br /> NULL = Crittografato|  
|**uses_ansi_nulls**|**bit**|Modulo creato con SET ANSI_NULLS ON.|  
|**uses_quoted_identifier**|**bit**|Modulo creato con SET QUOTED_IDENTIFIER ON.|  
|**is_schema_bound**|**bit**|Modulo creato con l'opzione SCHEMABINDING.|  
|**uses_database_collation**|**bit**|1 = La definizione del modulo associato a uno schema dipende dalle regole di confronto predefinite del database per una corretta valutazione; altrimenti, 0. Tale dipendenza impedisce la modifica delle regole di confronto predefinite del database.|  
|**is_recompiled**|**bit**|Procedura creata con l'opzione WITH RECOMPILE.|  
|**null_on_null_input**|**bit**|Modulo dichiarato per restituire un output NULL per ogni input NULL.|  
|**execute_as_principal_id**|**int**|ID dell'entità database EXECUTE AS.<br /><br /> Questo valore è NULL per impostazione predefinita e se viene utilizzato EXECUTE AS CALLER.<br /><br /> ID dell'entità specificata se EXECUTE AS SELF o EXECUTE AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|bit|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> 0 = non compilata in modo nativo<br /><br /> 1 = compilata in modo nativo<br /><br /> Il valore predefinito è 0.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.system_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-sql-modules-transact-sql.md)   
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
