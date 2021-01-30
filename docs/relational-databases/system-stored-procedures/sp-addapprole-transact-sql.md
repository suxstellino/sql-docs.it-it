---
description: sp_addapprole (Transact-SQL)
title: sp_addapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addapprole_TSQL
- sp_addapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addapprole
ms.assetid: 24200295-9a54-4cab-9922-fb2e88632721
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5a48cfb80bab8e9a754cbcb8186de8cb2955f096
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182849"
---
# <a name="sp_addapprole-transact-sql"></a>sp_addapprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge un ruolo applicazione al database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [Create application role](../../t-sql/statements/create-application-role-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addapprole [ @rolename = ] 'role' , [ @password = ] 'password'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rolename = ] 'role'` Nome del nuovo ruolo applicazione. *Role* è di **tipo sysname** e non prevede alcun valore predefinito. *Role* deve essere un identificatore valido e non può essere già presente nel database corrente.  
  
 I nomi dei ruoli applicazione possono includere da 1 a 128 caratteri, compresi lettere, simboli e numeri. I nomi dei ruoli non possono contenere una barra rovesciata ( \\ ) né essere null o una stringa vuota ('').  
  
`[ @password = ] 'password'` Password necessaria per attivare il ruolo applicazione. *password* è di **tipo sysname** e non prevede alcun valore predefinito. la *password* non può essere null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli utenti (e i ruoli) non si diversificano completamente dagli schemi. A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], gli schemi sono completamente distinti dai ruoli. Questa nuova architettura si riflette nel funzionamento dell'istruzione CREATE APPLICATION ROLE. Questa istruzione sostituisce **sp_addapprole**.  
  
 Per mantenere la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , **sp_addapprole** eseguirà le operazioni seguenti:  
  
-   Se uno schema con lo stesso nome del ruolo applicazione non esiste, tale schema verrà creato. Il nuovo schema sarà di proprietà del ruolo applicazione e sarà lo schema predefinito del ruolo applicazione.  
  
-   Se invece uno schema con lo stesso nome del ruolo applicazione esiste già, la procedura verrà interrotta.  
  
-   La complessità della password non viene controllata dal **sp_addapprole**. ma viene controllata dall'istruzione CREATE APPLICATION ROLE.  
  
 La *password* del parametro viene archiviata come hash unidirezionale.  
  
 Impossibile eseguire l'stored procedure **sp_addapprole** dall'interno di una transazione definita dall'utente.  
  
> [!IMPORTANT]  
>  L'opzione Microsoft ODBC **Encrypt** non è supportata da **SqlClient**. Se possibile, richiedere all'utente di immettere le credenziali del ruolo applicazione in fase di esecuzione. Evitare di archiviare le credenziali in un file. Se è necessario disporre di credenziali persistenti, crittografarle tramite le funzioni CryptoAPI.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY APPLICATION ROLE nel database. Se uno schema con lo stesso nome e lo stesso proprietario del nuovo ruolo non esiste, è richiesta anche l'autorizzazione CREATE SCHEMA nel database.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il nuovo ruolo applicazione `SalesApp` con la password viene aggiunto `x97898jLJfcooFUYLKm387gf3` al database corrente.  
  
```  
EXEC sp_addapprole 'SalesApp', 'x97898jLJfcooFUYLKm387gf3' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
