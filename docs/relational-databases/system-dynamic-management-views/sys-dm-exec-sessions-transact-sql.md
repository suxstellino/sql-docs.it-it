---
description: sys.dm_exec_sessions (Transact-SQL)
title: sys.dm_exec_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_sessions_TSQL
- sys.dm_exec_sessions
- dm_exec_sessions
- sys.dm_exec_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sessions dynamic management view
ms.assetid: 2b7e8e0c-eea0-431e-819f-8ccd12ec8cfa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 49f04a21e95c0cc7a63b97fd9fd4add1729b25a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098981"
---
# <a name="sysdm_exec_sessions-transact-sql"></a>sys.dm_exec_sessions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni sessione autenticata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_exec_sessions è una vista con ambito server che contiene informazioni su tutte le attività interne e le connessioni utente attive. Tali informazioni includono la versione del client, il nome del programma client, l'ora di accesso del client, l'utente che esegue l'accesso, l'impostazione di sessione corrente e altro. Utilizzare sys.dm_exec_sessions prima di tutto per visualizzare il carico di sistema corrente e individuare una sessione di interesse, quindi per raccogliere ulteriori informazioni su tale sessione utilizzando altre viste o funzioni a gestione dinamica.  
  
 Le viste a gestione dinamica sys.dm_exec_connections, sys.dm_exec_sessions e sys.dm_exec_requests vengono mappate alla tabella di sistema [sys.sysprocessi](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) .  
  
> **Nota:** Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_exec_sessions**.  
  
|Nome colonna|Tipo di dati|Descrizione e informazioni specifiche della versione|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifica la sessione associata a ogni connessione principale attiva. Non ammette i valori Null.|  
|login_time|**datetime**|Data e ora in cui è stata stabilita la sessione. Non ammette i valori Null. Le sessioni per le quali non è stata completata l'accesso nel momento in cui viene eseguita una query sulla DMV vengono visualizzate con l'ora di accesso `1900-01-01` .|  
|host_name|**nvarchar(128)**|Nome della workstation client specifica di una sessione. Il valore è NULL per le sessioni interne. Ammette i valori Null.<br /><br /> **Nota sulla sicurezza:** L'applicazione client fornisce il nome della workstation e può fornire dati non accurati. Non considerare HOST_NAME una caratteristica di sicurezza.|  
|program_name|**nvarchar(128)**|Nome del programma client che ha iniziato la sessione. Il valore è NULL per le sessioni interne. Ammette i valori Null.|  
|host_process_id|**int**|ID di processo del programma client che ha iniziato la sessione. Il valore è NULL per le sessioni interne. Ammette i valori Null.|  
|client_version|**int**|Versione del protocollo TDS dell'interfaccia utilizzata dal client per connettersi al server. Il valore è NULL per le sessioni interne. Ammette i valori Null.|  
|client_interface_name|**nvarchar(32)**|Nome della libreria/driver utilizzato dal client per comunicare con il server. Il valore è NULL per le sessioni interne. Ammette i valori Null.|  
|security_id|**varbinary(85)**|ID di sicurezza di Microsoft Windows associato all'account di accesso. Non ammette i valori Null.|  
|login_name|**nvarchar(128)**|Nome dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con cui la sessione è attualmente in esecuzione. Per il nome dell'account di accesso originale che ha creato la sessione, vedere original_login_name. Può essere un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nome di account di accesso autenticato o un nome utente di dominio autenticato di Windows. Non ammette i valori Null.|  
|nt_domain|**nvarchar(128)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Dominio di Windows per il client se la sessione utilizza l'autenticazione di Windows o una connessione trusted. Il valore è NULL per le sessioni interne e per gli utenti non di dominio. Ammette i valori Null.|  
|nt_user_name|**nvarchar(128)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Nome utente di Windows per il client se la sessione utilizza l'autenticazione di Windows o una connessione trusted. Il valore è NULL per le sessioni interne e per gli utenti non di dominio. Ammette i valori Null.|  
|status|**nvarchar(30)**|Stato della sessione. Valori possibili:<br /><br /> **Running**: una o più richieste sono in esecuzione<br /><br /> **Sleeping**: nessuna richiesta è in esecuzione<br /><br />  La sessione inattiva è stata reimpostata a causa del pool di connessioni ed è ora in stato di preaccesso.<br /><br /> **Preconnect**: la sessione si trova nel classificatore di Resource Governor.<br /><br /> Non ammette i valori Null.|  
|context_info|**varbinary(128)**|Valore di CONTEXT_INFO per la sessione. Le informazioni di contesto vengono impostate dall'utente utilizzando l'istruzione [set CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) . Ammette i valori Null.|  
|cpu_time|**int**|Tempo della CPU, espresso in millisecondi, utilizzato dalla sessione. Non ammette i valori Null.|  
|memory_usage|**int**|Numero di pagine da 8 KB utilizzate dalla sessione. Non ammette i valori Null.|  
|total_scheduled_time|**int**|Tempo totale, espresso in millisecondi, pianificato per l'esecuzione delle richieste nella sessione. Non ammette i valori Null.|  
|total_elapsed_time|**int**|Tempo, espresso in millisecondi, trascorso dal momento in cui è stata stabilita la sessione. Non ammette i valori Null.|  
|endpoint_id|**int**|ID dell'endpoint associato alla sessione. Non ammette i valori Null.|  
|last_request_start_time|**datetime**|Data e ora in cui è iniziata l'ultima richiesta nella sessione. Include la richiesta attualmente in esecuzione. Non ammette i valori Null.|  
|last_request_end_time|**datetime**|Data e ora dell'ultimo completamento di una richiesta nella sessione. Ammette i valori Null.|  
|reads|**bigint**|Numero di letture eseguite dalle richieste della sessione durante la sessione. Non ammette i valori Null.|  
|writes|**bigint**|Numero di scritture eseguite dalle richieste della sessione durante la sessione. Non ammette i valori Null.|  
|logical_reads|**bigint**|Numero di letture logiche eseguite nella sessione. Non ammette i valori Null.|  
|is_user_process|**bit**|0 se la sessione è una sessione di sistema. Negli altri casi è 1. Non ammette i valori Null.|  
|text_size|**int**|Impostazione di TEXTSIZE per la sessione. Non ammette i valori Null.|  
|Linguaggio|**nvarchar(128)**|Impostazione di LANGUAGE per la sessione. Ammette i valori Null.|  
|date_format|**nvarchar (3)**|Impostazione DATEFORMAT per la sessione. Ammette i valori Null.|  
|date_first|**smallint**|Impostazione DATEFIRST per la sessione. Non ammette i valori Null.|  
|quoted_identifier|**bit**|Impostazione QUOTED_IDENTIFIER per la sessione. Non ammette i valori Null.|  
|arithabort|**bit**|Impostazione ARITHABORT per la sessione. Non ammette i valori Null.|  
|ansi_null_dflt_on|**bit**|Impostazione ANSI_NULL_DFLT_ON per la sessione. Non ammette i valori Null.|  
|ansi_defaults|**bit**|Impostazione ANSI_DEFAULTS per la sessione. Non ammette i valori Null.|  
|ansi_warnings|**bit**|Impostazione ANSI_WARNINGS per la sessione. Non ammette i valori Null.|  
|ansi_padding|**bit**|Impostazione ANSI_PADDING per la sessione. Non ammette i valori Null.|  
|ansi_nulls|**bit**|Impostazione ANSI_NULLS per la sessione. Non ammette i valori Null.|  
|concat_null_yields_null|**bit**|Impostazione CONCAT_NULL_YIELDS_NULL per la sessione. Non ammette i valori Null.|  
|transaction_isolation_level|**smallint**|Livello di isolamento delle transazioni della sessione.<br /><br /> 0 = Non specificato<br /><br /> 1 = ReadUncommitted<br /><br /> 2 = ReadCommitted<br /><br /> 3 = RepeatableRead<br /><br /> 4 = Serializable<br /><br /> 5 = Snapshot<br /><br /> Non ammette i valori Null.|  
|lock_timeout|**int**|Impostazione LOCK_TIMEOUT per la sessione. Il valore è espresso in millisecondi. Non ammette i valori Null.|  
|deadlock_priority|**int**|Impostazione DEADLOCK_PRIORITY per la sessione. Non ammette i valori Null.|  
|row_count|**bigint**|Numero di righe restituite nella sessione fino a questo punto. Non ammette i valori Null.|  
|prev_error|**int**|ID dell'ultimo errore restituito nella sessione. Non ammette i valori Null.|  
|original_security_id|**varbinary(85)**|ID di sicurezza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows associato a original_login_name. Non ammette i valori Null.|  
|original_login_name|**nvarchar(128)**|Nome dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato dal client per creare la sessione. Può essere un nome di un account di accesso autenticato di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un nome di un utente di dominio autenticato di Windows o un utente di un database indipendente. Si noti che nella sessione potrebbero essersi verificati numerosi cambi di contesto impliciti o espliciti dopo la connessione iniziale, Ad esempio, se viene utilizzato [Execute As](../../t-sql/statements/execute-as-transact-sql.md) . Non ammette i valori Null.|  
|last_successful_logon|**datetime**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Data e ora dell'ultimo accesso riuscito di original_login_name prima dell'avvio della sessione corrente.|  
|last_unsuccessful_logon|**datetime**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Data e ora dell'ultimo accesso non riuscito di original_login_name prima dell'avvio della sessione corrente.|  
|unsuccessful_logons|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di tentativi di accesso non riusciti per original_login_name tra last_successful_logon e login_time.|  
|group_id|**int**|ID del gruppo di carico di lavoro a cui appartiene la sessione. Non ammette i valori Null.|  
|database_id|**smallint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> ID del database corrente per ogni sessione.|  
|authenticating_database_id|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> ID del database che ha eseguito l'autenticazione dell'entità. Per gli account di accesso, il valore sarà 0. Per gli utenti di database indipendenti, il valore sarà l'ID del database indipendente.|  
|open_transaction_count|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Numero di transazioni aperte per sessione.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
|page_server_reads|**bigint**|**Si applica a**: iperscalabilità del database SQL di Azure<br /><br /> Numero di letture di pagine del server eseguite dalle richieste della sessione durante la sessione. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
Tutti possono visualizzare le proprie informazioni sulla sessione.  
**[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] :** È richiesta l' `VIEW SERVER STATE` autorizzazione [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] per per visualizzare tutte le sessioni nel server.  
**[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] :** Richiede `VIEW DATABASE STATE` per visualizzare tutte le connessioni al database corrente. `VIEW DATABASE STATE` non è possibile concedere nel `master` database. 
  
  
## <a name="remarks"></a>Osservazioni  
 Quando è abilitata l'opzione di configurazione del server **common criteria compliance enabled** , le statistiche di accesso vengono visualizzate nelle colonne seguenti.  
  
-   last_successful_logon  
  
-   last_unsuccessful_logon  
  
-   unsuccessful_logons  
  
 Se l'opzione non è abilitata, tali colonne restituiranno valori Null. Per ulteriori informazioni su come impostare questa opzione di configurazione del server, vedere [opzione di configurazione del server common criteria compliance enabled](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md).  
 
 
 Per le connessioni amministrative nel database SQL di Azure viene visualizzata una riga per ogni sessione autenticata. Le sessioni "sa" visualizzate nel set di risultati non hanno alcun effetto sulla quota utente per le sessioni. Le connessioni non amministrative vedranno solo le informazioni relative alle sessioni utente del database.
 
  
## <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|In/Si applica a|Relationship|  
|----------|--------|---------------|------------------|  
|sys.dm_exec_sessions|[sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|session_id|Uno-a-zero o uno-a-molti|  
|sys.dm_exec_sessions|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)|session_id|Uno-a-zero o uno-a-molti|  
|sys.dm_exec_sessions|[sys.dm_tran_session_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)|session_id|Uno-a-zero o uno-a-molti|  
|sys.dm_exec_sessions|[sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)(session_id &#124; 0)|session_id CROSS APPLY<br /><br /> OUTER APPLY|Uno-a-zero o uno-a-molti|  
|sys.dm_exec_sessions|[sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)|session_id|Uno-a-uno|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-finding-users-that-are-connected-to-the-server"></a>R. Individuazione degli utenti connessi al server  
 Nell'esempio seguente vengono individuati gli utenti connessi al server e viene restituito il numero di sessioni per ogni utente.  
  
```sql  
SELECT login_name ,COUNT(session_id) AS session_count   
FROM sys.dm_exec_sessions   
GROUP BY login_name;  
```  
  
### <a name="b-finding-long-running-cursors"></a>B. Individuazione dei cursori con esecuzione prolungata  
 Nell'esempio seguente vengono individuati i cursori aperti per un periodo più lungo di quello specificato, l'utente che ha creato i cursori e la sessione in cui i cursori sono attivi.  
  
```sql  
USE master;  
GO  
SELECT creation_time ,cursor_id   
    ,name ,c.session_id ,login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s   
   ON c.session_id = s.session_id   
WHERE DATEDIFF(mi, c.creation_time, GETDATE()) > 5;  
```  
  
### <a name="c-finding-idle-sessions-that-have-open-transactions"></a>C. Individuazione delle sessioni inattive con transazioni aperte  
 Nell'esempio seguente vengono individuate le sessioni inattive con transazioni aperte. Si definisce inattiva una sessione per cui non sono in esecuzione richieste.  
  
```sql  
SELECT s.*   
FROM sys.dm_exec_sessions AS s  
WHERE EXISTS   
    (  
    SELECT *   
    FROM sys.dm_tran_session_transactions AS t  
    WHERE t.session_id = s.session_id  
    )  
    AND NOT EXISTS   
    (  
    SELECT *   
    FROM sys.dm_exec_requests AS r  
    WHERE r.session_id = s.session_id  
    );  
```  
  
### <a name="d-finding-information-about-a-queries-own-connection"></a>D. Ricerca di informazioni su una connessione query personalizzata  
 Query tipica per raccogliere informazioni su una connessione query personalizzata.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



