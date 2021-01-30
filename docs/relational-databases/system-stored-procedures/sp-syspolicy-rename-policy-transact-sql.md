---
description: sp_syspolicy_rename_policy (Transact-SQL)
title: sp_syspolicy_rename_policy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_rename_policy_TSQL
- sp_syspolicy_rename_policy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_rename_policy
ms.assetid: ce2b07f5-23b1-4f49-8e7b-c18cf3f3d45b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 962e15744221175abd5732df7fe4e29d7b924ae0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211840"
---
# <a name="sp_syspolicy_rename_policy-transact-sql"></a>sp_syspolicy_rename_policy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rinomina criteri esistenti nella gestione basata su criteri.  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_rename_policy { [ @name = ] 'name' | [ @policy_id = ] policy_id }  
    , [ @new_name = ] 'new_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome dei criteri che si desidera rinominare. *Name* è di **tipo sysname** e deve essere specificato se *policy_id* è null.  
  
`[ @policy_id = ] policy_id` Identificatore dei criteri che si desidera rinominare. *policy_id* è di **tipo int** e deve essere specificato se il *nome* è null.  
  
`[ @new_name = ] 'new_name'` Nuovo nome del criterio. *new_name* è di **tipo sysname** ed è obbligatorio. Non può essere NULL o una stringa vuota.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_rename_policy nel contesto del database di sistema msdb.  
  
 È necessario specificare un valore per *nome* o *policy_id*. Non possono essere entrambi NULL. Per ottenere questi valori, eseguire una query sulla vista di sistema msdb.dbo.syspolicy_policies.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo predefinito del database PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Possibile elevazione di credenziali: gli utenti nel ruolo PolicyAdministratorRole possono creare trigger server e pianificare le esecuzioni di criteri che influiscono sul funzionamento dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Gli utenti con il ruolo PolicyAdministratorRole possono ad esempio creare criteri che impediscono la creazione della maggior parte degli oggetti nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. A causa di questa possibile elevazione di credenziali, il ruolo PolicyAdministratorRole deve essere concesso solo a utenti ritenuti attendibili per il controllo della configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente i criteri 'Test Policy 1' vengono rinominati in 'Test Policy 2'.  
  
```  
EXEC msdb.dbo.sp_syspolicy_rename_policy @name = N'Test Policy 1'  
, @new_name = N'Test Policy 2';  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)  
  
  
