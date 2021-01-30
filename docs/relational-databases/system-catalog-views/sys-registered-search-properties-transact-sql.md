---
description: sys.registered_search_properties (Transact-SQL)
title: sys.registered_search_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.registered_search_properties
- registered_search_properties
- sys.registered_search_properties_TSQL
- registered_search_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search properties [SQL Server]
- property searching [SQL Server], viewing registered properties
- search property lists [SQL Server], viewing registered properties
- sys.registered_search_properties catalog view
ms.assetid: 1b9a7a5c-8c05-4819-83c3-7487dd08fcf7
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb13042972d23742398f38476262cf92b8a4a3f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206868"
---
# <a name="sysregistered_search_properties-transact-sql"></a>sys.registered_search_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una riga per ogni proprietà di ricerca contenuta in qualsiasi elenco delle proprietà di ricerca nel database corrente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**property_list_id**|**int**|ID dell'elenco delle proprietà di ricerca a cui appartiene questa proprietà.|  
|**property_set_guid**|**uniqueidentifier**|Identificatore univoco globale (GUID, Globally Unique Identifier) che identifica il set di proprietà a cui appartiene la proprietà di ricerca.|  
|**property_int_id**|**int**|Valore integer che identifica questa proprietà di ricerca all'interno del set di proprietà. **property_int_id** è univoco all'interno del set di proprietà.|  
|**property_name**|**nvarchar (64)**|Nome che identifica in modo univoco questa proprietà di ricerca nell'elenco delle proprietà di ricerca.<br /><br /> Nota: per eseguire la ricerca in una proprietà, specificare il nome della proprietà nel predicato [Contains](../../t-sql/queries/contains-transact-sql.md) .|  
|**property_description**|**nvarchar(512)**|Descrizione della proprietà.|  
|**property_id**|**int**|ID interno della proprietà di ricerca nell'elenco delle proprietà di ricerca identificato dal valore **property_list_id** .<br /><br /> Quando una determinata proprietà viene aggiunta a un elenco delle proprietà di ricerca specificato, il motore di ricerca full-text registra la proprietà e le assegna un ID interno specifico di tale elenco di proprietà. L'ID di proprietà interno, che è un valore intero, è univoco per ogni elenco delle proprietà di ricerca. Se una determinata proprietà viene registrata in più elenchi di proprietà di ricerca, è possibile che a ciascun elenco di proprietà di ricerca venga assegnato un ID di proprietà interno.<br /><br /> Nota: l'ID di proprietà interno è diverso dall'identificatore di tipo integer della proprietà specificato durante l'aggiunta della proprietà all'elenco di proprietà di ricerca. Per altre informazioni, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).<br /><br /> Per visualizzare tutto il contenuto correlato alla proprietà nell'indice full-text: <br />                  [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)|  
  
## <a name="remarks"></a>Commenti  
 Per altre informazioni sugli elenchi delle proprietà di ricerca, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 La visibilità dei metadati per le proprietà di ricerca è limitata a quelle incluse negli elenchi delle proprietà di ricerca di proprietà dell'utente o per cui è stata concessa un'autorizzazione REFERENCE.  
  
> [!NOTE]  
>  Il proprietario dell'elenco delle proprietà di ricerca può concedere autorizzazioni REFERENCE o CONTROL per l'elenco. Gli utenti con autorizzazione CONTROL possono inoltre concedere l'autorizzazione REFERENCE ad altri utenti.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono elencati tutti i metadati per le proprietà di ricerca registrate.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.registered_search_properties;   
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
