---
description: sys.fulltext_semantic_language_statistics_database (Transact-SQL)
title: sys.fulltext_semantic_language_statistics_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_semantic_language_statistics_database_TSQL
- fulltext_semantic_language_statistics_database_TSQL
- fulltext_semantic_language_statistics_database
- sys.fulltext_semantic_language_statistics_database
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_semantic_language_statistics_database catalog view
ms.assetid: 32e95614-ed88-4068-8c37-1e21544717bc
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: ec9f9811504ed9c58c8a37510a46a1857540584f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193871"
---
# <a name="sysfulltext_semantic_language_statistics_database-transact-sql"></a>sys.fulltext_semantic_language_statistics_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga sul database di statistiche lingua semantica installato nell'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 È possibile eseguire una query su questa vista per individuare il componente delle statistiche sulla lingua per l'indicizzazione semantica necessario per l'elaborazione semantica.  
   
  
||||  
|-|-|-|  
|**Nome colonna**|**Tipo**|**Descrizione**|  
|**database_id**|**int**|ID del database, univoco all'interno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**register_date**|**datetime**|Data in cui il database è stato registrato per l'elaborazione semantica.|  
|**registered_by**|**int**|ID dell'entità server che ha registrato il database per l'elaborazione semantica.|  
|**version**|**nvarchar(128)**|Informazioni sulla versione più recente specifiche per il database di statistiche lingua semantica.|  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Per altre informazioni, vedere [Installazione e configurazione della ricerca semantica](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
## <a name="metadata"></a>Metadati  
 Per informazioni sulle lingue supportate per l'indicizzazione semantica, eseguire una query sulla vista del catalogo [sys.fulltext_semantic_languages &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 La visibilità dei metadati nelle viste del catalogo è limitata alle entità a protezione diretta di cui l'utente è proprietario o per le quali dispone di autorizzazioni.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrato come eseguire una query **sys.fulltext_semantic_language_statistics_database** per ottenere informazioni sul database di statistiche lingua semantica registrato nell'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```  
SELECT * FROM sys.fulltext_semantic_language_statistics_database;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Installazione e configurazione della ricerca semantica](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  
