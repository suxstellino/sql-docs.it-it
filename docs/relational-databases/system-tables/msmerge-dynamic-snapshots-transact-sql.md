---
description: MSmerge_dynamic_snapshots (Transact-SQL)
title: MSmerge_dynamic_snapshots (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_dynamic_snapshots_TSQL
- MSmerge_dynamic_snapshots
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_dynamic_snapshots system table
ms.assetid: a5592b3c-731b-4fc9-ae4b-2602ed78248e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0bea9259e6218d32ae0e278046493aaeeafeaa41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181267"
---
# <a name="msmerge_dynamic_snapshots-transact-sql"></a>MSmerge_dynamic_snapshots (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_dynamic_snapshots** tiene traccia del percorso dello snapshot dei dati filtrati per ogni partizione definita per una pubblicazione di tipo merge con filtri di riga con parametri. Questa tabella è archiviata nel database di **pubblicazione** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**partition_id**|**int**|ID della partizione di tipo merge.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Percorso dello snapshot dei dati filtrati per la partizione.|  
|**last_updated**|**datetime**|Data di aggiornamento dello snapshot dei dati filtrati.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
