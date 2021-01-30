---
description: sp_delete_job (Transact-SQL)
title: sp_delete_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_job
- sp_delete_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_job
ms.assetid: b85db6e4-623c-41f1-9643-07e5ea38db09
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ef2fdfc32a0b8b076037b3d75d2c88df9a5883aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186339"
---
# <a name="sp_delete_job-transact-sql"></a>sp_delete_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina un processo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_job { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,  
     [ , [ @originating_server = ] 'server' ]   
     [ , [ @delete_history = ] delete_history ]  
     [ , [ @delete_unused_schedule = ] delete_unused_schedule ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo da eliminare. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo da eliminare. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name*. non è possibile specificare entrambi.  
  
`[ @originating_server = ] 'server'` Per uso interno.  
  
`[ @delete_history = ] delete_history` Specifica se eliminare la cronologia per il processo. *delete_history* è di **bit** e il valore predefinito è **1**. Quando *delete_history* è **1**, la cronologia del processo viene eliminata. Quando *delete_history* è **0**, la cronologia del processo non viene eliminata.  
  
 Si noti che quando un processo viene eliminato e la cronologia non viene eliminata, le informazioni cronologiche per il processo non verranno visualizzate nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cronologia processo dell'interfaccia utente grafica dell'agente, ma le informazioni rimarranno comunque nella tabella **sysjobhistory** del database **msdb** .  
  
`[ @delete_unused_schedule = ] delete_unused_schedule` Specifica se eliminare le pianificazioni associate a questo processo se non sono associate a un altro processo. *delete_unused_schedule* è di **bit** e il valore predefinito è **1**. Quando *delete_unused_schedule* è **1**, le pianificazioni associate a questo processo vengono eliminate se non vi sono altri processi che fanno riferimento alla pianificazione. Quando *delete_unused_schedule* è **0**, le pianificazioni non vengono eliminate.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 L'argomento **\@ originating_server** è riservato per uso interno.  
  
 L'argomento **\@ delete_unused_schedule** garantisce la compatibilità con le versioni precedenti di SQL Server rimuovendo automaticamente le pianificazioni non associate a un processo. Si noti che per impostazione predefinita questo parametro assume una funzionalità compatibile con le versioni precedenti. Per mantenere pianificazioni non associate a un processo, è necessario specificare il valore **0** come argomento **\@ delete_unused_schedule** .  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è incluso un semplice strumento grafico per la gestione dei processi, che è lo strumento consigliato per la creazione e la gestione dell'infrastruttura dei processi.  
  
 Questa stored procedure non può eliminare i piani di manutenzione né i processi facenti parti dei piani di manutenzione. Per eliminare i piani di manutenzione, utilizzare invece [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_delete_job** per l'eliminazione di qualsiasi processo. Se un utente non è un membro del ruolo predefinito del server **sysadmin** , può eliminare solo i processi di sua proprietà.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene eliminato il processo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_delete_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_help_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
