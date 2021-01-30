---
description: sp_helprolemember (Transact-SQL)
title: sp_helprolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprolemember_TSQL
- sp_helprolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprolemember
ms.assetid: 42797510-aa5d-4564-85ac-27418419af9c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 223d0fe0a72ed72f312e1aba5402ee9c14e9db63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195092"
---
# <a name="sp_helprolemember-transact-sql"></a>sp_helprolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui membri diretti di un ruolo del database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helprolemember [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rolename = ] ' role '` Nome di un ruolo nel database corrente. *Role* è di **tipo sysname** e il valore predefinito è null. il *ruolo* deve esistere nel database corrente. Se *Role* non è specificato, vengono restituiti tutti i ruoli che contengono almeno un membro del database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**DbRole**|**sysname**|Nome del ruolo nel database corrente.|  
|**MemberName**|**sysname**|Nome di un membro di **DbRole.**|  
|**MemberSID**|**varbinary(85)**|ID di sicurezza di **memberName.**|  
  
## <a name="remarks"></a>Commenti  
 Se il database contiene ruoli annidati, **memberName** può essere il nome di un ruolo. **sp_helprolemember** non Mostra l'appartenenza ottenuta tramite ruoli annidati. Se, ad esempio, User1 è un membro di Role1 e Role1 è un membro di Role2, `EXEC sp_helprolemember 'Role2'`, verrà restituito Role1, ma non i membri di Role1 (User1 in questo esempio). Per restituire le appartenenze annidate, è necessario eseguire ripetutamente **sp_helprolemember** per ogni ruolo annidato.  
  
 Utilizzare **sp_helpsrvrolemember** per visualizzare i membri di un ruolo predefinito del server.  
  
 Utilizzare [IS_ROLEMEMBER &#40;&#41;Transact-SQL ](../../t-sql/functions/is-rolemember-transact-sql.md) per verificare l'appartenenza ai ruoli per un utente specifico.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono visualizzati i membri del ruolo `Sales`.  
  
```  
EXEC sp_helprolemember 'Sales';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helpsrvrolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
