---
description: sys.dm_fts_index_keywords_by_property (Transact-SQL)
title: sys.dm_fts_index_keywords_by_property (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords_by_property
- dm_fts_index_keywords_by_property_TSQL
- sys.dm_fts_index_keywords_by_property
- sys.dm_fts_index_keywords_by_property_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- search property lists [SQL Server], viewing keywords by property
- full-text search [SQL Server], viewing keywords
- sys.dm_fts_index_keywords_by_property dynamic management view
ms.assetid: fa41e052-a79a-4194-9b1a-2885f7828500
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9052c7d64ab78c2e6eca1388ed08903365b6e129
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196256"
---
# <a name="sysdm_fts_index_keywords_by_property-transact-sql"></a>sys.dm_fts_index_keywords_by_property (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce tutto il contenuto correlato alla proprietà nell'indice full-text di una tabella specificata. Sono inclusi tutti i dati che appartengono a qualsiasi proprietà registrata dall'elenco delle proprietà di ricerca associato a tale indice full-text.  
  
 sys.dm_fts_index_keywords_by_property è una funzione a gestione dinamica che consente di visualizzare le proprietà registrate emesse da IFilters al momento dell'indicizzazione, nonché il contenuto esatto di ogni proprietà in ogni documento indicizzato.  
  
 **Per visualizzare tutto il contenuto a livello di documento (incluso il contenuto relativo a una proprietà)**  
  
-   [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
 **Per visualizzare informazioni sull'indice full-text di livello superiore**  
  
-   [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
> [!NOTE]  
>  Per informazioni sugli elenchi delle proprietà di ricerca, vedere [cercare le proprietà dei documenti con gli elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_fts_index_keywords_by_property  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Argomenti  
 db_id ('*database_name*')  
 Una chiamata alla funzione [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Questa funzione accetta un nome di database e restituisce l'ID del database, che sys.dm_fts_index_keywords_by_property utilizza per trovare il database specificato. Se *database_name* viene omesso, viene restituito l'ID del database corrente.  
  
 object_id ('*table_name*')  
 Una chiamata alla funzione [object_id ()](../../t-sql/functions/object-id-transact-sql.md) . Tale funzione accetta un nome di tabella e restituisce l'ID della tabella che contiene l'indice full-text da controllare.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|parola chiave|**nvarchar(4000)**|Rappresentazione esadecimale della parola chiave archiviata nell'indice full-text.<br /><br /> Nota: OxFF rappresenta il carattere speciale che indica la fine di un file o di un set di dati.|  
|display_term|**nvarchar(4000)**|Formato leggibile della parola chiave derivato dal formato interno archiviato nell'indice full-text.<br /><br /> Nota: OxFF rappresenta il carattere speciale che indica la fine di un file o di un set di dati.|  
|column_id|**int**|ID della colonna utilizzata per eseguire l'indicizzazione full-text della parola chiave corrente.|  
|document_id|**int**|ID della riga o del documento utilizzato per eseguire l'indicizzazione full-text del termine corrente. L'ID corrisponde al valore della chiave full-text della riga o del documento specificato.|  
|property_id|**int**|ID interno della proprietà di ricerca all'interno dell'indice full-text della tabella specificata nel parametro OBJECT_ID ('*table_name*').<br /><br /> Quando una determinata proprietà viene aggiunta a un elenco delle proprietà di ricerca, il motore di ricerca full-text registra la proprietà e le assegna un ID interno specifico di tale elenco di proprietà. L'ID di proprietà interno, che è un valore intero, è univoco per ogni elenco delle proprietà di ricerca. Se una determinata proprietà viene registrata in più elenchi di proprietà di ricerca, è possibile che a ciascun elenco di proprietà di ricerca venga assegnato un ID di proprietà interno.<br /><br /> Nota: l'ID di proprietà interno è diverso dall'identificatore di tipo integer della proprietà specificato durante l'aggiunta della proprietà all'elenco di proprietà di ricerca. Per altre informazioni, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).<br /><br /> Per visualizzare l'associazione tra property_id e il nome della proprietà:<br />                    [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)|  
  
## <a name="remarks"></a>Commenti  
 Questa DMV può rispondere a domande come le seguenti:  
  
-   Quale contenuto viene archiviato in una proprietà specificata per un dato DocID?  
  
-   Quanto è comune una proprietà specificata tra i documenti indicizzati?  
  
-   Quali documenti contiene effettivamente una proprietà specificata? Ciò si rivela utile se l'esecuzione di una query su una proprietà di ricerca specificata non restituisce il documento previsto.  
  
 Quando la colonna chiave full-text è, come consigliato, un tipo di dati integer, viene eseguito il mapping diretto di document_id al valore della chiave full-text nella tabella di base.  
  
 Quando invece la colonna chiave full-text utilizza un tipo di dati diverso da integer, document_id non rappresenta la chiave full-text della tabella di base. In questo caso, per identificare la riga nella tabella di base restituita da dm_fts_index_keywords_by_property, è necessario unire in join questa visualizzazione con i risultati restituiti da [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md). Prima di poter eseguire il join, è necessario archiviare l'output della stored procedure in una tabella temporanea È quindi possibile unire in join la colonna document_id di dm_fts_index_keywords_by_property con la colonna DocId restituita da questa stored procedure. Si noti che una colonna **timestamp** non può ricevere valori in fase di inserimento, perché vengono generati automaticamente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La colonna **timestamp** deve pertanto essere convertita in colonne **varbinary (8)** . Nell'esempio seguente sono illustrati i passaggi per l'operazione. In questo esempio *table_id* è l'ID della tabella, *database_name* è il nome del database e *table_name* è il nome della tabella.  
  
```  
USE database_name;  
GO  
CREATE TABLE #MyTempTable   
   (  
      docid INT PRIMARY KEY ,  
      [key] INT NOT NULL  
   );  
DECLARE @db_id int = db_id(N'database_name');  
DECLARE @table_id int = OBJECT_ID(N'table_name');  
INSERT INTO #MyTempTable EXEC sp_fulltext_keymappings @table_id;  
SELECT * FROM sys.dm_fts_index_keywords_by_property   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono necessarie l'autorizzazione SELECT per le colonne analizzate dall'indice full-text e le autorizzazioni CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite parole chiave dalla proprietà `Author` nell'indice full-text della tabella `Production.Document` del database di esempio `AdventureWorks`. Nell'esempio viene utilizzato l'alias `KWBPOP` per la tabella restituita da **sys.dm_fts_index_keywords_by_property**. Nell'esempio vengono utilizzati inner join per combinare colonne da [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) e [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
```  
-- Once the full-text index is configured to support property searching  
-- on the Author property, return any keywords indexed for this property.  
USE AdventureWorks2012;  
GO   
SELECT KWBPOP.* FROM   
   sys.dm_fts_index_keywords_by_property( DB_ID(),   
   object_id('Production.Document') ) AS KWBPOP  
   INNER JOIN  
      sys.registered_search_properties AS RSP ON(   
         (KWBPOP.property_id = RSP.property_id)   
         AND (RSP.property_name = 'Author') )  
   INNER JOIN  
      sys.fulltext_indexes AS FTI ON(   
         (FTI.[object_id] = object_id('Production.Document'))   
         AND (RSP.property_list_id = FTI.property_list_id) );  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [Migliorare le prestazioni degli indici Full-Text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [sp_fulltext_keymappings &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_document &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)   
 [sys.dm_fts_index_keywords &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
