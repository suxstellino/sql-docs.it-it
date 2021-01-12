---
description: MSsubscription_agents (Transact-SQL)
title: MSsubscription_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_agents
- MSsubscription_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_agents system table
ms.assetid: 86ad5891-0bef-4963-9381-7d5b45245a0c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e8569eba1925879f2f757953f36e4ae632292100
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094719"
---
# <a name="mssubscription_agents-transact-sql"></a>MSsubscription_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSsubscription_agents** viene utilizzata da agente di distribuzione e trigger di sottoscrizioni aggiornabili per tenere traccia delle proprietà della sottoscrizione. Questa tabella è archiviata nel database di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID della riga|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> 0 = push<br /><br /> 1 = Pull.<br /><br /> 2 = Pull anonima.|  
|**queue_id**|**sysname**|ID della coda di [!INCLUDE[msCoName](../../includes/msconame-md.md)] messaggi nel server di pubblicazione. *queue_id* è impostato su **SQL** per l'aggiornamento in coda basato su SQL.|  
|**update_mode**|**tinyint**|Tipo di aggiornamento:<br /><br /> **0** = sola lettura.<br /><br /> **1** = aggiornamento immediato.<br /><br /> **2** = aggiornamento in coda tramite Accodamento messaggi.<br /><br /> **3** = aggiornamento immediato con aggiornamento in coda come failover tramite Accodamento messaggi.<br /><br /> **4** = aggiornamento in coda tramite SQL Server coda.<br /><br /> **5** = aggiornamento immediato con failover di aggiornamento in coda, usando SQL Server coda.|  
|**failover_mode**|**bit**|Se è stato specificato un aggiornamento di tipo failover, indica il tipo di failover selezionato:<br /><br /> **0** = è in uso l'aggiornamento immediato. Il failover non è attivato.<br /><br /> **1** = l'aggiornamento in coda è in uso. Il failover è attivato. La coda utilizzata per il failover è specificata nel valore *update_mode* .|  
|**spid**|**int**|ID del processo di sistema per la connessione utilizzata dall'agente di distribuzione appena eseguito o in esecuzione.|  
|**login_time**|**datetime**|Data e ora della connessione dell'agente di distribuzione appena eseguito o in esecuzione.|  
|**allow_subscription_copy**|**bit**|Specifica se consentire o meno la copia del database di sottoscrizione.|  
|**attach_state**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**attach_version**|**binary(16)**|Identificatore univoco che rappresenta la versione di una sottoscrizione allegata.|  
|**last_sync_status**|**int**|Ultimo stato di esecuzione dell'agente di distribuzione appena eseguito o in esecuzione. I possibili valori sono i seguenti.<br /><br /> **1** = avviato.<br /><br /> **2** = operazione completata.<br /><br /> **3** = in corso.<br /><br /> **4** = inattivo.<br /><br /> **5** = nuovo tentativo.<br /><br /> **6** = esito negativo.|  
|**last_sync_summary**|**sysname**|Ultimo messaggio dell'agente di distribuzione appena eseguito o in esecuzione. I possibili valori sono i seguenti.<br /><br /> **Iniziato.**<br /><br /> **Completata.**<br /><br /> **In corso.**<br /><br /> **Inattivo.**<br /><br /> **Tentativi.**<br /><br /> **Esito negativo.**|  
|**last_sync_time**|**datetime**|Data e ora dell'aggiornamento delle colonne *last_sync_summary* e *last_sync_status* . Gli agenti di distribuzione pull o anonimi eseguiti come processi del servizio SQL Server Agent non aggiornano queste colonne. Vengono invece registrate informazioni cronologiche nella tabella di cronologia processo.|  
|**queue_server**|**sysname**|Solo per uso interno.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)  
  
  
