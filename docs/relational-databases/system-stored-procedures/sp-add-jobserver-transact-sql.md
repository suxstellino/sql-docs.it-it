---
description: sp_add_jobserver (Transact-SQL)
title: sp_add_jobserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobserver
- sp_add_jobserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_jobserver
ms.assetid: 485252cc-0081-490a-9bd1-cbbd68eea286
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 015e8c1cc727b14d58d49be16643f0d6324bf0dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206750"
---
# <a name="sp_add_jobserver-transact-sql"></a>sp_add_jobserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indirizza il processo specificato al server specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_jobserver [ @job_id = ] job_id | [ @job_name = ] 'job_name'  
     [ , [ @server_name = ] 'server' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @server_name = ] 'server'` Nome del server a cui indirizzare il processo. il *Server* è di **tipo nvarchar (30)** e il valore predefinito è n'(local)'. il *Server* può essere **(locale)** per un server locale oppure il nome di un server di destinazione esistente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **\@ automatic_post** esiste nel **sp_add_jobserver**, ma non è elencato in argomenti. **\@ automatic_post** è riservata per uso interno.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è incluso un semplice strumento grafico per la gestione dei processi, che è lo strumento consigliato per la creazione e la gestione dell'infrastruttura dei processi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_jobserver** per i processi che coinvolgono più server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-assigning-a-job-to-the-local-server"></a>R. Assegnazione di un processo al server locale  
 Nell'esempio seguente viene assegnato il processo `NightlyBackups` da eseguire nel server locale.  
  
> [!NOTE]  
>  In questo esempio si presuppone che il processo `NightlyBackups` esista già.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_jobserver  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-assigning-a-job-to-run-on-a-different-server"></a>B. Assegnazione di un processo da eseguire su un server diverso  
 Nell'esempio seguente il processo multiserver `Weekly Sales Backups` viene assegnato al server `SEATTLE2`.  
  
> [!NOTE]  
>  In questo esempio si presuppone che il processo `Weekly Sales Backups` esista già e che `SEATTLE2` sia registrato come server di destinazione per l'istanza corrente.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_jobserver  
    @job_name = N'Weekly Sales Backups',  
    @server_name = N'SEATTLE2' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_apply_job_to_targets &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
