---
description: sp_addpullsubscription_agent (Transact-SQL)
title: sp_addpullsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription_agent
- sp_addpullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addpullsubscription_agent
ms.assetid: b9c2eaed-6d2d-4b78-ae9b-73633133180b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 031911ae4e9f6f81e82ec9d0b4fb20c73d94a2cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206684"
---
# <a name="sp_addpullsubscription_agent-transact-sql"></a>sp_addpullsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 
  Aggiunge un nuovo processo pianificato dell'agente per la sincronizzazione di una sottoscrizione pull con una pubblicazione transazionale. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addpullsubscription_agent [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]          , [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor = ] 'distributor' ]  
    [ , [ @distribution_db = ] 'distribution_db' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]  
    [ , [ @distributor_login = ] 'distributor_login' ]  
    [ , [ @distributor_password = ] 'distributor_password' ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @distribution_jobid = ] distribution_jobid OUTPUT ]  
    [ , [ @encrypted_distributor_password = ] encrypted_distributor_password ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port = ] ftp_port ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]  
    [ , [ @working_directory = ] 'working_directory' ]  
    [ , [ @use_ftp = ] 'use_ftp' ]  
    [ , [ @publication_type = ] publication_type ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @offloadagent = ] 'remote_agent_activation' ]  
    [ , [ @offloadserver = ] 'remote_agent_server_name']  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  

> [!NOTE]
> Il nome del server può essere specificato come `<Hostname>,<PortNumber>` . Potrebbe essere necessario specificare il numero di porta per la connessione quando SQL Server viene distribuito in Linux o Windows con una porta personalizzata e il servizio browser è disabilitato.
  
`[ @publisher_db = ] 'publisher_db'_` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. *publisher_db* viene ignorato dai publisher Oracle.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome dell'istanza del Sottoscrittore o nome del listener del gruppo di disponibilità se il database del Sottoscrittore si trova in un gruppo di disponibilità. *Subscriber* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Modalità di sicurezza da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_security_mode* è di **tipo int e** il valore predefinito è null. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. **1** specifica l'autenticazione di Windows.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. L'agente di distribuzione si connette sempre al Sottoscrittore locale utilizzando l'autenticazione di Windows. Se per questo parametro viene specificato un valore diverso da NULL o **1** , viene restituito un messaggio di avviso.  
  
`[ @subscriber_login = ] 'subscriber_login'` Account di accesso del Sottoscrittore da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_login* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. Se viene specificato un valore per questo parametro, viene visualizzato un messaggio di avviso, ma il valore viene ignorato.  
  
`[ @subscriber_password = ] 'subscriber_password'` Password del Sottoscrittore. *subscriber_password* è obbligatorio se *subscriber_security_mode* è impostato su **0**. *subscriber_password* è di **tipo sysname** e il valore predefinito è null. Le password del Sottoscrittore vengono crittografate automaticamente.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. Se viene specificato un valore per questo parametro, viene visualizzato un messaggio di avviso, ma il valore viene ignorato.  
  
`[ @distributor = ] 'distributor'` Nome del server di distribuzione. *Distributor* è di **tipo sysname** e il valore predefinito è quello specificato dal *server di pubblicazione*.  
  
`[ @distribution_db = ] 'distribution_db'` Nome del database di distribuzione. *distribution_db* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Modalità di sicurezza da utilizzare per la connessione a un database di distribuzione durante la sincronizzazione. *distributor_security_mode* è di **tipo int** e il valore predefinito è **1**. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. **1** specifica l'autenticazione di Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Account di accesso del server di distribuzione da utilizzare per la connessione a un database di distribuzione durante la sincronizzazione. *distributor_login* è obbligatorio se *distributor_security_mode* è impostato su **0**. *distributor_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @distributor_password = ] 'distributor_password'` Password del server di distribuzione. *distributor_password* è obbligatorio se *distributor_security_mode* è impostato su **0**. *distributor_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa. Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @optional_command_line = ] 'optional_command_line'` Prompt dei comandi facoltativo fornito all'agente di distribuzione. Ad esempio, **-DefinitionFile** C:\Distdef.txt o **-CommitBatchSize** 10. *optional_command_line* è di **tipo nvarchar (4000)** e il valore predefinito è una stringa vuota.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare la agente di distribuzione. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Singola occorrenza|  
|**2** (impostazione predefinita)|On demand|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile relativa|  
|**64**|Avvio automatico|  
|**128**|Periodica|  
  
> [!NOTE]  
>  Se si specifica un valore **64** , il agente di distribuzione viene eseguito in modalità continua. Corrisponde all'impostazione del parametro **-Continuous** per l'agente. Per altre informazioni, vedere [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Valore da applicare alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e il valore predefinito è 1.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data del agente di distribuzione. Questo parametro viene usato quando *frequency_type* è impostato su **32** (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1** (impostazione predefinita)|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è **1**.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1** (impostazione predefinita)|Una sola volta|  
|**2**|Second|  
|**4**|Minuto|  
|**8**|Ora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è **1**.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di distribuzione viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di distribuzione viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di distribuzione, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di distribuzione pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @distribution_jobid = ] _distribution_jobidOUTPUT` ID del agente di distribuzione per questo processo. *distribution_jobid* è di tipo **Binary (16)** e il valore predefinito è null. si tratta di un parametro di output.  
  
`[ @encrypted_distributor_password = ] encrypted_distributor_password` L'impostazione di *encrypted_distributor_password* non è più supportata. Se si tenta di impostare questo parametro di **bit** su **1** , verrà generato un errore.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Indica se la sottoscrizione può essere sincronizzata tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestione sincronizzazione. *enabled_for_syncmgr* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **false**, la sottoscrizione non è registrata con Gestione sincronizzazione. Se **true**, la sottoscrizione viene registrata con Gestione sincronizzazione e può essere sincronizzata senza avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` Solo per compatibilità con le versioni precedenti.  
  
`[ @ftp_port = ] ftp_port` Solo per compatibilità con le versioni precedenti.  
  
`[ @ftp_login = ] 'ftp_login'` Solo per compatibilità con le versioni precedenti.  
  
`[ @ftp_password = ] 'ftp_password'` Solo per compatibilità con le versioni precedenti.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'_` Specifica la posizione della cartella alternativa per lo snapshot. *alternate_snapshot_folder* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @working_directory = ] 'working_director'` Nome della directory di lavoro utilizzata per archiviare i file di dati e di schema per la pubblicazione. *working_directory* è di **tipo nvarchar (255)** e il valore predefinito è null. Il nome deve essere specificato in formato UNC.  
  
`[ @use_ftp = ] 'use_ftp'` Specifica l'utilizzo di FTP anziché del protocollo normale per il recupero degli snapshot. *use_ftp* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
`[ @publication_type = ] publication_type` Specifica il tipo di replica della pubblicazione. *publication_type* è di un **tinyint** e il valore predefinito è **0**. Se è **0**, la pubblicazione è un tipo di transazione. Se è **1**, la pubblicazione è di tipo snapshot. Se è **2**, publication è di tipo merge.  
  
`[ @dts_package_name = ] 'dts_package_name'` Specifica il nome del pacchetto DTS. *dts_package_name* è di **tipo sysname** e il valore predefinito è null. Per specificare, ad esempio, il nome di pacchetto `DTSPub_Package`, il parametro deve essere `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Specifica la password per il pacchetto, se presente. *dts_package_password* è di **tipo sysname** e il valore predefinito è null, che indica che la password non è presente nel pacchetto.  
  
> [!NOTE]  
>  Se *dts_package_name* è specificato, è necessario specificare una password.  
  
`[ @dts_package_location = ] 'dts_package_location'` Specifica la posizione del pacchetto. *dts_package_location* è di **tipo nvarchar (12)** e il valore predefinito è **Subscriber**. Il percorso del pacchetto può essere **Distributor** o **Subscriber**.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  L'attivazione remota dell'agente è deprecata e non è più supportata. Questo parametro è supportato solo per compatibilità con gli script di versioni precedenti. Se si imposta *remote_agent_activation* su un valore diverso da **false** , verrà generato un errore.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  L'attivazione remota dell'agente è deprecata e non è più supportata. Questo parametro è supportato solo per compatibilità con gli script di versioni precedenti. Se si imposta *remote_agent_server_name* su un valore diverso da null, verrà generato un errore.  
  
`[ @job_name = ] 'job_name'` Nome di un processo di Agent esistente. *job_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando la sottoscrizione verrà sincronizzata mediante un processo esistente anziché un nuovo processo creato (impostazione predefinita). Se non si è membri del ruolo predefinito del server **sysadmin** , è necessario specificare *job_login* e *job_password* quando si specifica *job_name*.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e non prevede alcun valore predefinito. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al Sottoscrittore.  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addpullsubscription_agent** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addpullsubscription_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
