---
description: MSmerge_articlehistory (Transact-SQL)
title: MSmerge_articlehistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_articlehistory
- MSmerge_articlehistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_articlehistory system table
ms.assetid: 2870e7ea-dbec-4636-9171-c2cee96018ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: 32db379ebb966673efca772f7d1c33dde29d9f2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212260"
---
# <a name="msmerge_articlehistory-transact-sql"></a>MSmerge_articlehistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_articlehistory** tiene traccia delle modifiche apportate agli articoli durante una sessione di sincronizzazione agente di merge, con una riga per ogni articolo al quale sono state apportate le modifiche. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID di una sessione agente di merge processo nella tabella di sistema [MSmerge_sessions](../../relational-databases/system-tables/msmerge-sessions-transact-sql.md) .|  
|**phase_id**|**int**|Fase della sessione di sincronizzazione, i cui valori possono essere:<br /><br /> **1** = caricamento.<br /><br /> **2** = download.<br /><br /> **4** = pulizia.<br /><br /> **5** = arresto.<br /><br /> **6** = modifiche dello schema.<br /><br /> **7** = bcp.|  
|**article_name**|**sysname**|Nome dell'articolo al quale sono state apportate le modifiche.|  
|**start_time**|**datetime**|Ora di inizio dell'elaborazione dell'articolo.|  
|**duration**|**int**|Durata dell'elaborazione di un articolo, in secondi.|  
|**inserti**|**int**|Numero di inserimenti applicati a un articolo specifico durante la sincronizzazione. Questo valore verrà incrementato durante il processo di sincronizzazione e il valore finale rappresenta il numero totale.|  
|**aggiornamenti**|**int**|Numero di aggiornamenti applicati a un articolo specifico durante la sincronizzazione. Questo valore verrà incrementato durante il processo di sincronizzazione e il valore finale rappresenta il numero totale.|  
|**Elimina**|**int**|Numero di eliminazioni applicate a un articolo specifico durante la sincronizzazione. Questo valore verrà incrementato durante il processo di sincronizzazione e il valore finale rappresenta il numero totale.|  
|**conflitti**|**int**|Numero di conflitti che si sono verificati durante la sincronizzazione. Questo valore verrà incrementato durante il processo di sincronizzazione e il valore finale rappresenta il numero totale.|  
|**conflicts_resolved**|**int**|Numero di conflitti che si sono verificati durante la sincronizzazione e che sono stati risolti. Questo valore verrà incrementato durante il processo di sincronizzazione e il valore finale rappresenta il numero totale.|  
|**rows_retried**|**int**|Numero di righe per cui si è verificato un errore e che sono state ritentate durante la sincronizzazione. Questo valore verrà incrementato durante il processo di sincronizzazione e il valore finale rappresenta il numero totale.|  
|**percent_complete**|**decimal**|Percentuale del tempo di sincronizzazione totale impiegato dall'agente di merge per l'articolo durante la sessione. Questo valore è Null fino a quando la sessione non è stata completata.|  
|**estimated_changes**|**int**|Stima del numero di modifiche di riga che devono essere applicate all'articolo.|  
|**relative_cost**|**decimal**|Tempo impiegato per l'applicazione delle modifiche per questo articolo in rapporto al tempo totale per l'intera sessione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
