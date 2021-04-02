---
description: sys.dm_fts_semantic_similarity_population (Transact-SQL)
title: sys.dm_fts_semantic_similarity_population (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_semantic_similarity_population_TSQL
- sys.dm_fts_semantic_similarity_population
- dm_fts_semantic_similarity_population
- sys.dm_fts_semantic_similarity_population_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_semantic_similarity_population dynamic management view
ms.assetid: 33666f28-c370-47e2-a932-190316ed5f69
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: d44731be44d6e3356b74ac2f856dbb8c38e2970c
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054722"
---
# <a name="sysdm_fts_semantic_similarity_population-transact-sql"></a>sys.dm_fts_semantic_similarity_population (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga di informazioni sullo stato relative al popolamento dell'indice di somiglianza dei documenti per ogni indice di somiglianza in ogni tabella a cui è associato un indice semantico.  
  
 Il passaggio del popolamento segue il passaggio dell'estrazione. Per informazioni sullo stato relative al passaggio di estrazione della somiglianza, vedere [sys.dm_fts_index_population &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md).  
    
|**Nome colonna**|**Tipo**|**Descrizione**|  
|-|-|-|  
|**database_id**|**int**|ID del database contenente l'indice full-text in fase di popolamento.|  
|**catalog_id**|**int**|ID del catalogo full-text contenente l'indice full-text.|  
|**table_id**|**int**|ID della tabella per la quale l'indice full-text è in fase di popolamento.|  
|**document_count**|**int**|Numero dei documenti totali nel popolamento.|  
|**document_processed_count**|**int**|Numero di documenti elaborati dall'inizio del ciclo di popolamento.|  
|**completion_type**|**int**|Stato della modalità di completamento del popolamento.|  
|**completion_type_description**|**nvarchar(120)**|Descrizione del tipo di completamento.|  
|**worker_count**|**int**|Numero di thread di lavoro associati all'estrazione della somiglianza.|  
|**Stato**|**int**|Stato del popolamento. Nota: alcuni stati sono temporanei. I tipi validi sono:<br /><br /> 3 = avvio in corso<br /><br /> 5 = elaborazione normale in corso<br /><br /> 7 = elaborazione arrestata<br /><br /> 11 = popolamento interrotto|  
|**status_description**|**nvarchar(120)**|Descrizione dello stato del popolamento.|  
|**start_time**|**datetime**|Ora di inizio del popolamento.|  
|**incremental_timestamp**|**timestamp**|Rappresenta il timestamp iniziale per il popolamento completo. Per tutti gli altri tipi di popolamento questo valore corrisponde all'ultimo checkpoint di cui è stato eseguito il commit che rappresenta lo stato dei popolamenti.|  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Per altre informazioni, vedere [gestire e monitorare la ricerca semantica](../../relational-databases/search/manage-and-monitor-semantic-search.md).  
  
## <a name="metadata"></a>Metadati  
 Per ulteriori informazioni sullo stato dell'indicizzazione semantica, eseguire query [sys.dm_fts_index_population &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrato come eseguire una query sullo stato dei popolamenti dell'indice di somiglianza dei documenti per tutte le tabelle a cui è associato un indice semantico:  
  
```  
SELECT * FROM sys.dm_fts_semantic_similarity_population;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire e monitorare la ricerca semantica](../../relational-databases/search/manage-and-monitor-semantic-search.md)  
  
  
