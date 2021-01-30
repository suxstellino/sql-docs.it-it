---
description: sp_syspolicy_update_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_update_policy_category_subscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_update_policy_category_subscription_TSQL
- sp_syspolicy_update_policy_category_subscription
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_update_policy_category_subscription
ms.assetid: d0769566-8f5c-4c8a-84d3-ee17ea6e0cb4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 493da853bc06eb1c56986a99a0b2a2aac9c2e5c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174394"
---
# <a name="sp_syspolicy_update_policy_category_subscription-transact-sql"></a>sp_syspolicy_update_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esegue l'aggiornamento di una sottoscrizione di categoria di criteri per un database specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_update_policy_category_subscription [ @policy_category_subscription_id = ] policy_category_subscription_id  
    [ , [ @target_type = ] 'target_type' ]  
    [ , [ @target_object = ] 'target_object' ]  
    , [ @policy_category = ] 'policy_category'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` Identificatore della sottoscrizione di categoria di criteri che si desidera aggiornare. *policy_category_subscription_id* è di **tipo int** ed è obbligatorio.  
  
`[ @target_type = ] 'target_type'` Tipo di destinazione della sottoscrizione di categoria. *target_type* è di **tipo sysname** e il valore predefinito è null.  
  
 Se si specifica *target_type*, il valore deve essere impostato su' database '.  
  
`[ @target_object = ] 'target_object'` Nome del database che sottoscriverà la categoria di criteri. *target_object* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @policy_category = ] 'policy_category'` Nome della categoria di criteri a cui si desidera sottoscrivere il database. *policy_category* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_update_policy_category_subscription nel contesto del database di sistema msdb.  
  
 Per ottenere i valori per *policy_category_subscription_id* e per *policy_category*, è possibile utilizzare la query seguente:  
  
```  
SELECT a.policy_category_subscription_id, a.target_type, a.target_object  
    , b.name AS policy_category  
FROM msdb.dbo.syspolicy_policy_category_subscriptions AS a  
INNER JOIN msdb.dbo.syspolicy_policy_categories AS b  
ON a.policy_category_id = b.policy_category_id;  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo predefinito del database PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Possibile elevazione di credenziali: gli utenti nel ruolo PolicyAdministratorRole possono creare trigger server e pianificare le esecuzioni di criteri che influiscono sul funzionamento dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Gli utenti con il ruolo PolicyAdministratorRole possono ad esempio creare criteri che impediscono la creazione della maggior parte degli oggetti nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. A causa di questa possibile elevazione di credenziali, il ruolo PolicyAdministratorRole deve essere concesso solo a utenti ritenuti attendibili per il controllo della configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene aggiornata una sottoscrizione di categoria di criteri esistente, al fine di effettuare la sottoscrizione della categoria di criteri 'Finance' per il database AdventureWorks2012.  
  
```  
EXEC msdb.dbo.sp_syspolicy_update_policy_category_subscription @policy_category_subscription_id = 1  
, @target_object = 'AdventureWorks2012'  
, @policy_category = 'Finance';  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category_subscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_delete_policy_category_subscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-subscription-transact-sql.md)  
  
  
