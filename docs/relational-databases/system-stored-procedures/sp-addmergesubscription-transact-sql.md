---
description: sp_addmergesubscription (Transact-SQL)
title: sp_addmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergesubscription_TSQL
- sp_addmergesubscription
helpviewer_keywords:
- sp_addmergesubscription
ms.assetid: a191d817-0132-49ff-93ca-76f13e609b38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d6689fed03126a8e5bc65d8d9e7e556a070ec483
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209390"
---
# <a name="sp_addmergesubscription-transact-sql"></a>sp_addmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una sottoscrizione push o pull di tipo merge. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergesubscription [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db= ] 'subscriber_db' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @subscriber_type= ] 'subscriber_type' ]  
    [ , [ @subscription_priority= ] subscription_priority ]  
    [ , [ @sync_type= ] 'sync_type' ]  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @optional_command_line= ] 'optional_command_line' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @use_interactive_resolver= ] 'use_interactive_resolver' ]  
    [ , [ @merge_job_name= ] 'merge_job_name' ]  
    [ , [ @hostname = ] 'hostname'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario che la pubblicazione esista già.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscription_type = ] 'subscription_type'` Tipo di sottoscrizione. *subscription_type* è di **tipo nvarchar (15)** e il valore predefinito è push. Se **push**, viene aggiunta una sottoscrizione push e il agente di merge viene aggiunto al server di distribuzione. Se **pull**, viene aggiunta una sottoscrizione pull senza aggiungere un agente di merge nel server di distribuzione.  
  
> [!NOTE]  
>  Con le sottoscrizioni anonime non è necessario utilizzare questa stored procedure.  
  
`[ @subscriber_type = ] 'subscriber_type'` Tipo di Sottoscrittore. *subscriber_type* è di **tipo nvarchar (15)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**locale** (impostazione predefinita)|Sottoscrittore noto solo al server di pubblicazione.|  
|**globale**|Sottoscrittore noto a tutti i server.|  
  
 In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive le sottoscrizioni locali vengono dette sottoscrizioni client e le sottoscrizioni globali vengono dette sottoscrizioni server.  
  
`[ @subscription_priority = ] subscription_priority` Numero che indica la priorità della sottoscrizione. *subscription_priority* è **reale** e il valore predefinito è null. Per le sottoscrizioni locali e anonime, il livello di priorità è 0.0. Per le sottoscrizioni globali, la priorità deve essere inferiore a 100.0.  
  
`[ @sync_type = ] 'sync_type'` Tipo di sincronizzazione della sottoscrizione. *sync_type* è di **tipo nvarchar (15)** e il valore predefinito è **Automatic**. Può essere **automatico** o **None**. Se **automatico**, lo schema e i dati iniziali per le tabelle pubblicate vengono trasferiti per primi nel Sottoscrittore. Se non è presente **alcun** valore, si presuppone che il Sottoscrittore disponga già dello schema e dei dati iniziali per le tabelle pubblicate. Le tabelle e i dati di sistema vengono sempre trasferiti.  
  
> [!NOTE]  
>  Si consiglia di non specificare il valore **None**.  
  
`[ @frequency_type = ] frequency_type` Valore che indica quando verrà eseguito il agente di merge. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**10**|Ogni mese|  
|**20**|Mensile, in base all'intervallo di frequenza|  
|**40**|All'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent|  
|NULL (predefinito)||  
  
`[ @frequency_interval = ] frequency_interval` Giorno o giorni in cui viene eseguita la agente di merge. *frequency_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Sunday|  
|**2**|Monday|  
|**3**|Tuesday|  
|**4**|Wednesday|  
|**5**|Thursday|  
|**6**|Friday|  
|**7**|Sabato|  
|**8**|Giorno|  
|**9**|Giorni feriali|  
|**10**|Giorni festivi|  
|NULL (predefinito)||  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Occorrenza di merge pianificata dell'intervallo di frequenza in ogni mese. *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
|NULL (predefinito)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_subday = ] frequency_subday` Unità per *frequency_subday_interval*. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4**|Minuto|  
|**8**|Ora|  
|NULL (predefinito)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Frequenza di *frequency_subday* tra le operazioni di merge. *frequency_subday_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di merge viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di merge viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di merge, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di merge pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @optional_command_line = ] 'optional_command_line'` Prompt dei comandi facoltativo da eseguire. *optional_command_line* è di **tipo nvarchar (4000)** e il valore predefinito è null. Questo parametro viene utilizzato per aggiungere un comando per l'acquisizione e il salvataggio dell'output in un file o per specificare un file o un attributo di configurazione.  
  
`[ @description = ] 'description'` Breve descrizione della sottoscrizione di tipo merge. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null. Questo valore viene visualizzato da monitoraggio replica nella colonna **nome descrittivo** , che può essere utilizzato per ordinare le sottoscrizioni per una pubblicazione monitorata.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Specifica se la sottoscrizione può essere sincronizzata tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestione sincronizzazione Microsoft Windows. *enabled_for_syncmgr* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **false**, la sottoscrizione non è registrata con Gestione sincronizzazione. Se **true**, la sottoscrizione viene registrata con Gestione sincronizzazione e può essere sincronizzata senza avviare [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @offloadagent = ] remote_agent_activation` Specifica che l'agente può essere attivato in remoto. *remote_agent_activation* è di **bit** e il valore predefinito è **0**.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti.  
  
`[ @offloadserver = ] 'remote_agent_server_name'` Specifica il nome di rete del server da utilizzare per l'attivazione remota dell'agente. *remote_agent_server_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver'` Consente di risolvere i conflitti in modo interattivo per tutti gli articoli che consentono la risoluzione interattiva. *use_interactive_resolver* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
`[ @merge_job_name = ] 'merge_job_name'`Il parametro *\@ merge_job_name* è deprecato e non può essere impostato. *merge_job_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @hostname = ] 'hostname'` Esegue l'override del valore restituito da [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) quando questa funzione viene utilizzata nella clausola WHERE di un filtro con parametri. *Hostname* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Per motivi relativi alle prestazioni è consigliabile evitare di applicare funzioni ai nomi di colonna nelle clausole per filtri di riga con parametri, come `LEFT([MyColumn]) = SUSER_SNAME()`. Se si usa [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) in una clausola di filtro e si sostituisce il valore di HOST_NAME, potrebbe essere necessario convertire i tipi di dati tramite [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Per altre informazioni sulle procedure consigliate in questo caso, vedere la sezione relativa alla sostituzione del valore HOST_NAME() nell'argomento [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergesubscription** viene utilizzata nella replica di tipo merge.  
  
 Quando **sp_addmergesubscription** viene eseguita da un membro del ruolo predefinito del server **sysadmin** per creare una sottoscrizione push, il processo di agente di merge viene creato in modo implicito e viene eseguito con l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio Agent. Si consiglia di eseguire [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) e specificare le credenziali di un account di Windows diverso, specifico dell'agente per **\@ job_login** e **\@ job_password**. Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergesubscription-_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergesubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Risoluzione interattiva dei conflitti](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_changemergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  
