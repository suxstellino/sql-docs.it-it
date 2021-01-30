---
description: sp_replmonitorhelpsubscription (Transact-SQL)
title: sp_replmonitorhelpsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpsubscription_TSQL
- sp_replmonitorhelpsubscription
helpviewer_keywords:
- sp_replmonitorhelpsubscription
ms.assetid: a681b2db-c82d-4624-a10c-396afb0ac42f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 898104516115207bd0cee45bf05f187922c13f6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204372"
---
# <a name="sp_replmonitorhelpsubscription-transact-sql"></a>sp_replmonitorhelpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sullo stato corrente delle sottoscrizioni appartenenti a una o più pubblicazioni nel server di pubblicazione e restituisce una riga per ogni sottoscrizione restituita. Questa stored procedure, utilizzata per il monitoraggio della replica, viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replmonitorhelpsubscription [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @mode = ] mode ]  
    [ , [ @topnum = ] topnum ]   
    [ , [ @exclude_anonymous = ] exclude_anonymous ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione di cui viene monitorato lo stato. *Publisher* è di **tipo sysname** e il valore predefinito è null. Se **null**, vengono restituite informazioni per tutti i server di pubblicazione che utilizzano il server di distribuzione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database pubblicato. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Se NULL, vengono restituite informazioni su tutti i database pubblicati nel server di pubblicazione.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione da monitorare. *Publication* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publication_type = ] publication_type` Se il tipo di pubblicazione. *publication_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Pubblicazione transazionale.|  
|**1**|Pubblicazione snapshot.|  
|**2**|Pubblicazione di tipo merge.|  
|NULL (predefinito)|La replica tenta di determinare il tipo di pubblicazione.|  
  
`[ @mode = ] mode` Modalità di filtro da utilizzare per la restituzione delle informazioni di monitoraggio delle sottoscrizioni. *mode* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|Restituisce tutte le sottoscrizioni.|  
|**1**|Restituisce solo le sottoscrizioni con errori.|  
|**2**|Restituisce solo le sottoscrizioni che hanno generato avvisi correlati alla misurazione del valore soglia.|  
|**3**|Restituisce solo le sottoscrizioni con errori o che hanno generato avvisi correlati alla misurazione del valore soglia.|  
|**4**|Restituisce le prime 25 sottoscrizioni con prestazioni peggiori.|  
|**5**|Restituisce le 50 sottoscrizioni con le prestazioni peggiori.|  
|**6**|Restituisce solo le sottoscrizioni in fase di sincronizzazione.|  
|**7**|Restituisce solo le sottoscrizioni che non sono in fase di sincronizzazione.|  
  
`[ @topnum = ] topnum` Limita il set di risultati solo al numero specificato di sottoscrizioni all'inizio dei dati restituiti. *topnum* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @exclude_anonymous = ] exclude_anonymous` Indica se le sottoscrizioni pull anonime vengono escluse dal set di risultati. *exclude_anonymous* è di **bit** e il valore predefinito è **0**. il valore **1** indica che le sottoscrizioni anonime vengono escluse e il valore **0** indica che sono incluse.  
  
`[ @refreshpolicy = ] refreshpolicy` Solo per uso interno.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Stato**|**int**|Analizza lo stato di tutti gli agenti di replica associati alla pubblicazione e restituisce lo stato più elevato trovato nell'ordine seguente:<br /><br /> **6** = operazione non riuscita<br /><br /> **5** = nuovo tentativo<br /><br /> **2** = arrestato<br /><br /> **4** = inattivo<br /><br /> **3** = in corso<br /><br /> **1** = avviato|  
|**warning**|**int**|Avviso correlato alla soglia massima generato da una sottoscrizione appartenente alla pubblicazione. Può essere il risultato OR logico di uno o più dei valori seguenti.<br /><br /> **1** = scadenza: una sottoscrizione di una pubblicazione transazionale non è stata sincronizzata entro la soglia del periodo di memorizzazione.<br /><br /> **2** = latenza: il tempo impiegato per replicare i dati da un server di pubblicazione transazionale al Sottoscrittore supera la soglia, in secondi.<br /><br /> **4** = mergeexpiration-una sottoscrizione di una pubblicazione di tipo merge non è stata sincronizzata entro la soglia del periodo di memorizzazione.<br /><br /> **8** = mergefastrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete veloce.<br /><br /> **16** = mergeslowrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete lenta o remota.<br /><br /> **32** = mergefastrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete veloce.<br /><br /> **64** = mergeslowrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete lenta o remota.|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore.|  
|**subscriber_db**|**sysname**|Nome del database utilizzato per la sottoscrizione.|  
|**publisher_db**|**sysname**|Nome del database di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome di una pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione. I possibili valori sono i seguenti:<br /><br /> **0** = pubblicazione transazionale<br /><br /> **1** = pubblicazione snapshot<br /><br /> **2** = pubblicazione di tipo merge|  
|**sottotipo**|**int**|Tipo di sottoscrizione. I possibili valori sono i seguenti:<br /><br /> **0** = push<br /><br /> **1** = pull<br /><br /> **2** = Anonimo|  
|**latenza**|**int**|Latenza più alta, espressa in secondi, per le modifiche dei dati propagate dall'agente di lettura log o dagli agenti di distribuzione per una pubblicazione transazionale.|  
|**latencythreshold**|**int**|Latenza massima per una pubblicazione transazionale, superata la quale viene generato un avviso.|  
|**agentnotrunning**|**int**|Indica da quante ore l'agente non viene eseguito.|  
|**agentnotrunningthreshold**|**int**|Indica dopo quante ore di mancata esecuzione dell'agente viene generato un avviso.|  
|**timetoexpiration**|**int**|Indica il numero di ore che mancano alla scadenza della sottoscrizione, se questa non viene sincronizzata.|  
|**expirationthreshold**|**int**|Indica quante ore prima della scadenza della sottoscrizione deve essere generato un avviso.|  
|**last_distsync**|**datetime**|Data e ora dell'ultima esecuzione dell'agente di distribuzione.|  
|**distribution_agentname**|**sysname**|Nome del processo dell'agente di distribuzione per la sottoscrizione a una pubblicazione transazionale.|  
|**mergeagentname**|**sysname**|Nome del processo dell'agente di merge per la sottoscrizione di una pubblicazione di tipo merge.|  
|**mergesubscriptionfriendlyname**|**sysname**|Nome descrittivo della sottoscrizione.|  
|**mergeagentlocation**|**sysname**|Nome del server in cui viene eseguito l'agente di merge.|  
|**mergeconnectiontype**|**int**|Connessione utilizzata per la sincronizzazione di una sottoscrizione di una pubblicazione di tipo merge. I possibili valori sono i seguenti:<br /><br /> **1** = rete locale (LAN)<br /><br /> **2** = connessione di rete remota<br /><br /> **3** = sincronizzazione Web.|  
|**mergePerformance**|**int**|Prestazioni dell'ultima sincronizzazione confrontate con tutte le sincronizzazioni per la sottoscrizione, ottenute dividendo la velocità di recapito dell'ultima sincronizzazione per la media di tutte le velocità di recapito precedenti.|  
|**mergerunspeed**|**float**|Velocità di recapito dell'ultima sincronizzazione per la sottoscrizione.|  
|**mergerunduration**|**int**|Tempo necessario per completare l'ultima sincronizzazione della sottoscrizione.|  
|**monitorranking**|**int**|Valore di rango utilizzato per ordinare le sottoscrizioni nel set di risultati. I possibili valori sono i seguenti:<br /><br /> Per una pubblicazione transazionale:<br /><br /> **60** = errore<br /><br /> **56** = avviso: prestazioni critiche<br /><br /> **52** = avviso: scadenza imminente o scaduta<br /><br /> **50** = avviso: sottoscrizione non inizializzata<br /><br /> **40** = nuovo tentativo non riuscito<br /><br /> **30** = non in esecuzione (esito positivo)<br /><br /> **20** = in esecuzione (avvio, in esecuzione o inattivo)<br /><br /> Per una pubblicazione di tipo merge:<br /><br /> **60** = errore<br /><br /> **56** = avviso: prestazioni critiche<br /><br /> **54** = avviso: merge con esecuzione prolungata<br /><br /> **52** = avviso: scadenza imminente<br /><br /> **50** = avviso: sottoscrizione non inizializzata<br /><br /> **40** = nuovo tentativo non riuscito<br /><br /> **30** = in esecuzione (avvio, in esecuzione o inattivo)<br /><br /> **20** = non in esecuzione (esito positivo)|  
|**distributionagentjobid**|**binary(16)**|ID del processo dell'agente di distribuzione per le sottoscrizioni di una pubblicazione transazionale.|  
|**mergeagentjobid**|**binary(16)**|ID del processo dell'agente di merge per le sottoscrizioni di una pubblicazione di tipo merge.|  
|**distributionagentid**|**int**|ID del processo dell'agente di distribuzione per la sottoscrizione.|  
|**distributionagentprofileid**|**int**|ID del profilo dell'agente utilizzato dall'agente di distribuzione.|  
|**mergeagentid**|**int**|ID del processo dell'agente di merge per la sottoscrizione.|  
|**mergeagentprofileid**|**int**|ID del profilo dell'agente utilizzato dall'agente di merge.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replmonitorhelpsubscription** viene utilizzato con tutti i tipi di replica.  
  
 **sp_replmonitorhelpsubscription** Ordina il set di risultati in base alla gravità dello stato della sottoscrizione, determinato dal valore di *monitorranking*. Le righe di tutte le sottoscrizioni in errore, ad esempio, vengono ordinate prima delle righe delle sottoscrizioni con avviso.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del database **db_owner** o **replmonitor** nel database di distribuzione possono eseguire **sp_replmonitorhelpsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di programmazione](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
