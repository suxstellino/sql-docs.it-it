---
description: sys.securable_classes (Transact-SQL)
title: sys.securable_classes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- securable_classes_TSQL
- securable_classes
- sys.securable_classes_TSQL
- sys.securable_classes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.securable_classes catalog view
ms.assetid: ae2bf589-17be-4cad-b5d5-05a34173b32d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92b4082fc8cfdf4771813d128eb3cbbac0d27321
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194785"
---
# <a name="syssecurable_classes-transact-sql"></a>sys.securable_classes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un elenco di classi a protezione diretta.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**class_desc**|**sysname**|Nome della classe.|  
|**class**|**int**|Designazione numerica della classe.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite le classi di entità a protezione diretta supportate da questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT * FROM sys.securable_classes ORDER BY class;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Entità a protezione diretta](../../relational-databases/security/securables.md)  
  
  
