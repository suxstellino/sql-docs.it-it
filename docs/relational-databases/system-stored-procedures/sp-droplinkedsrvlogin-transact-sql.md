---
description: sp_droplinkedsrvlogin (Transact-SQL)
title: sp_droplinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droplinkedsrvlogin_TSQL
- sp_droplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droplinkedsrvlogin
ms.assetid: 75a4a040-72d5-4d29-8304-de0aa481ad4b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6e67dc1b33fb5c5655992b11367ab60639c8654
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209313"
---
# <a name="sp_droplinkedsrvlogin-transact-sql"></a>sp_droplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un mapping esistente tra un account di accesso nel server locale in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione e un account di accesso nel server collegato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_droplinkedsrvlogin [ @rmtsrvname= ] 'rmtsrvname' ,   
   [ @locallogin= ] 'locallogin'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rmtsrvname = ] 'rmtsrvname'` Nome di un server collegato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene applicato il mapping degli account di accesso. *rmtsrvname* è di **tipo sysname** e non prevede alcun valore predefinito. *rmtsrvname* deve esistere già.  
  
`[ @locallogin = ] 'locallogin'` Account di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accesso nel server locale con mapping al server collegato *rmtsrvname*. *locallogin* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario che esista già un mapping di *locallogin* a *rmtsrvname* . Se è NULL, il mapping predefinito creato da **sp_addlinkedserver**, che esegue il mapping di tutti gli account di accesso nel server locale agli account di accesso nel server collegato, viene eliminato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Quando viene eliminato il mapping esistente per un account di accesso, il server locale utilizza il mapping predefinito creato da **sp_addlinkedserver** quando si connette al server collegato per conto di tale account di accesso. Per modificare il mapping predefinito, utilizzare **sp_addlinkedsrvlogin**.  
  
 Se viene eliminato anche il mapping predefinito, solo gli account di accesso a cui è stato assegnato in modo esplicito un mapping di accesso al server collegato tramite **sp_addlinkedsrvlogin** possono accedere al server collegato.  
  
 Impossibile eseguire **sp_droplinkedsrvlogin** dall'interno di una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LOGIN nel server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-removing-the-login-mapping-for-an-existing-user"></a>R. Rimozione del mapping di accesso per un utente esistente  
 Nell'esempio seguente viene rimosso il mapping per l'account di accesso `Mary` tra il server locale e il server collegato `Accounts`. L'account di accesso `Mary` utilizzerà pertanto il mapping predefinito degli account di accesso.  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', 'Mary';  
```  
  
### <a name="b-removing-the-default-login-mapping"></a>B. Rimozione del mapping predefinito degli account di accesso  
 Nell'esempio seguente viene rimosso il mapping predefinito degli account di accesso creato tramite `sp_addlinkedserver` nel server locale `Accounts`.  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', NULL;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
