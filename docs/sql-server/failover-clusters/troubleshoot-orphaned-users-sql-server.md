---
title: Risolvere i problemi relativi agli utenti isolati
description: Gli utenti isolati si verificano quando un account di accesso utente del database non esiste più nel database master. Questo argomento illustra come identificare e risolvere gli utenti isolati.
ms.custom: seo-lt-2019
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- orphaned users [SQL Server]
- logins [SQL Server], orphaned users
- troubleshooting [SQL Server], user accounts
- user accounts [SQL Server], orphaned users
- failover [SQL Server], managing metadata
- database mirroring [SQL Server], metadata
- users [SQL Server], orphaned
ms.assetid: 11eefa97-a31f-4359-ba5b-e92328224133
author: cawrites
ms.author: chadam
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 3067428c780c675b7601f66abc023a07b056eabe
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753201"
---
# <a name="troubleshoot-orphaned-users-sql-server"></a>Risolvere i problemi relativi agli utenti isolati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli utenti isolati (orfani) appaiono quando un utente del database dipende da un account di accesso nel database **master** , ma tale account di accesso non esiste più in **master**. Ciò può verificarsi quando viene eliminato l'account di accesso o quando il database viene spostato in un altro server in cui l'accesso non esiste. In questo argomento viene descritto come trovare gli utenti isolati e come riassociarli agli account di accesso.  
  
> [!NOTE]  
>  Per ridurre la creazione di utenti isolati, definire utenti del database indipendente per i database che potrebbero essere spostati. Per altre informazioni, vedere [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="background"></a>Background  
 Per connettersi a un database in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando un'entità di sicurezza (identità utente del database) basata su un account di accesso, l'entità deve avere un account di accesso valido nel database **master** . Tale account di accesso viene usato nel processo di autenticazione, che verifica l'identità dell'entità e determina se è autorizzata a connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un'istanza del server sono riportati nella vista del catalogo **sys.server_principals** e nella vista di compatibilità **sys.sql_logins** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consentono di accedere a database singoli come "utente database", che viene mappato all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sono previste tre eccezioni a questa regola:  
  
-   Utenti del database indipendente  
  
     Gli utenti del database indipendente eseguono l'autenticazione a livello di database utente e non sono associati agli account di accesso. Questo approccio è consigliato perché facilita lo spostamento dei database. Inoltre gli utenti del database indipendente non possono diventare utenti isolati. Tuttavia, tali utenti vanno ricreati per ogni database. Ciò può risultare poco pratico in un ambiente con molti database.  
  
-   Account **guest** .  
  
     Se attivato nel database, consente agli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non mappati su un utente del database di accedere al database come utenti **guest** . L'account **guest** è disattivato per impostazione predefinita.  
  
-   Appartenenza ai gruppi di Microsoft Windows.  
  
     Un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato da un utente di Windows può accedere a un database se tale utente di Windows è membro di un gruppo di Windows a sua volta utente del database.  
  
 Le informazioni relative al mapping di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un utente del database sono archiviate all'interno del database. Includono il nome dell'utente del database e il SID dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrispondente. Le autorizzazioni di tale utente del database vengono applicate come autorizzazioni nel database.  
  
 Un utente del database (basato su un account di accesso) il cui account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrispondente non è definito o è definito in modo errato in un'istanza del server non potrà accedere a tale istanza. Questo utente viene definito *utente orfano* del database nell'istanza del server. È possibile che si verifichi l'isolamento se l'utente del database è mappato a un SID di accesso non presente nell'istanza di `master` . Un utente del database può diventare isolato dopo il ripristino o il collegamento del database a un'istanza diversa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nella quale non è mai stato creato l'account di accesso. Inoltre un utente del database può diventare isolato (orfano) se l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrispondente viene rimosso. Anche se viene ricreato, l'account di accesso avrà un SID diverso, pertanto l'utente del database resterà isolato.  
  
## <a name="detect-orphaned-users"></a>Rilevare gli utenti isolati  

**Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e PDW**

Per rilevare gli utenti isolati (orfani) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in base agli account di accesso con autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mancanti, eseguire l'istruzione seguente nel database utente:  
  
```  
SELECT dp.type_desc, dp.SID, dp.name AS user_name  
FROM sys.database_principals AS dp  
LEFT JOIN sys.server_principals AS sp  
    ON dp.SID = sp.SID  
WHERE sp.SID IS NULL  
    AND authentication_type_desc = 'INSTANCE';  
```  
  
 Nell'output sono elencati gli utenti autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e gli identificatori di sicurezza (SID) corrispondenti disponibili nel database corrente e non collegati ad alcun account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

**Per database SQL e Azure Synapse Analytics**

La tabella `sys.server_principals` non è disponibile nel database SQL o in Azure Synapse Analytics. Identificare gli utenti isolati (orfani) in questi ambienti con i passaggi seguenti:

1. Connettersi al database `master` e selezionare i SID degli account di accesso con la query seguente:
    ```
    SELECT sid 
    FROM sys.sql_logins 
    WHERE type = 'S'; 
    ```

2. Connettersi al database utente ed esaminare i SID degli utenti nella tabella `sys.database_principals` tramite la query seguente:

    ```
    SELECT name, sid, principal_id
    FROM sys.database_principals 
    WHERE type = 'S' 
      AND name NOT IN ('guest', 'INFORMATION_SCHEMA', 'sys')
      AND authentication_type_desc = 'INSTANCE';
    ```

3. Confrontare i due elenchi per determinare se nella tabella `sys.database_principals` del database utente sono presenti SID utente privi di SID di accesso corrispondente nella tabella `sql_logins` del database master. 
  
## <a name="resolve-an-orphaned-user"></a>Risolvere un utente isolato  
Nel database master usare l'istruzione [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) con l'opzione SID per ricreare un account di accesso mancante, fornendo il `SID` dell'utente database ottenuto nella sezione precedente:  
  
```  
CREATE LOGIN <login_name>   
WITH PASSWORD = '<use_a_strong_password_here>',  
SID = <SID>;  
```  
  
 Per eseguire il mapping di un utente isolato (orfano) a un account di accesso già esistente in **master**, eseguire l'istruzione [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) , specificando il nome dell'account di accesso.  
  
```  
ALTER USER <user_name> WITH Login = <login_name>;  
```  
  
 Quando si ricrea un account di accesso mancante, l'utente può accedere al database usando la password specificata. L'utente può quindi modificare la password dell'account di accesso mediante l'istruzione ALTER LOGIN.  
  
```  
ALTER LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
```  
  
> [!IMPORTANT]  
>  Qualsiasi account di accesso è autorizzato a modificare la propria password. Tuttavia, solo gli account di accesso con autorizzazione `ALTER ANY LOGIN` possono modificare la password dell'account di accesso di un altro utente. Solo i membri del ruolo **sysadmin** possono tuttavia modificare le password dei membri del ruolo **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_change_users_login &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)   
 [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)   
 [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) [sys.syslogins &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslogins-transact-sql.md)  
  
  
