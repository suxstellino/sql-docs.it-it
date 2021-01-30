---
description: sysmail_add_profileaccount_sp (Transact-SQL)
title: sysmail_add_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_profileaccount_sp
- sysmail_add_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profileaccount_sp
ms.assetid: 7cbf430f-1997-45ea-9707-0086184de744
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c01a72073b36aeba37cf28b5be8175fc283a07df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188772"
---
# <a name="sysmail_add_profileaccount_sp-transact-sql"></a>sysmail_add_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge un account di Posta elettronica database a un profilo di Posta elettronica database. Eseguire **sysmail_add_profileaccount_sp** dopo la creazione di un account di database con [sysmail_add_account_sp &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-account-sp-transact-sql.md)e viene creato un profilo di database con SYSMAIL_ADD_PROFILE_SP &#40;[Transact-SQL](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md)&#41;.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_add_profileaccount_sp { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
    [ , [ @sequence_number = ] sequence_number ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo a cui aggiungere l'account. *profile_id* è di **tipo int** e il valore predefinito è null. È necessario specificare il *profile_id* o l' *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo a cui aggiungere l'account. *profile_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare il *profile_id* o l' *profile_name* .  
  
`[ @account_id = ] account_id` ID dell'account da aggiungere al profilo. *account_id* è di **tipo int** e il valore predefinito è null. È necessario specificare il *account_id* o l' *account_name* .  
  
`[ @account_name = ] 'account_name'` Nome dell'account da aggiungere al profilo. *account_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare il *account_id* o l' *account_name* .  
  
`[ @sequence_number = ] sequence_number` Numero di sequenza dell'account all'interno del profilo. *sequence_number* è di **tipo int** e non prevede alcun valore predefinito. Il numero di sequenza determina l'ordine in cui gli account sono utilizzati nel profilo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Sia il profilo che l'account devono esistere. In caso contrario, la stored procedure restituisce un errore.  
  
 Questa stored procedure non modifica il numero di sequenza di un account che è già associato al profilo specificato. Per ulteriori informazioni sull'aggiornamento del numero di sequenza di un account, vedere [sysmail_update_profileaccount_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-update-profileaccount-sp-transact-sql.md).  
  
 Il numero di sequenza determina l'ordine in cui Posta elettronica database utilizza gli account nel profilo. Per un nuovo messaggio di posta elettronica, Posta elettronica database inizia con l'account che ha il numero di sequenza più basso. Se l'invio del messaggio con tale account ha esito negativo, Posta elettronica database prova con l'account con il numero di sequenza successivo e così via, finché il messaggio non viene inviato o finché anche l'invio con l'account con il numero di sequenza più alto non ha esito negativo. Se l'invio del messaggio con l'account con il numero di sequenza più alto non riesce, Posta elettronica database sospende i tentativi di invio del messaggio per il periodo di tempo specificato nel parametro *AccountRetryDelay* di **sysmail_configure_sp**. Trascorso questo periodo di tempo, Posta elettronica database prova di nuovo a inviare il messaggio, iniziando con l'account con il numero di sequenza più basso. Usare il parametro *AccountRetryAttempts* di **sysmail_configure_sp** per specificare quante volte il processo di posta elettronica esterno deve tentare di inviare il messaggio di posta elettronica usando ogni account indicato del profilo specificato.  
  
 Se esistono più account con lo stesso numero di sequenza, Posta elettronica database utilizzerà uno solo di tali account per un dato messaggio di posta elettronica. In questo caso, non viene garantito quale account viene utilizzato per quel numero di sequenza né che venga utilizzato lo stesso account per ogni messaggio.  
  
 Il stored procedure **sysmail_add_profileaccount_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il profilo `AdventureWorks Administrator` viene associato all'account `Audit Account`. Il numero di sequenza dell'account è 1.  
  
```  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account',  
    @sequence_number = 1 ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
