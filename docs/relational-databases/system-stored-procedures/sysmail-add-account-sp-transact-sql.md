---
description: sysmail_add_account_sp (Transact-SQL)
title: sysmail_add_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_account_sp
- sysmail_add_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_account_sp
ms.assetid: 65e15e2e-107c-49c3-b12c-f4edf0eb1617
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e5458a79b5cb44aef8f8a931051cc8346bf4e88f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178080"
---
# <a name="sysmail_add_account_sp-transact-sql"></a>sysmail_add_account_sp (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuovo account di Posta elettronica database contenente informazioni su un account SMTP.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_add_account_sp  [ @account_name = ] 'account_name',  
    [ @email_address = ] 'email_address' ,  
    [ [ @display_name = ] 'display_name' , ]  
    [ [ @replyto_address = ] 'replyto_address' , ]  
    [ [ @description = ] 'description' , ]  
    [ @mailserver_name = ] 'server_name'   
    [ , [ @mailserver_type = ] 'server_type' ]  
    [ , [ @port = ] port_number ]  
    [ , [ @username = ] 'username' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @use_default_credentials = ] use_default_credentials ]  
    [ , [ @enable_ssl = ] enable_ssl ]  
    [ , [ @account_id = ] account_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @account_name = ] 'account_name'` Nome dell'account da aggiungere. *account_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @email_address = ] 'email_address'` Indirizzo di posta elettronica da cui inviare il messaggio. Deve essere un indirizzo di posta elettronica Internet. *email_address* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito. Un account per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent può ad esempio inviare messaggi di posta elettronica dall'indirizzo **sqlagent \@ Adventure-Works.com**.  
  
`[ @display_name = ] 'display_name'` Nome visualizzato da utilizzare nei messaggi di posta elettronica da questo account. *display_name* è di **tipo nvarchar (128)** e il valore predefinito è null. È ad esempio possibile che un account per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent visualizzi il nome **SQL Server Agent Mailer automatico** nei messaggi di posta elettronica.  
  
`[ @replyto_address = ] 'replyto_address'` Indirizzo a cui vengono inviate le risposte ai messaggi da questo account. *replyto_address* è di **tipo nvarchar (128)** e il valore predefinito è null. Ad esempio, le risposte a un account per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent possono andare all'amministratore del database, **danw \@ Adventure-Works.com**.  
  
`[ @description = ] 'description'` Descrizione dell'account. *Description* è di **tipo nvarchar (256)** e il valore predefinito è null.  
  
`[ @mailserver_name = ] 'server_name'` Nome o indirizzo IP del server di posta SMTP da utilizzare per l'account. Il computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere in grado di risolvere il *server_name* in un indirizzo IP. *server_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @mailserver_type = ] 'server_type'` Tipo di server di posta elettronica. *server_type* è di **tipo sysname** e il valore predefinito è **' SMTP '**.  
  
`[ @port = ] port_number` Numero di porta del server di posta elettronica. *port_number* è di **tipo int** e il valore predefinito è 25.  
  
`[ @username = ] 'username'` Nome utente da utilizzare per accedere al server di posta elettronica. *username* è di **tipo nvarchar (128)** e il valore predefinito è null. Se questo parametro è NULL, Posta elettronica database non utilizza l'autenticazione per questo account. Se il server di posta elettronica non richiede l'autenticazione, specificare NULL per il nome utente.  
  
`[ @password = ] 'password'` Password da utilizzare per accedere al server di posta elettronica. *password* è di **tipo nvarchar (128)** e il valore predefinito è null. Non è necessario specificare una password se non si specifica un nome utente.  
  
`[ @use_default_credentials = ] use_default_credentials` Specifica se inviare il messaggio di posta elettronica al server SMTP utilizzando le credenziali del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . **use_default_credentials** è di bit e il valore predefinito è 0. Se questo parametro è 1, Posta elettronica database utilizza le credenziali di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Quando questo parametro è 0, Posta elettronica database invia i parametri **\@ username** e **\@ password** , se presente, altrimenti invia la posta elettronica senza parametri **\@ nome utente** e **\@ password** .  
  
`[ @enable_ssl = ] enable_ssl` Specifica se Posta elettronica database crittografa la comunicazione utilizzando Secure Sockets Layer. **Enable_ssl** è di bit e il valore predefinito è 0.  
  
`[ @account_id = ] account_id OUTPUT` Restituisce l'ID dell'account per il nuovo account. *account_id* è di **tipo int** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Posta elettronica database fornisce parametri distinti per **\@ email_address**, **\@ display_name** e **\@ replyto_address**. Il parametro **\@ email_address** è l'indirizzo da cui viene inviato il messaggio. Il **\@ display_name** parametro è il nome visualizzato nel campo **da:** del messaggio di posta elettronica. Il parametro **\@ replyto_address** è l'indirizzo a cui verranno inviate le risposte al messaggio di posta elettronica. Un account utilizzato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, ad esempio, potrebbe inviare messaggi di posta elettronica da un indirizzo di posta elettronica che viene utilizzato solo per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. I messaggi provenienti da tale indirizzo dovrebbero includere un nome descrittivo, in modo che i destinatari possano stabilire con facilità che il mittente è [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Se un destinatario risponde al messaggio, la risposta dovrebbe arrivare all'amministratore del database e non all'indirizzo utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per questo scenario, l'account usa **SqlAgent@Adventure-Works.com** come indirizzo di posta elettronica. Il nome visualizzato è impostato su **SQL Server Agent Mailer automatico**. L'account utilizza **danw@Adventure-Works.com** come indirizzo di risposta, quindi le risposte ai messaggi inviati da questo account vengono indirizzate all'amministratore del database anziché all'indirizzo di posta elettronica per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Grazie alla possibilità di specificare impostazioni indipendenti per questi tre parametri, con Posta elettronica database è possibile configurare i messaggi in base alle proprie esigenze.  
  
 Il parametro **\@ mailserver_type** supporta il valore **' SMTP '**.  
  
 Quando **\@ use_default_credentials** viene inviato un messaggio di posta elettronica al server SMTP utilizzando le credenziali del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Quando **\@ use_default_credentials** è 0 e vengono specificati un **\@ nome utente** e una **\@ password** per un account, l'account utilizza l'autenticazione SMTP. Il **\@ nome utente** e la **\@ password** sono le credenziali utilizzate dall'account per il server SMTP, non le credenziali per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la rete in cui si trova il computer.  
  
 Il stored procedure **sysmail_add_account_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creato un account denominato `AdventureWorks Administrator`. L'account utilizza l'indirizzo di posta elettronica `dba@Adventure-Works.com` e invia la posta al server di posta elettronica SMTP `smtp.Adventure-Works.com`. I messaggi di posta elettronica inviati da questo account vengono visualizzati `AdventureWorks Automated Mailer` nella riga **da** del messaggio. Le risposte ai messaggi vengono indirizzate a `danw@Adventure-Works.com`.  
  
```  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
