---
description: sysmail_delete_account_sp (Transact-SQL)
title: sysmail_delete_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_account_sp
- sysmail_delete_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_account_sp
ms.assetid: 2adcac78-4a4a-407e-9666-1d9c43c73cc2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1facb2a83b79c51fbfa53eaa4af4980b0e71b594
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203661"
---
# <a name="sysmail_delete_account_sp-transact-sql"></a>sysmail_delete_account_sp (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Elimina un account SMTP di Posta elettronica database. Per eliminare un account, è inoltre possibile utilizzare Configurazione guidata posta elettronica database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_delete_account_sp { [ @account_id = ] account_id | [ @account_name = ] 'account_name' }   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @account_id = ] account_id` Numero di ID dell'account da eliminare. *account_id* è di **tipo int** e non prevede alcun valore predefinito. È necessario specificare *account_id* o *account_name* .  
  
`[ @account_name = ] 'account_name'` Nome dell'account da eliminare. *account_name* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario specificare *account_id* o *account_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Questa procedura elimina l'account specificato anche se è utilizzato da un profilo. Un profilo che non include account non è in grado di inviare correttamente messaggi di posta elettronica.  
  
 Il stored procedure **sysmail_delete_account_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrata un'operazione di eliminazione dell'account di Posta elettronica database denominato `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_account_sp  
    @account_name = 'AdventureWorks Administrator' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [sysmail_add_account_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-add-account-sp-transact-sql.md)   
 [sysmail_delete_profile_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-delete-profile-sp-transact-sql.md)   
 [sysmail_delete_profileaccount_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-delete-profileaccount-sp-transact-sql.md)   
 [sysmail_help_account_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-help-account-sp-transact-sql.md)   
 [sysmail_help_profile_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-help-profile-sp-transact-sql.md)   
 [sysmail_help_profileaccount_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-help-profileaccount-sp-transact-sql.md)   
 [sysmail_update_profileaccount_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-update-profileaccount-sp-transact-sql.md)  
  
  
