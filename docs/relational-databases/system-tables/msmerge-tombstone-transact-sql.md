---
description: MSmerge_tombstone (Transact-SQL)
title: MSmerge_tombstone (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_tombstone_TSQL
- MSmerge_tombstone
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_tombstone system table
ms.assetid: 8b3fc7bf-729b-40f2-8a26-e7dfbe8ddb38
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b8f54ce66145ddc298bb44594c48f74ddd62be0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098600"
---
# <a name="msmerge_tombstone-transact-sql"></a>MSmerge_tombstone (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_tombstone** contiene informazioni sulle righe eliminate e consente la propagazione delle eliminazioni ad altri Sottoscrittori. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**rowguid**|**uniqueidentifier**|Identificatore di riga.|  
|**tablenick**|**int**|Nome alternativo della tabella.|  
|**type**|**tinyint**|Tipo di eliminazione:<br /><br /> 1 = Eliminazione eseguita dall'utente.<br /><br /> 5 = La riga non appartiene più alla partizione filtrata.<br /><br /> 6 = Eliminazione del sistema.|  
|**derivazione**|**varbinary (249)**|Indica la versione del record eliminato e gli aggiornamenti noti al momento dell'eliminazione. Consente di utilizzare le regole per la risoluzione coerente di un conflitto quando un Sottoscrittore esegue l'aggiornamento di una riga mentre questa viene eliminata in un altro Sottoscrittore.|  
|**generazione**|**int**|Viene assegnato quando si elimina una riga. Se un Sottoscrittore richiede la generazione N, verranno inviate solo le lapidi con la generazione >= N.|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identificatore del record logico al quale appartiene una riga eliminata.|  
|**logical_record_lineage**|**Varbinary (501)**|Coppie di nome alternativo del Sottoscrittore e numero di versione utilizzate per memorizzare la cronologia delle eliminazioni del record logico al quale appartiene la riga corrente.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
