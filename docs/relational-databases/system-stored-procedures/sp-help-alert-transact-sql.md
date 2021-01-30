---
description: sp_help_alert (Transact-SQL)
title: sp_help_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_alert
- sp_help_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_alert
ms.assetid: 850cef4e-6348-4439-8e79-fd1bca712091
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b39195e5f1dd21fead42a05850f9b5e5e5874b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208941"
---
# <a name="sp_help_alert-transact-sql"></a>sp_help_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sugli avvisi definiti per il server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_alert [ [ @alert_name = ] 'alert_name' ]   
     [ , [ @order_by = ] 'order_by' ]   
     [ , [ @alert_id = ] alert_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @legacy_format = ] legacy_format ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @alert_name = ] 'alert_name'` Nome dell'avviso. *alert_name* è di **tipo nvarchar (128)**. Se *alert_name* viene omesso, vengono restituite informazioni su tutti gli avvisi.  
  
`[ @order_by = ] 'order_by'` Ordine di ordinamento da utilizzare per produrre i risultati. *order_by* è di **tipo sysname** e il valore predefinito è N'*nome*'.  
  
`[ @alert_id = ] alert_id` Numero di identificazione dell'avviso per il quale segnalare le informazioni. *alert_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @category_name = ] 'category'` Categoria per l'avviso. *Category* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @legacy_format = ] legacy_format` Indica se generare un set di risultati legacy. *legacy_format* è di **bit** e il valore predefinito è **0**. Quando *legacy_format* è **1**, **sp_help_alert** restituisce il set di risultati restituito da **sp_help_alert** in Microsoft SQL Server 2000.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Quando **\@ legacy_format** è **0**, **sp_help_alert** produce il set di risultati seguente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificatore univoco di tipo integer assegnato dal sistema.|  
|**nome**|**sysname**|Nome dell'avviso (ad esempio, demo: log **msdb** completo).|  
|**event_source**|**nvarchar (100)**|Origine dell'evento. Sarà sempre **MSSQLSERVER** per la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione 7,0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Numero dell'errore del messaggio che definisce l'avviso Corrisponde in genere a un numero di errore nella tabella **sysmessages** . Se per la definizione dell'avviso viene utilizzata la gravità, **message_id** è **0** o null.|  
|**severity**|**int**|Livello di gravità (compreso tra **9** e **25**, **110**, **120**, **130** o **140**) che definisce l'avviso.|  
|**abilitato**|**tinyint**|Stato che indica se l'avviso è attualmente abilitato (**1**) o meno (**0**). Gli avvisi non abilitati non vengono inviati.|  
|**delay_between_responses**|**int**|Periodo di attesa in secondi tra risposte successive per l'avviso.|  
|**last_occurrence_date**|**int**|Data dell'ultima generazione dell'avviso.|  
|**last_occurrence_time**|**int**|Ora dell'ultima generazione dell'avviso.|  
|**last_response_date**|**int**|Data dell'ultima risposta all'avviso da parte del servizio **SQLServerAgent** .|  
|**last_response_time**|**int**|Ora dell'ultima risposta all'avviso da parte del servizio **SQLServerAgent** .|  
|**notification_message**|**nvarchar(512)**|Messaggio aggiuntivo facoltativo inviato all'operatore come parte della notifica tramite posta elettronica o cercapersone.|  
|**include_event_description**|**tinyint**|Indica se la descrizione dell'errore di SQL Server inclusa nel registro applicazioni di Microsoft Windows deve essere inserita nel messaggio di notifica.|  
|**database_name**|**sysname**|Database in cui deve verificarsi l'errore affinché l'avviso venga generato. Se il nome del database è NULL, l'avviso viene generato indipendentemente dal database in cui l'errore si verifica.|  
|**event_description_keyword**|**nvarchar (100)**|Descrizione dell'errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclusa nel registro applicazioni di Windows, che deve corrispondere alla sequenza di caratteri specificata.|  
|**occurrence_count**|**int**|Numero di volte che l'avviso è stato generato.|  
|**count_reset_date**|**int**|Data dell'ultima reimpostazione del **occurrence_count** .|  
|**count_reset_time**|**int**|Ora dell'ultima reimpostazione del **occurrence_count** .|  
|**job_id**|**uniqueidentifier**|Numero di identificazione del processo da eseguire in risposta a un avviso.|  
|**job_name**|**sysname**|Nome del processo da eseguire in risposta a un avviso.|  
|**has_notification**|**int**|È diverso da zero se uno o più operatori ricevono una notifica dell'avviso. Può essere uno o più d'uno dei valori seguenti uniti dall'operatore OR:<br /><br /> **1**= notifica tramite posta elettronica<br /><br /> **2**= notifica tramite cercapersone<br /><br /> **4**= notifica **net send** .|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**performance_condition**|**nvarchar(512)**|Se **Type** è **2**, in questa colonna viene visualizzata la definizione della condizione delle prestazioni. in caso contrario, la colonna è NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 è sempre '[Uncategorized]'.|  
|**wmi_namespace**|**sysname**|Se **Type** è **3**, in questa colonna viene visualizzato lo spazio dei nomi per l'evento WMI.|  
|**wmi_query**|**nvarchar(512)**|Se **Type** è **3**, in questa colonna viene visualizzata la query per l'evento WMI.|  
|**type**|**int**|Tipo dell'evento:<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avviso evento<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avviso di prestazioni<br /><br /> **3** = avviso evento WMI|  
  
 Quando **\@ legacy_format** è **1**, **sp_help_alert** produce il set di risultati seguente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificatore univoco di tipo integer assegnato dal sistema.|  
|**nome**|**sysname**|Nome dell'avviso (ad esempio, demo: log **msdb** completo).|  
|**event_source**|**nvarchar (100)**|Origine dell'evento. Sarà sempre **MSSQLSERVER** per la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione 7,0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Numero dell'errore del messaggio che definisce l'avviso Corrisponde in genere a un numero di errore nella tabella **sysmessages** . Se per la definizione dell'avviso viene utilizzata la gravità, **message_id** è **0** o null.|  
|**severity**|**int**|Livello di gravità (compreso tra **9** e **25**, **110**, **120**, **130** o 1 **40**) che definisce l'avviso.|  
|**abilitato**|**tinyint**|Stato che indica se l'avviso è attualmente abilitato (**1**) o meno (**0**). Gli avvisi non abilitati non vengono inviati.|  
|**delay_between_responses**|**int**|Periodo di attesa in secondi tra risposte successive per l'avviso.|  
|**last_occurrence_date**|**int**|Data dell'ultima generazione dell'avviso.|  
|**last_occurrence_time**|**int**|Ora dell'ultima generazione dell'avviso.|  
|**last_response_date**|**int**|Data dell'ultima risposta all'avviso da parte del servizio **SQLServerAgent** .|  
|**last_response_time**|**int**|Ora dell'ultima risposta all'avviso da parte del servizio **SQLServerAgent** .|  
|**notification_message**|**nvarchar(512)**|Messaggio aggiuntivo facoltativo inviato all'operatore come parte della notifica tramite posta elettronica o cercapersone.|  
|**include_event_description**|**tinyint**|Indica se la descrizione dell'errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclusa nel registro applicazioni di Windows deve essere inserita nel messaggio di notifica.|  
|**database_name**|**sysname**|Database in cui deve verificarsi l'errore affinché l'avviso venga generato. Se il nome del database è NULL, l'avviso viene generato indipendentemente dal database in cui l'errore si verifica.|  
|**event_description_keyword**|**nvarchar (100)**|Descrizione dell'errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclusa nel registro applicazioni di Windows, che deve corrispondere alla sequenza di caratteri specificata.|  
|**occurrence_count**|**int**|Numero di volte che l'avviso è stato generato.|  
|**count_reset_date**|**int**|Data dell'ultima reimpostazione del **occurrence_count** .|  
|**count_reset_time**|**int**|Ora dell'ultima reimpostazione del **occurrence_count** .|  
|**job_id**|**uniqueidentifier**|Numero di identificazione del processo.|  
|**job_name**|**sysname**|Processo su richiesta da eseguire in risposta a un avviso.|  
|**has_notification**|**int**|È diverso da zero se uno o più operatori ricevono una notifica dell'avviso. Può essere uno o più d'uno dei valori seguenti uniti dall'operatore OR:<br /><br /> **1**= notifica tramite posta elettronica<br /><br /> **2**= notifica tramite cercapersone<br /><br /> **4**= notifica **net send** .|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
|**performance_condition**|**nvarchar(512)**|Se **Type** è **2**, in questa colonna viene visualizzata la definizione della condizione delle prestazioni. Se **Type** è **3**, in questa colonna viene visualizzata la query per l'evento WMI. Negli altri casi la colonna è NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Sarà sempre '**[Uncategorized]**' per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0.|  
|**type**|**int**|Tipo di avviso:<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avviso evento<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avviso di prestazioni<br /><br /> **3** = avviso evento WMI|  
  
## <a name="remarks"></a>Commenti  
 **sp_help_alert** deve essere eseguito dal database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono appartenere al ruolo predefinito del database **SQLAgentOperatorRole** nel database **msdb** .  
  
 Per informazioni dettagliate su **SQLAgentOperatorRole**, vedere [SQL Server Agent ruoli](../../ssms/agent/sql-server-agent-fixed-database-roles.md)predefiniti del database.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sull'avviso `Demo: Sev. 25 Errors`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_alert @alert_name = 'Demo: Sev. 25 Errors';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_update_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
