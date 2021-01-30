---
description: sp_add_jobstep (Transact-SQL)
title: sp_add_jobstep (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobstep_TSQL
- sp_add_jobstep
helpviewer_keywords:
- sp_add_jobstep
ms.assetid: 97900032-523d-49d6-9865-2734fba1c755
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 03/15/2017
ms.openlocfilehash: 93d4818a1cffc14452bdf25bab8be6e1fc9a1a50
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206758"
---
# <a name="sp_add_jobstep-transact-sql"></a>sp_add_jobstep (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Aggiunge un passaggio (operazione) a un processo di SQL Agent.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

> [!IMPORTANT]
> Nel [istanza gestita SQL di Azure](/azure/sql-database/sql-database-managed-instance), la maggior parte dei tipi di processo SQL Server Agent sono supportati. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

## <a name="syntax"></a>Sintassi

```sql
sp_add_jobstep [ @job_id = ] job_id | [ @job_name = ] 'job_name'
     [ , [ @step_id = ] step_id ]
     { , [ @step_name = ] 'step_name' }
     [ , [ @subsystem = ] 'subsystem' ]
     [ , [ @command = ] 'command' ]
     [ , [ @additional_parameters = ] 'parameters' ]
          [ , [ @cmdexec_success_code = ] code ]
     [ , [ @on_success_action = ] success_action ]
          [ , [ @on_success_step_id = ] success_step_id ]
          [ , [ @on_fail_action = ] fail_action ]
          [ , [ @on_fail_step_id = ] fail_step_id ]
     [ , [ @server = ] 'server' ]
     [ , [ @database_name = ] 'database' ]
     [ , [ @database_user_name = ] 'user' ]
     [ , [ @retry_attempts = ] retry_attempts ]
     [ , [ @retry_interval = ] retry_interval ]
     [ , [ @os_run_priority = ] run_priority ]
     [ , [ @output_file_name = ] 'file_name' ]
     [ , [ @flags = ] flags ]
     [ , { [ @proxy_id = ] proxy_id
         | [ @proxy_name = ] 'proxy_name' } ]
```  
  
## <a name="arguments"></a>Argomenti

`[ @job_id = ] job_id` Numero di identificazione del processo a cui aggiungere il passaggio. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.

`[ @job_name = ] 'job_name'` Nome del processo a cui aggiungere il passaggio. *job_name* è di **tipo sysname** e il valore predefinito è null.

> [!NOTE]
> È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.

`[ @step_id = ] step_id` Numero di identificazione della sequenza per il passaggio di processo. I numeri di identificazione del passaggio iniziano da **1** e incrementano senza gap. Se viene inserito un passaggio nella sequenza esistente, i numeri di sequenza vengono automaticamente adeguati. Se non si specifica *step_id* , viene fornito un valore. *step_id* è di **tipo int** e il valore predefinito è null.

`[ @step_name = ] 'step_name'` Nome del passaggio. *step_name* è di **tipo sysname** e non prevede alcun valore predefinito.

`[ @subsystem = ] 'subsystem'` Sottosistema utilizzato dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio Agent per eseguire il *comando*. il *sottosistema* è di **tipo nvarchar (40)**. i possibili valori sono i seguenti.

|Valore|Descrizione|
|-----------|-----------------|
|'**ActiveScripting**'|Script ActiveX<br /><br /> **\*\* Importante \*\*** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|
|'**CmdExec**'|Comando del sistema operativo o programma eseguibile|
|'**Distribution**'|Processo di Agente distribuzione repliche|
|'**Snapshot**'|Processo di Agente snapshot repliche|
|'**Lettura log**'|Processo di Agente lettura log repliche|
|'**Unisci**'|Processo di Agente merge repliche|
|'**Processo QueueReader**'|Processo di Agente di lettura coda repliche|
|'**ANALYSISQUERY**'|Query di Analysis Services (MDX, DMX).|
|'**ANALYSISCOMMAND**'|Comando di Analysis Services (XMLA).|
|'**SSIS**'|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] esecuzione del pacchetto|  
|'**PowerShell**'|Script di PowerShell|  
|'**TSQL**' (impostazione predefinita)|[!INCLUDE[tsql](../../includes/tsql-md.md)] - istruzione|

`[ @command = ] 'command'` Comandi che devono essere eseguiti dal servizio **SQLServerAgent** tramite il *sottosistema*. *Command* è di **tipo nvarchar (max)** e il valore predefinito è null. SQL Server Agent consente di eseguire la sostituzione dei token, che garantisce la stessa flessibilità assicurata dalle variabili durante la scrittura dei programmi software.

> [!IMPORTANT]
> È necessario inserire una macro di escape con tutti i token utilizzati nei passaggi di processo. In caso contrario, questi passaggi avranno esito negativo. È ora necessario inoltre racchiudere tra parentesi i nomi dei token e inserire il simbolo di dollaro (`$`) all'inizio della sintassi del token, Ad esempio:
>
> `$(ESCAPE_` *macro name* `(DATE))`  

Per altre informazioni su questi token e sull'aggiornamento dei passaggi di processo per usare la nuova sintassi del token, vedere [usare i token nei passaggi di processo](../../ssms/agent/use-tokens-in-job-steps.md).

> [!IMPORTANT]
> Qualsiasi utente di Windows con autorizzazioni di scrittura per il registro eventi di Windows è in grado di accedere ai passaggi di processo attivati dagli avvisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent o di WMI. Per evitare rischi per la sicurezza, i token di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che possono essere utilizzati in processi attivati dagli avvisi sono disabilitati per impostazione predefinita. I token sono: **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** e **WMI(** _property_ **)** . Si noti che in questa versione l'utilizzo dei token è esteso a tutti gli avvisi.
>
> Se si desidera utilizzare questi token, verificare innanzitutto che solo i membri di gruppi di sicurezza di Windows trusted, ad esempio il gruppo Administrators, dispongano delle autorizzazioni di scrittura per il registro eventi del computer in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A questo punto, fare clic con il pulsante destro del mouse su **SQL Server Agent** in Esplora oggetti, scegliere **Proprietà** e nella pagina **Sistema avvisi** selezionare **Sostituisci token per tutte le risposte del processo ad avvisi** per abilitare questi token.

`[ @additional_parameters = ] 'parameters'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *Parameters* è di tipo **ntext** e il valore predefinito è null.

`[ @cmdexec_success_code = ] code` Il valore restituito da un comando del sottosistema **CmdExec** per indicare che il *comando* è stato eseguito correttamente. il *codice* è di **tipo int** e il valore predefinito è **0**.

`[ @on_success_action = ] success_action` Azione da eseguire se il passaggio ha esito positivo. *success_action* è di **tinyint**. i possibili valori sono i seguenti.
  
|Valore|Descrizione (azione)|  
|-----------|----------------------------|  
|**1** (impostazione predefinita)|Uscita in caso di esito positivo|  
|**2**|Uscita in caso di esito negativo|  
|**3**|Esecuzione del passaggio successivo|  
|**4**|Vai al passaggio *on_success_step_id*|  

`[ @on_success_step_id = ] success_step_id` ID del passaggio del processo da eseguire se il passaggio ha esito positivo e *success_action* è **4**. *success_step_id* è di **tipo int** e il valore predefinito è **0**.

`[ @on_fail_action = ] fail_action` Azione da eseguire se il passaggio ha esito negativo. *fail_action* è di **tinyint**. i possibili valori sono i seguenti.

|Valore|Descrizione (azione)|  
|-----------|----------------------------|  
|**1**|Uscita in caso di esito positivo|  
|**2** (impostazione predefinita)|Uscita in caso di esito negativo|  
|**3**|Esecuzione del passaggio successivo|  
|**4**|Vai al passaggio *on_fail_step_id*|  

`[ @on_fail_step_id = ] fail_step_id` ID del passaggio del processo da eseguire se il passaggio ha esito negativo e *fail_action* è **4**. *fail_step_id* è di **tipo int** e il valore predefinito è **0**.  

`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]il *Server* è di **tipo nvarchar (30)** e il valore predefinito è null.  

`[ @database_name = ] 'database'` Nome del database in cui eseguire un [!INCLUDE[tsql](../../includes/tsql-md.md)] passaggio. il *database* è di **tipo sysname** e il valore predefinito è null. in tal caso, viene utilizzato il database **Master** . I nomi racchiusi tra parentesi quadre ([ ]) non sono ammessi. Per un passaggio di processo ActiveX, il *database* è il nome del linguaggio di script utilizzato dal passaggio.  

`[ @database_user_name = ] 'user'` Nome dell'account utente da utilizzare durante l'esecuzione di un [!INCLUDE[tsql](../../includes/tsql-md.md)] passaggio. *User* è di **tipo sysname** e il valore predefinito è null. Quando l' *utente* è null, il passaggio viene eseguito nel contesto utente del proprietario del processo nel *database*.  SQL Server Agent includerà questo parametro solo se il proprietario del processo è un sysadmin di SQL Server. In tal caso il passaggio del processo Transact-SQL specificato sarà eseguito nel contesto del nome utente di SQL Server specificato. Se il proprietario del processo non è un SQL Server sysadmin, il passaggio Transact-SQL sarà sempre eseguito nel contesto dell'account di accesso proprietario del processo e il @database_user_name parametro verrà ignorato.  

`[ @retry_attempts = ] retry_attempts` Numero di tentativi da usare in caso di errore di questo passaggio. *retry_attempts* è di **tipo int** e il valore predefinito è **0**, che indica nessun tentativo.  

`[ @retry_interval = ] retry_interval` Quantità di tempo in minuti tra i tentativi di ripetizione. *retry_interval* è di **tipo int** e il valore predefinito è **0**, che indica un intervallo di **0** minuti.  

`[ @os_run_priority = ] run_priority` Riservati.

`[ @output_file_name = ] 'file_name'` Nome del file in cui viene salvato l'output di questo passaggio. *file_name* è di **tipo nvarchar (200)** e il valore predefinito è null. *file_name* possono includere uno o più token elencati sotto *Command*. Questo parametro è valido solo con i comandi in esecuzione nei [!INCLUDE[tsql](../../includes/tsql-md.md)] sottosistemi, **CmdExec**, **PowerShell**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  

`[ @flags = ] flags` È un'opzione che controlla il comportamento. *Flags* è di **tipo int**. i possibili valori sono i seguenti.  

|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|Il file di output viene sovrascritto|  
|**2**|L'output viene aggiunto alla fine del file di output.|  
|**4**|L'output del passaggio del processo [!INCLUDE[tsql](../../includes/tsql-md.md)] viene scritto nella cronologia dei passaggi|  
|**8**|Il log viene scritto nella tabella. La cronologia esistente viene sovrascritta|  
|**16**|Il log viene scritto nella tabella in aggiunta alla cronologia esistente|  
|**32**|Tutto l'output viene scritto nella cronologia processo|  
|**64**|Creare un evento Windows da utilizzare come segnale per l'interruzione dell'oggetto JobStep Cmd|  

`[ @proxy_id = ] proxy_id` Numero ID del proxy in cui viene eseguito il passaggio di processo. *proxy_id* è di tipo **int** e il valore predefinito è null. Se non viene specificato alcun *proxy_id* , non viene specificato alcun *proxy_name* e non viene specificato alcun *user_name* , il passaggio del processo viene eseguito come account del servizio per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  

`[ @proxy_name = ] 'proxy_name'` Nome del proxy in cui viene eseguito il passaggio di processo. *proxy_name* è di tipo **sysname** e il valore predefinito è null. Se non viene specificato alcun *proxy_id* , non viene specificato alcun *proxy_name* e non viene specificato alcun *user_name* , il passaggio del processo viene eseguito come account del servizio per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  

## <a name="return-code-values"></a>Valori del codice restituito

**0** (esito positivo) o **1** (esito negativo)

## <a name="result-sets"></a>Set di risultati

nessuno

## <a name="remarks"></a>Osservazioni

**sp_add_jobstep** deve essere eseguito dal database **msdb** .  

SQL Server Management Studio include un semplice strumento grafico per la gestione dei processi ed è lo strumento consigliato per la creazione e gestione dell'infrastruttura dei processi.  

Per impostazione predefinita, un passaggio di processo viene eseguito con l'account del servizio per Agent, a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] meno che non sia specificato un altro proxy. Un requisito di questo account deve essere un membro del ruolo di sicurezza predefinito **sysadmin** .

Un proxy può essere identificato da *proxy_name* o *proxy_id*.

## <a name="permissions"></a>Autorizzazioni

 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :

- **SQLAgentUserRole**

- **SQLAgentReaderRole**

- **SQLAgentOperatorRole**

Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).

L'autore del passaggio del processo deve avere accesso al proxy per il passaggio del processo. I membri del ruolo predefinito del server **sysadmin** hanno accesso a tutti i proxy. Per quanto riguarda gli altri utenti, è necessario concedere esplicitamente l'accesso a un proxy.

## <a name="examples"></a>Esempio

Nell'esempio seguente viene creato un passaggio di processo che modifica l'accesso al database impostando la modalità sola lettura per il database Sales. In questo esempio, inoltre, vengono specificati 5 tentativi, con un intervallo di 5 minuti tra ognuno.

> [!NOTE]
> In questo esempio si presuppone che il processo `Weekly Sales Data Backup` esista già.  

```sql
USE msdb;
GO
EXEC sp_add_jobstep
    @job_name = N'Weekly Sales Data Backup',
    @step_name = N'Set database to read only',
    @subsystem = N'TSQL',
    @command = N'ALTER DATABASE SALES SET READ_ONLY',
    @retry_attempts = 5,
    @retry_interval = 5 ;
GO
```

## <a name="next-steps"></a>Passaggi successivi

- [Visualizzare o modificare processi](../../ssms/agent/view-or-modify-jobs.md)
- [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)
- [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)
- [sp_delete_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)
- [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)
- [sp_help_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)
- [sp_update_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)
- [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)