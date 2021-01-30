---
description: MSqreader_history (Transact-SQL)
title: MSqreader_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSqreader_history
- MSqreader_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSqreader_history system table
ms.assetid: c5c91d39-513c-4a77-870b-c8ef74a1cd6b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7c158b9c035f14f7904a828c7a3628494096ddb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182600"
---
# <a name="msqreader_history-transact-sql"></a>MSqreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSqreader_history** contiene righe di cronologia per gli agenti di lettura coda associati al server di distribuzione locale. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID dell'agente di lettura coda.|  
|**publication_id**|**int**|ID della pubblicazione.|  
|**runstatus**|**int**|Stato di esecuzione dell'agente:<br /><br /> **1** = avvia.<br /><br /> **2** = esito positivo.<br /><br /> **3** = in corso.<br /><br /> **4** = inattivo.<br /><br /> **5** = nuovo tentativo.<br /><br /> **6** = esito negativo.|  
|**start_time**|**datetime**|Data e ora di inizio della sessione dell'agente.|  
|**time**|**datetime**|Data e ora dell'ultimo messaggio registrato.|  
|**duration**|**int**|Tempo trascorso, espresso in secondi, delle attività della sessione registrate.|  
|**Commenti**|**nvarchar(255)**|Testo descrittivo.|  
|**transaction_id**|**nvarchar(40)**|ID della transazione archiviato insieme al messaggio, se applicabile.|  
|**transaction_status**|**int**|Stato della transazione.|  
|**transactions_processed**|**int**|Numero totale di transazioni elaborate durante la sessione.|  
|**commands_processed**|**int**|Numero totale di comandi elaborati durante la sessione.|  
|**delivery_rate**|**float(53)**|Numero medio di comandi recapitati al secondo.|  
|**transaction_rate**|**float(53)**|Velocità delle transazioni elaborate.|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore.|  
|**SubscriberDB**|**sysname**|Nome del database di sottoscrizione.|  
|**error_id**|**int**|Se diverso da zero, il numero rappresenta un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] messaggio di errore.|  
|**timestamp**|**timestamp**|Colonna di tipo timestamp della tabella.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
