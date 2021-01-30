---
description: sp_changepublication_snapshot (Transact-SQL)
title: sp_changepublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changepublication_snapshot_TSQL
- sp_changepublication_snapshot
helpviewer_keywords:
- sp_changepublication_snapshot
ms.assetid: 518a4618-3592-4edc-8425-cbc33cdff891
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7dadc4e406c11ba93a5428a6dfc958ed96a36a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211937"
---
# <a name="sp_changepublication_snapshot-transact-sql"></a>sp_changepublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà dell'agente snapshot per la pubblicazione specificata. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changepublication_snapshot [ @publication= ] 'publication'  
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
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare l'agente. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
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
  
`[ @frequency_interval = ] frequency_interval` Specifica i giorni in cui viene eseguito l'agente. *frequency_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
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
  
`[ @frequency_subday = ] frequency_subday` Unità per *freq_subday_interval*. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4**|Minuto|  
|**8**|Ora|  
|NULL (predefinito)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data di esecuzione del agente di snapshot. *frequency_relative_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di snapshot, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di snapshot pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di snapshot viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di snapshot viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Nome di un agente di snapshot nome di un processo esistente se viene utilizzato un processo esistente. *snapshot_agent_name* è di **tipo nvarchar (100)** e il valore predefinito è null.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza utilizzata dall'agente per la connessione al server di pubblicazione. *publisher_security_mode* è di **smallint** e il valore predefinito è null. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione e **1** specifica l'autenticazione di Windows. Per i Publisher non è necessario specificare un valore pari a **0** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso utilizzato per la connessione al server di pubblicazione. *publisher_login* è di **tipo sysname** e il valore predefinito è null. Quando *publisher_security_mode* è **0**, è necessario specificare *publisher_login* . Se *publisher_login* è NULL e *publisher_security_mode* è **1**, per la connessione al server di pubblicazione viene utilizzato l'account di Windows specificato in *job_login* .  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa. Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e il valore predefinito è null. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al server di distribuzione. È necessario specificare questo parametro per la creazione di un nuovo processo per l'agente snapshot. Questa operazione non può essere modificata per un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione non.  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e il valore predefinito è null. È necessario specificare questo parametro per la creazione di un nuovo processo per l'agente snapshot.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *server di pubblicazione* non deve essere utilizzato durante la creazione di un agente di snapshot in un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changepublication_snapshot** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_changepublication_snapshot**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà della pubblicazione](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Modificare le proprietà di pubblicazioni e articoli](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addpublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
