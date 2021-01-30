---
description: sp_getagentparameterlist (Transact-SQL)
title: sp_getagentparameterlist (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getagentparameterlist
- sp_getagentparameterlist_TSQL
helpviewer_keywords:
- sp_getagentparameterlist
ms.assetid: 50d3d3c1-b9a1-417c-bad4-674089c9c60d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9a44f56d638de2b37e804e54ca94812cec1703e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204732"
---
# <a name="sp_getagentparameterlist-transact-sql"></a>sp_getagentparameterlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce l'elenco di tutti i parametri dell'agente di replica che possono essere impostati in un profilo agente per il tipo di agente specificato. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione in cui l'agente è in esecuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_getagentparameterlist [ @agent_type = ] 'agent_type'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @agent_type = ] 'agent_type'` Agente di replica per il quale viene aggiunto il parametro. *agent_type* è di **tipo int**. i possibili valori sono i seguenti:  
  
|Valore|Agente|  
|-----------|-----------|  
|**1**|Snapshot|  
|**2**|Agente di lettura log|  
|**3**|Distribuzione|  
|**4**|Unione|  
|**9**|Agente di lettura coda|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_getagentparameter**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_add_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)   
 [sp_drop_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_help_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [Profili degli agenti di replica](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
