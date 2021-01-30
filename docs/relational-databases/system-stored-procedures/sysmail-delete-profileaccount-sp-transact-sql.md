---
description: sysmail_delete_profileaccount_sp (Transact-SQL)
title: sysmail_delete_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_profileaccount_sp
- sysmail_delete_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_profileaccount_sp
ms.assetid: b58d06f2-d6c9-4c8e-95bd-027c50f4621a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d364dfb4de4408da2f424185e6cad4f62b0c54cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181991"
---
# <a name="sysmail_delete_profileaccount_sp-transact-sql"></a>sysmail_delete_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un account da un profilo di Posta elettronica database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_delete_profileaccount_sp  {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    {   [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo da eliminare. *profile_id* è di **tipo int** e il valore predefinito è null. È possibile specificare il *profile_id* o l' *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo da eliminare. *profile_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare il *profile_id* o l' *profile_name* .  
  
`[ @account_id = ] account_id` ID dell'account da eliminare. *account_id* è di **tipo int** e il valore predefinito è null. È possibile specificare il *account_id* o l' *account_name* .  
  
`[ @account_name = ] 'account_name'` Nome dell'account da eliminare. *account_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare il *account_id* o l' *account_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Restituisce un errore se l'account specificato non è associato al profilo.  
  
 Quando viene specificato un account senza specificare un profilo, questa stored procedure rimuove l'account specificato da tutti i profili. Se si sta preparando l'arresto di un server SMTP già esistente, ad esempio, verranno rimossi gli account che utilizzano il server SMTP da tutti i profili, e non ogni singolo account da ogni profilo.  
  
 Quando viene specificato un profilo senza specificare un account, questa stored procedure rimuove tutti gli account dal profilo specificato. Se si modificano i server SMTP utilizzati da un profilo, ad esempio, potrebbe risultare utile rimuovere tutti gli account dal profilo e quindi aggiungere nuovi account, se necessario.  
  
 Il stored procedure **sysmail_delete_profileaccount_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente l'account `Audit Account` viene rimosso dal profilo `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
