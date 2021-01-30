---
description: sp_dropmessage (Transact-SQL)
title: sp_dropmessage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropmessage_TSQL
- sp_dropmessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropmessage
ms.assetid: 17287a15-cdde-43d1-bb18-9f920bc15db8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41971224ec830b1fbc17fdc8f754d7ae99e8731e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197635"
---
# <a name="sp_dropmessage-transact-sql"></a>sp_dropmessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina un messaggio di errore definito dall'utente specificato da un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. I messaggi definiti dall'utente possono essere visualizzati utilizzando la vista del catalogo **sys. messages** .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropmessage [ @msgnum = ] message_number  
    [ , [ @lang = ] 'language' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @msgnum = ] message_number` Numero del messaggio da eliminare. *message_number* deve essere un messaggio definito dall'utente con un numero di messaggio maggiore di 50000. *message_number* è di **tipo int** e il valore predefinito è null.  
  
`[ @lang = ] 'language'` Lingua del messaggio da eliminare. Se si specifica **All** , vengono eliminate tutte le versioni in lingua *message_number* . *Language* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 No.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
## <a name="remarks"></a>Commenti  
 A meno che non sia specificato **All** per *Language*, è necessario eliminare tutte le versioni localizzate di un messaggio prima che sia possibile eliminare la versione inglese (Stati Uniti) del messaggio.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-a-user-defined-message"></a>R. Eliminazione di un messaggio definito dall'utente  
 Nell'esempio seguente viene eliminato un messaggio definito dall'utente, Number `50001` , da **sys. messages**.  
  
```  
USE master;  
GO  
EXEC sp_dropmessage 50001;  
```  
  
### <a name="b-dropping-a-user-defined-message-that-includes-a-localized-version"></a>B. Eliminazione di un messaggio definito dall'utente che include una versione localizzata  
 Nell'esempio seguente viene eliminato un messaggio definito dall'utente, con numero `60000`, in cui è inclusa una versione localizzata del messaggio.  
  
```  
USE master;  
GO  
  
-- Create a user-defined message in U.S. English  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'The item named %s already exists in %s.',   
    @lang = 'us_english';  
  
-- Create a localized version of the same message.  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'L''élément nommé %1! existe déjà dans %2!',  
    @lang = 'French';  
GO  
  
-- This statement will fail as long as the localized version  
-- of the message exists.  
EXEC sp_dropmessage 60000;  
GO  
  
-- This statement will drop the message.  
EXEC sp_dropmessage  
    @msgnum = 60000,  
    @lang = 'all';  
GO  
```  
  
### <a name="c-dropping-a-localized-version-of-a-user-defined-message"></a>C. Eliminazione di una versione localizzata di un messaggio definito dall'utente  
 Nell'esempio seguente viene eliminata una versione localizzata di un messaggio definito dall'utente, con numero `60000`, senza eliminare l'intero messaggio.  
  
```  
USE master;  
GO  
  
-- Create a user-defined message in U.S. English  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'The item named %s already exists in %s.',   
    @lang = 'us_english';  
  
-- Create a localized version of the same message.  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'L''élément nommé %1! existe déjà dans %2!',  
    @lang = 'French';  
GO  
-- This statement will remove only the localized version of the   
-- message.  
EXEC sp_dropmessage  
    @msgnum = 60000,  
    @lang = 'French';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_altermessage &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
