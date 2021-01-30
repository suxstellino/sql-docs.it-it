---
description: sp_notify_operator (Transact-SQL)
title: sp_notify_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_notify_operator_TSQL
- sp_notify_operator
dev_langs:
- TSQL
helpviewer_keywords:
- sp_notify_operator
ms.assetid: c440f5c9-9884-4196-b07c-55d87afb17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 954a314760a22d9b00996dc460062fcc75af16aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191998"
---
# <a name="sp_notify_operator-transact-sql"></a>sp_notify_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Invia un messaggio di posta elettronica a un operatore tramite Posta elettronica database.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_notify_operator  
    [ @profile_name = ] 'profilename' ,  
    [ @id = ] id ,  
    [ @name = ] 'name' ,  
    [ @subject = ] 'subject' ,  
    [ @body = ] 'message' ,  
    [ @file_attachments = ] 'attachment'  
    [ @mail_database = ] 'mail_host_database'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_name = ] 'profilename'` Nome del profilo Posta elettronica database da utilizzare per inviare il messaggio. *ProfileName* è di **tipo nvarchar (128)**. Se *ProfileName* non è specificato, viene usato il profilo di posta elettronica database predefinito.  
  
`[ @id = ] id` Identificatore dell'operatore a cui inviare il messaggio. *ID* è di **tipo int** e il valore predefinito è null. È necessario specificare un *ID* o un *nome* .  
  
`[ @name = ] 'name'` Nome dell'operatore a cui inviare il messaggio. *Name* è di **tipo nvarchar (128)** e il valore predefinito è null. È necessario specificare un *ID* o un *nome* .  
  
> **Nota:** Per poter ricevere messaggi, è necessario definire un indirizzo di posta elettronica per l'operatore.  
  
`[ @subject = ] 'subject'` Oggetto per il messaggio di posta elettronica. *Subject* è di **tipo nvarchar (256)** e non prevede alcun valore predefinito.  
  
`[ @body = ] 'message'` Corpo del messaggio di posta elettronica. il *messaggio* è di **tipo nvarchar (max)** e non prevede alcun valore predefinito.  
  
`[ @file_attachments = ] 'attachment'` Nome di un file da aggiungere al messaggio di posta elettronica. *Attachment* è di **tipo nvarchar (512)** e non prevede alcun valore predefinito.  
  
`[ @mail_database = ] 'mail_host_database'` Specifica il nome del database host della posta elettronica. *mail_host_database* è di **tipo nvarchar (128)**. Se non viene specificato alcun *mail_host_database* , per impostazione predefinita viene utilizzato il database **msdb** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Invia il messaggio specificato all'indirizzo di posta elettronica dell'operatore. Se non è stato configurato un indirizzo di posta elettronica per l'operatore, viene restituito un errore.  
  
 Per poter inviare una notifica a un operatore, è necessario innanzitutto configurare Posta elettronica database e un database host della posta elettronica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene inviato un messaggio di notifica all'operatore `François Ajenstat` tramite il profilo `AdventureWorks Administrator` di Posta elettronica database. L'oggetto del messaggio di posta elettronica è `Test Notification`. The e-mail message contains the sentence, "This is a test of notification via e-mail."  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_notify_operator  
   @profile_name = N'AdventureWorks Administrator',  
   @name = N'François Ajenstat',  
   @subject = N'Test Notification',  
   @body = N'This is a test of notification via e-mail.' ;  
GO  
```  
  
## <a name="see-also"></a>Vedi anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_help_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_delete_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)  
  
  
