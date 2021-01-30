---
description: sp_addqreader_agent (Transact-SQL)
title: sp_addqreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqreader_agent_TSQL
- sp_addqreader_agent
helpviewer_keywords:
- sp_addqreader_agent
ms.assetid: dc9f591a-e67e-4ba8-bf47-defd5eda0822
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f148297e264a0ca097671234655f9854b7e79aba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206645"
---
# <a name="sp_addqreader_agent-transact-sql"></a>sp_addqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un agente di lettura coda per un server di distribuzione specificato. Questa stored procedure viene eseguita nel database di distribuzione del server di distribuzione o nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addqreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name'  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e non prevede alcun valore predefinito. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al server di distribuzione.  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
> [!IMPORTANT]  
>  Non archiviare informazioni di autenticazione in file script. Per una sicurezza ottimale, i nomi e le password degli account di accesso dovrebbero essere passati in fase di esecuzione.  
  
`[ @job_name = ] 'job_name'` Nome di un processo di Agent esistente. *job_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando l'agente viene creato con un processo esistente anziché con un nuovo processo creato (impostazione predefinita).  
  
`[ @frompublisher = ] frompublisher` Specifica se la stored procedure viene eseguita nel server di pubblicazione. *frompublisher* è di bit e il valore predefinito è **0**. Il valore **1** indica che la stored procedure viene eseguita dal server di pubblicazione nel database di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_addqreader_agent** viene utilizzata nella replica transazionale.  
  
 **sp_addqreader_agent** deve essere eseguito almeno una volta in un server di distribuzione che supporta l'aggiornamento in coda dopo [sp_adddistributiondb](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) ma prima di [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
 Il processo di agente di lettura coda viene rimosso quando si esegue [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_addqreader_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Abilitare le sottoscrizioni aggiornabili per le pubblicazioni transazionali](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)   
 [Aggiornare gli script di replica &#40;programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)   
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_changeqreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changeqreader-agent-transact-sql.md)   
 [sp_helpqreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md)  
  
  
