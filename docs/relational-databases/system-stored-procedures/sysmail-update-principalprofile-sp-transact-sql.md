---
description: sysmail_update_principalprofile_sp (Transact-SQL)
title: sysmail_update_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_principalprofile_sp
- sysmail_update_principalprofile_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_principalprofile_sp
ms.assetid: 9fe96e9a-4758-4e4a-baee-3e1217c4426c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 85bab8892a9459ee5de954596ffea2be2a5aed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181863"
---
# <a name="sysmail_update_principalprofile_sp-transact-sql"></a>sysmail_update_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna le informazioni per un'associazione tra un'entità e un profilo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_update_principalprofile_sp { @principal_id = principal_id | @principal_name = 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    [ @is_default = ] 'is_default'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @principal_id = ] principal_id` ID dell'utente del database o del ruolo nel database **msdb** per l'associazione da modificare. *principal_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *principal_id* o *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Nome dell'utente del database o del ruolo nel database **msdb** per l'associazione da aggiornare. *principal_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare *principal_id* o *principal_name* .  
  
`[ @profile_id = ] profile_id` ID del profilo per l'associazione da modificare. *profile_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo per l'associazione da modificare. *profile_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *profile_id* o *profile_name* .  
  
`[ @is_default = ] 'is_default'` Indica se il profilo è il profilo predefinito per l'utente del database. A un utente del database può essere associato un solo profilo predefinito. *is_default* è di **bit** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Questa stored procedure consente di modificare il profilo predefinito per l'utente del database. A un utente del database può essere associato un solo profilo privato predefinito.  
  
 Quando il nome dell'entità per l'associazione è **pubblico** o l'ID entità per l'associazione è **0**, questo stored procedure modifica il profilo pubblico. È possibile associare un solo profilo pubblico predefinito.  
  
 Quando **\@ is_default** è'**1**' e l'entità è associata a più di un profilo, il profilo specificato diventa il profilo predefinito per l'entità. Il profilo che in precedenza era il profilo predefinito è tuttora associato all'entità, ma non è più il profilo predefinito.  
  
 Il stored procedure **sysmail_update_principalprofile_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 **A. Impostazione di un profilo come profilo pubblico predefinito per un database**  
  
 Nell'esempio seguente viene impostato il profilo come `General Use Profile` profilo pubblico predefinito per gli utenti nel database **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'General Use Profile',  
    @is_default = '1';  
```  
  
 **B. Impostazione di un profilo come profilo privato predefinito per un utente**  
  
 Nell'esempio seguente viene impostato il profilo come `AdventureWorks Administrator` profilo predefinito per l'entità `ApplicationUser` nel database **msdb** . Il profilo deve essere già associato all'entità. Il profilo che in precedenza era il profilo predefinito è tuttora associato all'entità, ma non è più il profilo predefinito.  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator',  
    @is_default = '1' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
