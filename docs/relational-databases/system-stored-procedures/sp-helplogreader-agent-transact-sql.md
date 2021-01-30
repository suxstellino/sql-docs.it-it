---
description: sp_helplogreader_agent (Transact-SQL)
title: sp_helplogreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helplogreader_agent
- sp_helplogreader_agent_TSQL
helpviewer_keywords:
- sp_helplogreader_agent
ms.assetid: ff837209-e2b3-481a-a48f-8530bfe53d97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 24ec79125cbbae4764eb2a88000969a4100c4917
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183117"
---
# <a name="sp_helplogreader_agent-transact-sql"></a>sp_helplogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le proprietà del processo dell'agente di lettura log per il database di pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helplogreader_agent [ [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'agente.|  
|**nome**|**nvarchar (100)**|Nome dell'agente.|  
|**publisher_security_mode**|**smallint**|Modalità di sicurezza utilizzata dall'agente durante la connessione al server di pubblicazione. Le possibili modalità sono le seguenti:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows.|  
|**publisher_login**|**sysname**|Account di accesso utilizzato per la connessione al server di pubblicazione.|  
|**publisher_password**|**nvarchar (524)**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito un valore.|  
|**job_id**|**uniqueidentifier**|ID univoco del processo dell'agente.|  
|**job_login**|**nvarchar(512)**|Account di Windows utilizzato per l'esecuzione del agente di lettura log, restituito nel formato *dominio* \\ *nomeutente*.|  
|**job_password**|**sysname**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito un valore.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helplogreader_agent** viene utilizzata nella replica transazionale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di pubblicazione o i membri del ruolo predefinito del database **db_owner** nel database di pubblicazione possono eseguire **sp_helplogreader_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le impostazioni di sicurezza della replica](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [sp_addlogreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_changelogreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)  
  
  
