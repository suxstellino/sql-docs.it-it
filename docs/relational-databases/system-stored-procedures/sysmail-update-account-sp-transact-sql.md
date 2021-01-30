---
description: sysmail_update_account_sp (Transact-SQL)
title: sysmail_update_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_account_sp
- sysmail_update_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_account_sp
ms.assetid: ba2fdccc-5ed4-40ef-a479-79497b4d61aa
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b441dfe7aa92bd78bb87ef05b4b7f9cdd6108919
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181887"
---
# <a name="sysmail_update_account_sp-transact-sql"></a>sysmail_update_account_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le informazioni di un account di Posta elettronica database esistente.  
 
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_update_account_sp [ [ @account_id = ] account_id ] [ , ] [ [ @account_name = ] 'account_name' ] ,  
    [ @email_address = ] 'email_address' ,   
    [ @display_name = ] 'display_name' ,   
    [ @replyto_address = ] 'replyto_address' ,  
    [ @description = ] 'description' ,   
    [ @mailserver_name = ] 'server_name' ,   
    [ @mailserver_type = ] 'server_type' ,   
    [ @port = ] port_number ,   
    [ @timeout = ] 'timeout' ,  
    [ @username = ] 'username' ,  
    [ @password = ] 'password' ,  
    [ @use_default_credentials = ] use_default_credentials ,  
    [ @enable_ssl = ] enable_ssl   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @account_id = ] account_id` ID dell'account da aggiornare. *account_id* è di **tipo int** e il valore predefinito è null. È necessario specificare almeno uno dei *account_id* o *account_name* . Se si specificano entrambi, la stored procedure modifica il nome dell'account.  
  
`[ @account_name = ] 'account_name'` Nome dell'account da aggiornare. *account_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare almeno uno dei *account_id* o *account_name* . Se si specificano entrambi, la stored procedure modifica il nome dell'account.  
  
`[ @email_address = ] 'email_address'` Nuovo indirizzo di posta elettronica da cui inviare il messaggio. Deve essere un indirizzo di posta elettronica Internet. Il nome del server nell'indirizzo è il server utilizzato da Posta elettronica database per l'invio di posta da questo account. *email_address* è di **tipo nvarchar (128)** e il valore predefinito è null.  
  
`[ @display_name = ] 'display_name'` Nuovo nome visualizzato da utilizzare nei messaggi di posta elettronica da questo account. *display_name* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito.  
  
`[ @replyto_address = ] 'replyto_address'` Nuovo indirizzo da utilizzare nel Reply-To intestazione dei messaggi di posta elettronica da questo account. *replyto_address* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito.  
  
`[ @description = ] 'description'` Nuova descrizione dell'account. *Description* è di **tipo nvarchar (256)** e il valore predefinito è null.  
  
`[ @mailserver_name = ] 'server_name'` Nuovo nome del server di posta SMTP da utilizzare per l'account. Il computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere in grado di risolvere il *server_name* in un indirizzo IP. *server_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @mailserver_type = ] 'server_type'` Nuovo tipo di server di posta elettronica. *server_type* è di **tipo sysname** e non prevede alcun valore predefinito. È supportato solo il valore **' SMTP '** .  
  
`[ @port = ] port_number` Nuovo numero di porta del server di posta elettronica. *port_number* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @timeout = ] 'timeout'` Parametro timeout per SmtpClient. Send di un singolo messaggio di posta elettronica. *Timeout* è di **tipo int** in secondi e non prevede alcun valore predefinito.  
  
`[ @username = ] 'username'` Nuovo nome utente da utilizzare per accedere al server di posta elettronica. *Nome utente* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @password = ] 'password'` Nuova password da utilizzare per accedere al server di posta elettronica. *password* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @use_default_credentials = ] use_default_credentials` Specifica se inviare il messaggio di posta elettronica al server SMTP utilizzando le credenziali del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] servizio. **use_default_credentials** è di bit e non prevede alcun valore predefinito. Se questo parametro è 1, Posta elettronica database utilizza le credenziali di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Quando questo parametro è 0, Posta elettronica database utilizza il **\@ nome utente** e la **\@ password** per l'autenticazione nel server SMTP. Se **\@ nome utente** e **\@ password** sono null, verrà utilizzata l'autenticazione anonima. Prima di specificare questo parametro consultare l'amministratore del server SMTP.  
  
`[ @enable_ssl = ] enable_ssl` Specifica se Posta elettronica database crittografa la comunicazione utilizzando Transport Layer Security (TLS), precedentemente nota come Secure Sockets Layer (SSL). Utilizzare questa opzione se TLS è necessario sul server SMTP. **enable_ssl** è di bit e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Se si specificano sia il nome dell'account che l'ID dell'account, la stored procedure modifica il nome dell'account, oltre ad aggiornare le informazioni relative all'account. Questo può essere utile se è necessario correggere un errore nel nome dell'account.  
  
 Il stored procedure **sysmail_update_account_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-information-for-an-account"></a>R. Modifica delle informazioni di un account  
 Nell'esempio seguente viene aggiornato l'account `AdventureWorks Administrator` nel database **msdb** . Le informazioni dell'account vengono impostate in base ai valori specificati.  
  
```  
EXECUTE msdb.dbo.sysmail_update_account_sp  
     @account_name = 'AdventureWorks Administrator'  
    ,@description = 'Mail account for administrative e-mail.'  
    ,@email_address = 'dba@Adventure-Works.com'  
    ,@display_name = 'AdventureWorks Automated Mailer'  
    ,@replyto_address = NULL  
    ,@mailserver_name = 'smtp.Adventure-Works.com'  
    ,@mailserver_type = 'SMTP'  
    ,@port = 25  
    ,@timeout = 60  
    ,@username = NULL  
    ,@password = NULL  
    ,@use_default_credentials = 0  
    ,@enable_ssl = 0;  
```  
  
### <a name="b-changing-the-name-of-an-account-and-the-information-for-an-account"></a>B. Modifica del nome e delle informazioni di un account  
 Nell'esempio seguente vengono modificati il nome e le informazioni dell'account con ID `125`. Il nuovo nome dell'account è `Backup Mail Server`.  
  
```  
EXECUTE msdb.dbo.sysmail_update_account_sp  
     @account_id = 125  
    ,@account_name = 'Backup Mail Server'  
    ,@description = 'Mail account for administrative e-mail.'  
    ,@email_address = 'dba@Adventure-Works.com'  
    ,@display_name = 'AdventureWorks Automated Mailer'  
    ,@replyto_address = NULL  
    ,@mailserver_name = 'smtp-backup.Adventure-Works.com'  
    ,@mailserver_type = 'SMTP'  
    ,@port = 25  
    ,@timeout = 60  
    ,@username = NULL  
    ,@password = NULL  
    ,@use_default_credentials = 0  
    ,@enable_ssl = 0;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
