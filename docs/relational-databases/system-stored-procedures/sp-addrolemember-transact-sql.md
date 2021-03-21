---
description: sp_addrolemember (Transact-SQL)
title: sp_addrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addrolemember_TSQL
- sp_addrolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrolemember
ms.assetid: a583c087-bdb3-46d2-b9e5-3921b3e6d10b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0174aef135fcf579b1363efb414ee1dcbee4ae26
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754061"
---
# <a name="sp_addrolemember-transact-sql"></a>sp_addrolemember (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Aggiunge un utente del database, un ruolo del database, un account di accesso di Windows o un gruppo di Windows a un ruolo del database nel database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
sp_addrolemember [ @rolename = ] 'role', [ @membername = ] 'security_account'  
```    

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argomenti  
 [ @rolename =]'*Role*'  
 Nome del ruolo del database nel database corrente. *Role* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [ @membername =]'*security_account*'  
 Account di sicurezza aggiunto al ruolo. *security_account* è di **tipo sysname** e non prevede alcun valore predefinito. *security_account* può essere un utente del database, un ruolo del database, un account di accesso di Windows o un gruppo di Windows.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Un membro aggiunto al ruolo tramite la stored procedure sp_addrolemember eredita le autorizzazioni del ruolo. Se il nuovo membro è un'entità a livello di Windows senza un corrispondente utente del database, verrà creato un utente del database, di cui tuttavia potrebbe non essere eseguito completamente il mapping all'account di accesso. Verificare sempre che l'account di accesso esista e che sia in grado di accedere al database.  
  
 Un ruolo non può includere sé stesso come membro. Tali definizioni "circolari" non sono valide anche quando l'appartenenza è indirettamente sottintesa da una o più appartenenze intermedie.  
  
 sp_addrolemember non è possibile aggiungere un ruolo predefinito del database, un ruolo predefinito del server o dbo a un ruolo.
  
 Utilizzare sp_addrolemember solo per aggiungere un membro a un ruolo del database. Per aggiungere un membro a un ruolo del server, utilizzare [sp_addsrvrolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per l'aggiunta di membri ai ruoli del database flessibili è necessario uno degli elementi seguenti:  
  
-   Appartenenza al ruolo predefinito del database db_securityadmin o db_owner.  
  
-   Appartenenza al ruolo proprietario del ruolo.  
  
-   Autorizzazione **ALTER ANY ROLE** oppure autorizzazione **ALTER** per il ruolo.  
  
 Per l'aggiunta di membri ai ruoli predefiniti del database è necessaria l'appartenenza al ruolo predefinito del database db_owner.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-adding-a-windows-login"></a>R. Aggiunta di un account di accesso di Windows  
 Nell'esempio seguente l'account di accesso di Windows viene aggiunto `Contoso\Mary5` al `AdventureWorks2012` database come utente `Mary5` . L'utente `Mary5` viene quindi aggiunto al ruolo `Production`.  
  
> [!NOTE]  
>  Poiché `Contoso\Mary5` è noto come utente del database `Mary5` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], è necessario specificare il nome utente `Mary5`. L'istruzione non verrà eseguita correttamente se non esiste un account di accesso `Contoso\Mary5`. Eseguire un test mediante un account di accesso dal dominio.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE USER Mary5 FOR LOGIN [Contoso\Mary5] ;  
GO  
```  
  
### <a name="b-adding-a-database-user"></a>B. Aggiunta di un utente del database  
 Nell'esempio seguente l'utente del database `Mary5` viene aggiunto al ruolo del database `Production` nel database corrente.  
  
```sql  
EXEC sp_addrolemember 'Production', 'Mary5';  
```  
  
## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-adding-a-windows-login"></a>C. Aggiunta di un account di accesso di Windows  
 Nell'esempio seguente l'account di accesso viene aggiunto `LoginMary` al `AdventureWorks2008R2` database come utente `UserMary` . L'utente `UserMary` viene quindi aggiunto al ruolo `Production`.  
  
> [!NOTE]  
>  Poiché l'account `LoginMary` di accesso è noto come utente del database `UserMary` nel [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] database, è necessario specificare il nome utente `UserMary` . L'istruzione non verrà eseguita correttamente se non esiste un account di accesso `Mary5`. Gli account di accesso e gli utenti di solito hanno lo stesso nome. Questo esempio usa nomi diversi per distinguere le azioni che interessano l'accesso e l'utente.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE USER UserMary FOR LOGIN LoginMary ;  
GO  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
### <a name="d-adding-a-database-user"></a>D. Aggiunta di un utente del database  
 Nell'esempio seguente l'utente del database `UserMary` viene aggiunto al ruolo del database `Production` nel database corrente.  
  
```sql  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md)  
  
  
