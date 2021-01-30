---
description: sp_syspolicy_update_policy_category (Transact-SQL)
title: sp_syspolicy_update_policy_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_update_policy_category_TSQL
- sp_syspolicy_update_policy_category
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_update_policy_category
ms.assetid: 6b6413c2-7a3b-4eff-91d9-5db2011869d6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8825e94add7ceb74f6adcc0d52fda768d5e777bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161281"
---
# <a name="sp_syspolicy_update_policy_category-transact-sql"></a>sp_syspolicy_update_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esegue l'aggiornamento se una categoria di criteri è impostata su Imponi sottoscrizioni di database. Se la sottoscrizione è obbligatoria, la categoria di criteri si applica a tutti i database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_update_policy_category { [ @name = ] 'name' | [ @policy_category_id = ] policy_category_id }  
    , [ @mandate_database_subscriptions = ] mandate_database_subscriptions ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome della categoria di criteri. *Name* è di **tipo sysname** e deve essere specificato se *policy_category_id* è null.  
  
`[ @policy_category_id = ] policy_category_id` Identificatore della categoria di criteri. *policy_category_id* è di **tipo int** e deve essere specificato se il *nome* è null.  
  
`[ @mandate_database_subscriptions = ] mandate_database_subscriptions` Determina se la sottoscrizione di database è obbligatoria per la categoria di criteri. *mandate_database_subscriptions* è un valore di **bit** e il valore predefinito è null. È possibile utilizzare uno dei valori seguenti:  
  
-   0 = Non obbligatoria  
  
-   1 = Obbligatoria  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_update_policy_category nel contesto del database di sistema msdb.  
  
 È necessario specificare un valore per *nome* o per *policy_category_id*. Non possono essere entrambi NULL. Per ottenere questi valori, eseguire una query sulla vista di sistema msdb.dbo.syspolicy_policy_categories.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo predefinito del database PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Possibile elevazione di credenziali: gli utenti nel ruolo PolicyAdministratorRole possono creare trigger server e pianificare le esecuzioni di criteri che influiscono sul funzionamento dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Gli utenti con il ruolo PolicyAdministratorRole possono ad esempio creare criteri che impediscono la creazione della maggior parte degli oggetti nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. A causa di questa possibile elevazione di credenziali, il ruolo PolicyAdministratorRole deve essere concesso solo a utenti ritenuti attendibili per il controllo della configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene eseguito l'aggiornamento della categoria "Finance" a Imponi sottoscrizioni di database.  
  
```  
EXEC msdb.dbo.sp_syspolicy_update_policy_category @name = N'Finance'  
, @mandate_database_subscriptions = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-transact-sql.md)   
 [sp_syspolicy_delete_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-transact-sql.md)   
 [sp_syspolicy_rename_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-rename-policy-category-transact-sql.md)  
  
  
