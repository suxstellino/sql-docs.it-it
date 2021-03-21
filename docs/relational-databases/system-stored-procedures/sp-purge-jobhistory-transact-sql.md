---
description: sp_purge_jobhistory (Transact-SQL)
title: sp_purge_jobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_purge_jobhistory_TSQL
- sp_purge_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_purge_jobhistory
ms.assetid: 237f9bad-636d-4262-9bfb-66c034a43e88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55f8c3d0bd3c5812a123ec0b3d437171fd2a3d71
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748249"
---
# <a name="sp_purge_jobhistory-transact-sql"></a>sp_purge_jobhistory (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Rimuove i record della cronologia relativi a un processo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_purge_jobhistory   
   {   [ @job_name = ] 'job_name' |   
     | [ @job_id = ] job_id }  
   [ , [ @oldest_date = ] oldest_date ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_name = ] 'job_name'` Nome del processo per il quale eliminare i record della cronologia. *job_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
> [!NOTE]  
>  I membri del ruolo predefinito del server **sysadmin** o i membri del ruolo predefinito del database **SQLAgentOperatorRole** possono eseguire **sp_purge_jobhistory** senza specificare un *job_name* o *job_id*. Quando gli utenti **sysadmin** non specificano questi argomenti, la cronologia processo per tutti i processi locali e multiserver viene eliminata entro il tempo specificato da *oldest_date*. Quando gli utenti di **SQLAgentOperatorRole** non specificano questi argomenti, la cronologia processo per tutti i processi locali viene eliminata entro il tempo specificato da *oldest_date*.  
  
`[ @job_id = ] job_id` Numero di identificazione del processo dei record da eliminare. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null. È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi. Vedere la nota nella descrizione di **\@ job_name** per informazioni sul modo in cui gli utenti **sysadmin** o **SQLAgentOperatorRole** possono usare questo argomento.  
  
`[ @oldest_date = ] oldest_date` Il record meno recente da conservare nella cronologia. *oldest_date* è di tipo **DateTime** e il valore predefinito è null. Quando si specifica *oldest_date* , **sp_purge_jobhistory** rimuove solo i record precedenti al valore specificato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Quando **sp_purge_jobhistory** viene completata correttamente, viene restituito un messaggio.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **SQLAgentOperatorRole** possono eseguire questo stored procedure. I membri di **sysadmin** possono eliminare la cronologia processo per tutti i processi locali e multiserver. I membri di **SQLAgentOperatorRole** possono eliminare la cronologia processo solo per tutti i processi locali.  
  
 Per gli altri utenti, inclusi i membri di **SQLAgentUserRole** e i membri di **SQLAgentReaderRole**, deve essere concessa in modo esplicito l'autorizzazione Execute per **sp_purge_jobhistory**. Dopo la concessione dell'autorizzazione EXECUTE per questa stored procedure, a tali utenti è consentito eliminare solo la cronologia dei processi di cui sono proprietari.  
  
 I ruoli predefiniti del database **SQLAgentUserRole**, **SQLAgentReaderRole** e **SQLAgentOperatorRole** si trovano nel database **msdb** . Per informazioni dettagliate sulle autorizzazioni, vedere [SQL Server Agent ruoli](../../ssms/agent/sql-server-agent-fixed-database-roles.md)predefiniti del database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-remove-history-for-a-specific-job"></a>R. Rimozione della cronologia di un processo specifico  
 Nell'esempio seguente viene rimossa la cronologia per un processo denominato `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-remove-history-for-all-jobs"></a>B. Rimozione della cronologia di tutti i processi  
  
> [!NOTE]  
>  Solo i membri del ruolo predefinito del server **sysadmin** e i membri di **SQLAgentOperatorRole** possono rimuovere la cronologia per tutti i processi. Quando gli utenti **sysadmin** eseguono questa stored procedure senza parametri, la cronologia processo per tutti i processi locali e multiserver viene eliminata. Quando gli utenti di **SQLAgentOperatorRole** eseguono questa stored procedure senza parametri, viene eliminata solo la cronologia processo per tutti i processi locali.  
  
 Nell'esempio seguente la procedura viene eseguita senza alcun parametro in modo da rimuovere tutti i record della cronologia.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_help_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobhistory &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobhistory-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)  
  
  
