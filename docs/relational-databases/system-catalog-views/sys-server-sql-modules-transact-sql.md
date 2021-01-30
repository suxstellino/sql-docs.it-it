---
description: sys.server_sql_modules (Transact-SQL)
title: sys.server_sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.server_sql_modules
- sys.server_sql_modules_TSQL
- server_sql_modules_TSQL
- server_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_sql_modules catalog view
ms.assetid: 9ef9a8b9-c470-4a61-b0c4-ee24ad871d63
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4113ba2be9e77d964d9b9bbfa9ae35f71e6adf8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172612"
---
# <a name="sysserver_sql_modules-transact-sql"></a>sys.server_sql_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene il set di moduli SQL per i trigger a livello di server di tipo TR. È possibile unire in join questa relazione a sys.server_triggers. La tupla (object_id) è la chiave della relazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|È un riferimento FOREIGN KEY al trigger di livello server in cui il modulo è definito.|  
|**definizione**|**nvarchar(max)**|Testo SQL che definisce il modulo.<br /><br /> NULL = Crittografato.|  
|**uses_ansi_nulls**|**bit**|Il modulo è stato creato con l'opzione ANSI NULLS impostata su ON.|  
|**uses_quoted_identifier**|**bit**|Il modulo è stato creato con l'opzione QUOTED IDENTIFIER impostata su ON.|  
|**execute_as_principal_id**|**int**|ID dell'entità server EXECUTE AS.<br /><br /> NULL per impostazione predefinita o se EXECUTE AS CALLER<br /><br /> ID dell'entità specificata se EXECUTE AS SELF EXECUTE AS Principal-2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
