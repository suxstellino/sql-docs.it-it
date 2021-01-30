---
description: sp_syspolicy_rename_policy_category (Transact-SQL)
title: sp_syspolicy_rename_policy_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_rename_policy_category_TSQL
- sp_syspolicy_rename_policy_category
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_rename_policy_category
ms.assetid: 8a9c4a3a-91e8-435e-b721-e0293c92be3e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 41bacd4093ebaf3337d311d0a3e2b4b20d246f90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203123"
---
# <a name="sp_syspolicy_rename_policy_category-transact-sql"></a>sp_syspolicy_rename_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rinomina una categoria di criteri esistente nella gestione basata su criteri.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_rename_policy_category { [ @name = ] 'name' | [ @policy_category_id = ] policy_category_id }  
    , [ @new_name = ] 'new_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome della categoria di criteri che si desidera rinominare. *Name* è di **tipo sysname** e deve essere specificato se *policy_category_id* è null.  
  
`[ @policy_category_id = ] policy_category_id` Identificatore della categoria di criteri che si desidera rinominare. *policy_category_id* è di **tipo int** e deve essere specificato se il *nome* è null.  
  
`[ @new_name = ] 'new_name'` Nuovo nome della categoria di criteri. *new_name* è di **tipo sysname** ed è obbligatorio. Non può essere NULL o una stringa vuota.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_rename_policy_category nel contesto del database di sistema msdb.  
  
 È necessario specificare un valore per *nome* o *policy_category_id*. Non possono essere entrambi NULL. Per ottenere questi valori, eseguire una query sulla vista di sistema msdb.dbo.syspolicy_policy_categories.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo predefinito del database PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Possibile elevazione di credenziali: gli utenti nel ruolo PolicyAdministratorRole possono creare trigger server e pianificare le esecuzioni di criteri che influiscono sul funzionamento dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Gli utenti con il ruolo PolicyAdministratorRole possono ad esempio creare criteri che impediscono la creazione della maggior parte degli oggetti nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. A causa di questa possibile elevazione di credenziali, il ruolo PolicyAdministratorRole deve essere concesso solo a utenti ritenuti attendibili per il controllo della configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente la categoria di criteri 'Test Policy 1' viene rinominata in 'Test Policy 2'.  
  
```  
EXEC msdb.dbo.sp_syspolicy_rename_policy_category @name = N'Test Category 1'  
, @new_name = N'Test Category 2';  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-transact-sql.md)   
 [sp_syspolicy_delete_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-transact-sql.md)   
 [sp_syspolicy_update_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md)  
  
  
