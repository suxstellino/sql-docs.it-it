---
description: sp_helplinkedsrvlogin (Transact-SQL)
title: sp_helplinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplinkedsrvlogin_TSQL
- sp_helplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplinkedsrvlogin
ms.assetid: a2b1eba0-bf71-47e7-a4c7-9f55feec82a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274d1d9a39feac333d5b7d572ec475bb89444c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190014"
---
# <a name="sp_helplinkedsrvlogin-transact-sql"></a>sp_helplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui mapping degli account di accesso definiti per un determinato server collegato utilizzato per query distribuite e stored procedure remote.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helplinkedsrvlogin [ [ @rmtsrvname = ] 'rmtsrvname' ]   
     [ , [ @locallogin = ] 'locallogin' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rmtsrvname = ] 'rmtsrvname'` Nome del server collegato a cui viene applicato il mapping degli account di accesso. *rmtsrvname* è di **tipo sysname** e il valore predefinito è null. NULL indica che vengono restituiti i mapping degli account di accesso definiti per tutti i server collegati nel computer locale in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione.  
  
`[ @locallogin = ] 'locallogin'` Account di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accesso nel server locale con mapping al server collegato *rmtsrvname*. *locallogin* è di **tipo sysname** e il valore predefinito è null. NULL specifica che vengono restituiti tutti i mapping degli account di accesso definiti in *rmtsrvname* . Se non è NULL, è necessario che esista già un mapping di *locallogin* a *rmtsrvname* . *locallogin* può essere un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso o un utente di Windows. È necessario che l'utente di Windows disponga dell'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ottenuto tramite concessione diretta o in base all'appartenenza a un gruppo di Windows che dispone dell'accesso.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Server collegato**|**sysname**|Nome del server collegato.|  
|**Account di accesso locale**|**sysname**|Account di accesso locale a cui fa riferimento il mapping.|  
|**Is Self Mapping**|**smallint**|0 = l' **account di accesso locale** viene mappato a **login remoto** durante la connessione al **server collegato**.<br /><br /> 1 = l' **account di accesso locale** viene mappato allo stesso account di accesso e alla stessa password durante la connessione al **server collegato**.|  
|**Remote Login**|**sysname**|Nome dell'account di accesso su **LinkedServer** mappato a **locallogin** quando **IsSelfMapping** è 0. Se **IsSelfMapping** è 1, **RemoteLogin** è null.|  
  
## <a name="remarks"></a>Commenti  
 Prima di eliminare i mapping degli account di accesso, utilizzare **sp_helplinkedsrvlogin** per determinare i server collegati interessati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni non vengono controllate.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-all-login-mappings-for-all-linked-servers"></a>R. Visualizzazione dei mapping degli account di accesso per tutti i server collegati  
 Nell'esempio seguente vengono visualizzati i mapping degli account di accesso per tutti i server collegati definiti nel computer locale in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione.  
  
```  
EXEC sp_helplinkedsrvlogin;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Accounts         NULL          1               NULL  
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
Marketing        NULL          1               NULL  
  
(4 row(s) affected)  
```  
  
### <a name="b-displaying-all-login-mappings-for-a-linked-server"></a>B. Visualizzazione di tutti i mapping degli account di accesso per un server collegato  
 Nell'esempio seguente vengono visualizzati tutti i mapping degli account di accesso definiti a livello locale per il server collegato `Sales`.  
  
```  
EXEC sp_helplinkedsrvlogin 'Sales';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
### <a name="c-displaying-all-login-mappings-for-a-local-login"></a>C. Visualizzazione di tutti i mapping per un account di accesso locale  
 Nell'esempio seguente vengono visualizzati tutti i mapping definiti a livello locale per l'account di accesso `Mary`.  
  
```  
EXEC sp_helplinkedsrvlogin NULL, 'Mary';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
