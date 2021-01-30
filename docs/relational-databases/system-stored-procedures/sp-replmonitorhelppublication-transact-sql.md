---
description: sp_replmonitorhelppublication (Transact-SQL)
title: sp_replmonitorhelppublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublication_TSQL
- sp_replmonitorhelppublication
helpviewer_keywords:
- sp_replmonitorhelppublication
ms.assetid: 7928c50c-617f-41c5-9e0f-4e42e8be55dc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 70f401f52926fc389232b82167a94f9a4ded1e0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193751"
---
# <a name="sp_replmonitorhelppublication-transact-sql"></a>sp_replmonitorhelppublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sullo stato corrente per una o più pubblicazioni nel server di pubblicazione. Questa stored procedure, utilizzata per il monitoraggio della replica, viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replmonitorhelppublication [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db'   
    [ , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione di cui viene monitorato lo stato. *Publisher* è di **tipo sysname** e il valore predefinito è null. Se **null**, verranno restituite informazioni per tutti i server di pubblicazione che utilizzano il server di distribuzione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database pubblicato. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Se NULL, vengono restituite informazioni su tutti i database pubblicati nel server di pubblicazione.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione da monitorare. *Publication* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publication_type = ] publication_type` Se il tipo di pubblicazione. *publication_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Pubblicazione transazionale.|  
|**1**|Pubblicazione snapshot.|  
|**2**|Pubblicazione di tipo merge.|  
|NULL (predefinito)|La replica cerca di determinare il tipo di pubblicazione.|  
  
`[ @refreshpolicy = ] refreshpolicy` Solo per uso interno.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisher_db**|**sysname**|Nome del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome di una pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione. I possibili valori sono i seguenti.<br /><br /> **0** = pubblicazione transazionale<br /><br /> **1** = pubblicazione snapshot<br /><br /> **2** = pubblicazione di tipo merge|  
|**Stato**|**int**|Stato massimo di tutti gli agenti di replica associati alla pubblicazione. I possibili valori sono i seguenti.<br /><br /> **1** = avviato<br /><br /> **2** = operazione completata<br /><br /> **3** = in corso<br /><br /> **4** = inattivo<br /><br /> **5** = nuovo tentativo<br /><br /> **6** = operazione non riuscita|  
|**warning**|**int**|Avviso correlato alla soglia massima generato da una sottoscrizione appartenente alla pubblicazione. Può essere il risultato OR logico di uno o più dei valori seguenti.<br /><br /> **1** = scadenza: una sottoscrizione di una pubblicazione transazionale non è stata sincronizzata entro la soglia del periodo di memorizzazione.<br /><br /> **2** = latenza: il tempo impiegato per replicare i dati da un server di pubblicazione transazionale al Sottoscrittore supera la soglia, in secondi.<br /><br /> **4** = mergeexpiration-una sottoscrizione di una pubblicazione di tipo merge non è stata sincronizzata entro la soglia del periodo di memorizzazione.<br /><br /> **8** = mergefastrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete veloce.<br /><br /> **16** = mergeslowrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete lenta o remota.<br /><br /> **32** = mergefastrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete veloce.<br /><br /> **64** = mergeslowrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete lenta o remota.|  
|**worst_latency**|**int**|Latenza più alta, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**best_latency**|**int**|Latenza più bassa, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**average_latency**|**int**|Latenza media, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**last_distsync**|**datetime**|Data e ora dell'ultima esecuzione dell'agente di distribuzione.|  
|**conservazione**|**int**|Periodo di memorizzazione della pubblicazione.|  
|**latencythreshold**|**int**|Soglia della latenza impostato per la pubblicazione transazionale.|  
|**expirationthreshold**|**int**|Soglia della scadenza impostato per la pubblicazione di tipo merge.|  
|**agentnotrunningthreshold**|**int**|Soglia impostato per il periodo più lungo di mancata esecuzione di un agente.|  
|**subscriptioncount**|**int**|Numero di sottoscrizioni a una pubblicazione.|  
|**runningdistagentcount**|**int**|Numero di agenti di distribuzione in esecuzione per la pubblicazione.|  
|**snapshot_agentname**|**sysname**|Nome del processo dell'agente snapshot per la pubblicazione.|  
|**logreader_agentname**|**sysname**|Nome del processo dell'agente di lettura log per la pubblicazione transazionale.|  
|**qreader_agentname**|**sysname**|Nome del processo dell'agente di lettura coda per una pubblicazione transazionale che supporta l'aggiornamento in coda.|  
|**worst_runspeedPerf**|**int**|Tempo di sincronizzazione più lungo per la pubblicazione di tipo merge.|  
|**best_runspeedPerf**|**int**|Tempo minimo di sincronizzazione per la pubblicazione di tipo merge.|  
|**average_runspeedPerf**|**int**|Tempo medio di sincronizzazione per la pubblicazione di tipo merge.|  
|**retention_period_unit**|**int**|Unità utilizzata per esprimere la *conservazione*.|  
|**pubblicazione**|**sysname**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite cui viene effettuata la pubblicazione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replmonitorhelppublication** viene utilizzato con tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del database **db_owner** o **replmonitor** nel database di distribuzione possono eseguire **sp_replmonitorhelppublication**.  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di programmazione](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
