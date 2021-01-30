---
description: sysmail_update_profile_sp (Transact-SQL)
title: sysmail_update_profile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_profile_sp
- sysmail_update_profile_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_profile_sp
ms.assetid: eaedf7ce-a8d5-4ab9-99e0-d77d5be19e90
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cf631b1acd099d8d7821a49163b80d643331ec28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181876"
---
# <a name="sysmail_update_profile_sp-transact-sql"></a>sysmail_update_profile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica la descrizione o il nome di un profilo di Posta elettronica database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_update_profile_sp [ [ @profile_id = ] profile_id , ] [ [ @profile_name = ] 'profile_name' , ]  
    [ [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo da aggiornare. *profile_id* è di **tipo int** e il valore predefinito è null. È necessario specificare almeno uno dei *profile_id* o *profile_name* . Se si specificano entrambi, la procedura modifica il nome del profilo.  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo da aggiornare o nuovo nome del profilo. *profile_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare almeno uno dei *profile_id* o *profile_name* . Se si specificano entrambi, la procedura modifica il nome del profilo.  
  
`[ @description = ] 'description'` Nuova descrizione del profilo. *Description* è di **tipo nvarchar (256)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Se si specificano sia l'ID che il nome del profilo, la procedura modifica il nome del profilo utilizzando il nome specificato e quindi aggiorna la descrizione del profilo. Se si specifica solo uno di questi argomenti, la procedura aggiorna la descrizione del profilo.  
  
 Il stored procedure **sysmail_update_profile_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 **A. Modifica della descrizione di un profilo**  
  
 Nell'esempio seguente viene modificata la descrizione del profilo denominato `AdventureWorks Administrator` nel database **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_update_profile_sp  
    @profile_name = 'AdventureWorks Administrator'  
    ,@description = 'Administrative mail profile.';  
```  
  
 **B. Modifica del nome e delle descrizione di un profilo**  
  
 Nell'esempio seguente vengono modificati il nome e la descrizione del profilo con ID `750`.  
  
```  
EXECUTE msdb.dbo.sysmail_update_profile_sp  
    @profile_id = 750  
    ,@profile_name = 'Operator'  
    ,@description = 'Profile to send alert e-mail to operators.';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
