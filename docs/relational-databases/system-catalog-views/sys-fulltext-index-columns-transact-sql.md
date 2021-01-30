---
description: sys.fulltext_index_columns (Transact SQL)
title: sys.fulltext_index_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_index_columns
- fulltext_index_columns
- sys.fulltext_index_columns_TSQL
- fulltext_index_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], columns
- sys.fulltext_index_columns catalog view
- full-text indexes [SQL Server], properties
ms.assetid: c34b8625-e53c-4281-ace6-d46230d5cb84
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e04044d2f19c02ce4a5bfb049ad1efced7a73b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193884"
---
# <a name="sysfulltext_index_columns-transact-sql"></a>sys.fulltext_index_columns (Transact SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una riga per ogni colonna che fa parte di un indice full-text.    
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto di cui la colonna fa parte.|  
|**column_id**|**int**|ID della colonna che fa parte dell'indice full-text.|  
|**type_column_id**|**int**|ID della colonna di tipo in cui è archiviata l'estensione del file di documento fornito dall'utente, ovvero ". doc", ". xls" e così via, del documento in una determinata riga. La colonna del tipo viene specificata solo per le colonne i cui dati richiedono l'applicazione di filtri durante l'indicizzazione full-text. Se questo attributo non è applicabile, il valore è NULL. Per altre informazioni, vedere [Configurazione e gestione di filtri per la ricerca](../../relational-databases/search/configure-and-manage-filters-for-search.md).|  
|**language_id**|**int**|Identificatore LCID della lingua il cui word breaker viene utilizzato per indicizzare questa colonna full-text.<br /><br /> 0 = lingua neutra.<br /><br /> Per ulteriori informazioni, vedere [sys.fulltext_languages &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).|  
|**statistical_semantics**|**int**|1 = per questa colonna è abilitata la semantica statistica oltre all'indicizzazione full-text.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
