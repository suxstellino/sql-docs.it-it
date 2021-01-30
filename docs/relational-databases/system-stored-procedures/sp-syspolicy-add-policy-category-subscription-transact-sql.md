---
description: sp_syspolicy_add_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_add_policy_category_subscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_add_policy_category_subscription
- sp_syspolicy_add_policy_category_subscription_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_add_policy_category_subscription
ms.assetid: 4284f550-9a3f-4726-8181-15e407fbf08f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 279cec943ce0e93556442c3ec29c2139764e078c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195966"
---
# <a name="sp_syspolicy_add_policy_category_subscription-transact-sql"></a>sp_syspolicy_add_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge una sottoscrizione di categoria di criteri al database specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_add_policy_category_subscription [ @target_type = ] 'target_type'  
    , [ @target_object = ] 'target_object'  
    , [ @policy_category = ] 'policy_category'  
    [ , [ @policy_category_subscription_id = ] policy_category_subscription_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @target_type = ] 'target_type'` Tipo di destinazione della sottoscrizione di categoria. *target_type* è di **tipo sysname**, è obbligatorio e deve essere impostato su' database '.  
  
`[ @target_object = ] 'target_object'` Nome del database che sottoscriverà la categoria. *target_object* è di **tipo sysname** ed è obbligatorio.  
  
`[ @policy_category = ] 'policy_category'` Nome della categoria di criteri da sottoscrivere. *policy_category* è di **tipo sysname** ed è obbligatorio.  
  
 Per ottenere i valori per *policy_category*, eseguire una query sulla vista di sistema msdb.dbo.syspolicy_policy_categories.  
  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` Identificatore della sottoscrizione di categoria. *policy_category_subscription_id* è di **tipo int** e viene restituito come output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_add_policy_category_subscription nel contesto del database di sistema msdb.  
  
 Se si specifica una categoria di criteri non esistente, verrà creata una nuova categoria di criteri e la sottoscrizione sarà obbligatoria per tutti i database all'esecuzione della stored procedure. Se la sottoscrizione obbligatoria per la nuova categoria viene successivamente cancellata, la sottoscrizione sarà valida solo per il database specificato come *target_object*. Per altre informazioni sulla modifica dell'impostazione di una sottoscrizione obbligatoria, vedere [sp_syspolicy_update_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa stored procedure viene eseguita nel contesto del proprietario corrente della stessa.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene configurata la sottoscrizione di una categoria di criteri denominata 'Table Naming Policies' da parte del database specificato.  
  
```  
EXEC msdb.dbo.sp_syspolicy_add_policy_category_subscription @target_type = N'DATABASE'  
, @target_object = N'AdventureWorks2012'  
, @policy_category = N'Table Naming Policies';  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_update_policy_category_subscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_unsubscribe_from_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md)  
  
  
