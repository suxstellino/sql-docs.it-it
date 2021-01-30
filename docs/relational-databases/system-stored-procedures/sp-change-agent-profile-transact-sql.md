---
description: sp_change_agent_profile (Transact-SQL)
title: sp_change_agent_profile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_change_agent_profile
- sp_change_agent_profile_TSQL
helpviewer_keywords:
- sp_change_agent_profile
ms.assetid: e73acf8d-0be8-4197-ba11-fe798d0e2820
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dbd36aff1dcb94d796a2c491699e0c3b874b0e5a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197667"
---
# <a name="sp_change_agent_profile-transact-sql"></a>sp_change_agent_profile (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica un parametro di un profilo dell'agente di replica archiviato nella [MSagent_profiles &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) . Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_agent_profile [ @profile_id = ] profile_id   
        , [ @property = ] 'property'   
        , [ @value = ] 'value'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo. *profile_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @property = ] 'property'` Nome della proprietà. *Property* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @value = ] 'value'` Nuovo valore della proprietà. *value* è di **tipo nvarchar (3000)** e non prevede alcun valore predefinito.  
  
 Nella tabella seguente vengono descritte le proprietà del profilo che è possibile modificare.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|**description**|Descrizione del profilo.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_change_agent_profile** viene utilizzato in tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_change_agent_profile**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)   
 [sp_drop_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_profile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
