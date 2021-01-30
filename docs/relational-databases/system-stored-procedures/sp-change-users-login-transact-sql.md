---
description: sp_change_users_login (Transact-SQL)
title: sp_change_users_login (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/13/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_users_login
- sp_change_users_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_users_login
ms.assetid: 1554b39f-274b-4ef8-898e-9e246b474333
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 109b2a6aa015f8f66a327714264aa986060f9a0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203722"
---
# <a name="sp_change_users_login-transact-sql"></a>sp_change_users_login (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene eseguito il mapping di un utente di database esistente a un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
 > [!IMPORTANT]
 > [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) .  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_users_login [ @Action = ] 'action'   
    [ , [ @UserNamePattern = ] 'user' ]   
    [ , [ @LoginName = ] 'login' ]   
    [ , [ @Password = ] 'password' ]  
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @Action =]'*azione*'  
 Descrive l'azione che deve essere eseguita dalla procedura. *Action* è di tipo **varchar (10)**. l' *azione* può avere uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**Auto_Fix**|Collega una voce utente presente nella vista del catalogo di sistema sys.database_principals del database corrente a un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con lo stesso nome. Se non esiste un account di accesso con lo stesso nome, ne verrà creato uno. Esaminare il risultato dell'istruzione **Auto_Fix** per verificare che sia effettivamente stato eseguito il collegamento corretto. Evitare l'uso di **Auto_Fix** in situazioni sensibili alla sicurezza.<br /><br /> Quando si utilizza **Auto_Fix**, è necessario specificare *utente* e *password* se l'account di accesso non esiste già. in caso contrario, è necessario specificare l' *utente* , ma la *password* verrà ignorata. l' *account di accesso* deve essere null. l' *utente* deve essere un utente valido nel database corrente. Non è possibile eseguire il mapping dell'account di accesso a un altro utente.|  
|**Report**|Elenca gli utenti e gli ID di sicurezza (SID) corrispondenti disponibili nel database corrente e non collegati ad alcun account di accesso. l' *utente*, l' *account di accesso* e la *password* devono essere null o non specificati.<br /><br /> Per sostituire l'opzione del report con una query utilizzando le tabelle di sistema, confrontare le voci **sys.server_prinicpals** con le voci **sys.database_principals**.|  
|**Update_One**|Collega l' *utente* specificato nel database corrente a un account di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *accesso* esistente. è necessario specificare l' *utente* e l' *account di accesso* . la *password* deve essere null o non specificata.|  
  
 [ @UserNamePattern =]'*utente*'  
 Nome di un utente nel database corrente. *User* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @LoginName =]'*login*'  
 Nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* è di tipo **sysname** e il valore predefinito è NULL.  
  
 [ @Password =]'*password*'  
 Password assegnata a un nuovo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso creato specificando **Auto_Fix**. Se un account di accesso corrispondente esiste già, viene eseguito il mapping dell'utente e dell'account di accesso e la *password* viene ignorata. Se un account di accesso corrispondente non esiste, sp_change_users_login crea un nuovo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso e assegna la *password* come password per il nuovo account di accesso. *password* è di **tipo sysname** e non può essere null.  
  
> **IMPORTANTE** Usare sempre una [password complessa.](../../relational-databases/security/strong-passwords.md)
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|UserName|**sysname**|Nome dell'utente del database.|  
|UserSID|**varbinary(85)**|ID di sicurezza (SID) dell'utente.|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare sp_change_users_login per collegare un utente di database nel database corrente a un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se l'account di accesso di un utente è stato modificato, utilizzare sp_change_users_login per collegare l'utente al nuovo account di accesso senza perdere le autorizzazioni corrispondenti. Il nuovo *account di accesso* non può essere SA e l' *utente* non può essere dbo, guest o un utente INFORMATION_SCHEMA.  
  
 La stored procedure sp_change_users_login non può essere utilizzata per eseguire il mapping degli utenti del database a entità, certificati o chiavi asimmetriche a livello di Windows.  
  
 Non è possibile utilizzare sp_change_users_login con un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato da un'entità di Windows o con un utente creato mediante CREATE USER WITHOUT LOGIN.  
  
 La stored procedure sp_change_users_login non può essere eseguita in una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner. Solo i membri del ruolo predefinito del server sysadmin possono specificare l'opzione **Auto_Fix** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-showing-a-report-of-the-current-user-to-login-mappings"></a>R. Visualizzazione di un report dei mapping tra utente corrente e account di accesso  
 Nell'esempio seguente viene creato un report che include gli utenti del database corrente e i relativi ID di sicurezza (SID).  
  
```  
EXEC sp_change_users_login 'Report';  
```  
  
### <a name="b-mapping-a-database-user-to-a-new-sql-server-login"></a>B. Mapping tra un utente del database e un nuovo account di accesso di SQL Server  
 Nell'esempio seguente un utente del database viene associato a un nuovo account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'utente del database `MB-Sales`, sul quale inizialmente viene eseguito il mapping a un account di accesso diverso, viene eseguito il mapping all'account di accesso `MaryB`.  
  
```  
--Create the new login.  
CREATE LOGIN MaryB WITH PASSWORD = '982734snfdHHkjj3';  
GO  
--Map database user MB-Sales to login MaryB.  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Update_One', 'MB-Sales', 'MaryB';  
GO  
```  
  
### <a name="c-automatically-mapping-a-user-to-a-login-creating-a-new-login-if-it-is-required"></a>C. Mapping automatico tra un utente e un account di accesso e creazione di un nuovo account di accesso se necessario  
 Nell'esempio seguente viene illustrato come utilizzare l'opzione `Auto_Fix` per eseguire il mapping tra un utente esistente e un account di accesso con lo stesso nome oppure per creare l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominato `Mary` associato alla password `B3r12-3x$098f6` se l'account di accesso `Mary` non esiste.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Auto_Fix', 'Mary', NULL, 'B3r12-3x$098f6';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_helplogins &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
