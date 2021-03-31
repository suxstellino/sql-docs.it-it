---
description: sp_addpushsubscription_agent (Transact-SQL)
title: sp_addpushsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpushsubscription_agent_TSQL
- sp_addpushsubscription_agent
helpviewer_keywords:
- sp_addpushsubscription_agent
ms.assetid: 1fdd2052-50d8-4318-8aa7-fc635d5cad18
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81bf487607d0cfea9f7ab623e12d732e4463aded
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937833"
---
# <a name="sp_addpushsubscription_agent-transact-sql"></a>sp_addpushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un nuovo processo di agente pianificato per sincronizzare una sottoscrizione push di una pubblicazione transazionale. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addpushsubscription_agent [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
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
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @distribution_job_name = ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @subscriber_provider = ] 'subscriber_provider' ]   
    [ , [ @subscriber_datasrc = ] 'subscriber_datasrc' ]   
    [ , [ @subscriber_location = ] 'subscriber_location' ]  
    [ , [ @subscriber_provider_string = ] 'subscriber_provider_string' ]   
    [ , [ @subscriber_catalog = ] 'subscriber_catalog' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome dell'istanza del Sottoscrittore o nome del listener del gruppo di disponibilità se il database del Sottoscrittore è un gruppo di disponibilità. *Subscriber* è di **tipo sysname** e il valore predefinito è null. 

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Il nome del server può essere specificato come `<Hostname>,<PortNumber>` . Potrebbe essere necessario specificare il numero di porta per la connessione quando SQL Server viene distribuito in Linux o Windows con una porta personalizzata e il servizio browser è disabilitato. L'uso di numeri di porta personalizzati per il server di distribuzione remoto si applica solo a SQL Server 2019.

::: moniker-end

`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e il valore predefinito è null. Per un Sottoscrittore non SQL Server, specificare il valore **(destinazione predefinita)** per *subscriber_db*.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Modalità di sicurezza da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_security_mode* è di **tipo int** e il valore predefinito è 1. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. **1** specifica l'autenticazione di Windows.  
  
> [!IMPORTANT]  
>  Per le sottoscrizioni ad aggiornamento in coda utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le connessioni ai Sottoscrittori e specificare un account diverso per la connessione a ogni Sottoscrittore. Per tutte le altre sottoscrizioni utilizzare l'autenticazione di Windows.  
  
`[ @subscriber_login = ] 'subscriber_login'` Account di accesso del Sottoscrittore da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscriber_password = ] 'subscriber_password'` Password del Sottoscrittore. *subscriber_password* è obbligatorio se *subscriber_security_mode* è impostato su **0**. *subscriber_password* è di **tipo sysname** e il valore predefinito è null. Le password del Sottoscrittore vengono crittografate automaticamente.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa. Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account con cui viene eseguito l'agente. In Azure SQL Istanza gestita usare un account di SQL Server. *job_login* è di **tipo nvarchar (257)** e il valore predefinito è null. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al server di distribuzione e per le connessioni al Sottoscrittore quando si utilizza l'autenticazione integrata di Windows.  
  
`[ @job_password = ] 'job_password'` Password per l'account con cui viene eseguito l'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @job_name = ] 'job_name'` Nome di un processo di Agent esistente. *job_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando la sottoscrizione verrà sincronizzata mediante un processo esistente anziché un nuovo processo creato (impostazione predefinita). Se non si è membri del ruolo predefinito del server **sysadmin** , è necessario specificare *job_login* e *job_password* quando si specifica *job_name*.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare la agente di distribuzione. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Singola occorrenza|  
|**2**|On demand|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile relativa|  
|**64** (impostazione predefinita)|Avvio automatico|  
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
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è 0.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4** (impostazione predefinita)|Minuto|  
|**8**|Ora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è 5.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di distribuzione viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di distribuzione viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è 235959.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di distribuzione, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di distribuzione pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è 99991231.  
  
`[ @dts_package_name = ] 'dts_package_name'` Specifica il nome del pacchetto Data Transformation Services (DTS). *dts_package_name* è di **tipo sysname** e il valore predefinito è null. Per specificare, ad esempio, il nome di pacchetto `DTSPub_Package`, il parametro deve essere `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Specifica la password necessaria per eseguire il pacchetto. *dts_package_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Se *dts_package_name* è specificato, è necessario specificare una password.  
  
`[ @dts_package_location = ] 'dts_package_location'` Specifica la posizione del pacchetto. *dts_package_location* è di **tipo nvarchar (12)** e il valore predefinito è Distributor. Il percorso del pacchetto può essere **Distributor** o **Subscriber**.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Indica se la sottoscrizione può essere sincronizzata tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestione sincronizzazione. *enabled_for_syncmgr* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **false**, la sottoscrizione non è registrata con Gestione sincronizzazione. Se **true**, la sottoscrizione viene registrata con Gestione sincronizzazione e può essere sincronizzata senza avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @distribution_job_name = ] 'distribution_job_name'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscriber_provider = ] 'subscriber_provider'` Identificatore a livello di codice (PROGID) univoco con il quale viene registrato il provider di OLE DB per l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] origine dati non. *subscriber_provider* è di **tipo sysname** e il valore predefinito è null. *subscriber_provider* deve essere univoco per il provider di OLE DB installato nel server di distribuzione. *subscriber_provider* è supportato solo per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
`[ @subscriber_datasrc = ] 'subscriber_datasrc'` Nome dell'origine dati come riconosciuto dal provider OLE DB. *subscriber_datasrc* è di **tipo nvarchar (4000)** e il valore predefinito è null. *subscriber_datasrc* viene passato come proprietà DBPROP_INIT_DATASOURCE per inizializzare il provider di OLE DB. *subscriber_datasrc* è supportato solo per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
`[ @subscriber_location = ] 'subscriber_location'` È il percorso del database riconosciuto dal provider OLE DB. *subscriber_location* è di **tipo nvarchar (4000)** e il valore predefinito è null. *subscriber_location* viene passato come proprietà DBPROP_INIT_LOCATION per inizializzare il provider di OLE DB. *subscriber_location* è supportato solo per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
`[ @subscriber_provider_string = ] 'subscriber_provider_string'` Stringa di connessione specifica del provider OLE DB che identifica l'origine dati. *subscriber_provider_string* è di **tipo nvarchar (4000)** e il valore predefinito è null. *subscriber_provider_string* viene passato a IDataInitialize o impostato come proprietà DBPROP_INIT_PROVIDERSTRING per inizializzare il provider di OLE DB. *subscriber_provider_string* è supportato solo per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
`[ @subscriber_catalog = ] 'subscriber_catalog'` Catalogo da utilizzare quando si effettua una connessione al provider OLE DB. *subscriber_catalog* è di **tipo sysname** e il valore predefinito è null. *subscriber_catalog* viene passato come proprietà DBPROP_INIT_CATALOG per inizializzare il provider di OLE DB. *subscriber_catalog* è supportato solo per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addpushsubscription_agent** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addpushsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addpushsubscription_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Creare una sottoscrizione per un Sottoscrittore non SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
