---
description: sp_migrate_user_to_contained (Transact-SQL)
title: sp_migrate_user_to_contained (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_migrate_user_to_contained
- sp_migrate_user_to_contained_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_migrate_user_to_contained
ms.assetid: b3a49ff6-46ad-4ee7-b6fe-7e54213dc33e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81450361252fcdc2d02a9d9e118839db3c5af64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195491"
---
# <a name="sp_migrate_user_to_contained-transact-sql"></a>sp_migrate_user_to_contained (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Converte un utente del database di cui è stato eseguito il mapping a un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un utente del database indipendente con password. In un database indipendente, utilizzare questa procedura per rimuovere le dipendenze nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene installato il database. **sp_migrate_user_to_contained** separa l'utente dall' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso originale, in modo che sia possibile amministrare separatamente impostazioni quali password e lingua predefinita per il database indipendente. **sp_migrate_user_to_contained** possibile utilizzare prima di trasferire il database indipendente in un'istanza diversa di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] per eliminare le dipendenze dagli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso dell'istanza corrente.  
  
> [!NOTE]
> Prestare attenzione quando si usa **sp_migrate_user_to_contained**, perché non sarà possibile invertire l'effetto. Questa procedura viene utilizzata solo in un database indipendente. Per altre informazioni, vedere [Database indipendenti](../../relational-databases/databases/contained-databases.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_migrate_user_to_contained [ @username = ] N'user' ,   
    [ @rename = ] { N'copy_login_name' | N'keep_name' } ,   
    [ @disablelogin = ] { N'disable_login' | N'do_not_disable_login' }   
```  
  
## <a name="arguments"></a>Argomenti  
 [**@username =** ] **N'**_User_*_'_*  
 Nome di un utente nel database indipendente corrente di cui è stato eseguito il mapping a un account di accesso con autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore è di **tipo sysname** e il valore predefinito è **null**.  
  
 [**@rename =** ] **N'**_copy_login_name_*_'_*  |  **N'**_keep_name_*_'_*  
 Quando un utente del database basato su un account di accesso dispone di un nome utente diverso da quello del nome account di accesso, utilizzare *keep_name* per mantenere il nome utente del database durante la migrazione. Utilizzare *copy_login_name* per creare il nuovo utente del database indipendente con il nome dell'account di accesso, anziché l'utente. Quando il nome utente di un utente del database basato su un account di accesso è uguale al nome dell'account di accesso, entrambe le opzioni consentono di creare l'utente del database indipendente senza la modifica del nome.  
  
 [**@disablelogin =** ] **N'**_disable_login_*_'_*  |  **N'**_do_not_disable_login_*_'_*  
 *disable_login* Disabilita l'account di accesso nel database master. Per connettersi quando l'account di accesso è disabilitato, la connessione deve fornire il nome del database indipendente come **catalogo iniziale** come parte della stringa di connessione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_migrate_user_to_contained** crea l'utente del database indipendente con password, indipendentemente dalle proprietà o dalle autorizzazioni dell'account di accesso. Ad esempio, la procedura può avere esito positivo se l'account di accesso è disabilitato o se all'utente viene negata l'autorizzazione **Connect** per il database.  
  
 **sp_migrate_user_to_contained** presenta le restrizioni seguenti.  
  
-   Il nome utente non può essere già esistente nel database.  
  
-   Impossibile convertire gli utenti predefiniti, ad esempio dbo e guest.  
  
-   Impossibile specificare l'utente nella clausola **Execute As** di un stored procedure firmato.  
  
-   L'utente non può essere proprietario di un stored procedure che include la clausola **Execute As Owner** .  
  
-   Impossibile utilizzare **sp_migrate_user_to_contained** in un database di sistema.  
  
## <a name="security"></a>Sicurezza  
 Quando si esegue la migrazione di utenti, fare attenzione a non disabilitare o eliminare tutti gli account di accesso di amministratore dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se tutti gli account di accesso vengono eliminati, vedere [connettersi a SQL Server quando gli amministratori di sistema sono bloccati](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md).  
  
 Se è presente l'account di accesso **BUILTIN\Administrators** , gli amministratori possono connettersi avviando l'applicazione utilizzando l'opzione **Esegui come amministratore** .  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **CONTROL SERVER** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-migrating-a-single-user"></a>R. Migrazione di un solo utente  
 Nell'esempio seguente viene eseguita la migrazione di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominato `Barry` a un database indipendente con password. Nell'esempio non viene modificato il nome utente e l'account di accesso viene mantenuto come abilitato.  
  
```sql  
sp_migrate_user_to_contained   
@username = N'Barry',  
@rename = N'keep_name',  
@disablelogin = N'do_not_disable_login' ;  
  
```  
  
### <a name="b-migrating-all-database-users-with-logins-to-contained-database-users-without-logins"></a>B. Migrazione di tutti gli utenti del database con account di accesso a utenti del database indipendente senza account di accesso  
 Nell'esempio seguente viene eseguita la migrazione di tutti gli utenti basati sugli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a utenti del database indipendente con password. Nell'esempio sono inclusi account di accesso non abilitati. L'esempio deve essere eseguito nel database indipendente.  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Database indipendenti](../../relational-databases/databases/contained-databases.md)  
  
  
