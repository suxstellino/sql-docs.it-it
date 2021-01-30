---
description: Viste a gestione dinamica di ricerca Full-Text e semantica-funzioni
title: Viste a gestione dinamica di ricerca Full-Text e semantica-funzioni | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- dynamic management objects [SQL Server], full-text search
- full-text search [SQL Server], dynamic management views
ms.assetid: 199dbd5a-29f6-4ef0-8e65-86e32c0aaa3a
author: pmasl
ms.author: pelopes
ms.openlocfilehash: cd9e072fa2c4aafceaad2d4aad57eb9dcc4bb38b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201354"
---
# <a name="full-text-and-semantic-search-dynamic-management-views---functions"></a>Viste a gestione dinamica di ricerca Full-Text e semantica-funzioni
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questa sezione sono contenute le funzioni e le DMV relative alla ricerca full-text e semantica riportate di seguito.  
  
## <a name="full-text-search-dynamic-management-views-and-functions"></a>DMV e funzioni per la ricerca full-text  
 [sys.dm_fts_active_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
 Restituisce informazioni sui cataloghi full-text caratterizzati da attività di popolamento in corso nel server.  
  
 [sys.dm_fts_fdhosts](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
 Restituisce informazioni sull'attività corrente dell'host o degli host del daemon di filtri nell'istanza del server.  
  
 [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
 Restituisce informazioni sul contenuto di un indice full-text per la tabella specificata.  
  
 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
 Restituisce informazioni sul contenuto a livello di documento di un indice full-text per la tabella specificata. Una determinata parola chiave può essere inclusa in diversi documenti.  
  
 [sys.dm_fts_index_keywords_by_property](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
 Restituisce tutto il contenuto correlato alla proprietà nell'indice full-text di una tabella specificata. Sono inclusi tutti i dati che appartengono a qualsiasi proprietà registrata dall'elenco delle proprietà di ricerca associato a tale indice full-text.  
  
 sys.dm_fts_index_keywords_position_by_document  
 Restituisce la posizione delle parole chiave in un documento.  
  
 [sys.dm_fts_index_population](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
 Restituisce informazioni sui popolamenti di indici full-text in corso.  
  
 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
 Restituisce informazioni sui buffer di memoria appartenenti a uno specifico pool di memoria utilizzati in una ricerca o in un intervallo di ricerche per indicizzazione full-text.  
  
 [sys.dm_fts_memory_pools](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
 Restituisce informazioni sui pool di memoria condivisi disponibili per il componente gatherer full-text per una ricerca o un intervallo di ricerche per indicizzazione full-text.  
  
 [sys.dm_fts_outstanding_batches](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
 Restituisce informazioni su ogni batch di indicizzazione full-text.  
  
 [sys.dm_fts_parser](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
 Restituisce il risultato della tokenizzazione finale dopo avere applicato una combinazione specifica di word breaker, thesaurus ed elenchi di parole non significative all'input di una stringa di query. Il risultato equivale all'output del motore di ricerca full-text per la stringa di query specificata.  
  
 [sys.dm_fts_population_ranges](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
 Restituisce informazioni sugli intervalli specifici correlati a un popolamento dell'indice full-text in corso.  
  
## <a name="semantic-search-dynamic-management-views-and-functions"></a>DMV e funzioni per la ricerca semantica  
 [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
 Restituisce una riga di informazioni sullo stato relative al popolamento dell'indice di somiglianza dei documenti per ogni indice di somiglianza in ogni tabella a cui è associato un indice semantico.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste di sistema &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)  
  
