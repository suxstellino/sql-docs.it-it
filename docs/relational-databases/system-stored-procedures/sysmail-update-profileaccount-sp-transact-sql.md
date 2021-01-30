---
description: sysmail_update_profileaccount_sp (Transact-SQL)
title: sysmail_update_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_profileaccount_sp_TSQL
- sysmail_update_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_profileaccount_sp
ms.assetid: 92ca7488-29db-414e-8e36-08b0a8f542bb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 140256e73adee3593c620b17f81ac10c163030f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181844"
---
# <a name="sysmail_update_profileaccount_sp-transact-sql"></a>sysmail_update_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna il numero di sequenza di un account in un profilo di Posta elettronica database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_update_profileaccount_sp  { [ @profile_id = ] profile_id   
| [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' } ,  
    [ @sequence_number = ] sequence_number  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo da aggiornare. *profile_id* è di **tipo int** e il valore predefinito è null. È necessario specificare il *profile_id* o l' *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo da aggiornare. *profile_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare il *profile_id* o l' *profile_name* .  
  
`[ @account_id = ] account_id` ID dell'account da aggiornare. *account_id* è di **tipo int** e il valore predefinito è null. È necessario specificare il *account_id* o l' *account_name* .  
  
`[ @account_name = ] 'account_name'` Nome dell'account da aggiornare. *account_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare il *account_id* o l' *account_name* .  
  
`[ @sequence_number = ] sequence_number` Nuovo numero di sequenza per l'account. *sequence_number* è di **tipo int** e non prevede alcun valore predefinito. Il numero di sequenza determina l'ordine in cui gli account sono utilizzati nel profilo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Restituisce un errore se l'account specificato non è associato al profilo.  
  
 Il numero di sequenza determina l'ordine in cui Posta elettronica database utilizza gli account nel profilo. Per un nuovo messaggio di posta elettronica, Posta elettronica database inizia con l'account che ha il numero di sequenza più basso. Se l'invio del messaggio con tale account ha esito negativo, Posta elettronica database prova con l'account con il numero di sequenza successivo e così via, finché il messaggio non viene inviato o finché anche l'invio con l'account con il numero di sequenza più alto non ha esito negativo. Se l'account con il numero di sequenza più alto restituisce un errore, l'invio del messaggio non viene completato.  
  
 Se esistono più account con lo stesso numero di sequenza, Posta elettronica database utilizza solo uno di questi account per un messaggio di posta specifico. In questo caso, non viene garantito quale account viene utilizzato per quel numero di sequenza né che venga utilizzato lo stesso account per ogni messaggio.  
  
 Il stored procedure **sysmail_update_profileaccount_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene modificato il numero di sequenza dell'account `Admin-BackupServer` all'interno del profilo `AdventureWorks Administrator` nel database **msdb** . Dopo l'esecuzione del codice, il numero di sequenza dell'account è `3`, ad indicare che sarà utilizzato se i primi due account restituiscono un messaggio di errore.  
  
```  
EXECUTE msdb.dbo.sysmail_update_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator'  
    ,@account_name = 'Admin-BackupServer',  
    ,@sequence_number = 3;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
