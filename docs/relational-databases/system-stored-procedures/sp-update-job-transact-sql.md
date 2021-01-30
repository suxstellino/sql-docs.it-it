---
description: sp_update_job (Transact-SQL)
title: sp_update_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_job
- sp_update_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_job
ms.assetid: cbdfea38-9e42-47f3-8fc8-5978b82e2623
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e559c50589681f7d00b1bd6048b1e23579213bee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184703"
---
# <a name="sp_update_job-transact-sql"></a>sp_update_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica gli attributi di un processo.  
  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_update_job [ @job_id =] job_id | [@job_name =] 'job_name'  
     [, [@new_name =] 'new_name' ]   
     [, [@enabled =] enabled ]  
     [, [@description =] 'description' ]   
     [, [@start_step_id =] step_id ]  
     [, [@category_name =] 'category' ]   
     [, [@owner_login_name =] 'login' ]  
     [, [@notify_level_eventlog =] eventlog_level ]  
     [, [@notify_level_email =] email_level ]  
     [, [@notify_level_netsend =] netsend_level ]  
     [, [@notify_level_page =] page_level ]  
     [, [@notify_email_operator_name =] 'operator_name' ]  
     [, [@notify_netsend_operator_name =] 'netsend_operator' ]  
     [, [@notify_page_operator_name =] 'page_operator' ]  
     [, [@delete_level =] delete_level ]   
     [, [@automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo da aggiornare. *job_id* è di tipo **uniqueidentifier**.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo nvarchar (128)**.  
  
> **Nota:** È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @new_name = ] 'new_name'` Nuovo nome del processo. *new_name* è di **tipo nvarchar (128)**.  
  
`[ @enabled = ] enabled` Specifica se il processo è abilitato (**1**) o non abilitato (**0**). *abilitato* è di **tinyint**.  
  
`[ @description = ] 'description'` Descrizione del processo. *Description* è di **tipo nvarchar (512)**.  
  
`[ @start_step_id = ] step_id` Numero di identificazione del primo passaggio da eseguire per il processo. *step_id* è di **tipo int**.  
  
`[ @category_name = ] 'category'` Categoria del processo. *Category* è di **tipo nvarchar (128)**.  
  
`[ @owner_login_name = ] 'login'` Nome dell'account di accesso proprietario del processo. *login* è di **tipo nvarchar (128)** solo i membri del ruolo predefinito del server **sysadmin** possono modificare la proprietà del processo.  
  
`[ @notify_level_eventlog = ] eventlog_level` Specifica quando inserire una voce nel registro applicazioni di Microsoft Windows per questo processo. *eventlog_level* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione (azione)|  
|-----------|----------------------------|  
|**0**|Mai|  
|**1**|In caso di esito positivo|  
|**2**|In caso di esito negativo|  
|**3**|Always|  
  
`[ @notify_level_email = ] email_level` Specifica quando inviare un messaggio di posta elettronica al termine del processo. *email_level* è di **tipo int**. *email_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @notify_level_netsend = ] netsend_level` Specifica quando inviare un messaggio di rete al termine del processo. *netsend_level* è di **tipo int**. *netsend_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @notify_level_page = ] page_level` Specifica quando inviare una pagina al termine del processo. *page_level* è di **tipo int**. *page_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @notify_email_operator_name = ] 'operator_name'` Nome dell'operatore a cui viene inviato il messaggio di posta elettronica quando viene raggiunto *email_level* . *email_name* è di **tipo nvarchar (128)**.  
  
`[ @notify_netsend_operator_name = ] 'netsend_operator'` Nome dell'operatore a cui viene inviato il messaggio di rete. *netsend_operator* è di **tipo nvarchar (128)**.  
  
`[ @notify_page_operator_name = ] 'page_operator'` Nome dell'operatore a cui viene inviata una pagina. *page_operator* è di **tipo nvarchar (128)**.  
  
`[ @delete_level = ] delete_level` Specifica quando eliminare il processo. *delete_value* è di **tipo int**. *delete_level* usa gli stessi valori di *eventlog_level*.  
  
`[ @automatic_post = ] automatic_post` Riservati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_update_job** deve essere eseguito dal database **msdb** .  
  
 **sp_update_job** modifica solo le impostazioni per le quali vengono forniti i valori dei parametri. Se si omette un parametro, viene mantenuta l'impostazione corrente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri di **sysadmin** possono utilizzare questa stored procedure per modificare gli attributi dei processi di proprietà di altri utenti.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono modificati il nome, la descrizione e lo stato di attivazione del processo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_job  
    @job_name = N'NightlyBackups',  
    @new_name = N'NightlyBackups -- Disabled',  
    @description = N'Nightly backups disabled during server migration.',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
