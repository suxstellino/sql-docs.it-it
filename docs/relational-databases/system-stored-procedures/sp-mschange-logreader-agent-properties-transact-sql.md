---
title: sp_MSchange_logreader_agent_properties (T-SQL)
description: Descrive il sp_MSchange_logreader_agent_properties stored procedure utilizzato per modificare le proprietà del agente di lettura log per una topologia replica di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_logreader_agent_properties_TSQL
- sp_MSchange_logreader_agent_properties
helpviewer_keywords:
- sp_MSchange_logreader_agent_properties
ms.assetid: 925df9d3-a041-4046-8e17-c47f40edb86d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 84508c1437ae6ef51f1904dfbd414b0e373b32e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195450"
---
# <a name="sp_mschange_logreader_agent_properties-transact-sql"></a>sp_MSchange_logreader_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà di un processo di agente di lettura log eseguito in un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] server di distribuzione o versione successiva. Questa stored procedure viene utilizzata per modificare le proprietà quando il server di pubblicazione viene eseguito in un'istanza di [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. La stored procedure viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_MSchange_logreader_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
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
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza utilizzata dall'agente per la connessione al server di pubblicazione. *publisher_security_mode* è di **smallint** e non prevede alcun valore predefinito.  
  
 **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.  
  
 **1** specifica l'autenticazione di Windows.  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso utilizzato per la connessione al server di pubblicazione. *publisher_login* è di **tipo sysname** e non prevede alcun valore predefinito. Quando *publisher_security_mode* è **0**, è necessario specificare *publisher_login* . Se *publisher_login* è NULL e *publisher_security_mode* è **1**, verrà usato l'account di Windows specificato in *Job_login* durante la connessione al server di pubblicazione.  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e non prevede alcun valore predefinito. *Non è possibile modificare questo valore per un non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *server di pubblicazione.*  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_type = ] 'publisher_type'` Specifica il tipo di server di pubblicazione quando il server di pubblicazione non è in esecuzione in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* è di **tipo sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**|Specifica un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Specifica un server di pubblicazione Oracle standard.|  
|**ORACLE GATEWAY**|Specifica un server di pubblicazione Oracle Gateway.|  
  
 Per ulteriori informazioni sulle differenze tra un server di pubblicazione Oracle e un server di pubblicazione Oracle Gateway, vedere [Cenni preliminari sulla pubblicazione Oracle](../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="remarks"></a>Commenti  
 **sp_MSchange_logreader_agent_properties** viene utilizzata nella replica transazionale.  
  
 È necessario specificare tutti i parametri quando si esegue **sp_MSchange_logreader_agent_properties**. Eseguire [sp_helplogreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) per restituire le proprietà correnti del processo di agente di lettura log.  
  
 Dopo la modifica dell'account di accesso o della password di un agente, è necessario arrestare e riavviare l'agente per rendere effettiva la modifica.  
  
 Quando il server di pubblicazione viene eseguito in un'istanza di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versione successiva, è necessario utilizzare [sp_changelogreader_agent](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md) per modificare le proprietà del agente di lettura log.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione possono eseguire **sp_MSchange_logreader_agent_properties**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
  
