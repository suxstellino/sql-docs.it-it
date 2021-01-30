---
description: sp_revokelogin (Transact-SQL)
title: sp_revokelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revokelogin_TSQL
- sp_revokelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revokelogin
ms.assetid: cb1ab102-1ae0-4811-9144-9a8121ef2d7e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 130ba750ed2e6dfadac111578ee9c42c6578c313
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161340"
---
# <a name="sp_revokelogin-transact-sql"></a>sp_revokelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove le voci di accesso da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per un utente o un gruppo di Windows creato mediante create login, **sp_grantlogin** o **sp_denylogin**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [Drop login](../../t-sql/statements/drop-login-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_revokelogin [ @loginame= ] 'login'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @loginame = ] 'login'` Nome dell'utente o del gruppo di Windows. *login* è di **tipo sysname** e non prevede alcun valore predefinito. l' *account di accesso* può essere qualsiasi nome utente o gruppo di Windows esistente nel formato *nome computer* \\ *utente o dominio* \\ *utente*.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_revokelogin** Disabilita le connessioni utilizzando l'account specificato dal parametro *login* . Gli utenti di Windows ai quali è stato concesso l'accesso a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'appartenenza a un gruppo di Windows possono tuttavia continuare a connettersi come gruppo dopo la revoca del proprio accesso individuale. In modo analogo, se il parametro *login* specifica il nome di un gruppo di Windows, i membri del gruppo a cui è stato concesso l'accesso separatamente all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potranno ancora connettersi.  
  
 Ad esempio, se l'utente di Windows **ADVWORKS\john** è un membro del gruppo di Windows **ADVWORKS\Admins** e **sp_revokelogin** revoca l'accesso di `ADVWORKS\john` :  
  
```  
sp_revokelogin [ADVWORKS\john]  
```  
  
 L'utente **ADVWORKS\john** può comunque connettersi se a **ADVWORKS\Admins** è stato concesso l'accesso a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Analogamente, se per il gruppo di Windows **ADVWORKS\Admins** è stato revocato l'accesso, ma a **ADVWORKS\john** è stato concesso l'accesso, **ADVWORKS\john** può comunque connettersi.  
  
 Utilizzare **sp_denylogin** per impedire esplicitamente agli utenti di connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , indipendentemente dall'appartenenza a gruppi di Windows.  
  
 Impossibile eseguire **sp_revokelogin** in una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LOGIN nel server.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono rimosse le voci dell'account di accesso per l'utente di Windows `Corporate\MollyA`.  
  
```  
EXEC sp_revokelogin 'Corporate\MollyA';  
```  
  
 Oppure  
  
```  
EXEC sp_revokelogin [Corporate\MollyA];  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DROP LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/drop-login-transact-sql.md)   
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
