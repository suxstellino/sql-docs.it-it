---
description: MSdistribution_agents (Transact-SQL)
title: MSdistribution_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
author: cawrites
ms.author: chadam
ms.openlocfilehash: a74b3cd9a569f782112ee1feee6400b01c32f00b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160416"
---
# <a name="msdistribution_agents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSdistribution_agents** contiene una riga per ogni agente di distribuzione in esecuzione nel server di distribuzione locale. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'agente di distribuzione.|  
|**nome**|**nvarchar (100)**|Nome dell'agente di distribuzione.|  
|**publisher_database_id**|**int**|ID del database del server di pubblicazione.|  
|**publisher_id**|**smallint**|ID del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**subscriber_id**|**smallint**|ID del Sottoscrittore, utilizzato solo da agenti noti. Per gli agenti anonimi questa colonna è riservata.|  
|**subscriber_db**|**sysname**|Nome del database di sottoscrizione.|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> **0** = push.<br /><br /> **1** = pull.<br /><br /> **2** = anonimo.|  
|**local_job**|**bit**|Indica se nel server di distribuzione locale è presente un processo di SQL Server Agent.|  
|**job_id**|**binary(16)**|Numero di identificazione del processo.|  
|**subscription_guid**|**binary(16)**|ID delle sottoscrizioni dell'agente.|  
|**profile_id**|**int**|ID configurazione dal [MSagent_profiles &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**anonymous_subid**|**uniqueidentifier**|ID di un agente anonimo.|  
|**subscriber_name**|**sysname**|Nome del Sottoscrittore utilizzato solo dagli agenti anonimi.|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**creation_date**|**datetime**|Data e ora in cui è stato creato l'agente di distribuzione o di merge.|  
|**queue_id**|**sysname**|Identificatore per l'individuazione della coda per le sottoscrizioni ad aggiornamento in coda. Per le sottoscrizioni non impostate per l'aggiornamento in coda il valore è NULL. Per le pubblicazioni basate sul servizio di accodamento messaggi [!INCLUDE[msCoName](../../includes/msconame-md.md)], corrisponde a un valore GUID che identifica in modo univoco la coda da utilizzare per la sottoscrizione. Per le pubblicazioni della coda basate su SQL Server, la colonna contiene il valore **SQL**.<br /><br /> Nota: l'utilizzo [!INCLUDE[msCoName](../../includes/msconame-md.md)] di Accodamento messaggi è stato deprecato e non è più supportato.|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|Indica se è possibile attivare l'agente in remoto.<br /><br /> **0** indica che l'agente non può essere attivato in remoto.<br /><br /> **1** specifica che l'agente verrà attivato in remoto e nel computer remoto specificato nella proprietà *offload_server* .|  
|**offload_server**|**sysname**|Nome di rete del server da utilizzare per l'attivazione remota dell'agente.|  
|**dts_package_name**|**sysname**|Nome del pacchetto DTS. Per un pacchetto denominato **DTSPub_Package**, ad esempio, specificare `@dts_package_name = N'DTSPub_Package'` .|  
|**dts_package_password**|**nvarchar (524)**|Password del pacchetto.|  
|**dts_package_location**|**int**|Posizione del pacchetto. Il percorso del pacchetto può essere **Distributor** o **Subscriber**.|  
|**SID**|**varbinary(85)**|ID di sicurezza (SID) dell'agente di distribuzione o di merge durante la prima esecuzione dell'agente.|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|Modalità di sicurezza utilizzata dall'agente per la connessione al Sottoscrittore. I possibili valori sono i seguenti:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server autenticazione<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticazione di Windows.|  
|**subscriber_login**|**sysname**|Account di accesso utilizzato per la connessione al Sottoscrittore.|  
|**subscriber_password**|**nvarchar (524)**|Valore crittografato della password utilizzata per la connessione al Sottoscrittore.|  
|**reset_partial_snapshot_progress**|**bit**|Indica se uno snapshot scaricato parzialmente deve essere scartato in modo da consentire il riavvio dell'intero processo di snapshot.|  
|**job_step_uid**|**uniqueidentifier**|ID univoco del passaggio di processo di SQL Server Agent in cui viene avviato l'agente.|  
|**SubscriptionStreams**|**tinyint**|Imposta il numero di connessioni consentite per ogni agente di distribuzione per l'applicazione parallela di più batch di modifiche in un Sottoscrittore. È supportato un intervallo di valori compreso tra 1 e 64.|  
|**memory_optimized**|**bit**|1 indica che il Sottoscrittore può essere utilizzato per le tabelle con ottimizzazione per la memoria.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
