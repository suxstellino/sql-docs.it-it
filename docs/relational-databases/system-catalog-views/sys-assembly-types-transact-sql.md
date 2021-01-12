---
description: sys.assembly_types (Transact-SQL)
title: sys.assembly_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- assembly_types
- sys.assembly_types
- sys.assembly_types_TSQL
- assembly_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_types catalog view
ms.assetid: 35f0384f-7a6d-41b1-9461-f1406d68f317
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b19cc8dff0ec3c1784ac9424f3dba328b1d080d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093258"
---
# <a name="sysassembly_types-transact-sql"></a>sys.assembly_types (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni tipo definito dall'utente che viene definito da un assembly CLR. I **sys.assembly_types** seguenti vengono visualizzati nell'elenco delle colonne ereditate (vedere [sys. Types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)) dopo **rule_object_id**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID dell'assembly da cui è stato creato questo tipo.|  
|**assembly_class**|**sysname**|Nome della classe all'interno dell'assembly che definisce questo tipo.|  
|**is_binary_ordered**|**bit**|L'ordinamento dei byte di questo tipo equivale all'ordinamento tramite gli operatori di confronto sul tipo.|  
|**is_fixed_length**|**bit**|La lunghezza del tipo corrisponde sempre a max_length.|  
|**prog_id**|**nvarchar(40)**|ProgID del tipo esposto a COM.|  
|**assembly_qualified_name**|**nvarchar(4000)**|Nome di tipo completo dell'assembly. Il nome è in un formato appropriato per essere passato a Type.GetType().|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo di tipi scalari &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)  
  
  
