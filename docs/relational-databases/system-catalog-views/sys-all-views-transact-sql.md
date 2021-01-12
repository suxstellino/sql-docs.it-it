---
description: sys.all_views (Transact-SQL)
title: sys.all_views (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.all_views_TSQL
- sys.all_views
- all_views
- all_views_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_views catalog view
ms.assetid: d8829213-fce2-41c6-9ab2-aaab5836c941
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7def7a7a7ba74c5605862dc0a4928f267e94ef0e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095656"
---
# <a name="sysall_views-transact-sql"></a>sys.all_views (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Visualizza tutte viste di sistema e definite dall'utente combinate tramite l'operatore UNION.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Per un elenco di colonne ereditate da questa vista, vedere [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_replicated**|**bit**|1 = la vista viene replicata.|  
|**has_replication_filter**|**bit**|1 = la vista dispone di un filtro di replica.|  
|**has_opaque_metadata**|**bit**|1 = per la vista è specificata l'opzione VIEW_METADATA. Per altre informazioni, vedere [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).|  
|**has_unchecked_assembly_data**|**bit**|1 = La tabella contiene dati persistenti che dipendono da un assembly la cui definizione è stata modificata durante l'ultima esecuzione di ALTER ASSEMBLY. Dopo il completamento della successiva operazione DBCC CHECKDB o DBCC CHECKTABLE, il valore viene reimpostato su 0.|  
|**with_check_option**|**bit**|1 = nella definizione della vista è specificato WITH CHECK OPTION.|  
|**is_date_correlation_view**|**bit**|1 = la vista è stata creata automaticamente dal sistema per l'archiviazione delle informazioni relative alla correlazione tra le colonne di tipo datetime. La creazione di questa vista è stata abilitata impostando DATE_CORRELATION_OPTIMIZATION **su on**.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
  
  
