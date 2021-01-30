---
description: sp_drop_agent_parameter (Transact-SQL)
title: sp_drop_agent_parameter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_drop_agent_parameter_TSQL
- sp_drop_agent_parameter
helpviewer_keywords:
- sp_drop_agent_parameter
ms.assetid: b99e65ff-9cca-4dce-a2ce-2968de23a76a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e050ab1ccb6b9343a672dc2f6c04be33989046b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200728"
---
# <a name="sp_drop_agent_parameter-transact-sql"></a>sp_drop_agent_parameter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina uno o tutti i parametri da un profilo nella tabella **MSagent_parameters** . Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione in cui l'agente è in esecuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_drop_agent_parameter [ @profile_id = ] profile_id  
    [ , [ @parameter_name = ] 'parameter_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo per il quale è necessario eliminare un parametro. *profile_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @parameter_name = ] 'parameter_name'` Nome del parametro da eliminare. *parameter_name* è di **tipo sysname** e il valore predefinito è **%** . Se **%** , vengono eliminati tutti i parametri per il profilo specificato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_drop_agent_parameter** viene utilizzato in tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_drop_agent_parameter**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_help_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
