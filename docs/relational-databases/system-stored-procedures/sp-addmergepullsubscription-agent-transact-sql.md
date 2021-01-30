---
description: sp_addmergepullsubscription_agent (Transact-SQL)
title: sp_addmergepullsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepullsubscription_agent
- sp_addmergepullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addmergepullsubscription_agent
ms.assetid: a2f4b086-078d-49b5-8971-8a1e3f6a6feb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ecc9b2fded678671bc608235fa3080f2ed31573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209432"
---
# <a name="sp_addmergepullsubscription_agent-transact-sql"></a>sp_addmergepullsubscription_agent (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un nuovo processo agente utilizzato per pianificare la sincronizzazione di una sottoscrizione pull con una pubblicazione di tipo merge. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergepullsubscription_agent [ [ @name = ] 'name' ]   
        , [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication =] 'publication'   
    [ , [ @publisher_security_mod e= ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher_encrypted_password = ] publisher_encrypted_password ]   
    [ , [ @subscriber = ] 'subscriber' ]   
    [ , [ @subscriber_db = ] 'subscriber_db' ]   
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]   
    [ , [ @subscriber_login = ] 'subscriber_login' ]   
    [ , [ @subscriber_password= ] 'subscriber_password' ]   
    [ , [ @distributor = ] 'distributor' ]   
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @encrypted_password = ] encrypted_password ]   
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
    [ , [ @optional_command_line = ] 'optional_command_line' ]   
    [ , [ @merge_jobid = ] merge_jobid ]   
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]    
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @working_directory = ] 'working_directory' ]   
    [ , [ @use_ftp = ] 'use_ftp' ]   
    [ , [ @reserved = ] 'reserved' ]   
    [ , [ @use_interactive_resolver = ] 'use_interactive_resolver' ]   
    [ , [ @offloadagent = ] 'remote_agent_activation' ]   
    [ , [ @offloadserver = ] 'remote_agent_server_name']   
    [ , [ @job_name = ] 'job_name' ]   
    [ , [ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]  
    [ , [ @use_web_sync = ] use_web_sync ]  
        [ , [ @internet_url = ] 'internet_url' ]  
    [ , [ @internet_login = ] 'internet_login' ]  
        [ , [ @internet_password = ] 'internet_password' ]  
    [ , [ @internet_security_mode = ] internet_security_mode ]  
        [ , [ @internet_timeout = ] internet_timeout ]  
    [ , [ @hostname = ] 'hostname' ]  
        [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome dell'agente. *Name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza da utilizzare per la connessione a un server di pubblicazione durante la sincronizzazione. *publisher_security_mode* è di **tipo int** e il valore predefinito è 1. Se è **0**, viene specificata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. Se è **1**, viene specificata l'autenticazione di Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso da utilizzare per la connessione a un server di pubblicazione durante la sincronizzazione. *publisher_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @publisher_encrypted_password = ]publisher_encrypted_password` L'impostazione di *publisher_encrypted_password* non è più supportata. Se si tenta di impostare questo parametro di **bit** su **1** , verrà generato un errore.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Modalità di sicurezza da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_security_mode* è di **tipo int** e il valore predefinito è 1. Se è **0**, viene specificata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. Se è **1**, viene specificata l'autenticazione di Windows.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. L'agente di merge si connette sempre al Sottoscrittore locale utilizzando l'autenticazione di Windows. Se si specifica un valore per questo parametro, viene restituito un messaggio di avviso, ma il valore viene ignorato.  
  
`[ @subscriber_login = ] 'subscriber_login'` Account di accesso del Sottoscrittore da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_login* è obbligatorio se *subscriber_security_mode* è impostato su **0**. *subscriber_login* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. Se si specifica un valore per questo parametro, viene restituito un messaggio di avviso, ma il valore viene ignorato.  
  
`[ @subscriber_password = ] 'subscriber_password'` Password del Sottoscrittore per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione. *subscriber_password* è obbligatorio se *subscriber_security_mode* è impostato su **0**. *subscriber_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. Se si specifica un valore per questo parametro, viene restituito un messaggio di avviso, ma il valore viene ignorato.  
  
`[ @distributor = ] 'distributor'` Nome del server di distribuzione. *Distributor* è di **tipo sysname** e il valore predefinito è *Publisher*. ovvero, il server di pubblicazione è anche il server di distribuzione.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Modalità di sicurezza da utilizzare per la connessione a un database di distribuzione durante la sincronizzazione. *distributor_security_mode* è di **tipo int** e il valore predefinito è 0. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. **1** specifica l'autenticazione di Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Account di accesso del server di distribuzione da utilizzare per la connessione a un database di distribuzione durante la sincronizzazione. *distributor_login* è obbligatorio se *distributor_security_mode* è impostato su **0**. *distributor_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @distributor_password = ] 'distributor_password'` Password del server di distribuzione. *distributor_password* è obbligatorio se *distributor_security_mode* è impostato su **0**. *distributor_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @encrypted_password = ] encrypted_password` L'impostazione di *encrypted_password* non è più supportata. Se si tenta di impostare questo parametro di **bit** su **1** , verrà generato un errore.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare la agente di merge. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Singola occorrenza|  
|**2**|On demand|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile relativa|  
|**64**|Avvio automatico|  
|**128**|Periodica|  
|NULL (predefinito)||  
  
> [!NOTE]  
>  Se si specifica un valore **64** , il agente di merge viene eseguito in modalità continua. Corrisponde all'impostazione del parametro **-Continuous** per l'agente. Per altre informazioni, vedere [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
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
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data del agente di merge. Questo parametro viene usato quando *frequency_type* è impostato su **32** (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
|NULL (predefinito)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4**|Minuto|  
|**8**|Ora|  
|NULL (predefinito)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di merge viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di merge viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di merge, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di merge pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @optional_command_line = ] 'optional_command_line'` Prompt dei comandi facoltativo fornito all'agente di merge. *optional_command_line* è di **tipo nvarchar (255)** e il valore predefinito è''. Può essere utilizzato per rendere disponibili ulteriori parametri all'agente di merge, come nell'esempio seguente, in cui il timeout predefinito per le query viene aumentato a `600`:  
  
```  
@optional_command_line = N'-QueryTimeOut 600'  
```  
  
`[ @merge_jobid = ] merge_jobid` Parametro di output per l'ID del processo. *merge_jobid* è di tipo **Binary (16)** e il valore predefinito è null.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Specifica se la sottoscrizione può essere sincronizzata tramite Gestione sincronizzazione Microsoft Windows. *enabled_for_syncmgr* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **false**, la sottoscrizione non è registrata con Gestione sincronizzazione. Se **true**, la sottoscrizione viene registrata con Gestione sincronizzazione e può essere sincronizzata senza avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` Solo per compatibilità con le versioni precedenti.  
  
`[ @ftp_port = ] ftp_port` Solo per compatibilità con le versioni precedenti.  
  
`[ @ftp_login = ] 'ftp_login'` Solo per compatibilità con le versioni precedenti.  
  
`[ @ftp_password = ] 'ftp_password'` Solo per compatibilità con le versioni precedenti.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` Specifica la posizione da cui prelevare i file di snapshot. *alternate_snapshot_folder* è di **tipo nvarchar (255)** e il valore predefinito è null. che indica che i file di snapshot vengono prelevati dal percorso predefinito specificato dal server di pubblicazione.  
  
`[ @working_directory = ] 'working_directory'` Nome della directory di lavoro utilizzata per archiviare temporaneamente i file di dati e di schema per la pubblicazione quando si utilizza FTP per trasferire i file di snapshot. *working_directory* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @use_ftp = ] 'use_ftp'` Specifica l'utilizzo di FTP anziché del protocollo tipico per il recupero degli snapshot. *use_ftp* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver' ]` Usa il sistema di risoluzione interattivo per risolvere i conflitti per tutti gli articoli che consentono la risoluzione interattiva. *use_interactive_resolver* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  L'attivazione remota dell'agente è deprecata e non è più supportata. Questo parametro è supportato solo per compatibilità con gli script di versioni precedenti. Se si imposta *remote_agent_activation* su un valore diverso da **false** , verrà generato un errore.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  L'attivazione remota dell'agente è deprecata e non è più supportata. Questo parametro è supportato solo per compatibilità con gli script di versioni precedenti. Se si imposta *remote_agent_server_name* su un valore diverso da null, verrà generato un errore.  
  
`[ @job_name = ] 'job_name' ]` Nome di un processo di Agent esistente. *job_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando la sottoscrizione verrà sincronizzata mediante un processo esistente anziché un nuovo processo creato (impostazione predefinita). Se non si è membri del ruolo predefinito del server **sysadmin** , è necessario specificare *job_login* e *job_password* quando si specifica *job_name*.  
  
`[ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]` Percorso della cartella da cui verranno letti i file di snapshot se è necessario utilizzare uno snapshot dei dati filtrati. *dynamic_snapshot_location* è di **tipo nvarchar (260)** e il valore predefinito è null. Per altre informazioni sui filtri di riga con parametri, vedere [Filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
`[ @use_web_sync = ] use_web_sync` Indica che la sincronizzazione Web è abilitata. *use_web_sync* è di **bit** e il valore predefinito è 0. **1** specifica che la sottoscrizione pull può essere sincronizzata tramite Internet tramite http.  
  
`[ @internet_url = ] 'internet_url'` Posizione del listener per la replica (REPLISAPI.DLL) per la sincronizzazione Web. *internet_url* è di **tipo nvarchar (260)** e il valore predefinito è null. *internet_url* è un URL completo nel formato `http://server.domain.com/directory/replisapi.dll` . Se il server è configurato per l'attesa su una porta diversa dalla porta 80, è necessario specificare anche il numero di porta nel formato `http://server.domain.com:portnumber/directory/replisapi.dll`, dove `portnumber` rappresenta la porta.  
  
`[ @internet_login = ] 'internet_login'` Account di accesso utilizzato dal agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite l'autenticazione di base HTTP. *internet_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @internet_password = ] 'internet_password'` Password utilizzata dal agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite l'autenticazione di base HTTP. *internet_password* è di **tipo nvarchar (524)** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
`[ @internet_security_mode = ] internet_security_mode` Metodo di autenticazione utilizzato dal agente di merge durante la connessione al server Web durante la sincronizzazione Web tramite HTTPS. *internet_security_mode* è di **tipo int** . i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Viene utilizzata l'autenticazione di base.|  
|**1** (impostazione predefinita)|Viene utilizzata l'autenticazione integrata di Windows.|  
  
> [!NOTE]  
>  È consigliabile utilizzare l'autenticazione di base per la sincronizzazione Web. Per utilizzare la sincronizzazione Web, è necessario effettuare una connessione TLS al server Web. Per altre informazioni, vedere [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md).  
  
`[ @internet_timeout = ] internet_timeout` Periodo di tempo, in secondi, prima della scadenza di una richiesta di sincronizzazione Web. *internet_timeout* è di **tipo int** e il valore predefinito è **300** secondi.  
  
`[ @hostname = ] 'hostname'` Esegue l'override del valore di HOST_NAME () quando questa funzione viene utilizzata nella clausola WHERE di un filtro con parametri. *hostname* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e non prevede alcun valore predefinito. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al Sottoscrittore e per le connessioni al server di distribuzione e al server di pubblicazione quando viene utilizzata l'autenticazione integrata di Windows.  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per una sicurezza ottimale, i nomi e le password degli account di accesso dovrebbero essere passati in fase di esecuzione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergepullsubscription_agent** viene utilizzata nella replica di tipo merge e utilizza funzionalità simili a [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md).  
  
 Per un esempio di come specificare correttamente le impostazioni di sicurezza durante l'esecuzione di **sp_addmergepullsubscription_agent**, vedere [creare una sottoscrizione pull](../../relational-databases/replication/create-a-pull-subscription.md).  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_1_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergepullsubscription_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
