---
description: sp_addlogreader_agent (Transact-SQL)
title: sp_addlogreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addlogreader_agent
- sp_addlogreader_agent_TSQL
helpviewer_keywords:
- sp_addlogreader_agent
ms.assetid: d83096b9-96ee-4789-bde0-940d4765b9ed
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a511c4f47f84a8f083dbab86bf1d851762142d90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202367"
---
# <a name="sp_addlogreader_agent-transact-sql"></a>sp_addlogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un agente di lettura log per un database specificato. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addlogreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e il valore predefinito è null. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al server di distribuzione.  
  
> [!NOTE]
>  Per [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i Publisher non è necessario che sia lo stesso account di accesso specificato in [Sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per una sicurezza ottimale, i nomi e le password degli account di accesso dovrebbero essere passati in fase di esecuzione.  
  
`[ @job_name = ] 'job_name'` Nome di un processo di Agent esistente. *job_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando l'agente viene avviato con un processo esistente anziché con un nuovo processo creato (impostazione predefinita).  
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza utilizzata dall'agente per la connessione al server di pubblicazione. *publisher_security_mode* è di **smallint** e il valore predefinito è **1**. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione e **1** specifica l'autenticazione di Windows. Per i Publisher non è necessario specificare un valore pari a **0** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso utilizzato per la connessione al server di pubblicazione. *publisher_login* è di **tipo sysname** e il valore predefinito è null. Quando *publisher_security_mode* è **0**, è necessario specificare *publisher_login* . Se *publisher_login* è NULL e *publisher_security_mode* è **1**, verrà usato l'account di Windows specificato in *Job_login* durante la connessione al server di pubblicazione.  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per una sicurezza ottimale, i nomi e le password degli account di accesso dovrebbero essere passati in fase di esecuzione.  
  
`[ @publisher = ] 'publisher'` Nome del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Non specificare questo parametro per un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addlogreader_agent** viene utilizzata nella replica transazionale.  
  
 È necessario eseguire **sp_addlogreader_agent** per aggiungere un agente di lettura log se è stato eseguito l'aggiornamento di un database abilitato per la replica a questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prima della creazione di una pubblicazione che utilizza il database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addlogreader_agent**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addlogreader-agent-tr_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [sp_addpublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changelogreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
