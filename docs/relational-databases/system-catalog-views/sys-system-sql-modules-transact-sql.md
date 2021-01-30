---
description: sys.system_sql_modules (Transact-SQL)
title: sys.system_sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- system_sql_modules_TSQL
- sys.system_sql_modules
- sys.system_sql_modules_TSQL
- system_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_sql_modules catalog view
ms.assetid: ad3548bc-4780-4821-b962-b421d52daed9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 28a554a677c619a0c31c069759487dfa7913635b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190620"
---
# <a name="syssystem_sql_modules-transact-sql"></a>sys.system_sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per oggetto di sistema contenente un modulo definito tramite il linguaggio SQL. Gli oggetti di sistema di tipo FN, IF, P, PC, TF, V sono associati a un modulo SQL. Per identificare l'oggetto contenitore, è possibile unire in join questa visualizzazione per [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Numero di identificazione dell'oggetto contenitore, univoco all'interno di un database.|  
|**definizione**|**nvarchar(max)**|Testo SQL che definisce il modulo.|  
|**uses_ansi_nulls**|**bit**|1 = Il modulo è stato creato con l'opzione di database SET ANSI_NULLS impostata su ON.<br /><br /> Restituisce sempre 1.|  
|**uses_quoted_identifier**|**bit**|1= Il modulo è stato creato con SET QUOTED_IDENTIFIER ON.<br /><br /> Restituisce sempre 1.|  
|**is_schema_bound**|**bit**|0 = Il modulo non è stato creato con l'opzione SCHEMABINDING.<br /><br /> Restituisce sempre 0.|  
|**uses_database_collation**|**bit**|0 = Il modulo non dipende dalle regole di confronto predefinite del database.<br /><br /> Restituisce sempre 0.|  
|**is_recompiled**|**bit**|0 = La procedura non è stata creata tramite l'opzione WITH RECOMPILE.<br /><br /> Restituisce sempre 0.|  
|**null_on_null_input**|**bit**|0 = Il modulo è stato creato in modo da produrre un output NULL per ogni input NULL.<br /><br /> Restituisce sempre 0.|  
|**execute_as_principal_id**|**int**|Restituisce sempre NULL.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.all_sql_modules &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-sql-modules-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
