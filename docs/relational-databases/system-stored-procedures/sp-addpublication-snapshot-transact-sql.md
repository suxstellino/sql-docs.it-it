---
description: sp_addpublication_snapshot (Transact-SQL)
title: sp_addpublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpublication_snapshot_TSQL
- sp_addpublication_snapshot
helpviewer_keywords:
- sp_addpublication_snapshot
ms.assetid: 192b6214-df6e-44a3-bdd4-9d933a981619
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7fb5fbea8a87d1bacb4a086825ecfe62b292f76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209366"
---
# <a name="sp_addpublication_snapshot-transact-sql"></a>sp_addpublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea l'agente snapshot per la pubblicazione specificata. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addpublication_snapshot [ @publication= ] 'publication'  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @snapshot_job_name = ] 'snapshot_agent_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui viene eseguita la agente di snapshot. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Singola occorrenza.|  
|**4** (impostazione predefinita)|Giornaliera.|  
|**8**|Settimanale.|  
|**16**|Mensile.|  
|**32**|Mensile relativa, in base all'intervallo di frequenza.|  
|**64**|All'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|  
|**128**|Quando il computer è inattivo|  
  
`[ @frequency_interval = ] frequency_interval` Valore da applicare alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore di frequency_type|Effetto su frequency_interval|  
|------------------------------|-----------------------------------|  
|**1**|*frequency_interval* non è utilizzato.|  
|**4** (impostazione predefinita)|Ogni *frequency_interval* giorni e il valore predefinito è giornaliero.|  
|**8**|*frequency_interval* è uno o più degli elementi seguenti, combinati con un operatore logico [&#124; (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) :<br /><br /> **1** = domenica &#124;<br /><br /> **2** = lunedì &#124;<br /><br /> **4** = &#124; martedì<br /><br /> **8** = mercoledì &#124;<br /><br /> **16** = giovedì &#124;<br /><br /> **32** = &#124; venerdì<br /><br /> **64** = sabato|  
|**16**|Il *frequency_interval* giorno del mese.|  
|**32**|*frequency_interval* è uno dei seguenti:<br /><br /> **1** = domenica &#124;<br /><br /> **2** = lunedì &#124;<br /><br /> **3** = martedì &#124;<br /><br /> **4** = mercoledì &#124;<br /><br /> **5** = giovedì &#124;<br /><br /> **6** = &#124; venerdì<br /><br /> **7** = &#124; sabato<br /><br /> **8** = giorno &#124;<br /><br /> **9** = &#124; giorno della settimana<br /><br /> **10** = giorno festivo|  
|**64**|*frequency_interval* non è utilizzato.|  
|**128**|*frequency_interval* non è utilizzato.|  
  
`[ @frequency_subday = ] frequency_subday` Unità per *freq_subday_interval*. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4** (impostazione predefinita)|Minuto|  
|**8**|Ora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è 5, ovvero ogni 5 minuti.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data di esecuzione del agente di snapshot. *frequency_relative_interval* è di **tipo int** e il valore predefinito è 1.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di snapshot, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di snapshot pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è 99991231, ovvero il 31 dicembre 9999.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di snapshot viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di snapshot viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è 235959, che indica le 11:59:59. nel formato 24 ore.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Nome di un agente di snapshot nome di un processo esistente se viene utilizzato un processo esistente. *snapshot_agent_name* è di **tipo nvarchar (100)** e il valore predefinito è null. Questo parametro è per uso interno e non deve essere specificato per la creazione di una nuova pubblicazione. Se viene specificato *snapshot_agent_name* , *job_login* e *job_password* devono essere null.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza utilizzata dall'agente per la connessione al server di pubblicazione. *publisher_security_mode* è di **smallint** e il valore predefinito è 1. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione e **1** specifica l'autenticazione di Windows. Per i Publisher non è necessario specificare un valore pari a **0** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso utilizzato per la connessione al server di pubblicazione. *publisher_login* è di **tipo sysname** e il valore predefinito è null. Quando *publisher_security_mode* è **0**, è necessario specificare *publisher_login* . Se *publisher_login* è NULL e *publisher_security_mode* è **1**, l'account specificato in *Job_login* verrà utilizzato per la connessione al server di pubblicazione.  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per migliorare la sicurezza, si consiglia di specificare nomi e password di accesso in fase di esecuzione.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account con cui viene eseguito l'agente. In Istanza gestita SQL di Azure usare un account SQL Server. *job_login* è di **tipo nvarchar (257)** e il valore predefinito è null. Questo account viene sempre utilizzato per le connessioni dell'agente al server di distribuzione. È necessario specificare questo parametro per la creazione di un nuovo processo per l'agente snapshot.  
  
> [!NOTE]
>  Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i Publisher non è necessario che sia lo stesso account di accesso specificato in [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario specificare questo parametro per la creazione di un nuovo processo per l'agente snapshot.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per migliorare la sicurezza, si consiglia di specificare nomi e password di accesso in fase di esecuzione.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *server di pubblicazione* non deve essere utilizzato durante la creazione di un agente di snapshot in un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addpublication_snapshot** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-snapsh_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addpublication_snapshot**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Creare e applicare lo snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [sp_addpublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_startpublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
