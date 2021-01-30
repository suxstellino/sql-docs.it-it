---
description: sp_add_job (Transact-SQL)
title: sp_add_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_job_TSQL
- sp_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_job
ms.assetid: 6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39f083d083a38e4a581c507354efaf6548471e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206801"
---
# <a name="sp_add_job-transact-sql"></a>sp_add_job (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un nuovo processo eseguito dal servizio SQL Agent.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
 > [!IMPORTANT]  
 > In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).
 
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_job [ @job_name = ] 'job_name'  
     [ , [ @enabled = ] enabled ]   
     [ , [ @description = ] 'description' ]   
     [ , [ @start_step_id = ] step_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @category_id = ] category_id ]   
     [ , [ @owner_login_name = ] 'login' ]   
     [ , [ @notify_level_eventlog = ] eventlog_level ]   
     [ , [ @notify_level_email = ] email_level ]   
     [ , [ @notify_level_netsend = ] netsend_level ]   
     [ , [ @notify_level_page = ] page_level ]   
     [ , [ @notify_email_operator_name = ] 'email_name' ]   
          [ , [ @notify_netsend_operator_name = ] 'netsend_name' ]   
     [ , [ @notify_page_operator_name = ] 'page_name' ]   
     [ , [ @delete_level = ] delete_level ]   
     [ , [ @job_id = ] job_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_name = ] 'job_name'` Nome del processo. Il nome deve essere univoco e non può contenere il carattere di percentuale ( **%** ). *job_name* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito.  
  
`[ @enabled = ] enabled` Indica lo stato del processo aggiunto. *Enabled* è di **tinyint** e il valore predefinito è 1 (abilitato). Se è **0**, il processo non è abilitato e non viene eseguito in base alla pianificazione. Tuttavia, può essere eseguita manualmente.  
  
`[ @description = ] 'description'` Descrizione del processo. *Description* è di **tipo nvarchar (512)** e il valore predefinito è null. Se *Description* viene omesso, viene usato "nessuna descrizione disponibile".  
  
`[ @start_step_id = ] step_id` Numero di identificazione del primo passaggio da eseguire per il processo. *step_id* è di **tipo int** e il valore predefinito è 1.  
  
`[ @category_name = ] 'category'` Categoria per il processo. *Category* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @category_id = ] category_id` Meccanismo indipendente dal linguaggio per specificare una categoria di processi. *category_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @owner_login_name = ] 'login'` Nome dell'account di accesso proprietario del processo. *login* è di **tipo sysname** e il valore predefinito è null, che viene interpretato come nome di account di accesso corrente. Solo i membri del ruolo predefinito del server **sysadmin** possono impostare o modificare il valore per **\@ owner_login_name**. Se gli utenti che non sono membri del ruolo **sysadmin** o modificano il valore di **\@ owner_login_name**, l'esecuzione di questo stored procedure ha esito negativo e viene restituito un errore.  
  
`[ @notify_level_eventlog = ] eventlog_level` Valore che indica quando inserire una voce nel registro applicazioni di Microsoft Windows per questo processo. *eventlog_level* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Mai|  
|**1**|In caso di esito positivo|  
|**2** (impostazione predefinita)|In caso di esito negativo|  
|**3**|Always|  
  
`[ @notify_level_email = ] email_level` Valore che indica quando inviare un messaggio di posta elettronica al termine del processo. *email_level* è di **tipo int** e il valore predefinito è **0**, che indica Never. *email_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @notify_level_netsend = ] netsend_level` Valore che indica quando inviare un messaggio di rete al termine del processo. *netsend_level* è di **tipo int** e il valore predefinito è **0**, che indica Never. *netsend_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @notify_level_page = ] page_level` Valore che indica quando inviare una pagina al termine del processo. *page_level* è di **tipo int** e il valore predefinito è **0**, che indica Never. *page_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @notify_email_operator_name = ] 'email_name'` Nome di posta elettronica della persona a cui inviare il messaggio di posta elettronica quando viene raggiunto *email_level* . *email_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @notify_netsend_operator_name = ] 'netsend_name'` Nome dell'operatore a cui viene inviato il messaggio di rete al termine del processo. *netsend_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @notify_page_operator_name = ] 'page_name'` Nome della persona a cui eseguire il paging al completamento del processo. *page_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @delete_level = ] delete_level` Valore che indica quando eliminare il processo. *delete_value* è di **tipo int** e il valore predefinito è 0, ovvero Never. *delete_level* usa gli stessi valori di *eventlog_level*.  
  
> [!NOTE]  
>  Quando *delete_level* è **3**, il processo viene eseguito una sola volta, indipendentemente dalle pianificazioni definite per il processo. Inoltre, se un processo si autoelimina, viene eliminato anche il contenuto della cronologia corrispondente.  
  
`[ @job_id = ] _job_idOUTPUT` Numero di identificazione assegnato al processo se viene creato correttamente. *job_id* è una variabile di output di tipo **uniqueidentifier** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **\@ originating_server** esiste nel **sp_add_job,** ma non è elencato in argomenti. **\@ originating_server** è riservata per uso interno.  
  
 Dopo l'esecuzione di **sp_add_job** per l'aggiunta di un processo, è possibile usare **sp_add_jobstep** per aggiungere passaggi che eseguono le attività del processo. **sp_add_jobschedule** possibile utilizzare per creare la pianificazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzata dal servizio Agent per eseguire il processo. Utilizzare **sp_add_jobserver** per impostare l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza di in cui viene eseguito il processo e **sp_delete_jobserver** per rimuovere il processo dall' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza di.  
  
 Se il processo verrà eseguito in uno o più server di destinazione in un ambiente multiserver, utilizzare **sp_apply_job_to_targets** per impostare i server di destinazione o i gruppi di server di destinazione per il processo. Per rimuovere i processi dai server di destinazione o dai gruppi di server di destinazione, utilizzare **sp_remove_job_from_targets**.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è incluso un semplice strumento grafico per la gestione dei processi, che è lo strumento consigliato per la creazione e la gestione dell'infrastruttura dei processi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, gli utenti devono essere membri del ruolo predefinito del server **sysadmin** o disporre di uno dei ruoli predefiniti del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database di Agent seguenti, che risiedono nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni sulle autorizzazioni specifiche associate a ognuno di questi ruoli predefiniti del database, vedere [SQL Server Agent ruoli](../../ssms/agent/sql-server-agent-fixed-database-roles.md)predefiniti del database.  
  
 Solo i membri del ruolo predefinito del server **sysadmin** possono impostare o modificare il valore per **\@ owner_login_name**. Se gli utenti che non sono membri del ruolo **sysadmin** o modificano il valore di **\@ owner_login_name**, l'esecuzione di questo stored procedure ha esito negativo e viene restituito un errore.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-adding-a-job"></a>R. Aggiunta di un processo  
 In questo esempio viene aggiunto il nuovo processo denominato `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-adding-a-job-with-pager-e-mail-and-net-send-information"></a>B. Aggiunta di un processo con informazioni inviate tramite il cercapersone, la posta elettronica e la rete  
 In questo esempio viene creato il processo `Ad hoc Sales Data Backup` che in caso di esito negativo invia una notifica all'operatore `François Ajenstat` (tramite cercapersone, posta elettronica o messaggio popup di rete), mentre in caso di esito positivo si autoelimina.  
  
> [!NOTE]  
>  In questo esempio si presuppone che l'operatore `François Ajenstat` e l'account di accesso `françoisa` esistano già.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'Ad hoc Sales Data Backup',   
    @enabled = 1,  
    @description = N'Ad hoc backup of sales data',  
    @owner_login_name = N'françoisa',  
    @notify_level_eventlog = 2,  
    @notify_level_email = 2,  
    @notify_level_netsend = 2,  
    @notify_level_page = 2,  
    @notify_email_operator_name = N'François Ajenstat',  
    @notify_netsend_operator_name = N'François Ajenstat',   
    @notify_page_operator_name = N'François Ajenstat',  
    @delete_level = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_add_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_apply_job_to_targets &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [sp_help_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_update_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
