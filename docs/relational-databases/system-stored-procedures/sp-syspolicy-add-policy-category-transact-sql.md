---
description: sp_syspolicy_add_policy_category (Transact-SQL)
title: sp_syspolicy_add_policy_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_add_policy_category
- sp_syspolicy_add_policy_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_add_policy_category
ms.assetid: b682fac4-23c6-4662-8d05-c38f3b45507e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4f2c1cd70576342c14028c3bb313a7492bdd0391
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201259"
---
# <a name="sp_syspolicy_add_policy_category-transact-sql"></a>sp_syspolicy_add_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge la categoria di criteri da utilizzare con la gestione basata su criteri. Le categorie consentono di organizzare i criteri e di impostarne l'ambito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_add_policy_category [ @name = ] 'name'  
    [ , [ @mandate_database_subscriptions = ] mandate_database_subscriptions ]  
    , [ @policy_category_id = ] policy_category_id OUTPUT  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome della categoria di criteri. *Name* è di **tipo sysname** ed è obbligatorio. il *nome* non può essere null o una stringa vuota.  
  
`[ @mandate_database_subscriptions = ] mandate_database_subscriptions` Determina se la sottoscrizione di database è obbligatoria per la categoria di criteri. *mandate_database_subscriptions* è un valore di **bit** e il valore predefinito è 1 (abilitato).  
  
`[ @policy_category_id = ] policy_category_id` Identificatore della categoria di criteri. *policy_category_id* è di **tipo int** e viene restituito come output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_add_policy_category nel contesto del database di sistema msdb.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo predefinito del database PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Possibile elevazione di credenziali: gli utenti nel ruolo PolicyAdministratorRole possono creare trigger server e pianificare le esecuzioni di criteri che influiscono sul funzionamento dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Gli utenti con il ruolo PolicyAdministratorRole possono ad esempio creare criteri che impediscono la creazione della maggior parte degli oggetti nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. A causa di questa possibile elevazione di credenziali, il ruolo PolicyAdministratorRole deve essere concesso solo a utenti ritenuti attendibili per il controllo della configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creata una categoria di criteri per la quale la sottoscrizione non è obbligatoria. È pertanto possibile configurare l'inclusione o l'esclusione di criteri nella categoria in singoli database.  
  
```  
DECLARE @policy_category_id int;  
  
EXEC msdb.dbo.sp_syspolicy_add_policy_category  
  @name = N'Table Naming Policies'  
, @mandate_database_subscriptions = 0  
, @policy_category_id = @policy_category_id OUTPUT;  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category_subscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_delete_policy_category &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-transact-sql.md)  
  
  
