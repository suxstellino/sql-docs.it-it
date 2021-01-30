---
description: sp_help_jobactivity (Transact-SQL)
title: sp_help_jobactivity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobactivity_TSQL
- sp_help_jobactivity
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobactivity
ms.assetid: d344864f-b4d3-46b1-8933-b81dec71f511
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b399b08120b8153af01d0faab42633f5a8325142
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199644"
---
# <a name="sp_help_jobactivity-transact-sql"></a>sp_help_jobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elenca le informazioni sullo stato di run-time dei processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobactivity { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @session_id = ] session_id ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @session_id = ] session_id` ID della sessione per cui segnalare le informazioni. *session_id* è di **tipo int** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Restituisce il set di risultati seguente:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Numero di identificazione della sessione dell'agente.|  
|**job_id**|**uniqueidentifier**|Identificatore del processo.|  
|**job_name**|**sysname**|Nome del processo.|  
|**run_requested_date**|**datetime**|Data e ora previste per l'esecuzione del processo.|  
|**run_requested_source**|**sysname**|Origine dalla richiesta di esecuzione del processo. Uno dei valori possibili:<br /><br /> **1** = esecuzione in base a una pianificazione<br /><br /> **2** = esecuzione in risposta a un avviso<br /><br /> **3** = esecuzione all'avvio<br /><br /> **4** = esecuzione eseguita dall'utente<br /><br /> **6** = esecuzione in base alla pianificazione di inattività della CPU|  
|**queued_date**|**datetime**|Data e ora di inserimento della richiesta nella coda. NULL se il processo è stato eseguito direttamente.|  
|**start_execution_date**|**datetime**|Data e ora di assegnazione del processo a un thread eseguibile.|  
|**last_executed_step_id**|**int**|ID dell'ultimo passaggio del processo eseguito.|  
|**last_exectued_step_date**|**datetime**|Data e ora di inizio dell'esecuzione dell'ultimo passaggio del processo.|  
|**stop_execution_date**|**datetime**|Data e ora di arresto dell'esecuzione del processo.|  
|**next_scheduled_run_date**|**datetime**|Data e ora pianificate per la successiva esecuzione del processo.|  
|**job_history_id**|**int**|Identificatore della cronologia processo nella tabella delle cronologie processi.|  
|**message**|**nvarchar(1024)**|Messaggio generato durante l'ultima esecuzione del processo.|  
|**run_status**|**int**|Stato restituito dall'ultima esecuzione del processo:<br /><br /> **0** = errore non riuscito<br /><br /> **1** = operazione completata<br /><br /> **3** = annullato<br /><br /> **5** = stato sconosciuto|  
|**operator_id_emailed**|**int**|ID dell'operatore comunicato tramite posta elettronica al completamento del processo.|  
|**operator_id_netsent**|**int**|ID dell'operatore che ha ricevuto una notifica tramite **net send** al completamento del processo.|  
|**operator_id_paged**|**int**|ID dell'operatore comunicato tramite cercapersone al completamento del processo.|  
  
## <a name="remarks"></a>Commenti  
 Tramite questa procedura viene generato uno snapshot dello stato corrente dei processi. I risultati restituiti rappresentano le informazioni disponibili al momento dell'elaborazione della richiesta.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent crea un ID di sessione ogni volta che viene avviato. L'ID sessione viene archiviato nella tabella **msdb.dbo.syssessioni**.  
  
 Quando non viene specificato alcun *session_id* , elenca le informazioni relative alla sessione più recente.  
  
 Quando non viene specificato alcun *job_name* o *job_id* , elenca le informazioni per tutti i processi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, i membri del ruolo predefinito del server **sysadmin** possono eseguire questo stored procedure. Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri di **sysadmin** possono visualizzare l'attività per i processi di proprietà di altri utenti.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sull'attività di tutti i processi per i quali l'utente corrente dispone dell'autorizzazione di visualizzazione.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobactivity ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
