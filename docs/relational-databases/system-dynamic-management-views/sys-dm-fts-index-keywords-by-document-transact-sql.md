---
description: sys.dm_fts_index_keywords_by_document (Transact-SQL)
title: sys.dm_fts_index_keywords_by_document (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_keywords_by_document_TSQL
- dm_fts_index_keywords_by_document_TSQL
- sys.dm_fts_index_keywords_by_document
- dm_fts_index_keywords_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- sys.dm_fts_index_keywords_by_document dynamic management function
- full-text search [SQL Server], viewing keywords
ms.assetid: 793b978b-c8a1-428c-90c2-a3e49d81b5c9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c11b68f45ef5f42cb4e46fdfb7c2c975df5483c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193802"
---
# <a name="sysdm_fts_index_keywords_by_document-transact-sql"></a>sys.dm_fts_index_keywords_by_document (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Restituisce informazioni sul contenuto a livello di documento di un indice full-text associato alla tabella specificata.  
  
 sys.dm_fts_index_keywords_by_document è una funzione a gestione dinamica.  
  
 **Per visualizzare informazioni sull'indice full-text di livello superiore**  
  
-   [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
 **Per visualizzare informazioni sul contenuto a livello di proprietà relativo a una proprietà del documento**  
  
-   [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_fts_index_keywords_by_document  
(   
    DB_ID('database_name'),     OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Argomenti  
 db_id ('*database_name*')  
 Una chiamata alla funzione [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Tale funzione accetta un nome di database e restituisce l'ID del database utilizzato da sys.dm_fts_index_keywords_by_document per eseguire la ricerca del database specificato. Se *database_name* viene omesso, viene restituito l'ID del database corrente.  
  
 object_id ('*table_name*')  
 Una chiamata alla funzione [object_id ()](../../t-sql/functions/object-id-transact-sql.md) . Tale funzione accetta un nome di tabella e restituisce l'ID della tabella che contiene l'indice full-text da controllare.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|parola chiave|**nvarchar(4000)**|Rappresentazione esadecimale della parola chiave archiviata nell'indice full-text.<br /><br /> Nota: OxFF rappresenta il carattere speciale che indica la fine di un file o di un set di dati.|  
|display_term|**nvarchar(4000)**|Formato leggibile della parola chiave derivato dal formato interno archiviato nell'indice full-text.<br /><br /> Nota: OxFF rappresenta il carattere speciale che indica la fine di un file o di un set di dati.|  
|column_id|**int**|ID della colonna utilizzata per eseguire l'indicizzazione full-text della parola chiave corrente.|  
|document_id|**int**|ID della riga o del documento utilizzato per eseguire l'indicizzazione full-text del termine corrente. L'ID corrisponde al valore della chiave full-text della riga o del documento specificato.|  
|occurrence_count|**int**|Numero di occorrenze della parola chiave corrente nel documento o nella riga indicata da **document_id**. Quando si specifica '*search_property_name*', occurrence_count visualizza solo il numero di occorrenze della parola chiave corrente nella proprietà di ricerca specificata all'interno del documento o della riga.|  
  
## <a name="remarks"></a>Commenti  
 Le informazioni restituite da sys.dm_fts_index_keywords_by_document sono utili per individuare, tra gli altri, anche gli elementi seguenti:  
  
-   Numero totale di parole chiave contenute in un indice full-text.  
  
-   Appartenenza di una parola chiave a una riga oppure a un documento specificato.  
  
-   Numero di volte in cui una parola chiave è presente nell'indice full-text intero, ovvero:  
  
     ([Sum](../../t-sql/functions/sum-transact-sql.md)(**occurrence_count**) ( **parola chiave** = ) *keyword_value* )  
  
-   Numero di volte in cui una parola chiave è presente in una riga oppure in un documento specificato.  
  
-   Numero di parole chiave contenute in una riga oppure in un documento specificato.  
  
 È possibile inoltre utilizzare le informazioni fornite da sys.dm_fts_index_keywords_by_document per recuperare tutte le parole chiave che appartengono a una riga oppure a un documento specificato.  
  
 Quando la colonna chiave full-text è, come consigliato, un tipo di dati integer, viene eseguito il mapping diretto di document_id al valore della chiave full-text nella tabella di base.  
  
 Quando invece la colonna chiave full-text utilizza un tipo di dati diverso da integer, document_id non rappresenta la chiave full-text della tabella di base. In questo caso, per identificare la riga nella tabella di base restituita da dm_fts_index_keywords_by_document, è necessario unire in join questa visualizzazione con i risultati restituiti da [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md). Prima di poter eseguire il join, è necessario archiviare l'output della stored procedure in una tabella temporanea e unire in join la colonna document_id di dm_fts_index_keywords_by_document alla colonna DocId restituita da questa stored procedure. Si noti che una colonna **timestamp** non può ricevere valori in fase di inserimento, perché vengono generati automaticamente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La colonna **timestamp** deve pertanto essere convertita in colonne **varbinary (8)** . Nell'esempio seguente sono illustrati i passaggi per l'operazione. In questo esempio *table_id* è l'ID della tabella, *database_name* è il nome del database e *table_name* è il nome della tabella.  
  
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
SELECT * FROM sys.dm_fts_index_keywords_by_document   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono necessarie l'autorizzazione SELECT per le colonne analizzate dall'indice full-text e le autorizzazioni CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-full-text-index-content-at-the-document-level"></a>R. Visualizzazione del contenuto dell'indice full-text a livello di documento  
 Nell'esempio seguente viene visualizzato il contenuto dell'indice full-text a livello di documento nella tabella `HumanResources.JobCandidate` del database di esempio `AdventureWorks2012`.  
  
> [!NOTE]  
>  È possibile creare questo indice eseguendo l'esempio fornito per la `HumanResources.JobCandidate` tabella in [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_by_document(db_id('AdventureWorks'),   
object_id('HumanResources.JobCandidate'));  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [sp_fulltext_keymappings &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [Migliorare le prestazioni degli indici full-text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)  
  
  
