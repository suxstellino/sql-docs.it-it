---
title: Abilitare TDE in SQL Server con EKM | Microsoft Docs
description: Abilitare Transparent Data Encryption in SQL Server per proteggere una chiave di crittografia del database usando una chiave asimmetrica archiviata in un modulo Extensible Key Management (EKM) con Transact-SQL.
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], TDE using an EKM
- TDE, EKM how to
- EKM, TDE how to
- Transparent Data Encryption, using EKM
ms.assetid: b892e7a7-95bd-4903-bf54-55ce08e225af
author: shohamMSFT
ms.author: shohamd
ms.openlocfilehash: ab339cd8fed2456d8794926b59f2bf730ec30b0c
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298894"
---
# <a name="enable-tde-on-sql-server-using-ekm"></a>Abilitare TDE in SQL Server con EKM
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo articolo descrive come abilitare Transparent Data Encryption (TDE) in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] per proteggere una chiave di crittografia del database tramite una chiave asimmetrica archiviata in un modulo Extensible Key Management (EKM) con [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 TDE esegue la crittografia dell'archiviazione di un intero database usando una chiave simmetrica detta "chiave di crittografia del database". È anche possibile proteggere la chiave di crittografia del database usando un certificato protetto dalla chiave master del database master. Per altre informazioni sulla protezione della chiave di crittografia del database tramite la chiave master del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md). Per informazioni sulla configurazione di TDE quando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è in esecuzione in una macchina virtuale di Azure, vedere [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md). Per informazioni sulla configurazione di Transparent Data Encryption usando una chiave nell'insieme di credenziali delle chiavi di Azure, vedere [Usare Connettore SQL Server con le funzionalità di crittografia SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md). 

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Per creare una chiave di crittografia del database e crittografare un database, è necessario essere un utente con privilegi elevati, ad esempio un amministratore di sistema, che possa essere autenticato dal modulo EKM.  
  
-   All'avvio il [!INCLUDE[ssDE](../../../includes/ssde-md.md)] deve aprire il database. A tal fine, è necessario creare le credenziali che dovranno essere autenticate dal modulo EKM e aggiungerle a un account di accesso basato su una chiave asimmetrica. Gli utenti non possono eseguire l'accesso usando tale account, ma il [!INCLUDE[ssDE](../../../includes/ssde-md.md)] sarà in grado di eseguire l'autenticazione con il dispositivo EKM.  
  
-   In caso di perdita della chiave asimmetrica archiviata nel modulo EKM, il database non potrà essere aperto in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se il provider EKM consente di eseguire il backup della chiave asimmetrica, è consigliabile creare una copia di backup e archiviarla in una posizione sicura.  
  
-   Le opzioni e i parametri richiesti dal provider EKM possono differire da quanto indicato nell'esempio di codice seguente. Per altre informazioni, rivolgersi al provider EKM.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 In questo articolo vengono usate le autorizzazioni seguenti:  
  
-   Per modificare un'opzione di configurazione ed eseguire l'istruzione RECONFIGURE, è necessario disporre dell'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
-   È richiesta l'autorizzazione ALTER ANY CREDENTIAL.  
  
-   È richiesta l'autorizzazione ALTER ANY LOGIN.  
  
-   È necessaria l'autorizzazione CREATE ASYMMETRIC KEY.  
  
-   È necessaria l'autorizzazione CONTROL nel database per crittografare il database.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-enable-tde-using-ekm"></a>Per abilitare Transparent Data Encryption tramite Extensible Key Management  
  
1.  Copiare i file forniti dal provider EKM in un percorso appropriato nel computer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . In questo esempio viene usata la cartella **C:\EKM** .  
  
2.  Installare i certificati nel computer in base a quanto richiesto dal provider EKM.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non fornisce un provider EKM. Ogni provider EKM può richiedere procedure diverse per l'installazione, la configurazione e l'autorizzazione degli utenti.  Per completare questo passaggio, consultare la documentazione del provider EKM.  
  
3.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
4.  Sulla barra Standard fare clic su **Nuova query**.  
  
5.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Enable advanced options.  
    sp_configure 'show advanced options', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Enable EKM provider  
    sp_configure 'EKM provider enabled', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Create a cryptographic provider, which we have chosen to call "EKM_Prov," based on an EKM provider  
  
    CREATE CRYPTOGRAPHIC PROVIDER EKM_Prov   
    FROM FILE = 'C:\EKM_Files\KeyProvFile.dll' ;  
    GO  
  
    -- Create a credential that will be used by system administrators.  
    CREATE CREDENTIAL sa_ekm_tde_cred   
    WITH IDENTITY = 'Identity1',   
    SECRET = 'q*gtev$0u#D1v'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
    GO  
  
    -- Add the credential to a high privileged user such as your   
    -- own domain login in the format [DOMAIN\login].  
    ALTER LOGIN Contoso\Mary  
    ADD CREDENTIAL sa_ekm_tde_cred ;  
    GO  
    -- create an asymmetric key stored inside the EKM provider  
    USE master ;  
    GO  
    CREATE ASYMMETRIC KEY ekm_login_key   
    FROM PROVIDER [EKM_Prov]  
    WITH ALGORITHM = RSA_512,  
    PROVIDER_KEY_NAME = 'SQL_Server_Key' ;  
    GO  
  
    -- Create a credential that will be used by the Database Engine.  
    CREATE CREDENTIAL ekm_tde_cred   
    WITH IDENTITY = 'Identity2'   
    , SECRET = 'jeksi84&sLksi01@s'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
  
    -- Add a login used by TDE, and add the new credential to the login.  
    CREATE LOGIN EKM_Login   
    FROM ASYMMETRIC KEY ekm_login_key ;  
    GO  
    ALTER LOGIN EKM_Login   
    ADD CREDENTIAL ekm_tde_cred ;  
    GO  
  
    -- Create the database encryption key that will be used for TDE.  
    USE AdventureWorks2012 ;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM  = AES_128  
    ENCRYPTION BY SERVER ASYMMETRIC KEY ekm_login_key ;  
    GO  
  
    -- Alter the database to enable transparent data encryption.  
    ALTER DATABASE AdventureWorks2012   
    SET ENCRYPTION ON ;  
    GO  
    ```  
  
 Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
-   [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
-   [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
-   [ALTER DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  
