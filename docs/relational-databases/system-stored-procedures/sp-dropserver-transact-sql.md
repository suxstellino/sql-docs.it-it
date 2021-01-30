---
description: sp_dropserver (Transact-SQL)
title: sp_dropserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropserver_TSQL
- sp_dropserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropserver
ms.assetid: 0fc83e35-0caa-49a3-a4b6-a1890d4f46ef
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 9dd47ba517e0a38005887af42c71346e8af64645
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187016"
---
# <a name="sp_dropserver-transact-sql"></a>sp_dropserver (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Rimuove un server dall'elenco dei server remoti e collegati noti nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento](../../database-engine/configure-windows/media/topic-link.gif "icona di collegamento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```sql  
sp_dropserver [ @server = ] 'server'   
     [ , [ @droplogins = ] { 'droplogins' | NULL} ]  
```  
  
## <a name="arguments"></a>Argomenti  
 *server*  
 Server da rimuovere. *server* è di tipo **sysname** e non prevede alcun valore predefinito. il *Server* deve esistere.  
  
 *droplogins*  
 Indica che è necessario rimuovere anche gli account di accesso al server collegato e remoto correlati per il *Server* se è specificato **droplogins** . **`@droplogins`** è di **carattere (10)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Se si esegue **sp_dropserver** in un server a cui sono associate voci di accesso del server remoto e collegato oppure è configurato come server di pubblicazione di replica, viene restituito un messaggio di errore. Per rimuovere tutti gli account di accesso al server collegato e remoto per un server quando si rimuove il server, utilizzare l'argomento **droplogins** .  
  
 Impossibile eseguire **sp_dropserver** all'interno di una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LINKED SERVER per il server.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene rimosso il server remoto `ACCOUNTS` e tutti gli account di accesso remoti associati dall'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
sp_dropserver 'ACCOUNTS', 'droplogins';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropremotelogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
