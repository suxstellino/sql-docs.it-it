---
description: sysmail_add_profile_sp (Transact-SQL)
title: sysmail_add_profile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_profile_sp_TSQL
- sysmail_add_profile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profile_sp
ms.assetid: a828e55c-633a-41cf-9769-a0698b446e6c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4eeb85dee2493a07b1476a25fc90f23c5ad42aeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207974"
---
# <a name="sysmail_add_profile_sp-transact-sql"></a>sysmail_add_profile_sp (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuovo profilo di Posta elettronica database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_add_profile_sp [ @profile_name = ] 'profile_name'  
    [ , [ @description = ] 'description' ]  
    [ , [ @profile_id = ] new_profile_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_name = ] 'profile\_name'` Nome del nuovo profilo. *profile_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
 
   > [!NOTE]
   > Il nome del profilo che usa Azure SQL Istanza gestita SQL Agent deve essere chiamato **AzureManagedInstance_dbmail_profile**
  
`[ @description = ] 'description'` Descrizione facoltativa del nuovo profilo. *Description* è di **tipo nvarchar (256)** e non prevede alcun valore predefinito.  
  
`[ @profile_id = ] _new\_profile\_id OUTPUT` Restituisce l'ID del nuovo profilo. *new_profile_id* è di **tipo int** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Un profilo di Posta elettronica database include qualsiasi numero di account di Posta elettronica database. Le stored procedure di Posta elettronica database possono far riferimento a un profilo attraverso il nome del profilo o l'ID del profilo generato da questa procedura. Per ulteriori informazioni sull'aggiunta di un account a un profilo, vedere [sysmail_add_profileaccount_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-add-profileaccount-sp-transact-sql.md).  
  
 Il nome e la descrizione del profilo possono essere modificati con il stored procedure **sysmail_update_profile_sp**, mentre l'ID del profilo rimane costante per tutta la durata del profilo.  
  
 Il nome del profilo deve essere univoco per Microsoft [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. In caso contrario, la stored procedure restituisce un errore.  
  
 Il stored procedure **sysmail_add_profile_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 **A. Creazione di un nuovo profilo**  
  
 Nell'esempio seguente viene creato un nuovo profilo di Posta elettronica database denominato `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
       @profile_name = 'AdventureWorks Administrator',  
       @description = 'Profile used for administrative mail.' ;  
```  
  
 **B. Creazione di un nuovo profilo e salvataggio dell'ID del profilo in una variabile**  
  
 Nell'esempio seguente viene creato un nuovo profilo di Posta elettronica database denominato `AdventureWorks Administrator`. Nell'esempio il numero di ID del profilo viene archiviato nella variabile `@profileId` e viene restituito un set di risultati contenente il numero di ID del nuovo profilo.  
  
```  
DECLARE @profileId INT ;  
  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
       @profile_name = 'AdventureWorks Administrator',  
       @description = 'Profile used for administrative mail.',  
       @profile_id = @profileId OUTPUT ;  
  
SELECT @profileId ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
