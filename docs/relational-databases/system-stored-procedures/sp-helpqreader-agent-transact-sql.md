---
description: sp_helpqreader_agent (Transact-SQL)
title: sp_helpqreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpqreader_agent_TSQL
- sp_helpqreader_agent
helpviewer_keywords:
- sp_helpqreader_agent
ms.assetid: 8e74e1aa-e95b-4183-8017-bf123439b08d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d4d0a96017b6bc336f73535648ed8a411b465c05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210826"
---
# <a name="sp_helpqreader_agent-transact-sql"></a>sp_helpqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce le proprietà dell'agente di lettura coda. Questa stored procedure viene eseguita nel database di distribuzione del server di distribuzione oppure in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpqreader_agent [ [ @frompublisher = ] frompublisher ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @frompublisher = ] frompublisher` Specifica se il stored procedure viene chiamato nel server di pubblicazione o nel database di distribuzione. *frompublisher* è di bit e il valore predefinito è 0. **1** indica che il stored procedure viene chiamato dal server di pubblicazione e **0** indica che la stored procedure viene chiamata dal server di distribuzione.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'agente.|  
|**nome**|**nvarchar (100)**|Nome dell'agente.|  
|**job_id**|**uniqueidentifier**|ID univoco del processo dell'agente.|  
|**job_login**|**nvarchar(512)**|Account di Windows utilizzato per l'esecuzione dell'agente di distribuzione, restituito nel formato *dominio* \\ *nomeutente*.|  
|**job_password**|**sysname**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito un valore.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpqreader_agent** viene utilizzata nella replica transazionale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Quando il valore di *frompublisher* è **1**, solo i membri del ruolo predefinito del server **sysadmin** nel server di pubblicazione o i membri del ruolo predefinito del database **db_owner** nel database di pubblicazione possono eseguire **sp_helpqreader_agent**. In caso contrario, solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione o i membri del ruolo predefinito del database **db_owner** nel database di distribuzione possono eseguire **sp_helpqreader_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Abilitare le sottoscrizioni aggiornabili per le pubblicazioni transazionali](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
  
