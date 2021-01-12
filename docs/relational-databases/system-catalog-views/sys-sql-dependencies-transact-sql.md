---
description: sys.sql_dependencies (Transact-SQL)
title: sys.sql_dependencies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sql_dependencies
- sql_dependencies_TSQL
- sys.sql_dependencies_TSQL
- sys.sql_dependencies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_dependencies catalog view
ms.assetid: 1779aa87-a0b8-470a-a286-d7cc0b93ad2e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7c20d20cedd1304136af2d57f599aa2797dd4eee
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099189"
---
# <a name="syssql_dependencies-transact-sql"></a>sys.sql_dependencies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni dipendenza in un'entità a cui viene fatto riferimento nell'espressione o nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che definiscono un altro oggetto di riferimento.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) .  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifica la classe dell'entità con riferimenti:<br /><br /> 0 = Oggetto o colonna (solo riferimenti non associati a schema)<br /><br /> 1 = Oggetto o colonna (riferimenti associati a schema)<br /><br /> 2 = Tipi (riferimenti associati a schema)<br /><br /> 3 = Raccolte di XML Schema (riferimenti associati a schema)<br /><br /> 4 = Funzione di partizione (riferimenti associati a schema)|  
|**class_desc**|**nvarchar(60)**|Descrizione della classe dell'entità con riferimenti:<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_NON_SCHEMA_BOUND**<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_SCHEMA_BOUND**<br /><br /> **TYPE_REFERENCE**<br /><br /> **XML_SCHEMA_COLLECTION_REFERENCE**<br /><br /> **PARTITION_FUNCTION_REFERENCE**|  
|**object_id**|**int**|ID dell'oggetto di riferimento.|  
|**column_id**|**int**|Se l'ID di riferimento è una colonna, il valore corrisponde all'ID della colonna di riferimento. In caso contrario il valore è 0.|  
|**referenced_major_id**|**int**|ID dell'entità con riferimenti, interpretato in base al valore della classe come indicato di seguito:<br /><br /> 0, 1 = ID dell'oggetto o della colonna.<br /><br /> 2 = ID del tipo.<br /><br /> 3 = ID della raccolta di XML Schema.|  
|**referenced_minor_id**|**int**|ID secondario dell'entità con riferimenti, interpretato in base al valore della classe, come indicato di seguito.<br /><br /> Se class =:<br /><br /> 0, **referenced_minor_id** è un ID di colonna; o se non è una colonna, è 0.<br /><br /> 1, **referenced_minor_id** è un ID di colonna; o se non è una colonna, è 0.<br /><br /> In caso contrario, **referenced_minor_id** = 0.|  
|**is_selected**|**bit**|Indica se la colonna o l'oggetto è selezionato.|  
|**is_updated**|**bit**|Indica se la colonna o l'oggetto è aggiornato.|  
|**is_select_all**|**bit**|Indica se l'oggetto è utilizzato nella clausola SELECT * (solo a livello di oggetto).|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
