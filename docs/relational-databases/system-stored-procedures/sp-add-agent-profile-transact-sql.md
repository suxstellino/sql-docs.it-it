---
description: sp_add_agent_profile (Transact-SQL)
title: sp_add_agent_profile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_add_agent_profile
- sp_add_agent_profile_TSQL
helpviewer_keywords:
- sp_add_agent_profile
ms.assetid: 5c246a33-2c21-4a77-9c2a-a2c9f0c5dda1
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: bd89f620f2640c9e88f03ef44909c844187d8c8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186080"
---
# <a name="sp_add_agent_profile-transact-sql"></a>sp_add_agent_profile (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuovo profilo per un agente di replica. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_agent_profile [ [ @profile_id= ] profile_id OUTPUT ]  
        , [ @profile_name= ] 'profile_name'   
        , [ @agent_type= ] 'agent_type' ]   
    [ , [ @profile_type= ] profile_type ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @default= ] default ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID associato al profilo appena inserito. *profile_id* è di **tipo int** ed è un parametro di output facoltativo. Se viene specificato, il valore è impostato sull'ID del nuovo profilo.  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo. *profile_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @agent_type = ] 'agent_type'` Tipo di agente di replica. *agent_type* è di **tipo int** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|agente snapshot|  
|**2**|Agente di lettura log|  
|**3**|Agente di distribuzione|  
|**4**|Agente di merge|  
|**9**|Agente di lettura coda|  
  
`[ @profile_type = ] profile_type` Tipo di profilo. *profile_type* è di **tipo int** e il valore predefinito è **1**.  
  
 **0** indica un profilo di sistema. **1** indica un profilo personalizzato. È possibile creare solo profili personalizzati utilizzando questo stored procedure; Pertanto l'unico valore valido è **1**. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Crea solo profili di sistema.  
  
`[ @description = ] 'description'` Descrizione del profilo. *Description* è di **tipo nvarchar (3000)** e non prevede alcun valore predefinito.  
  
`[ @default = ] default` Indica se il profilo è l'impostazione predefinita per *agent_type * *.* il *valore predefinito* è **bit** e il valore predefinito è **0**. **1** indica che il profilo aggiunto diventerà il nuovo profilo predefinito per l'agente specificato da *agent_type*.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_add_agent_profile** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
 I profili agenti personalizzati vengono aggiunti con valori predefiniti dei parametri degli agenti. Utilizzare [sp_change_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md) per modificare questi valori predefiniti o [sp_add_agent_parameter &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md) per aggiungere ulteriori parametri.  
  
 Quando **sp_add_agent_profile** viene eseguita, viene aggiunta una riga per il nuovo profilo personalizzato nella tabella [MSagent_profiles &#40;transact-SQL&#41;](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) e i parametri predefiniti associati per questo profilo vengono aggiunti alla tabella MSAGENT_PARAMETERS &#40;[Transact-SQL](../../relational-databases/system-tables/msagent-parameters-transact-sql.md)&#41;.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_agent_profile**.  
  
## <a name="see-also"></a>Vedere anche  
 [Usare i profili agenti di replica](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [Profili degli agenti di replica](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_change_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md)   
 [sp_change_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_drop_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_drop_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [sp_help_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)  
  
  
