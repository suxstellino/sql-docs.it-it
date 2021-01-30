---
title: sp_MSchange_snapshot_agent_properties (T-SQL)
description: Descrive il sp_MSchange_snapshot_agent_properties stored procedure utilizzato per modificare le proprietà del agente di snapshot utilizzato per replica di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_snapshot_agent_properties_TSQL
- sp_MSchange_snapshot_agent_properties
helpviewer_keywords:
- sp_MSchange_snapshot_agent_properties
ms.assetid: 7947a788-3fd7-469f-84db-b03ba89a153c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f07673e9dd7b613e34a5f56d6fa4c3629d3e1540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174501"
---
# <a name="sp_mschange_snapshot_agent_properties-transact-sql"></a>sp_MSchange_snapshot_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà di un processo di agente di snapshot eseguito in un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] server di distribuzione o versione successiva. Questa stored procedure viene utilizzata per modificare le proprietà quando il server di pubblicazione viene eseguito in un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. La stored procedure viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_MSchange_snapshot_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @frequency_type= ] frequency_type  
        , [ @frequency_interval= ] frequency_interval  
        , [ @frequency_subday= ] frequency_subday  
        , [ @frequency_subday_interval= ] frequency_subday_interval  
        , [ @frequency_relative_interval= ] frequency_relative_interval  
        , [ @frequency_recurrence_factor= ] frequency_recurrence_factor  
        , [ @active_start_date= ] active_start_date  
        , [ @active_end_date= ] active_end_date  
        , [ @active_start_time_of_day= ] active_start_time_of_day  
        , [ @active_end_time_of_day= ] active_end_time_of_day  
        , [ @snapshot_job_name = ] 'snapshot_agent_name'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui viene eseguita la agente di snapshot. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|On demand|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**10**|Ogni mese|  
|**20**|Mensile, in base all'intervallo di frequenza|  
|**40**|All'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent|  
  
`[ @frequency_interval = ] frequency_interval` Valore da applicare alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @frequency_subday = ] frequency_subday` Unità per *freq_subday_interval*. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4**|Minuto|  
|**8**|Ora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data di esecuzione del agente di snapshot. *frequency_relative_interval* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di snapshot, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di snapshot pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di snapshot viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di snapshot viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Nome di un agente di snapshot nome di un processo esistente se viene utilizzato un processo esistente. *snapshot_agent_name* è di **tipo nvarchar (100)** e non prevede alcun valore predefinito.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza utilizzata dall'agente per la connessione al server di pubblicazione. *publisher_security_mode* è di **tipo int** e non prevede alcun valore predefinito. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione e **1** specifica l'autenticazione di Windows. Per i Publisher non è necessario specificare un valore pari a **0** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso utilizzato per la connessione al server di pubblicazione. *publisher_login* è di **tipo sysname** e non prevede alcun valore predefinito. Quando *publisher_security_mode* è **0**, è necessario specificare *publisher_login* . Se *publisher_login* è null e Publisher *_ * * security_mode* è **1**, verrà usato l'account di Windows specificato in *Job_login* durante la connessione al server di pubblicazione.  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo nvarchar (524)** e non prevede alcun valore predefinito.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per migliorare la sicurezza, si consiglia di specificare nomi e password di accesso in fase di esecuzione.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e non prevede alcun valore predefinito. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al server di distribuzione. È necessario specificare questo parametro per la creazione di un nuovo processo per l'agente snapshot. *Non è possibile modificare questo valore per un non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Server di pubblicazione.*  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario specificare questo parametro per la creazione di un nuovo processo per l'agente snapshot.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per migliorare la sicurezza, si consiglia di specificare nomi e password di accesso in fase di esecuzione.  
  
`[ @publisher_type = ] 'publisher_type'` Specifica il tipo di server di pubblicazione quando il server di pubblicazione non è in esecuzione in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* è di **tipo sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**|Specifica un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Specifica un server di pubblicazione Oracle standard.|  
|**ORACLE GATEWAY**|Specifica un server di pubblicazione Oracle Gateway.|  
  
 Per ulteriori informazioni sulle differenze tra un server di pubblicazione Oracle e un server di pubblicazione Oracle Gateway, vedere [Cenni preliminari sulla pubblicazione Oracle](../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_MSchange_snapshot_agent_properties** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
 È necessario specificare tutti i parametri quando si esegue **sp_MSchange_snapshot_agent_properties**. Eseguire [sp_helppublication_snapshot](../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md) per restituire le proprietà correnti del processo agente di snapshot.  
  
 Quando il server di pubblicazione viene eseguito in un'istanza di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versione successiva, è necessario utilizzare [sp_changepublication_snapshot](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md) per modificare le proprietà di un processo agente di snapshot.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione possono eseguire **sp_MSchange_snapshot_agent_properties**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
  
