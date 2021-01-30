---
description: sp_update_alert (Transact-SQL)
title: sp_update_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_alert_TSQL
- sp_update_alert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_alert
ms.assetid: 4bbaeaab-8aca-4c9e-abc1-82ce73090bd3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c9dce24d3f8dd20d2138ba153fee74212e603e6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184758"
---
# <a name="sp_update_alert-transact-sql"></a>sp_update_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna le impostazioni di un avviso esistente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_update_alert   
     [ @name =] 'name'   
     [ , [ @new_name =] 'new_name']   
     [ , [ @enabled =] enabled]   
     [ , [ @message_id =] message_id]   
     [ , [ @severity =] severity]   
     [ , [ @delay_between_responses =] delay_between_responses]   
     [ , [ @notification_message =] 'notification_message']   
     [ , [ @include_event_description_in =] include_event_description_in]   
     [ , [ @database_name =] 'database']   
     [ , [ @event_description_keyword =] 'event_description_keyword']   
     [ , [ @job_id =] job_id | [@job_name =] 'job_name']   
     [ , [ @occurrence_count = ] occurrence_count]   
     [ , [ @count_reset_date =] count_reset_date]   
     [ , [ @count_reset_time =] count_reset_time]   
     [ , [ @last_occurrence_date =] last_occurrence_date]   
     [ , [ @last_occurrence_time =] last_occurrence_time]   
     [ , [ @last_response_date =] last_response_date]   
     [ , [ @last_response_time =] last_response _time]  
     [ , [ @raise_snmp_trap =] raise_snmp_trap]  
     [ , [ @performance_condition =] 'performance_condition' ]   
     [ , [ @category_name =] 'category']  
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome dell'avviso da aggiornare. *Name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @new_name = ] 'new_name'` Nuovo nome per l'avviso. Il nome deve essere univoco. *new_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @enabled = ] enabled` Specifica se l'avviso è abilitato (**1**) o non abilitato (**0**). *Enabled* è di **tinyint** e il valore predefinito è null. Per consentire la generazione di un avviso, è necessario che l'avviso sia abilitato.  
  
`[ @message_id = ] message_id` Nuovo messaggio o numero di errore per la definizione di avviso. In genere, *message_id* corrisponde a un numero di errore nella tabella **sysmessages** . *message_id* è di **tipo int** e il valore predefinito è null. Un ID messaggio può essere utilizzato solo se l'impostazione del livello di gravità per l'avviso è **0**.  
  
`[ @severity = ] severity` Un nuovo livello di gravità (da **1** a **25**) per la definizione di avviso. Qualsiasi [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] messaggio inviato al registro applicazioni di Windows con il livello di gravità specificato attiverà l'avviso. *gravità* è di **tipo int** e il valore predefinito è null. È possibile utilizzare un livello di gravità solo se l'impostazione dell'ID messaggio per l'avviso è **0**.  
  
`[ @delay_between_responses = ] delay_between_responses` Nuovo periodo di attesa, in secondi, tra le risposte all'avviso. *delay_between_responses* è di **tipo int** e il valore predefinito è null.  
  
`[ @notification_message = ] 'notification_message'` Testo modificato di un messaggio aggiuntivo inviato all'operatore come parte della notifica tramite posta elettronica, **net send** o cercapersone. *notification_message* è di **tipo nvarchar (512)** e il valore predefinito è null.  
  
`[ @include_event_description_in = ] include_event_description_in` Specifica se la descrizione dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] errore dal registro applicazioni di Windows deve essere inclusa nel messaggio di notifica. *include_event_description_in* è di **tinyint** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|nessuno|  
|**1**|Posta elettronica|  
|**2**|Cercapersone|  
|**4**|**net send**|  
|**7**|Tutti|  
  
`[ @database_name = ] 'database'` Nome del database in cui deve verificarsi l'errore affinché l'avviso venga generato. il *database* è di **tipo sysname.** I nomi racchiusi tra parentesi quadre ([ ]) non sono ammessi. Il valore predefinito è NULL.  
  
`[ @event_description_keyword = ] 'event_description_keyword'` Sequenza di caratteri che è necessario trovare nella descrizione dell'errore nel log dei messaggi di errore. È possibile utilizzare i caratteri dei criteri di ricerca dell'espressione LIKE [!INCLUDE[tsql](../../includes/tsql-md.md)]. *event_description_keyword* è di **tipo nvarchar (100)** e il valore predefinito è null. Questo parametro è utile per filtrare i nomi degli oggetti (ad esempio, **% customer_table%**).  
  
`[ @job_id = ] job_id` Numero di identificazione del processo. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null. Se *job_id* è specificato, è necessario omettere *job_name* .  
  
`[ @job_name = ] 'job_name'` Nome del processo eseguito in risposta a questo avviso. *job_name* è di **tipo sysname** e il valore predefinito è null. Se *job_name* è specificato, è necessario omettere *job_id* .  
  
`[ @occurrence_count = ] occurrence_count` Reimposta il numero di volte in cui si è verificato l'avviso. *occurrence_count* è di **tipo int** e il valore predefinito è null. può essere impostato solo su **0**.  
  
`[ @count_reset_date = ] count_reset_date` Reimposta la data dell'ultima reimpostazione del numero di occorrenze. *count_reset_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @count_reset_time = ] count_reset_time` Reimposta l'ora dell'ultima reimpostazione del numero di occorrenze. *count_reset_time* è di **tipo int** e il valore predefinito è null.  
  
`[ @last_occurrence_date = ] last_occurrence_date` Reimposta la data dell'ultima volta in cui l'avviso è stato generato. *last_occurrence_date* è di **tipo int** e il valore predefinito è null. può essere impostato solo su **0**.  
  
`[ @last_occurrence_time = ] last_occurrence_time` Reimposta l'ora dell'ultima verifica dell'avviso. *last_occurrence_time* è di **tipo int** e il valore predefinito è null. può essere impostato solo su **0**.  
  
`[ @last_response_date = ] last_response_date` Reimposta la data dell'ultima risposta all'avviso da parte del servizio SQLServerAgent. *last_response_date* è di **tipo int** e il valore predefinito è null. può essere impostato solo su **0**.  
  
`[ @last_response_time = ] last_response_time` Reimposta l'ora dell'ultima risposta all'avviso da parte del servizio SQLServerAgent. *last_response_time* è di **tipo int** e il valore predefinito è null. può essere impostato solo su **0**.  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` Riservati.  
  
`[ @performance_condition = ] 'performance_condition'` Valore espresso nel formato **'**_itemcomparatorvalue_**'**. *performance_condition* è di **tipo nvarchar (512)** e il valore predefinito è null ed è costituito da questi elementi.  
  
|Componente del formato|Descrizione|  
|--------------------|-----------------|  
|*Item*|Oggetto prestazioni, contatore delle prestazioni o istanza denominata del contatore|  
|*Confronto*|Uno di questi operatori: **>** , **<** , **=**|  
|*Valore*|Valore numerico del contatore|  
  
`[ @category_name = ] 'category'` Nome della categoria di avvisi. *Category* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @wmi_namespace = ] 'wmi_namespace'` Spazio dei nomi WMI in cui eseguire query per gli eventi. *wmi_namespace* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @wmi_query = ] 'wmi_query'` Query che specifica l'evento WMI per l'avviso. *wmi_query* è di **tipo nvarchar (512)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Solo **sysmessages** scritti nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro applicazioni di Windows possono generare un avviso.  
  
 **sp_update_alert** modifica solo le impostazioni di avviso per le quali vengono forniti i valori dei parametri. Se si omette un parametro, viene mantenuta l'impostazione corrente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, gli utenti devono essere membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente l'impostazione di abilitazione di `Test Alert` viene sostituita con `0`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_alert  
    @name = N'Test Alert',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_help_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
