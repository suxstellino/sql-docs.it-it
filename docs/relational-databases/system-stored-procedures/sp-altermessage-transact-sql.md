---
description: sp_altermessage (Transact-SQL)
title: sp_altermessage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_altermessage_TSQL
- sp_altermessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_altermessage
ms.assetid: 1b28f280-8ef9-48e9-bd99-ec14d79abaca
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5f7a871967c854376e9601373e4e28c3532cf9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203253"
---
# <a name="sp_altermessage-transact-sql"></a>sp_altermessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica lo stato dei messaggi di sistema o definiti dall'utente in un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. I messaggi definiti dall'utente possono essere visualizzati utilizzando la vista del catalogo **sys. messages** .  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_altermessage [ @message_id = ] message_number   ,[ @parameter = ]'write_to_log'  
   ,[ @parameter_value = ]'value'   
```  
  
## <a name="arguments"></a>Argomenti  
 [**@message_id =** ] *message_number*  
 Numero di errore del messaggio da modificare da **sys. messages**. *message_number* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @parameter = ] 'write\_to\_log_'`Viene utilizzato con **\@ parameter_value** per indicare che il messaggio deve essere scritto nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro applicazioni di Windows. *write_to_log* è di **tipo sysname** e non prevede alcun valore predefinito. *write_to_log* deve essere impostato su WITH_LOG o null. Se *write_to_log* è impostato su WITH_LOG o null e il valore per **\@ parameter_value** è **true**, il messaggio viene scritto nel registro applicazioni di Windows. Se *write_to_log* è impostato su WITH_LOG o null e il valore per **\@ parameter_value** è **false**, il messaggio non viene sempre scritto nel registro applicazioni di Windows, ma può essere scritto in base alla modalità di generazione dell'errore. Se *write_to_log* è specificato, è necessario specificare anche il valore per **\@ parameter_value** .  
  
> [!NOTE]  
>  Se un messaggio viene scritto nel registro applicazioni di Windows, viene registrato inoltre nel file di log degli errori di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
`[ @parameter_value = ]'value_'`Viene usato con il **\@ parametro** per indicare che l'errore deve essere scritto nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro applicazioni di Windows. *value* è di tipo **varchar (5)** e non prevede alcun valore predefinito. Se **true**, l'errore viene sempre scritto nel registro applicazioni di Windows. Se **false**, l'errore non viene sempre scritto nel registro applicazioni di Windows, ma può essere scritto in base al modo in cui è stato generato l'errore. Se viene specificato *value* , è necessario specificare anche *write_to_log* per il **\@ parametro** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 L'effetto di **sp_altermessage** con l'opzione WITH_LOG è simile a quello del parametro RAISERROR with log, ad eccezione del fatto che **sp_altermessage** modificare il comportamento di registrazione di un messaggio esistente. I messaggi modificati con l'opzione WITH_LOG vengono sempre scritti nel registro applicazioni di Windows, indipendentemente dalla modalità con cui sono stati generati. Gli errori vengono infatti scritti nel registro applicazioni di Windows anche se RAISERROR viene eseguito senza l'opzione WITH_LOG.  
  
 È possibile modificare i messaggi di sistema utilizzando **sp_altermessage**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **serveradmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene registrato il messaggio `55001` esistente nel registro applicazioni di Windows.  
  
```  
EXECUTE sp_altermessage 55001, 'WITH_LOG', 'true';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
