---
description: sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
title: sys.dm_fts_index_keywords_position_by_document (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document
- sys.dm_fts_index_keywords_position_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords_position_by_document dynamic management view
ms.assetid: 0d70184f-baa2-411b-a32d-a4c5af890edd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 099574becb70e3812864c4acc7dc0a395f3169b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211231"
---
# <a name="sysdm_fts_index_keywords_position_by_document-transact-sql"></a>sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni posizionali della parola chiave nei documenti indicizzati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Argomenti  
 db_id ('*database_name*')  
 Una chiamata alla funzione [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Questa funzione accetta un nome di database e restituisce l'ID del database, che sys.dm_fts_index_keywords_position_by_document utilizza per trovare il database specificato.  
  
 object_id ('*table_name*')  
 Una chiamata alla funzione [object_id ()](../../t-sql/functions/object-id-transact-sql.md) . Tale funzione accetta un nome di tabella e restituisce l'ID della tabella che contiene l'indice full-text da controllare.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|parola chiave|**varbinary(128)**|Stringa binaria che rappresenta la parola chiave.|  
|display_term|**nvarchar(4000)**|Formato leggibile della parola chiave derivato dal formato interno archiviato nell'indice full-text.|  
|column_id|**int**|ID della colonna utilizzata per eseguire l'indicizzazione full-text della parola chiave corrente.|  
|document_id|**bigint**|ID della riga o del documento utilizzato per eseguire l'indicizzazione full-text del termine corrente. L'ID corrisponde al valore della chiave full-text della riga o del documento specificato.|  
|position|**int**|Posizione della parola chiave nel documento.|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la DMV per identificare la posizione delle parole indicizzate nei documenti indicizzati. Questa DMV può essere utilizzata per risolvere i problemi quando **sys.dm_fts_index_keywords_by_document** indica che le parole si trovano nell'indice full-text, ma quando si esegue una query utilizzando tali parole, il documento non viene restituito.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono necessarie l'autorizzazione SELECT per le colonne analizzate dall'indice full-text e le autorizzazioni CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite le parole chiave dell'indice full-text della `Production.Document` tabella del `AdventureWorks` database di esempio.  
  
```  
USE AdventureWorks2012;  
GO   
  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
);   
GO  
```  
  
 È possibile aggiungere un predicato nell'altro columns_id come nella query di esempio seguente per isolare ulteriormente le posizioni.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
)  
WHERE document_id = 7 AND display_term = 'performance';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [Migliorare le prestazioni degli indici Full-Text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [Funzioni di ricerca full-text e di ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-functions/full-text-search-and-semantic-search-functions-transact-sql.md)   
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Stored procedure per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
