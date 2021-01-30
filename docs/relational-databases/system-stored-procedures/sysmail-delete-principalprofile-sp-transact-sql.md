---
description: sysmail_delete_principalprofile_sp (Transact-SQL)
title: sysmail_delete_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_principalprofile_sp_TSQL
- sysmail_delete_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_principalprofile_sp
ms.assetid: 8fc14700-e17a-4073-9a96-7fc23e775c69
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a80485aca8c7f0adb6a539f57540952ccd758a31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182017"
---
# <a name="sysmail_delete_principalprofile_sp-transact-sql"></a>sysmail_delete_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove l'autorizzazione per un utente o ruolo del database per l'utilizzo di un profilo di Posta elettronica database pubblico o privato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_delete_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @principal_id = ] principal_id` ID dell'utente del database o del ruolo nel database **msdb** per l'associazione da eliminare. *principal_id* è di **tipo int** e il valore predefinito è null. Per trasformare un profilo pubblico in un profilo privato, fornire l'ID principale **0** o il nome principale **"public"**. È necessario specificare *principal_id* o *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Nome dell'utente del database o del ruolo nel database **msdb** per l'associazione da eliminare. *principal_name* è di **tipo sysname** e il valore predefinito è null. Per trasformare un profilo pubblico in un profilo privato, fornire l'ID principale **0** o il nome principale **"public"**. È necessario specificare *principal_id* o *principal_name* .  
  
`[ @profile_id = ] profile_id` ID del profilo per l'associazione da eliminare. *profile_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo per l'associazione da eliminare. *profile_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *profile_id* o *profile_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Per rendere un profilo pubblico in un profilo privato, fornire **"public"** per il nome dell'entità o **0** per l'ID entità.  
  
 Prestare attenzione quando si rimuovono le autorizzazioni per il profilo privato predefinito di un utente o per il profilo pubblico predefinito. Quando non è disponibile alcun profilo predefinito, **sp_send_dbmail** richiede il nome di un profilo come argomento. Pertanto, la rimozione di un profilo predefinito può causare l'esito negativo delle chiamate a **sp_send_dbmail** . Per ulteriori informazioni, vedere [sp_send_dbmail &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).  
  
 Il stored procedure **sysmail_delete_principalprofile_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrata l'eliminazione dell'associazione tra il profilo **AdventureWorks Administrator** e l'account di accesso **ApplicationUser** nel database **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_delete_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
