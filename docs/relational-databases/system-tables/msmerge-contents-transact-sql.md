---
description: MSmerge_contents (Transact-SQL)
title: MSmerge_contents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_contents
- MSmerge_contents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_contents system table
ms.assetid: 8d68a61a-683f-4b20-92f9-c0a8d9ba0ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 78d7fb9732013970956554e932b3d4804d461df9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205056"
---
# <a name="msmerge_contents-transact-sql"></a>MSmerge_contents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_contents** contiene una riga per ogni riga modificata nel database corrente dal momento in cui è stata pubblicata. Questa tabella viene utilizzata durante il processo di unione per determinare le righe modificate. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Nome alternativo della tabella pubblicata.|  
|**rowguid**|**uniqueidentifier**|Identificatore della riga specificata.|  
|**generazione**|**bigint**|Generazione della riga identificata da **tablenick** e **rowguid**.|  
|**partchangegen**|**bigint**|Generazione associata all'ultima modifica dei dati che potrebbe aver modificato l'appartenenza della riga a una pubblicazione filtrata.|  
|**derivazione**|**varbinary (501)**|Coppie di nome alternativo del Sottoscrittore e numero di versione utilizzate per memorizzare una cronologia delle modifiche apportate alla riga.|  
|**colvl**|**varbinary (7489)**|Informazioni sulla versione della colonna.|  
|**marcatore**|**uniqueidentifier**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identifica la riga padre di primo livello nel **MSmerge_contents** (per **rowguid**) per ogni riga figlio corrispondente in un record logico.|  
|**logical_record_lineage**|**varbinary (501)**|Coppie di nome alternativo del Sottoscrittore e numero di versione utilizzate per memorizzare una cronologia delle modifiche apportate alla riga padre di livello principale in un record logico. Per tutte le righe figlio in un record logico, questo valore è NULL.|  
|**logical_relation_change_gen**|**bigint**|Valore di generazione associato all'ultima modifica che ha comportato il riallineamento nel record logico, in cui una riga esistente è stata inserita o eliminata.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
