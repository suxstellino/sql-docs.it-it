---
description: sp_replmonitorhelpmergesession (Transact-SQL)
title: sp_replmonitorhelpmergesession (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpmergesession_TSQL
- sp_replmonitorhelpmergesession
helpviewer_keywords:
- sp_replmonitorhelpmergesession
ms.assetid: a0400ba8-9609-4901-917e-925e119103a1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d81a500eccbc8d969e9e0ef957ae4db09616d768
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211886"
---
# <a name="sp_replmonitorhelpmergesession-transact-sql"></a>sp_replmonitorhelpmergesession (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sulle sessioni passate per un agente di merge specifico. Viene restituita una riga per ogni sessione che soddisfa i criteri del filtro. Questa stored procedure, utilizzata per il monitoraggio della replica di tipo merge, viene eseguita nel database di distribuzione del server di distribuzione o nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replmonitorhelpmergesession [ [ @agent_name = ] 'agent_name' ]  
    [ , [ @hours = ] hours ]  
    [ , [ @session_type = ] session_type ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @agent_name = ] 'agent_name'` Nome dell'agente. *agent_name* è di **tipo nvarchar (100)** e non prevede alcun valore predefinito.  
  
`[ @hours = ] hours` Intervallo di tempo, in ore, per cui vengono restituite le informazioni sulla sessione dell'agente cronologico. *hours* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|< **0**|Restituisce informazioni sulle esecuzioni passate dell'agente, per al massimo 100 esecuzioni.|  
|**0** (predefinito)|Restituisce informazioni su tutte le esecuzioni passate dell'agente.|  
|> **0**|Restituisce informazioni sulle esecuzioni di agenti che si sono verificate nel numero di ore dell'ultima *ora* .|  
  
`[ @session_type = ] session_type` Filtra il set di risultati in base al risultato finale della sessione. *session_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1** (impostazione predefinita)|Sessioni dell'agente con esito positivo o da ritentare.|  
|**0**|Sessioni dell'agente con esito negativo.|  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene utilizzato per l'esecuzione di **sp_replmonitorhelpmergesession** nel Sottoscrittore.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene utilizzato per l'esecuzione di **sp_replmonitorhelpmergesession** nel Sottoscrittore.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene utilizzato per l'esecuzione di **sp_replmonitorhelpmergesession** nel Sottoscrittore.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Session_id**|**int**|ID della sessione del processo dell'agente.|  
|**Status**|**int**|Stato dell'esecuzione dell'agente:<br /><br /> **1** = avvio<br /><br /> **2** = esito positivo<br /><br /> **3** = in corso<br /><br /> **4** = inattivo<br /><br /> **5** = nuovo tentativo<br /><br /> **6** = esito negativo|  
|**StartTime**|**datetime**|Data e ora di inizio della sessione del processo dell'agente.|  
|**EndTime**|**datetime**|Data e ora di completamento della sessione del processo dell'agente.|  
|**Duration**|**int**|Durata cumulativa, espressa in secondi, della sessione del processo.|  
|**UploadedCommands**|**int**|Numero di comandi caricati durante la sessione dell'agente.|  
|**DownloadedCommands**|**int**|Numero di comandi scaricati durante la sessione dell'agente.|  
|**ErrorMessages**|**int**|Numero di messaggi di errore generati durante la sessione dell'agente.|  
|**ErrorID**|**int**|ID dell'errore che si è verificato|  
|**PercentageDone**|**decimal**|Percentuale stimata del numero totale di modifiche già recapitate in una sessione attiva.|  
|**TimeRemaining**|**int**|Numero stimato di secondi rimanenti in una sessione attiva.|  
|**CurrentPhase**|**int**|Fase corrente di una sessione attiva. I possibili valori sono i seguenti.<br /><br /> **1** = caricamento<br /><br /> **2** = download|  
|**LastMessage**|**nvarchar (500)**|Ultimo messaggio registrato dall'agente di merge durante la sessione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replmonitorhelpmergesession** viene utilizzato per monitorare la replica di tipo merge.  
  
 Quando viene eseguito nel Sottoscrittore, **sp_replmonitorhelpmergesession** restituisce solo le informazioni sulle ultime cinque sessioni di agente di merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del database **db_owner** o **replmonitor** nel database di distribuzione nel server di distribuzione o nel database di sottoscrizione nel Sottoscrittore possono eseguire **sp_replmonitorhelpmergesession**.  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di programmazione](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
