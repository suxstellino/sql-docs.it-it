---
description: sp_update_schedule (Transact-SQL)
title: sp_update_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_schedule
- sp_update_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_schedule
ms.assetid: 97b3119b-e43e-447a-bbfb-0b5499e2fefe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13359a3d3588e238ef0b78ed5bfe5148fb447dae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189533"
---
# <a name="sp_update_schedule-transact-sql"></a>sp_update_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le impostazioni per una pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_update_schedule   
    {   [ @schedule_id = ] schedule_id   
      | [ @name = ] 'schedule_name' }  
    [ , [ @new_name = ] new_name ]  
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @schedule_id = ] schedule_id` Identificatore della pianificazione da modificare. *schedule_id* è di **tipo int** e non prevede alcun valore predefinito. È necessario specificare *schedule_id* o *schedule_name* .  
  
`[ @name = ] 'schedule_name'` Nome della pianificazione da modificare. *schedule_name* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario specificare *schedule_id* o *schedule_name* .  
  
`[ @new_name = ] new_name` Nuovo nome della pianificazione. *new_name* è di **tipo sysname** e il valore predefinito è null. Quando *new_name* è null, il nome della pianificazione è invariato.  
  
`[ @enabled = ] enabled` Indica lo stato corrente della pianificazione. *Enabled* è di **tinyint** e il valore predefinito è **1** (abilitato). Se è **0**, la pianificazione non è abilitata. Quando la pianificazione non è abilitata, non viene eseguito alcun processo su questa pianificazione.  
  
`[ @freq_type = ] freq_type` Valore che indica quando deve essere eseguito un processo. *freq_type* è di **tipo int** e il valore predefinito è **0**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile rispetto all' *intervallo freq*|  
|**64**|All'avvio del servizio SQLServerAgent|  
|**128**|Quando il computer è inattivo|  
  
`[ @freq_interval = ] freq_interval` Giorni in cui viene eseguito un processo. *freq_interval* è di **tipo int** e il valore predefinito è **0** e dipende dal valore di *freq_type*.  
  
|Valore di *freq_type*|Effetti sui *freq_interval*|  
|---------------------------|--------------------------------|  
|**1** (una volta)|*freq_interval* non è utilizzato.|  
|**4** (giornaliera)|Ogni *freq_interval* giorni.|  
|**8** (settimanale)|*freq_interval* è uno o più degli elementi seguenti (combinati con un operatore logico **or** ):<br /><br /> **1** = domenica<br /><br /> **2** = lunedì<br /><br /> **4** = martedì<br /><br /> **8** = mercoledì<br /><br /> **16** = giovedì<br /><br /> **32** = venerdì<br /><br /> **64** = sabato|  
|**16** (ogni mese)|Il *freq_interval* giorno del mese.|  
|**32** (mensile relativo)|*freq_interval* è uno dei seguenti:<br /><br /> **1** = domenica<br /><br /> **2** = lunedì<br /><br /> **3** = martedì<br /><br /> **4** = mercoledì<br /><br /> **5** = giovedì<br /><br /> **6** = venerdì<br /><br /> **7** = sabato<br /><br /> **8** = giorno<br /><br /> **9** = giorno feriale<br /><br /> **10** = giorno festivo|  
|**64** (all'avvio del servizio SQLServerAgent)|*freq_interval* non è utilizzato.|  
|**128**|*freq_interval* non è utilizzato.|  
  
`[ @freq_subday_type = ] freq_subday_type` Specifica le unità per *freq_subday_interval * *.* *freq_subday_type* è di **tipo int** e il valore predefinito è **0**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione (unità)|  
|-----------|--------------------------|  
|**0x1**|All'ora specificata|  
|**0x2**|Secondi|  
|**0x4**|Minuti|  
|**0x8**|Ore|  
  
`[ @freq_subday_interval = ] freq_subday_interval` Numero di periodi di *freq_subday_type* da eseguire tra ogni esecuzione di un processo. *freq_subday_interval* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @freq_relative_interval = ] freq_relative_interval` Occorrenza di un processo di *freq_interval* ogni mese, se *freq_interval* è **32** (mensile relativo). *freq_relative_interval* è di **tipo int** e il valore predefinito è **0**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione (unità)|  
|-----------|--------------------------|  
|**1**|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
  
`[ @freq_recurrence_factor = ] freq_recurrence_factor` Numero di settimane o mesi che intercorre tra l'esecuzione pianificata di un processo. *freq_recurrence_factor* viene utilizzato solo se *freq_type* è **8**, **16** o **32**. *freq_recurrence_factor* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_start_date = ] active_start_date` Data in cui può iniziare l'esecuzione di un processo. *active_start_date* è di **tipo int** e il valore predefinito è null, che indica la data odierna. La data è nel formato AAAAMMGG. Se *active_start_date* non è null, la data deve essere maggiore o uguale a 19900101.  
  
 Al termine della creazione della pianificazione, esaminare la data di inizio per verificare che corrisponda alla data corretta. Per ulteriori informazioni, vedere la sezione "pianificazione della data di inizio" in [creare e alleghi pianificazioni ai processi](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
`[ @active_end_date = ] active_end_date` Data in cui l'esecuzione di un processo può essere arrestata. *active_end_date* è di **tipo int** e il valore predefinito è **99991231**, che indica il 31 dicembre 9999. La data è nel formato AAAAMMGG.  
  
`[ @active_start_time = ] active_start_time` Ora di ogni giorno tra *active_start_date* e *active_end_date* per iniziare l'esecuzione di un processo. *active_start_time* è di **tipo int** e il valore predefinito è 000000, che indica 12:00:00 A.M. nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @active_end_time = ] active_end_time` Tempo di ogni giorno tra *active_start_date* e *active_end_date* per terminare l'esecuzione di un processo. *active_end_time* è di **tipo int** e il valore predefinito è **235959**, che indica le 11:59:59. nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @owner_login_name = ] 'owner_login_name']` Nome dell'entità server proprietaria della pianificazione. *owner_login_name* è di **tipo sysname** e il valore predefinito è null, che indica che la pianificazione è di proprietà dell'autore.  
  
`[ @automatic_post = ] automatic_post` Riservati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Tutti i processi che utilizzano la pianificazione, adottano immediatamente le nuove impostazioni. Cambiando la pianificazione, tuttavia, non vengono arrestati i processi attualmente in esecuzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri del **ruolo sysadmin** possono modificare una pianificazione di proprietà di un altro utente.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene modificato lo stato abilitato della pianificazione `NightlyJobs` impostandolo su `0` e impostando il proprietario su `terrid`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_schedule  
    @name = 'NightlyJobs',  
    @enabled = 0,  
    @owner_login_name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e alconnessione di pianificazioni ai processi](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [Pianificare un processo](../../ssms/agent/schedule-a-job.md)   
 [Creare una pianificazione](../../ssms/agent/create-a-schedule.md)   
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobschedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
