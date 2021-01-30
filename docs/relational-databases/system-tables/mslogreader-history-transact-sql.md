---
description: MSlogreader_history (Transact-SQL)
title: MSlogreader_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSlogreader_history_TSQL
- MSlogreader_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSlogreader_history system table
ms.assetid: 2e399fa1-3591-4c1c-96b7-7964fe82c7c4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71963149d6ad8be972acd6a2da3380212eb8548a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212306"
---
# <a name="mslogreader_history-transact-sql"></a>MSlogreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSlogreader_history** contiene righe di cronologia per gli agenti di lettura log associati al server di distribuzione locale. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID dell'agente di lettura log.|  
|**runstatus**|**int**|Stato di esecuzione:<br /><br /> 1 = In fase di avvio.<br /><br /> 2 = Esito positivo.<br /><br /> 3 = Operazione in corso.<br /><br /> 4 = Inattivo.<br /><br /> 5 = Nuovo tentativo.<br /><br /> 6 = Esito negativo.|  
|**start_time**|**datetime**|Ora di inizio dell'esecuzione del processo.|  
|**time**|**datetime**|Ora di registrazione del messaggio.|  
|**duration**|**int**|Durata espressa in secondi della sessione del messaggio.|  
|**Commenti**|**nvarchar(255)**|Testo del messaggio.|  
|**xact_seqno**|**varbinary(16)**|Numero di sequenza dell'ultima transazione elaborata.|  
|**delivery_time**|**int**|Ora in cui è stata recapitata la prima transazione.|  
|**delivered_transactions**|**int**|Numero totale di transazioni recapitate durante la sessione.|  
|**delivered_commands**|**int**|Numero totale di comandi recapitati durante la sessione.|  
|**average_commands**|**int**|Numero medio di comandi recapitati durante la sessione.|  
|**delivery_rate**|**float**|Numero medio dei comandi recapitati al secondo.|  
|**delivery_latency**|**int**|Latenza tra l'immissione del commando nel database pubblicato e l'immissione del comando nel database di distribuzione. In millisecondi.|  
|**error_id**|**int**|ID dell'errore nella tabella di sistema **Msrepl_error** .|  
|**timestamp**|**timestamp**|Colonna timestamp della tabella.|  
|**updateable_row**|**bit**|Impostare su **1** se la riga della cronologia può essere sovrascritta.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
