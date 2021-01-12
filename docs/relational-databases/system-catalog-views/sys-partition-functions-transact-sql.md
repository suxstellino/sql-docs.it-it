---
description: sys.partition_functions (Transact-SQL)
title: sys.partition_functions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.partition_functions_TSQL
- partition_functions
- sys.partition_functions
- partition_functions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_functions catalog view
ms.assetid: 96515727-728b-4bea-804a-36ce915b8b75
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e5049d582e43e39c18cb7554a566a1212deb80f8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100641"
---
# <a name="syspartition_functions-transact-sql"></a>sys.partition_functions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni funzione di partizione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome della funzione di partizione. Valore univoco all'interno del database.|  
|**function_id**|**int**|ID della funzione di partizione. Valore univoco all'interno del database.|  
|**type**|**char(2)**|Tipo di funzione.<br /><br /> R = Intervallo|  
|**type_desc**|**nvarchar(60)**|Tipo di funzione.<br /><br /> RANGE|  
|**fanout**|**int**|Numero di partizioni create dalla funzione.|  
|**boundary_value_on_right**|**bit**|Per il partizionamento per intervalli.<br /><br /> 1 = Il valore limite è incluso nell'intervallo RIGHT del limite.<br /><br /> 0 = LEFT.|  
|**is_system**||**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> 1 = L'oggetto viene utilizzato per i frammenti dell'indice full-text.<br /><br /> 0 = L'oggetto non viene utilizzato per i frammenti dell'indice full-text.|  
|**create_date**|**datetime**|Data di creazione della funzione.|  
|**modify_date**|**datetime**|Data dell'ultima modifica della funzione eseguita mediante un'istruzione ALTER.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo delle funzioni di partizione &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)  
  
  
