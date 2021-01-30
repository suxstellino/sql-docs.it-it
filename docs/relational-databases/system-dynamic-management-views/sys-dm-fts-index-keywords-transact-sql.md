---
description: sys.dm_fts_index_keywords (Transact-SQL)
title: sys.dm_fts_index_keywords (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords
- sys.dm_fts_index_keywords
- sys.dm_fts_index_keywords_TSQL
- dm_fts_index_keywords_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords dynamic management function
- full-text search [SQL Server], viewing keywords
- troubleshooting [SQL Server], full-text search
ms.assetid: fce7b2a1-7e74-4769-86a8-c77c7628decd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 55bd43c5d4704d145618401230be62e59f3cd3c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211203"
---
# <a name="sysdm_fts_index_keywords-transact-sql"></a>sys.dm_fts_index_keywords (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sul contenuto di un indice full-text per la tabella specificata.  
  
 **sys.dm_fts_index_keywords** è una funzione a gestione dinamica.  
  
> [!NOTE]  
>  Per visualizzare informazioni sull'indice full-text di livello inferiore, utilizzare la [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md) funzione a gestione dinamica a livello di documento.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_fts_index_keywords( DB_ID('database_name'), OBJECT_ID('table_name') )  
```  
  
## <a name="arguments"></a>Argomenti  
 db_id ('*database_name*')  
 Una chiamata alla funzione [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Questa funzione accetta un nome di database e restituisce l'ID del database, che **sys.dm_fts_index_keywords** utilizza per trovare il database specificato. Se *database_name* viene omesso, viene restituito l'ID del database corrente.  
  
 object_id ('*table_name*')  
 Una chiamata alla funzione [object_id ()](../../t-sql/functions/object-id-transact-sql.md) . Tale funzione accetta un nome di tabella e restituisce l'ID della tabella che contiene l'indice full-text da controllare.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**parola chiave**|**nvarchar(4000)**|Rappresentazione esadecimale della parola chiave archiviata nell'indice full-text.<br /><br /> Nota: OxFF rappresenta il carattere speciale che indica la fine di un file o di un set di dati.|  
|**display_term**|**nvarchar(4000)**|Formato leggibile della parola chiave derivato dal formato esadecimale.<br /><br /> Nota: il valore **display_term** per OxFF è "End of file".|  
|**column_id**|**int**|ID della colonna utilizzata per eseguire l'indicizzazione full-text della parola chiave corrente.|  
|**document_count**|**int**|Numero di documenti o righe che contengono il termine corrente.|  
  
## <a name="remarks"></a>Commenti  
 Le informazioni restituite da **sys.dm_fts_index_keywords** sono utili per individuare gli elementi seguenti:  
  
-   Appartenenza di una parola chiave all'indice full-text.  
  
-   Numero di documenti o righe che contengono una parola chiave specificata.  
  
-   Parola chiave più comune nell'indice full-text:  
  
    -   **document_count** di ogni *keyword_value* rispetto al **document_count** totale, il numero di documenti di 0xFF.  
  
    -   In genere è più appropriato definire come parole non significative le parole chiave più comuni.  
  
> [!NOTE]  
>  Il **document_count** restituito da **sys.dm_fts_index_keywords** può essere meno accurato per un documento specifico rispetto al numero restituito da **sys.dm_fts_index_keywords_by_document** o da una query **Contains** . Questa possibile imprecisione è stimata essere minore dell'1%. Questa inesattezza può verificarsi perché un **document_id** può essere conteggiato due volte quando continua in più di una riga nel frammento di indice o quando viene visualizzato più di una volta nella stessa riga. Per ottenere un conteggio più accurato per un documento specifico, usare **sys.dm_fts_index_keywords_by_document** o una query **Contains** .  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-high-level-full-text-index-content"></a>R. Visualizzazione del contenuto dell'indice full-text di alto livello  
 Nell'esempio seguente vengono visualizzate informazioni sul contenuto di alto livello dell'indice full-text nella tabella `HumanResources.JobCandidate`.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords(db_id('AdventureWorks2012'), object_id('HumanResources.JobCandidate'))  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
