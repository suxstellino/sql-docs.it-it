---
description: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
title: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_gc_cycle_stats_TSQL
- dm_db_xtp_gc_cycle_stats
- sys.dm_db_xtp_gc_cycle_stats_TSQL
- sys.dm_db_xtp_gc_cycle_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_gc_cycle_stats dynamic management view
ms.assetid: bbc9704e-158e-4d32-b693-f00dce31cd2f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd6973004f499bfefc950b61382a1d0fe5c6495d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092894"
---
# <a name="sysdm_db_xtp_gc_cycle_stats-transact-sql"></a>sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Restituisce lo stato corrente delle transazioni su cui è stato eseguito il commit e che hanno eliminato una o più righe. Il thread inattivo di Garbage Collection viene attivato ogni minuto o quando il numero delle transazioni DML su cui è stato eseguito il commit supera una soglia interna dopo l'ultimo ciclo di Garbage Collection. Come parte del ciclo di Garbage Collection, sposta le transazioni che sono state sottoposte al commit in una o più code associate alle generazioni. Le transazioni che hanno generato versioni non aggiornate vengono raggruppate in unità di 16 transazioni di 16 generazioni come indicato di seguito:  
  
-   Generazione 0: archivia tutte le transazioni di cui è stato eseguito il commit prima della transazione attiva meno recente. Le versioni di riga generate da queste transazioni sono immediatamente disponibili per il Garbage Collection.  
  
-   Generazione 1-14: archivia le transazioni con un timestamp maggiore della transazione attiva meno recente. Le versioni di riga non possono essere sottoposte a Garbage Collection. Ogni generazione può contenere fino a 16 transazioni. In queste generazioni possono essere presenti 224 (14 * 16) transazioni.  
  
-   Generazione 15: le transazioni rimanenti con un timestamp maggiore della transazione attiva meno recente passano alla generazione 15. Analogamente alla generazione 0, non vi sono limiti di numero di transazioni nella generazione 15.  
  
 Quando sono presenti richieste di memoria, il thread di Garbage Collection aggiorna l'hint della transazione attiva meno recente in modo aggressivo forzando il Garbage Collection.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|cycle_id|**bigint**|Identificatore univoco del ciclo di Garbage Collection.|  
|ticks_at_cycle_start|**bigint**|Tick all'avvio del ciclo.|  
|ticks_at_cycle_end|**bigint**|Tick alla fine del ciclo.|  
|base_generation|**bigint**|Il valore di generazione di base corrente nel database. Rappresenta il timestamp della transazione attiva meno recente utilizzato per identificare le transazioni per il Garbage Collection. L'ID della transazione attiva meno recente viene aggiornato in incrementi di 16. Ad esempio, se si dispone di ID transazione come 124, 125, 126... 139, il valore sarà 124. Quando si aggiunge un'altra transazione, ad esempio 140, il valore sarà 140.|  
|xacts_copied_to_local|**bigint**|Numero di transazioni copiate dalla pipeline di transazione nella matrice di generazione del database.|  
|xacts_in_gen_0- xacts_in_gen_15|**bigint**|Numero di transazioni in ogni generazione.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il server.  
  
## <a name="usage-scenario"></a>Scenario di utilizzo  
 Di seguito è riportato un output campione con un subset di colonne, che mostra 27 generazioni:  
  
```  
cycle_id   ticks_at_cycle_start ticks_at_cycle_end   base_generation  xacts_in_gen_0    xacts_in_gen_1  
  
1          123160509            123160509            1                    0                    0  
2          123176822            123176822            1                    0                    1  
3          123236826            123236826            1                    0                    1  
4          123296829            123296829            1                    0                    1  
5          123356832            123356941            129                  0                    0  
6          123357473            123357473            129                  0                    0  
7          123417486            123417486            129                  0                    0  
8          123477489            123477489            129                  0                    0  
9          123537492            123537492            129                  0                    0  
10         123597500            123597500            129                  0                    0  
11         123657504            123657504            129                  0                    0  
12         123717507            123717507            129                  0                    0  
13         123777510            123777510            129                  0                    0  
14         123837513            123837513            129                  0                    0  
15         123897516            123897516            129                  0                    0  
16         123957516            123957516            129                  0                    0  
17         124017516            124017516            129                  0                    0  
18         124077517            124077517            129                  0                    0  
19         124137517            124137517            129                  0                    0  
20         124197518            124197518            129                  0                    0  
21         124257518            124257518            129                  0                    0  
22         124317523            124317523            129                  0                    0  
23         124377526            124377526            129                  0                    0  
24         124437529            124437529            129                  0                    0  
25         124497533            124497533            129                  0                    0  
26         124557536            124557536            129                  0                    0  
27         124617539            124617539            129                  0                    0  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
