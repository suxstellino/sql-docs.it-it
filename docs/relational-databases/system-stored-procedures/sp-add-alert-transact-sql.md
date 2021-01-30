---
description: sp_add_alert (Transact-SQL)
title: sp_add_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_alert
- sp_add_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_alert
ms.assetid: d9b41853-e22d-4813-a79f-57efb4511f09
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b6d7d1e451949d17dc7e4637dc82b86c4225f06c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186042"
---
# <a name="sp_add_alert-transact-sql"></a>sp_add_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un avviso.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_alert [ @name = ] 'name'   
     [ , [ @message_id = ] message_id ]   
     [ , [ @severity = ] severity ]   
     [ , [ @enabled = ] enabled ]  
     [ , [ @delay_between_responses = ] delay_between_responses ]   
     [ , [ @notification_message = ] 'notification_message' ]   
     [ , [ @include_event_description_in = ] include_event_description_in ]   
     [ , [ @database_name = ] 'database' ]   
     [ , [ @event_description_keyword = ] 'event_description_keyword_pattern' ]   
     [ , { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ]   
     [ , [ @raise_snmp_trap = ] raise_snmp_trap ]   
     [ , [ @performance_condition = ] 'performance_condition' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome dell'avviso. Tale nome viene visualizzato nel messaggio di posta elettronica o di cercapersone inviato in risposta all'avviso. Deve essere univoco e può contenere il carattere di percentuale ( **%** ). *Name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @message_id = ] message_id` Numero di errore del messaggio che definisce l'avviso. Corrisponde in genere a un numero di errore nella tabella **sysmessages** . *message_id* è di **tipo int** e il valore predefinito è **0**. Se per la definizione dell'avviso viene utilizzata la *gravità* , *message_id* deve essere **0** o null.  
  
> [!NOTE]  
>  Solo gli errori **sysmessages** scritti nel registro applicazioni di Microsoft Windows possono causare l'invio di un avviso.  
  
`[ @severity = ] severity` Livello di gravità (da **1** a **25**) che definisce l'avviso. Qualsiasi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] messaggio archiviato nella tabella **sysmessages** inviato al [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro applicazioni di Windows con la gravità indicata comporta l'invio dell'avviso. *gravità* è di **tipo int** e il valore predefinito è 0. Se *message_id* viene utilizzato per definire l'avviso, la *gravità* deve essere **0**.  
  
`[ @enabled = ] enabled` Indica lo stato corrente dell'avviso. *Enabled* è di **tinyint** e il valore predefinito è 1 (abilitato). Se è **0**, l'avviso non è abilitato e non viene attivato.  
  
`[ @delay_between_responses = ] delay_between_responses` Periodo di attesa, in secondi, tra le risposte all'avviso. *delay_between_responses* è di **tipo int** e il valore predefinito è **0**, che indica che non esiste alcuna attesa tra le risposte. ogni occorrenza dell'avviso genera una risposta. La risposta può assumere una delle due forme seguenti:  
  
-   Una o più notifiche inviate tramite posta elettronica o cercapersone.  
  
-   Un processo da eseguire.  
  
 L'impostazione di tale valore impedisce, ad esempio, l'invio di più messaggi di posta elettronica quando un avviso viene generato ripetutamente in un breve periodo di tempo.  
  
`[ @notification_message = ] 'notification_message'` Messaggio aggiuntivo facoltativo inviato all'operatore come parte della notifica tramite posta elettronica, **net send** o cercapersone. *notification_message* è di **tipo nvarchar (512)** e il valore predefinito è null. Specificare *notification_message* è utile per aggiungere note speciali, ad esempio procedure correttive.  
  
`[ @include_event_description_in = ] include_event_description_in` Indica se la descrizione dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] errore deve essere inclusa come parte del messaggio di notifica. *include_event_description_in* è di **tinyint** e il valore predefinito è **5** (posta elettronica e **net send**) e può includere uno o più di questi valori combinati con un operatore logico **or** .  
  
> [!IMPORTANT]
>  Le opzioni Cercapersone e **net send** verranno rimosse da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in una versione futura di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare pertanto di utilizzarle in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui sono state implementate.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|nessuno|  
|**1**|Posta elettronica|  
|**2**|Cercapersone|  
|**4**|**net send**|  
  
`[ @database_name = ] 'database'` Database in cui deve verificarsi l'errore affinché l'avviso venga generato. Se il *database* non viene specificato, l'avviso viene attivato indipendentemente dalla posizione in cui si è verificato l'errore. il *database* è di **tipo sysname**. I nomi racchiusi tra parentesi quadre ([ ]) non sono ammessi. Il valore predefinito è NULL.  
  
`[ @event_description_keyword = ] 'event_description_keyword_pattern'` Sequenza di caratteri che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere simile alla descrizione dell'errore. È possibile utilizzare i caratteri dei criteri di ricerca dell'espressione LIKE [!INCLUDE[tsql](../../includes/tsql-md.md)]. *event_description_keyword_pattern* è di **tipo nvarchar (100)** e il valore predefinito è null. Questo parametro è utile per filtrare i nomi degli oggetti (ad esempio, **% customer_table%**).  
  
`[ @job_id = ] job_id` Numero di identificazione del processo da eseguire in risposta a questo avviso. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo da eseguire in risposta a questo avviso. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` Non implementato nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione 7,0. *raise_snmp_trap* è di **tinyint** e il valore predefinito è 0.  
  
`[ @performance_condition = ] 'performance_condition'` È un valore espresso nel formato '*itemcomparatorvalue*'. *performance_condition* è di **tipo nvarchar (512)** e il valore predefinito è null ed è costituito da questi elementi.  
  
|Componente del formato|Descrizione|  
|--------------------|-----------------|  
|*Item*|Oggetto prestazioni, contatore delle prestazioni o istanza denominata del contatore|  
|*Confronto*|Uno di questi operatori: >, < o =|  
|*Valore*|Valore numerico del contatore|  
  
`[ @category_name = ] 'category'` Nome della categoria di avvisi. *Category* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @wmi_namespace = ] 'wmi_namespace'` Spazio dei nomi WMI in cui eseguire query per gli eventi. *wmi_namespace* è di **tipo sysname** e il valore predefinito è null. Sono supportati solo gli spazi di nomi nel server locale.  
  
`[ @wmi_query = ] 'wmi_query'` Query che specifica l'evento WMI per l'avviso. *wmi_query* è di **tipo nvarchar (512)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_add_alert** deve essere eseguito dal database **msdb** .  
  
 Di seguito sono descritti i casi in cui gli errori/messaggi generati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e da applicazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono inviati al registro applicazioni di Windows in modo da poter generare avvisi:  
  
-   Errori **sys. messages** con gravità 19 o superiore  
  
-   Qualsiasi istruzione RAISERROR richiamata con la clausola WITH LOG  
  
-   Qualsiasi errore **sys. messages** modificato o creato con **sp_altermessage**  
  
-   Qualsiasi evento registrato utilizzando **xp_logevent**  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include un semplice strumento grafico per la gestione del sistema di avvisi ed è lo strumento consigliato per la configurazione di un'infrastruttura di avvisi.  
  
 Se un avviso non funziona adeguatamente, controllare se:  
  
-   Il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è in esecuzione.  
  
-   L'evento è incluso nel registro applicazioni di Windows.  
  
-   L'avviso è attivato.  
  
-   Gli eventi generati con la stored procedure **xp_logevent** si verificano nel database master. Pertanto, **xp_logevent** genera un avviso solo se **\@database_name** per l'avviso è **'master'** o NULL.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_alert**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene aggiunto un avviso (Test Alert) che esegue il processo `Back up the AdventureWorks2012 Database` quando viene generato.  
  
> [!NOTE]  
>  Nell'esempio si presuppone che il messaggio 55001 e il processo `Back up the AdventureWorks2012 Database` siano già esistenti. Questo esempio viene fornito solo a scopo illustrativo.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_alert  
    @name = N'Test Alert',  
    @message_id = 55001,   
   @severity = 0,   
   @notification_message = N'Error 55001 has occurred. The database will be backed up...',   
   @job_name = N'Back up the AdventureWorks2012 Database' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_altermessage &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_delete_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)   
 [sp_help_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [sp_update_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [sys.sysPerfInfo &#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
