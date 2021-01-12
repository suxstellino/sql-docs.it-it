---
description: MSsnapshot_agents (Transact-SQL)
title: MSsnapshot_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsnapshot_agents
- MSsnapshot_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsnapshot_agents system table
ms.assetid: aeae0a2e-4c21-4c45-be65-1e426fa52bdd
author: cawrites
ms.author: chadam
ms.openlocfilehash: bcd51354cab293afcdce2845fef5bd6e51e1c70f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096185"
---
# <a name="mssnapshot_agents-transact-sql"></a>MSsnapshot_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSsnapshot_agents** contiene una riga per ogni agente di snapshot associata al server di distribuzione locale. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'agente snapshot.|  
|**nome**|**nvarchar (100)**|Nome dell'agente snapshot.|  
|**publisher_id**|**smallint**|ID del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione:<br /><br /> **0** = transazionale.<br /><br /> **1** = snapshot.<br /><br /> **2** = Unione.|  
|**local_job**|**bit**|Specifica se nel server di distribuzione locale è presente un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|  
|**job_id**|**binary(16)**|Numero di identificazione del processo.|  
|**profile_id**|**int**|ID configurazione dal [MSagent_profiles &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**dynamic_filter_login**|**sysname**|Valore utilizzato per la valutazione della [SUSER_SNAME &#40;funzione&#41;Transact-SQL ](../../t-sql/functions/suser-sname-transact-sql.md) nei filtri con parametri che definiscono una partizione. Questa colonna viene utilizzata per uno snapshot partizionato.|  
|**dynamic_filter_hostname**|**sysname**|Valore utilizzato per la valutazione della [HOST_NAME &#40;funzione&#41;Transact-SQL ](../../t-sql/functions/host-name-transact-sql.md) nei filtri con parametri che definiscono una partizione. Questa colonna viene utilizzata per uno snapshot partizionato.|  
|**publisher_security_mode**|**smallint**|Modalità di sicurezza utilizzata dall'agente per la connessione al server di pubblicazione. I possibili valori sono i seguenti:<br /><br />   =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticazione di Windows.|  
|**publisher_login**|**sysname**|Account di accesso utilizzato per la connessione al server di pubblicazione.|  
|**publisher_password**|**nvarchar (524)**|Valore crittografato della password utilizzata per la connessione al server di pubblicazione.|  
|**job_step_uid**|**uniqueidentifier**|ID univoco del passaggio di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in cui viene avviato l'agente.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
