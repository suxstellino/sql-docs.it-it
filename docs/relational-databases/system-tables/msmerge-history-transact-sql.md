---
description: MSmerge_history (Transact-SQL)
title: MSmerge_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_history_TSQL
- MSmerge_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_history system table
ms.assetid: 936195ad-ca07-41a8-a1a0-6699b6e63403
author: cawrites
ms.author: chadam
ms.openlocfilehash: 618a12dd664136e212bc6f31b9b59c1b87d413d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203630"
---
# <a name="msmerge_history-transact-sql"></a>MSmerge_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_history** contiene righe di cronologia con descrizioni dettagliate dei risultati delle sessioni di processo agente di merge precedenti. Questa tabella contiene una riga per ogni riga dell'output dell'agente. Questa tabella viene utilizzata nel database di distribuzione e in ogni database di sottoscrizione. Nel database di distribuzione contiene la cronologia per tutte le pubblicazioni di tipo merge e le sottoscrizioni che utilizzano il server di distribuzione. In ogni database di sottoscrizione contiene la cronologia per le pubblicazioni sottoscritte dal Sottoscrittore.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID del processo dell'agente di merge.|  
|**agent_id**|**int**|ID dell'agente di merge.|  
|**Commenti**|**nvarchar(255)**|Testo del messaggio.|  
|**error_id**|**int**|ID di un errore nella tabella di sistema [MSrepl_errors](../../relational-databases/system-tables/msrepl-errors-transact-sql.md) .|  
|**timestamp**|**timestamp**|Colonna timestamp della tabella.|  
|**updatable_row**|**bit**|Impostare su **1** se la riga della cronologia può essere sovrascritta.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
