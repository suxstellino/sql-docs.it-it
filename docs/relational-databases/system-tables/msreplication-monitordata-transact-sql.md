---
description: MSreplication_monitordata (Transact-SQL)
title: MSreplication_monitordata (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_monitordata_TSQL
- MSreplication_monitordata
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_monitordata system table
ms.assetid: 843d3ffd-a1ef-4fd5-a744-c2252199793e
author: cawrites
ms.author: chadam
ms.openlocfilehash: bbe408296895f593de0af1da852b807082e8e7cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098161"
---
# <a name="msreplication_monitordata-transact-sql"></a>MSreplication_monitordata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSreplication_monitordata** contiene i dati memorizzati nella cache utilizzati da monitoraggio replica, con una riga per ogni sottoscrizione monitorata. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**lastrefresh**|**datetime**|Data e ora dell'ultimo aggiornamento dei dati di monitoraggio.|  
|**computetime**|**int**|Tempo, espresso in secondi, necessario per calcolare i dati di monitoraggio.|  
|**publication_id**|**int**|ID della pubblicazione.|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_srvid**|**int**|ID del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione. I possibili valori sono i seguenti.<br /><br /> **0** = pubblicazione transazionale<br /><br /> **1** = pubblicazione snapshot<br /><br /> **2** = pubblicazione di tipo merge|  
|**agent_type**|**int**|Tipo di agente di replica. I possibili valori sono i seguenti.<br /><br /> **1** = agente di snapshot<br /><br /> **2** = agente di lettura log<br /><br /> **3** = agente di distribuzione<br /><br /> **4** = agente di merge<br /><br /> **9** = agente di lettura coda|  
|**agent_id**|**int**|ID dell'agente di replica.|  
|**agent_name**|**sysname**|Nome del processo dell'agente di replica.|  
|**job_id**|**uniqueidentifier**|GUID del processo dell'agente di replica.|  
|**Stato**|**int**|Stato dell'agente di replica. I possibili valori sono i seguenti.<br /><br /> **1** = avviato<br /><br /> **2** = operazione completata<br /><br /> **3** = in corso<br /><br /> **4** = inattivo<br /><br /> **5** = nuovo tentativo<br /><br /> **6** = operazione non riuscita|  
|**isagentrunningnow**|**bit**|Flag che indica se il processo dell'agente è attualmente in esecuzione, dove il valore **1** indica che il processo è in esecuzione.|  
|**warning**|**int**|Avviso di soglia generato da una sottoscrizione, che può corrispondere al risultato dell'applicazione dell'operatore OR logico a uno o più dei valori seguenti.<br /><br /> **1** = scadenza: una sottoscrizione di una pubblicazione transazionale ha superato il periodo di memorizzazione superiore alla soglia consentita, come percentuale del periodo di memorizzazione.<br /><br /> **2** = latenza: il tempo impiegato per replicare i dati da un server di pubblicazione transazionale al Sottoscrittore supera la soglia, in secondi.<br /><br /> **4** = mergeexpiration-una sottoscrizione di una pubblicazione di tipo merge ha superato il periodo di memorizzazione superiore alla soglia consentita, come percentuale del periodo di memorizzazione. 8 = mergefastrunduration - è stata superata la soglia espressa in secondi relativa al tempo necessario per completare la sincronizzazione di una sottoscrizione di tipo merge tramite una connessione di rete veloce.<br /><br /> **16** = mergeslowrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete lenta o remota.<br /><br /> **32** = mergefastrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete veloce.<br /><br /> **64** = mergeslowrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete lenta o remota.|  
|**last_distsync**|**datetime**|Data e ora dell'ultima esecuzione dell'agente di distribuzione.|  
|**agentstoptime**|**datetime**|Data e ora di arresto dell'agente.|  
|**distdb**|**sysname**|Nome del database di distribuzione per la sottoscrizione.|  
|**conservazione**|**int**|Periodo di memorizzazione della pubblicazione.|  
|**time_stamp**|**datetime**|Solo per uso interno.|  
|**worst_latency**|**int**|Latenza più alta, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**best_latency**|**int**|Latenza più bassa, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**avg_latency**|**int**|Latenza media, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**cur_latency**|**int**|Latenza, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione durante l'esecuzione corrente.|  
|**worst_runspeedPerf**|**int**|Tempo di sincronizzazione più lungo per la pubblicazione di tipo merge.|  
|**best_runspeedPerf**|**int**|Tempo di sincronizzazione più breve per la pubblicazione di tipo merge.|  
|**average_runspeedPerf**|**int**|Media del tempo di sincronizzazione per la pubblicazione di tipo merge.|  
|**mergePerformance**|**int**|Prestazioni dell'ultima sincronizzazione confrontate con tutte le sincronizzazioni per la sottoscrizione, ottenute dividendo la velocità di recapito dell'ultima sincronizzazione per la media di tutte le velocità di recapito precedenti.|  
|**mergelatestsessionrunduration**|**int**|Durata dell'esecuzione più recente dell'agente di merge.|  
|**mergelatestsessionrunspeed**|**float(53)**|Frequenza di recapito dell'esecuzione più recente dell'agente di merge.|  
|**mergelatestsessionconnectiontype**|**int**|Connessione utilizzata per la sessione più recente dell'agente di merge. I possibili valori sono i seguenti.<br /><br /> **1** = rete locale (LAN)<br /><br /> **2** = connessione di rete remota|  
|**retention_period_unit**|**tinyint**|Definisce l'unità utilizzata per la definizione dell'opzione retention. I possibili valori sono i seguenti.<br /><br /> **1** = Settimana<br /><br /> **2** = Mese<br /><br /> **3** = Anno|  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di codice](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replmonitorhelpsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md)   
 [sp_replmonitorhelppublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md)   
 [sp_replmonitorhelppublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md)   
 [sp_replmonitorhelpmergesession &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md)   
 [sp_replmonitorhelppublicationthresholds &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md)   
 [sp_replmonitorhelpmergesessiondetail &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md)  
  
  
