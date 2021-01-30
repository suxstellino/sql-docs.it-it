---
description: sp_help_jobhistory (Transact-SQL)
title: sp_help_jobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobhistory_TSQL
- sp_help_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobhistory
ms.assetid: a944d44e-411b-4735-8ce4-73888d4262d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2cfef1ba5f28b498ab360daf67cc1bf3c79f0d2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200135"
---
# <a name="sp_help_jobhistory-transact-sql"></a>sp_help_jobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui processi per i server del dominio di amministrazione multiserver.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_jobhistory [ [ @job_id = ] job_id ]   
     [ , [ @job_name = ] 'job_name' ]   
     [ , [ @step_id = ] step_id ]   
     [ , [ @sql_message_id = ] sql_message_id ]   
     [ , [ @sql_severity = ] sql_severity ]   
     [ , [ @start_run_date = ] start_run_date ]   
     [ , [ @end_run_date = ] end_run_date ]   
     [ , [ @start_run_time = ] start_run_time ]   
     [ , [ @end_run_time = ] end_run_time ]   
     [ , [ @minimum_run_duration = ] minimum_run_duration ]   
     [ , [ @run_status = ] run_status ]   
     [ , [ @minimum_retries = ] minimum_retries ]   
     [ , [ @oldest_first = ] oldest_first ]   
     [ , [ @server = ] 'server' ]   
     [ , [ @mode = ] 'mode' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @step_id = ] step_id` Numero di identificazione del passaggio. *step_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @sql_message_id = ] sql_message_id` Numero di identificazione del messaggio di errore restituito da Microsoft SQL Server durante l'esecuzione del processo. *sql_message_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @sql_severity = ] sql_severity` Livello di gravità del messaggio di errore restituito da SQL Server durante l'esecuzione del processo. *sql_severity* è di **tipo int** e il valore predefinito è null.  
  
`[ @start_run_date = ] start_run_date` Data di avvio del processo. *start_run_date* è di **tipo int** e il valore predefinito è null. *start_run_date* deve essere immesso nel formato AAAAMMGG, dove AAAA è un anno di quattro caratteri, mm è il nome di un mese con due caratteri e GG è il nome di un giorno a due cifre.  
  
`[ @end_run_date = ] end_run_date` Data di completamento del processo. *end_run_date* è di **tipo int** e il valore predefinito è null. *end_run_date* deve essere immesso nel formato AAAAMMGG, dove AAAA è un anno a quattro cifre, mm è il nome di un mese con due caratteri e GG è il giorno di due caratteri.  
  
`[ @start_run_time = ] start_run_time` Ora di avvio del processo. *start_run_time* è di **tipo int** e il valore predefinito è null. *start_run_time* deve essere immesso nel formato HHMMSS, dove HH è un'ora del giorno con due caratteri, mm è un minuto di due caratteri e SS è il secondo del giorno.  
  
`[ @end_run_time = ] end_run_time` Ora di completamento dell'esecuzione del processo. *end_run_time* è di **tipo int** e il valore predefinito è null. *end_run_time* deve essere immesso nel formato HHMMSS, dove HH è un'ora del giorno con due caratteri, mm è un minuto di due caratteri e SS è il secondo del giorno.  
  
`[ @minimum_run_duration = ] minimum_run_duration` Periodo di tempo minimo per il completamento del processo. *minimum_run_duration* è di **tipo int** e il valore predefinito è null. *minimum_run_duration* deve essere immesso nel formato HHMMSS, dove HH è un'ora del giorno con due caratteri, mm è un minuto di due caratteri e SS è il secondo del giorno.  
  
`[ @run_status = ] run_status` Stato di esecuzione del processo. *run_status* è di **tipo int** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Non riuscito|  
|**1**|Completato|  
|**2**|Nuovo tentativo (solo passaggio)|  
|**3**|Cancellati|  
|**4**|Messaggio di esecuzione in corso|  
|**5**|Sconosciuto|  
  
`[ @minimum_retries = ] minimum_retries` Il numero minimo di volte in cui un processo deve ritentare l'esecuzione. *minimum_retries* è di **tipo int** e il valore predefinito è null.  
  
`[ @oldest_first = ] oldest_first` Indica se presentare prima l'output con i processi meno recenti. *oldest_first* è di **tipo int** e il valore predefinito è **0**, che presenta prima i processi più recenti. **1** presenta prima i processi meno recenti.  
  
`[ @server = ] 'server'` Nome del server in cui è stato eseguito il processo. il *Server* è di **tipo nvarchar (30)** e il valore predefinito è null.  
  
`[ @mode = ] 'mode'` Indica se SQL Server stampa tutte le colonne del set di risultati (**full**) o un riepilogo delle colonne. la *modalità* è **varchar (7)** e il valore predefinito è **Summary**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 L'elenco di colonne effettivo dipende dal valore di *mode*. Il set di colonne più completo è riportato di seguito e viene restituito quando la *modalità* è piena.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Numero di identificazione della voce di cronologia.|  
|**job_id**|**uniqueidentifier**|Numero di identificazione del processo.|  
|**job_name**|**sysname**|Nome del processo.|  
|**step_id**|**int**|Numero di identificazione del passaggio (sarà **0** per la cronologia di un processo).|  
|**step_name**|**sysname**|Nome del passaggio (è NULL per la cronologia dei processi).|  
|**sql_message_id**|**int**|Per un passaggio [!INCLUDE[tsql](../../includes/tsql-md.md)], numero di errore [!INCLUDE[tsql](../../includes/tsql-md.md)] più recente restituito durante l'esecuzione del comando.|  
|**sql_severity**|**int**|Per un passaggio [!INCLUDE[tsql](../../includes/tsql-md.md)], gravità di errore [!INCLUDE[tsql](../../includes/tsql-md.md)] più elevata restituita durante l'esecuzione del comando.|  
|**message**|**nvarchar(1024)**|Messaggio della cronologia relativo al processo o al passaggio.|  
|**run_status**|**int**|Risultato del processo o del passaggio.|  
|**run_date**|**int**|Data di inizio dell'esecuzione del processo o del passaggio.|  
|**run_time**|**int**|Ora di inizio dell'esecuzione del processo o del passaggio.|  
|**run_duration**|**int**|Periodo trascorso, in formato HHMMSS, nella fase di esecuzione del processo o passaggio.|  
|**operator_emailed**|**nvarchar (20)**|Operatore che ha ricevuto la notifica relativa al processo tramite posta elettronica (è NULL per la cronologia dei passaggi).|  
|**operator_netsent**|**nvarchar (20)**|Operatore che ha ricevuto la notifica relativa al processo tramite un messaggio di rete (è NULL per la cronologia dei passaggi).|  
|**operator_paged**|**nvarchar (20)**|Operatore che ha ricevuto la notifica relativa al processo tramite cercapersone (è NULL per la cronologia dei passaggi).|  
|**retries_attempted**|**int**|Numero di tentativi di esecuzione del passaggio (è sempre 0 per la cronologia dei processi).|  
|**server**|**nvarchar(30)**|Server in cui viene eseguito il processo o il passaggio. È sempre (**locale**).|  
  
## <a name="remarks"></a>Commenti  
 **sp_help_jobhistory** restituisce un report con la cronologia dei processi pianificati specificati. Se non viene specificato alcun parametro, il report include la cronologia di tutti i processi pianificati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri del ruolo del database **SQLAgentUserRole** possono visualizzare solo la cronologia dei processi di cui sono proprietari.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-all-job-information-for-a-job"></a>R. Visualizzazione di un elenco di tutte le informazioni di un processo  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per il processo `NightlyBackups`  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobhistory   
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-that-match-certain-conditions"></a>B. Visualizzazione di un elenco di informazioni per i processi che corrispondono a determinate condizioni  
 Nell'esempio seguente vengono stampate tutte le colonne e tutte le informazioni relative ai processi e ai passaggi non eseguiti correttamente che hanno generato un messaggio di errore `50100` (messaggio di errore definito dall'utente) con livello di gravità `20`.  
  
```  
USE msdb  
GO  
  
EXEC dbo.sp_help_jobhistory  
    @sql_message_id = 50100,  
    @sql_severity = 20,  
    @run_status = 0,  
    @mode = N'FULL' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_purge_jobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
