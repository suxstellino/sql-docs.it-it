---
description: Viste del catalogo Rilevamento modifiche-sys.change_tracking_tables
title: sys.change_tracking_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- change_tracking_tables_TSQL
- sys.change_tracking_tables
- change_tracking_tables
- sys.change_tracking_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], sys.change_tracking_tables
- sys.change_tracking_tables
ms.assetid: 97ec69b6-0d49-4d98-82f0-d3e77ba1ad2b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 371fa8a112239e0edd214a5c4484e3b4ce06423f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193041"
---
# <a name="change-tracking-catalog-views---syschange_tracking_tables"></a>Viste del catalogo Rilevamento modifiche-sys.change_tracking_tables
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni tabella nel database corrente in cui è abilitato il rilevamento delle modifiche.  
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID di una tabella con un registro delle modifiche. La tabella può presentare un registro delle modifiche anche se il rilevamento delle modifiche è al momento disattivato.<br /><br /> L'ID della tabella è univoco all'interno del database.|  
|is_track_columns_updated_on|**bit**|Stato corrente di rilevamento delle modifiche nella tabella:<br /><br /> 0 = OFF<br /><br /> 1 = ON|  
|begin_version|**bigint**|Versione del database all'inizio del rilevamento delle modifiche per la tabella. Questa versione indica generalmente quando è stato abilitato il rilevamento delle modifiche, tuttavia se la tabella viene troncata il valore viene reimpostato.|  
|cleanup_version|**bigint**|Versione fino alla quale la pulizia potrebbe aver rimosso le informazioni sul rilevamento delle modifiche.|  
|min_valid_version|**bigint**|Minima versione valida delle informazioni sul rilevamento delle modifiche disponibili per la tabella.<br /><br /> Quando si ottengono modifiche dalla tabella associata a questa riga, il valore di last_sync_version deve essere maggiore o uguale alla versione indicata in questa colonna. Per ulteriori informazioni, vedere [CHANGE_TRACKING_MIN_VALID_VERSION &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md).|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)   
 [Viste del catalogo di Rilevamento modifiche &#40;&#41;Transact-SQL ](./catalog-views-transact-sql.md)   
 [Rilevare le modifiche ai dati &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
